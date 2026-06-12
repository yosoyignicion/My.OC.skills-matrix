---
name: caching-patterns-write-through
description: "Cache-aside (lazy loading): application checks cache first on read; on miss, loads from database and populates cache"
---
# Caching Patterns: Write-Through, Write-Behind & Cache-Aside

## Semantic Triggers
```
cache aside lazy loading patrón, write through cache actualización síncrona, write behind cache escritura asíncrona, cache invalidation strategies, redis caching patterns, cache stampede protección
```

---

## 1. Definición Teórica

Cache-aside (lazy loading): application checks cache first on read; on miss, loads from database and populates cache. Write-Through: writes go to cache first, then synchronously to database — ensuring cache is always consistent with DB. Write-Behind: writes go to cache, asynchronously batched to database — highest throughput but risk of data loss on cache failure. Each pattern balances consistency vs throughput. Cache invalidation (TTL, event-driven) prevents stale data. Cache stampede protection prevents multiple concurrent reconstructions.

---

## 2. Implementación de Referencia

TypeScript with Redis implementing all three caching patterns with invalidation and stampede protection.

### Ejemplo Práctico Avanzado

```typescript
import { createClient } from 'redis';

type RedisClient = ReturnType<typeof createClient>;

// ===== CACHE-ASIDE (Lazy Loading) =====
// Best for: read-heavy, infrequent writes
class CacheAside<T> {
  constructor(
    private redis: RedisClient,
    private ttlSeconds = 300  // 5 min default
  ) {}

  async get(key: string, loader: () => Promise<T>): Promise<T> {
    const cached = await this.redis.get(key);
    if (cached !== null) return JSON.parse(cached) as T;

    // Cache miss — load from DB
    const value = await loader();

    // Populate cache (fire and forget)
    await this.redis.setEx(key, this.ttlSeconds, JSON.stringify(value));
    return value;
  }

  async invalidate(key: string): Promise<void> {
    await this.redis.del(key);
  }

  async invalidatePattern(pattern: string): Promise<void> {
    const keys = await this.redis.keys(pattern);
    if (keys.length > 0) await this.redis.del(keys);
  }
}

// ===== WRITE-THROUGH =====
// Best for: consistent reads, frequent reads
class WriteThroughCache<T extends { id: string }> {
  constructor(
    private redis: RedisClient,
    private db: Database,
    private ttlSeconds = 300
  ) {}

  async get(key: string): Promise<T | null> {
    const cached = await this.redis.get(key);
    return cached ? JSON.parse(cached) as T : null;
  }

  async upsert(key: string, data: T): Promise<T> {
    // 1. Write to cache first
    await this.redis.setEx(key, this.ttlSeconds, JSON.stringify(data));

    // 2. Write to database synchronously
    await this.db.transaction(async (tx) => {
      await tx
        .insertInto('entities')
        .values({ id: data.id, data: JSON.stringify(data) })
        .onConflict((oc) => oc.doUpdateSet({ data: JSON.stringify(data) }))
        .execute();
    });

    return data;
  }

  async delete(key: string): Promise<void> {
    await this.redis.del(key);
    await this.db.deleteFrom('entities').where('key', '=', key).execute();
  }
}

// ===== WRITE-BEHIND (Write-Back) =====
// Best for: write-heavy, high throughput
class WriteBehindCache<T extends { id: string }> {
  private writeQueue: T[] = [];
  private flushing = false;

  constructor(
    private redis: RedisClient,
    private db: Database,
    private flushIntervalMs = 5000,
    private maxBatchSize = 100
  ) {
    setInterval(() => this.flush(), this.flushIntervalMs);
  }

  async set(key: string, data: T): Promise<void> {
    // Write to cache immediately
    await this.redis.setEx(key, 3600, JSON.stringify(data));

    // Queue for async DB write
    this.writeQueue.push(data);
    if (this.writeQueue.length >= this.maxBatchSize) {
      await this.flush();
    }
  }

  async get(key: string): Promise<T | null> {
    const cached = await this.redis.get(key);
    return cached ? JSON.parse(cached) as T : null;
  }

  private async flush(): Promise<void> {
    if (this.flushing || this.writeQueue.length === 0) return;
    this.flushing = true;

    const batch = this.writeQueue.splice(0, this.maxBatchSize);
    try {
      await this.db.transaction(async (tx) => {
        for (const data of batch) {
          await tx
            .insertInto('entities')
            .values({ id: data.id, data: JSON.stringify(data) })
            .onConflict((oc) => oc.doUpdateSet({ data: JSON.stringify(data) }))
            .execute();
        }
      });
    } catch (err) {
      console.error('Write-behind flush failed, re-queuing:', err);
      this.writeQueue.unshift(...batch);  // re-queue on failure
    } finally {
      this.flushing = false;
    }
  }

  async flushNow(): Promise<void> {
    await this.flush();
  }
}

// ===== CACHE STAMPEDE PROTECTION =====
class StampedeProtectedCache<T> {
  constructor(
    private redis: RedisClient,
    private ttlSeconds = 300,
    private lockTtlSeconds = 10
  ) {}

  async get(key: string, loader: () => Promise<T>): Promise<T> {
    const cached = await this.redis.get(key);
    if (cached !== null) {
      const parsed = JSON.parse(cached);
      // Early refresh if near expiry (probabilistic)
      if (this.shouldEarlyRefresh(parsed._meta)) {
        this.refreshInBackground(key, loader);
      }
      return parsed.data as T;
    }

    // Lock to prevent stampede
    const lockKey = `lock:${key}`;
    const acquired = await this.redis.setNX(lockKey, '1', { EX: this.lockTtlSeconds });
    if (!acquired) {
      // Wait and retry
      await sleep(50);
      return this.get(key, loader);
    }

    try {
      const value = await loader();
      await this.redis.setEx(key, this.ttlSeconds, JSON.stringify({
        data: value,
        _meta: { createdAt: Date.now(), ttl: this.ttlSeconds * 1000 },
      }));
      return value;
    } finally {
      await this.redis.del(lockKey);
    }
  }

  private shouldEarlyRefresh(meta: { createdAt: number; ttl: number }): boolean {
    const age = Date.now() - meta.createdAt;
    const threshold = meta.ttl * 0.75;
    return age > threshold;
  }

  private async refreshInBackground(key: string, loader: () => Promise<T>): Promise<void> {
    try {
      const value = await loader();
      await this.redis.setEx(key, this.ttlSeconds, JSON.stringify({
        data: value,
        _meta: { createdAt: Date.now(), ttl: this.ttlSeconds * 1000 },
      }));
    } catch (err) {
      console.error('Background refresh failed:', err);
    }
  }
}

// ===== EVENT-DRIVEN INVALIDATION =====
class CacheInvalidator {
  constructor(private redis: RedisClient) {}

  subscribeToChanges(): void {
    // Listen to DB changes via PostgreSQL NOTIFY
    this.db.listen('data_changes', async (payload) => {
      const { table, id } = JSON.parse(payload);
      const key = `${table}:${id}`;
      await this.redis.del(key);

      // Also invalidate list patterns
      const listKeys = await this.redis.keys(`${table}:list:*`);
      if (listKeys.length > 0) await this.redis.del(listKeys);
    });
  }
}
```

