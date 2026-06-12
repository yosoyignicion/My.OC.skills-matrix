---
name: prisma-orm-database
description: "Prisma ORM resuelve el problema de acceder a bases de datos relacionales desde TypeScript/JavaScript con type-safety total y latencia mínima"
---
# prisma-orm-database

## Semantic Triggers
```
Prisma schema model datasource generator, migrations prisma migrate dev deploy, relations one-to-many many-to-many @relation, cursor pagination findMany skip take, interactive transactions $transaction, Prisma extensions $extends middleware soft delete
```

---

## 1. Definición Teórica

Prisma ORM resuelve el problema de acceder a bases de datos relacionales desde TypeScript/JavaScript con type-safety total y latencia mínima. El principio fundamental es *schema-first*: un único archivo `schema.prisma` define modelos, relaciones, índices y generadores, del cual se genera un cliente TypeScript con tipos completos. A diferencia de ORMs tradicionales (TypeORM, Sequelize), Prisma no usa decoradores ni classes de entidad — el schema es la única fuente de verdad. Arquitectónicamente, Prisma consta de tres capas: el schema (DSL declarativo), el migrator (gestión de cambios DB) y el cliente (generado, type-safe, con query engine en Rust). Existe como reemplazo idiomático para el stack TypeScript+PostgreSQL que valora seguridad de tipos sobre flexibilidad de query.

## 2. Implementación de Referencia

La implementación recomendada usa schema-first con `prisma.schema`. Auto-generated TypeScript client. Migraciones: `prisma migrate dev` (dev), `prisma migrate deploy` (CI). Relations con `@relation`. Cursor pagination para performance. Interactive transactions para atomicidad multi-step. Extensions para cross-cutting concerns (soft delete, logging).

### Ejemplo Práctico Avanzado

```prisma
generator client { provider = "prisma-client-js" }
datasource db { provider = "postgresql" url = env("DATABASE_URL") }

model User {
  id        String   @id @default(uuid()) @db.Uuid
  email     String   @unique
  name      String?
  posts     Post[]
  profile   Profile?
  createdAt DateTime @default(now()) @map("created_at")
  updatedAt DateTime @updatedAt @map("updated_at")
  deletedAt DateTime? @map("deleted_at")
  @@map("users")
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  author    User     @relation(fields: [authorId], references: [id])
  authorId  String   @map("author_id")
  tags      String[]
  @@index([authorId])
  @@map("posts")
}
```

```typescript
// Cursor pagination
const posts = await prisma.post.findMany({
  take: 20, skip: 1, cursor: { id: cursor },
  orderBy: { createdAt: "desc" },
  where: { published: true, title: { contains: query, mode: "insensitive" } },
  include: { author: { select: { name: true } } },
})

// Interactive transaction
await prisma.$transaction(async (tx) => {
  await tx.user.update({ where: { id }, data: { balance: { increment: -amount } } })
  await tx.log.create({ data: { userId: id, amount } })
})
```

**Fuente oficial:** https://www.prisma.io/docs — https://www.prisma.io/docs/orm/prisma-client

### Alternativa de Implementación Específica

Para proyectos que prefieren SQL explícito sobre query builder, usar `Drizzle ORM` (SQL-like, schema inferido de TypeScript, bundle pequeño). Para proyectos legacy con esquemas existentes, TypeORM con decorators y active record pattern puede ser más natural. Para proyectos que necesitan multi-tenancy complejo con schemas separados, Prisma raw queries con `$queryRaw` permiten control total.

---

## 3. Trade-offs y Decisiones de Arquitectura

| Aspecto | Recomendación |
|---|---|
| **Cuándo usar** | Proyectos TypeScript/React/Next.js con PostgreSQL, equipos que priorizan type-safety, APIs que requieren tipado fuerte desde DB hasta UI, proyectos nuevos sin esquemas legacy |
| **Cuándo evitar** | Tablas con nombres dinámicos (multi-tenancy por schema); queries extremadamente complejas (JSON anidado, window functions, FTS) — usar raw SQL; equipos que prefieren SQL directo sobre abstracciones |
| **Alternativas** | Drizzle ORM: SQL-first, schema inferido de TS, bundle pequeño; TypeORM: decoradores, active record, maduro pero verboso; Kysely: query builder type-safe sin ORM pesado; Prisma + PgTyped: raw SQL con tipos generados |
| **Coste/Complejidad** | Bajo para CRUD simple; medio para relaciones anidadas y cursor pagination; alto para migrations en equipos grandes (merge conflicts en schema.prisma) y queries raw que pierden type-safety |

---

## 4. Preguntas Frecuentes (FAQ)

### Caso: N+1 query problem con include anidados

**¿Qué ocasionó el error?**
`prisma.user.findMany({ include: { posts: true } })` genera 1 + N queries (N = usuarios). Prisma no hace JOIN — hace fetch separado por relación.

**¿Cómo se solucionó?**
Usar `include` directamente (Prisma optimiza automáticamente a través del query engine con batching). El query engine de Prisma usa DataLoader internamente para batching. Alternativamente, usar `join` raw SQL para relaciones específicas.

**¿Por qué funciona esta técnica?**
El query engine de Prisma en Rust ejecuta las queries hijas en batch, no en secuencia. Para la mayoría de los casos, `include` es suficientemente eficiente.