**Fuente oficial:** https://redis.io/docs/manual/patterns/

### Alternativa de Implementación Específica

Python with `aioredis` and `fastapi-cache` decorator. Use `cachetools.TTLCache` for in-process caching with TTL.

---

## 3. Trade-offs y Decisiones de Arquitectura

| Aspecto | Recomendación |
|---|---|
| **Cuándo usar** | Read-heavy workloads, APIs con alta latencia de DB, sistemas donde la consistencia eventual es aceptable |
| **Cuándo evitar** | Datos que cambian frecuentemente (causa invalidación constante), sistemas con requerimientos de consistencia fuerte, datasets pequeños |
| **Alternativas** | CDN (caching a nivel HTTP), In-process cache (memoria local, más rápido), Read replicas (escalar DB en vez de cachear) |
| **Coste/Complejidad** | Medio. Cache-aside es simple. Write-through añade latencia de escritura. Write-behind riesgo de pérdida de datos. Invalidación es el problema más difícil |

---

## 4. Preguntas Frecuentes (FAQ)

### Caso: Cache stampede en high traffic

**¿Qué ocasionó el error?**
Un key expiró y 1000 requests simultáneas intentaron recargar el cache, todas golpeando la DB simultáneamente.

**¿Cómo se solucionó?**
```typescript
// Probabilistic early expiration + lock
const stampedeKey = `stampede:${key}`;
if (jitter() < probabilisticChance) {
  // Only ~5% of requests trigger refresh
  refreshInBackground();
}

// O lock como en StampedeProtectedCache arriba
```

**¿Por qué funciona esta técnica?**
Probabilistic early expiration refresca antes de que expire, distribuyendo la carga. El lock garantiza solo una recarga simultánea.

### Caso: Stale reads después de write en cache-aside

**¿Qué ocasionó el error?**
Cache-aside nunca invalida en escritura, solo actualiza en lectura. Datos escritos directamente en DB no se reflejaban en cache.

**¿Cómo se solucionó?**
```typescript
// Write-through en writes + cache-aside en reads
class HybridCache {
  async read(key: string, loader: () => Promise<any>) {
    return this.cacheAside.get(key, loader);
  }

  async write(key: string, data: any, writer: () => Promise<void>) {
    // Write-through: update cache + DB
    await this.cache.setEx(key, this.ttl, JSON.stringify(data));
    await writer();
  }
}
```

**¿Por qué funciona esta técnica?**
Combinar cache-aside para reads con write-through para writes da lo mejor de ambos: reads flexibles, writes consistentes.

---

## 5. Vector de IA Agéntica

### Optimización de Contexto

- **Tokens a cargar:** ~820 tokens estimados al invocar este skill
- **Trigger de activación:** "cache aside", "write through", "write behind", "cache invalidation", "redis cache", "cache stampede"
- **Prioridad de carga:** Alta — patrón esencial para performance
- **Dependencias:** `08-ingenieria-herramientas/10-redis-caching-patterns`, `07-frontend-web-fullstack/05-state-management-frontend`

### Tool Integration

```json
{
  "tool_name": "caching-patterns-write-through",
  "description": "Implements caching patterns: Cache-Aside, Write-Through, Write-Behind, stampede protection, invalidation strategies",
  "triggers": ["cache aside", "write through", "write behind", "cache invalidation", "redis", "stampede"],
  "context_hint": "Inject when user asks about caching strategies or performance optimization",
  "output_format": "code examples with Redis implementation of all caching patterns",
  "max_tokens": 1800
}
```

### Prompt Snippet (carga rápida)

```
Cuando el usuario pregunte sobre patrones de caché o estrategias de cacheo, carga el skill caching-patterns-write-through
y responde siguiendo la sección de implementación de referencia. Prioriza ejemplos idiomáticos sobre teoría.
```

---

## 6. Uso en Terminal y GUI/Web

### Terminal (CLI)

```bash
# Redis cache inspection
redis-cli KEYS "user:*"
redis-cli TTL user:123
redis-cli MEMORY USAGE user:123

# Cache hit ratio
redis-cli INFO stats | grep -i hits
```

### GUI / Web

- **RedisInsight**: GUI para inspeccionar keys, TTL, memoria
- **Grafana + Redis Exporter**: Métricas de hit ratio, memory usage
- **Prometheus**: Monitorización de latencia de cache vs DB

### Hotkeys / Atajos

| Acción | Atajo CLI | Atajo GUI |
|---|---|---|
| List cached keys | `redis-cli KEYS "pattern:*"` | RedisInsight Browser |
| Check TTL | `redis-cli TTL key` | RedisInsight → Key → TTL |

---

## 7. Cheatsheet Rápido

```typescript
// Cache-Aside: read → cache? hit? return : load DB → set cache → return
// Write-Through: write → set cache → write DB (sync)
// Write-Behind: write → set cache → queue DB write (async batch)
// Invalidation: del cache key on DB write (event-driven or TTL)
// Stampede: lock + probabilistic early refresh
```

---

## 8. Skills Relacionados

| Skill ID | Relación | ¿Cargar junto? |
|---|---|---|
| `08-ingenieria-herramientas/10-redis-caching-patterns` | Dependiente | Sí |
| `02-arquitectura-diseno/25-caching-patterns-write-through` | — | — |
| `03-sistemas-distribuidos/22-distributed-cache-redis-cluster` | Complementario | Sí |
| `07-frontend-web-fullstack/05-state-management-frontend` | Complementario | No |
| `02-arquitectura-diseno/31-rest-api-design` | Complementario | No |

---

## 9. Metadatos del Skill

```yaml
---
id: caching-patterns-write-through
domain: 02-arquitectura-diseno
version: 1.0.0
created: 2026-06-12
updated: 2026-06-12
author: opencode-agent
status: active
archive_after: 2026-08-11
source: oficial
tags: [caching, cache-aside, write-through, write-behind, redis, invalidation, stampede]
---
```

---

*Template v1.0 — 9 secciones. Última actualización: 2026-06-12*