### Caso: prisma migrate dev — conflicto en migrations al hacer rebase

**¿Qué ocasionó el error?**
Dos desarrolladores crearon migrations en paralelo con timestamps similares. Al hacer rebase, `prisma migrate dev` detecta un hueco en la secuencia.

**¿Cómo se solucionó?**
```bash
# Resetear migrations locales y regenerar desde schema actual
prisma migrate reset --force
prisma migrate dev --name init
```

**¿Por qué funciona esta técnica?**
`prisma migrate reset` recrea la DB desde cero aplicando todas las migrations. Al regenerar, Prisma crea una única migration que refleja el schema actual.

---

## 5. Vector de IA Agéntica

### Optimización de Contexto

- **Tokens a cargar:** ~700 tokens estimados al invocar este skill
- **Trigger de activación:** Prisma schema, modelo, migración, query con relaciones
- **Prioridad de carga:** Alta — Prisma es el ORM TypeScript dominante en 2026
- **Dependencias:** Cargar junto con `postgresql-advanced` si se necesitan queries raw o índices personalizados

### Tool Integration

```json
{
  "tool_name": "prisma-orm-database",
  "description": "Define modelos Prisma, migraciones, queries type-safe y transacciones para TypeScript",
  "triggers": ["prisma", "orm", "database schema", "migration", "type-safe database", "schema.prisma"],
  "context_hint": "Inyectar schema ejemplo + queries findMany/transaction cuando el usuario necesite acceso a DB",
  "output_format": "markdown",
  "max_tokens": 1000
}
```

### Prompt Snippet (carga rápida)

```
Cuando el usuario pregunte sobre acceso a base de datos desde TypeScript, carga el skill prisma-orm-database
y responde siguiendo la sección de implementación de referencia con schema + queries type-safe.
```

---

## 6. Uso en Terminal y GUI/Web

### Terminal (CLI)

```bash
# Inicializar Prisma en proyecto existente
npx prisma init

# Schema change → migration + apply
npx prisma migrate dev --name add-user-table

# En CI: aplicar migrations sin generar nuevas
npx prisma migrate deploy

# Regenerar cliente (tras schema change sin migrate)
npx prisma generate

# Abrir Prisma Studio (GUI web local)
npx prisma studio

# Seed data
npx prisma db seed

# Reset DB (borra datos + re-aplica migrations)
npx prisma migrate reset --force

# Ver migrations status
npx prisma migrate status

# Formatear schema.prisma
npx prisma format
```

### GUI / Web

- **Prisma Studio:** `npx prisma studio` — GUI web local para navegar y editar datos con relaciones
- **VSCode:** Prisma extension (syntax highlighting, autocomplete, lint, format-on-save)
- **Prisma Data Platform:** Cloud dashboard, query profiling, connection pooling (accelerate)
- **Prisma Pulse:** CDC (Change Data Capture) — eventos en tiempo real desde PostgreSQL logical replication

### Hotkeys / Atajos

| Acción | Atajo CLI | Atajo GUI |
|---|---|---|
| Create migration | `prisma migrate dev` | Ctrl+Shift+P → Prisma: Create Migration |
| Open Studio | `prisma studio` | Ctrl+Shift+P → Prisma: Open Studio |
| Format schema | `prisma format` | Shift+Alt+F (VSCode + extension) |
| Generate client | `prisma generate` | Save file → auto-generate |
| Deploy migrations | `prisma migrate deploy` | CI pipeline step |

---

## 7. Cheatsheet Rápido

```prisma
model User {
  id    String @id @default(uuid()) @db.Uuid
  email String @unique
  posts Post[]
  @@map("users")
}
model Post {
  id       Int  @id @default(autoincrement())
  authorId String @map("author_id")
  author   User @relation(fields: [authorId], references: [id])
  @@index([authorId])
  @@map("posts")
}
```

```typescript
const user = await prisma.user.findUnique({ where: { email }, include: { posts: true } })
await prisma.$transaction(async (tx) => { /* atomic */ })
```

```bash
prisma init && prisma migrate dev --name init && prisma studio
```

---

## 8. Skills Relacionados

| Skill ID | Relación | ¿Cargar junto? |
|---|---|---|
| `postgresql-advanced` | Dependiente — Prisma usa PostgreSQL como datasource primario | Sí |
| `fastapi-rest-development` | Alternativa — FastAPI usa SQLAlchemy + Pydantic (Python vs TS) | No |
| `sqlite-sqlalchemy-persistence` | Alternativa — SQLAlchemy para Python, Prisma para TypeScript | No |
| `dotenv-environment-vars` | Complementario — DATABASE_URL en .env | Sí |

---

## 9. Metadatos del Skill

```yaml
---
id: prisma-orm-database
domain: 08-ingenieria-herramientas
version: 1.0.0
created: 2026-06-12
updated: 2026-06-12
author: opencode-agent
status: active
archive_after: 2026-08-11
source: old-skills/opencode-bak-skills/prisma
tags: [prisma, orm, typescript, database, postgresql, schema, migration, type-safe]
---
```

---

*Template v1.0 — 9 secciones. Última actualización: 2026-06-12*
