# Skills Matrix + Good Agents

> **250 skills técnicas + 14 design skills + 1 template. Catálogo que el agente carga bajo demanda vía router semántico, sin inflar el contexto del LLM.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Skills: 250 + 1 template](https://img.shields.io/badge/skills-250%20%2B%201%20template-purple)](./skills-matrix)
[![Design: 14](https://img.shields.io/badge/design-14-pink)](./design-skills)
[![Format: SKILL.md (9 sections)](https://img.shields.io/badge/format-9%20sections-blue)](./skills-matrix/00-standard-skill-template/SKILL.md)

---

## 📦 ¿Qué es esto?

Una **matriz de habilidades** (skills matrix) que tu agente de IA (opencode, Claude Code, Cline, etc.) puede consultar **bajo demanda** para resolver problemas técnicos específicos. Cada skill es un archivo `SKILL.md` con 9 secciones que cubren desde definición teórica hasta ejemplos prácticos, trade-offs, FAQ y cheatsheet.

En lugar de tener todo el conocimiento del agente en un solo prompt gigante, los skills se cargan **solo cuando se necesitan**, basándose en *triggers semánticos* que el agente detecta en tu consulta.

Las skills se activan por **triggers** (palabras clave en la consulta del usuario). El router en [`AGENTS.md`](./AGENTS.md) decide cuál cargar basándose en §2.1 (tabla de triggers) y §2.2 (reglas de carga: max 3 simultáneas, `predict-failure-risk` primero, etc).

---

## 🏗️ Arquitectura

```
opencode.jsonc  ← skills-matrix registrado en `skills.paths`
      │
~/.config/opencode/AGENTS.md  ← Router semántico global (~424 líneas, v3.0)
      │
skills-matrix/                ← 251 entries (250 skills + 1 template)
├── 00-standard-skill-template/
│   └── SKILL.md              ← Template de 9 secciones
├── io-multiplexing-iouring/
│   └── SKILL.md
├── react-ui-development/
│   └── SKILL.md
├── ... (248 más)
└── zero-token-optimization/
    └── SKILL.md
```

### Flujo de carga

```
Usuario pregunta → Agente escanea triggers semánticos en AGENTS.md
                        ↓
                  Coincidencia encontrada
                        ↓
                  Agente lee solo ese SKILL.md
                        ↓
                  Aplica patrones y responde
                        ↓
              (Siguiente pregunta → loop)
```

---

## 🧠 250 Skills en 8 Dominios

> Conteos exactos: `find skills-matrix -maxdepth 1 -mindepth 1 -type d | wc -l` → 251 (250 skills + 1 template).
> Lista completa auto-generada en [`INDEX.md`](./skills-matrix/INDEX.md).

### 01 — Sistemas Bajo Nivel (~33 skills)
Optimización a nivel de kernel, hardware y lenguajes de sistemas.

```
assembly-inline-optimizations     cpu-cache-locality-alignment      go-systems-production
ast-manipulation                  cpp-audio-development             hardware-timers-clock-precision
compilation-linking-loader        cpp-graphics-rendering            hardware-transactional-memory
concurrency-actor-model           cpp-memory-safety                 instruction-level-parallelism
io-multiplexing-iouring           garbage-collection-algorithms     io-scheduling-linux
ipc-shared-memory-pipes           kernel-bypass-dpdk                jit-compilation-engines
lock-free-data-structures         linux-ebpf-tracing                memory-raii-borrowing
modern-cpp-development            numa-architectures-tuning         performance-profiling-optimization
process-scheduler-namespaces      qt6-framework                     rust-systems-programming
signals-and-interrupts-handling   simd-vectorization                system-calls-overhead-tracing
virtual-memory-paging             webassembly-runtimes-sandboxing   zero-copy-serialization
```

### 02 — Arquitectura y Diseño (~37 skills)
Patrones de diseño, DDD, CQRS, Event Sourcing, arquitecturas, SOLID, API design.

```
api-gateway-bff-patterns          api-versioning-evolution-strategies  asynchronous-messaging-patterns
bulkhead-circuit-breaker-resilience caching-patterns-write-through     clean-architecture-principles
command-pattern-undo-redo         concurrency-patterns-pipelines       configuration-management
data-mapper-active-record         data-serialization-formats           ddd-tactical-patterns
dependency-injection-inversion    design-systems-atomic                domain-events-dispatching
error-handling-patterns           event-driven-cqrs                    event-sourcing-eventstore
gof-behavioral-patterns           gof-creational-patterns              gof-structural-patterns
hexagonal-architecture            idempotency-keys-processing          micro-frontends-routing
microservices-decomposition       modular-monolithic-design            multi-tenant-data-isolation
outbox-inbox-patterns             pipeline-filter-architecture          plugins-and-extensibility-architectures
reactive-programming-extensions   rest-api-design                      saga-orchestration-choreography
solid-deep-dive                   state-machine-workflows              state-management-patterns
structured-logging-patterns
```

### 03 — Sistemas Distribuidos (~36 skills)
Protocolos de red, consenso distribuido, streaming, colas, caching, Service Mesh.

```
api-idempotency-in-distributed-networks  backpressure-and-flow-control    cap-theorem-tradeoffs
cdn-edge-caching-georouting        change-data-capture-cdc              consistent-hashing-topologies
crdts-conflict-free-replicated     data-encryption-in-transit-mtls      data-lakehouses-parquet-iceberg
database-replication-lag-strategies database-sharding-partitioning       distributed-cache-redis-cluster
distributed-consensus-raft         distributed-locking-redlock          distributed-queues-rabbitmq-amqp
distributed-tracing-context-propagation distributed-transactions-2pc-3pc dns-routing-anycast-latency
fault-injection-chaos-engineering  gossip-protocols-membership          graphql-federation-gateways
grpc-protobuf                      http-client-patterns                 http3-quic
hybrid-logical-clocks              load-balancing-algorithms-l4-l7      message-brokers-kafka-internals
network-partitions-split-brain     pacelc-theorem-implications          rate-limiting-algorithms
saga-pattern-distributed-coordination service-mesh-envoy-sidecars       session-management-stateless-vs-stateful
vector-clocks-lamport-timestamps   websockets-sse-realtime              zero-trust-network-architectures
```

### 04 — DevOps Platform (~34 skills)
Kubernetes, GitOps, CI/CD, monitoring, IaC, service mesh, secret management.

```
artifact-registries-security      autoscaling-hpa-vpa-keda             bare-metal-vs-virtualization
blue-green-deployment-strategies  chaos-mesh-reliability-testing       cicd-declarative-pipelines
configmap-secrets-hot-reloading   container-internals-namespaces       container-orchestration-k8s-scheduling
container-runtimes-containerd-cri cost-optimization-finops-kubernetes ebpf-based-networking-cilium
git-workflows-conventional        gitops-declarative-reconciliation    immutable-infrastructure-packer
infrastructure-as-code-terraform  kubernetes-operators-controllers     load-testing-k6-distributed
log-aggregation-loki-elasticsearch mesh-data-planes-control-planes     monitoring-prometheus-metrics
monorepo-management               network-policies-segmentation        opentelemetry-distributed-tracing
package-management-helm-kustomize policy-as-code-opa-rego              progressive-delivery-canary
secret-management-vault-integration serverless-knative-cold-starts     service-discovery-dns-consul
slas-slis-slos-error-budgets      storage-classes-pv-pvc-csi
```

> ℹ️ `second-termux-processes/` (legacy v1) está en `.gitignore`. El proyecto usa `second-termux-v2` desde el bundle [`My.OpenCode.Conf`](https://github.com/yosoyignicion/My.OpenCode.Conf).

### 05 — IA Agéntica y Datos (~39 skills)
Agentes LLM, RAG, fine-tuning, vector DBs, memoria episódica, MCP, subagentes.

```
advanced-graph-rag                agent-benchmarking-evaluation        agent-human-in-the-loop-hitl
agent-memory-persistence-episodic agentic-multiloop-orchestration      ai-agent-state-recovery-checkpoints
autoprompting-engineering         bridge-mcp-engram-sync               context-token-budgeting
context7-mcp-docs                 curator-loop-hermes                  embeddings-similarity-metrics
engram-memory-system              guardrails-nemo-llamaguard           hybrid-search-sparse-dense
knowledge-graphs-neo4j            llm-fine-tuning-lora-qlora           llm-inference-engines-vllm
llm-integration-patterns          mcp-tools-protocol                   multi-agent-collaboration-protocols
nlp-pipeline-processing           ocs-identity-charter                 openclaw-isolation
plugins-extensibility-agent       predict-failure-risk                 prompt-compression-routing
quantization-gguf-awq-gptq        reinforcement-learning-human-feedback
retrieval-reranking-models        self-reflection-corrective-agents    semantic-chunking-embedding-pipelines
streaming-llm-outputs-sse         structured-outputs-json-schema       subagents-parallelism
synthetic-data-generation-pipelines tool-use-function-calling           vector-db-indexing-hnsw
zero-token-optimization
```

### 06 — Seguridad SDLC (~23 skills)
Threat modeling, OWASP, OAuth2/OIDC, cifrado, SAST/DAST, SBOM, hardening.

```
auth-jwt-oauth-detailed           compliance-auditing-frameworks       compliance-frameworks-soc2-iso27001
cryptography-symmetric-asymmetric data-masking-anonymization           defensive-security-hardening
dynamic-application-security-testing-dast
fuzzing-security-boundaries       identity-access-management-rbac-abac incident-response-forensics-logging
integration-testing-wiremock-testcontainers
key-management-kms-rotation       mutation-testing-pitest-stryker      oauth2-oidc-flows
owasp-top-10-mitigation           property-based-testing               rate-limiting-abuse-prevention
securing-cicd-pipelines           software-bill-of-materials-sbom      static-application-security-testing-sast
threat-modeling-stride            vulnerability-scanning-dependency-check
zero-trust-architecture-sdlc
```

### 07 — Frontend Web Fullstack (~12 skills)
React, Next.js, Tailwind, TypeScript, testing, Flutter, React Native, desktop.

```
a11y-accessibility-wcag           electron-desktop-apps               flutter-dart-mobile
next-js-app-router                playwright-e2e-testing              react-native-mobile
react-ui-development              rest-api-integration-client         state-management-frontend
tailwind-css-utility              tauri-rust-desktop                  typescript-type-system
```

### 08 — Ingeniería y Herramientas (~15 skills)
Bash, Python, pytest, FastAPI, asyncio, PostgreSQL, Prisma, Redis, Docker, SVG, SQLAlchemy.

```
async-python-concurrency          background-jobs-queues              bash-scripting-advanced
docker-compose-watch              dotenv-environment-vars             fastapi-rest-development
postgresql-advanced               prisma-orm-database                 pytest-testing-quality
python-packaging-pyproject        redis-caching-patterns              sqlite-sqlalchemy-persistence
svg-converter-rasterization       svg-generation-programmatic         typer-cli-applications
```

### 🎨 Bonus — 14 Design Skills (carpeta [`design-skills/`](./design-skills))

Proceden de portaciones bajo el protocolo dual-agente desde `Skills-o-extra/` (5 carpetas legacy: `elite-skills`, `master-skills`, `legend-skills`, `loyalty-skills`, `initiation-skills`).

```
advanced-effects       badge-system        color-basics         composition-layout
cultural-references    dark-mode           design-thinking      emotional-design
gamification-rewards   icon-symbolism      motion-ui            trends-forecasting
typography-phonk       visual-narrative
```

**Triggers**: `badge`, `icon`, `animación`, `tipografía`, `Octalysis`, `FOMO`, `Campbell`, `phonk`, `Y2K`, `Disney principles`, `Peirce`, `Norman 3 levels`, `WCAG`, `OLED`, `gamificación`, `brutalismo`, `retrowave`.

(Conteos aproximados — para cifras exactas usa `find` o el `AGENTS.md` del router.)

---

## 🧩 Estructura de cada SKILL.md

| Sección | Contenido |
|---|---|
| **Semantic Triggers** | Frases que el agente busca para activar el skill |
| **1. Definición Teórica** | 4 líneas de fundamento técnico preciso |
| **2. Implementación de Referencia** | Ejemplo práctico avanzado + alternativa oficial |
| **3. Trade-offs** | Cuándo usar, evitar, alternativas, coste |
| **4. FAQ** | Problema → Solución → Por qué funciona (con fuentes) |
| **5. Vector de IA Agéntica** | Tokens, triggers, prioridad, tool JSON, prompt snippet |
| **6. Terminal y GUI/Web** | CLI commands, atajos, IDEs, dashboards |
| **7. Cheatsheet Rápido** | <15 líneas para consulta exprés |
| **8. Skills Relacionados** | Dependencias y carga en cadena |
| **9. Metadatos** | YAML con id, versión, tags, fecha de expiración |

---

## ⚡ Instalación

### 1. Clonar

```bash
git clone https://github.com/yosoyignicion/My.OC.skills-matrix.git
cd My.OC.skills-matrix
```

### 2. Configurar opencode

Añadir a `~/.config/opencode/opencode.jsonc`:

```jsonc
{
  "skills": {
    "paths": [
      "/ruta/a/My.OC.skills-matrix/skills-matrix",
      "/ruta/a/My.OC.skills-matrix/design-skills"
    ]
  }
}
```

El instalador del bundle padre ya hace esto. Ver [`My.OpenCode.Conf`](https://github.com/yosoyignicion/My.OpenCode.Conf) `README.md → "Prompt maestro"`.

### 3. Instalar router global

```bash
cp AGENTS.md ~/.config/opencode/AGENTS.md
```

> El bundle `My.OpenCode.Conf` ya incluye un router global de 424 líneas (v3.0, protocolo dual-agente). Si lo usas, **no** copies este `AGENTS.md` encima — el del bundle es el canónico.

### 4. ¡Reiniciar opencode!

Los cambios de configuración se cargan al iniciar. Una vez dentro, el agente tendrá acceso a los 250 skills.

### Reglas de carga (router §2.2)

- **Max 3 skills simultáneas** — más infla el context sin beneficio.
- **`predict-failure-risk` primero** — cuando hay error/fix/debug, se carga antes que cualquier otra.
- **Skills de contexto (como `context7-mcp-docs`) antes que skills de patrón genérico** — la doc oficial > el patrón.
- **Si una skill falta una sección** del template, el router infiere de docs upstream (no rompe el flujo).

---

## 🔧 Personalización

### ¿Quieres solo un subconjunto de skills?

Elimina las carpetas de skills que no necesites. El router `AGENTS.md` se puede regenerar automáticamente.

### ¿Quieres modificar un skill?

Cada `SKILL.md` es independiente. Edita el que necesites siguiendo la estructura de 9 secciones. El template está en `00-standard-skill-template/`.

### ¿Quieres añadir un skill nuevo?

1. Crea `skills-matrix/tu-nuevo-skill/SKILL.md`
2. Sigue el template de 9 secciones
3. Añade la entrada en `AGENTS.md` (o regenera con el script)

### ¿Quieres que el agente cargue skills automáticamente?

Ajusta la `Prioridad de carga` en la sección 5 (Vector de IA Agéntica) de cada SKILL.md a `Alta`. Los skills con prioridad Alta se cargan inmediatamente, Media bajo demanda, Baja solo con trigger exacto.

---

## 📏 AGENTS.md — El Router Semántico

El archivo `AGENTS.md` actúa como **tabla de enrutamiento** para el agente. Cuando haces una pregunta, el agente escanea los *triggers semánticos* en el router y carga solo el skill relevante.

### ¿Por qué está optimizado para global?

- **<560 líneas**: entra cómodamente en el contexto del LLM sin inflarlo
- **Carga bajo demanda**: no se leen los 250 SKILL.md hasta que se necesitan
- **Triggers semánticos**: cada skill tiene 6-10 frases clave que activan su carga
- **Metadatos por skill**: prioridad, tokens estimados, líneas, dominio
- **Prioridades**: Alta (carga inmediata), Media (bajo demanda), Baja (trigger exacto)

### Aún mejorable según tus gustos

- **Añadir/quitar triggers**: modifica la sección `Semantic Triggers` de cada SKILL.md
- **Cambiar prioridades**: edita `Prioridad de carga` en sección 5
- **Reordenar dominios**: el orden de la tabla en AGENTS.md es el orden de búsqueda
- **Añadir skills propietarios**: skills de tu empresa o proyecto específico
- **Fusionar con otros routers**: puedes combinar varios AGENTS.md con `instructions: []`

---

## 🛠️ Mantenimiento y verificación

```bash
# Conteo actual
find skills-matrix -maxdepth 1 -mindepth 1 -type d | wc -l
# Esperado: 251 (250 skills + 1 template)

find design-skills -maxdepth 1 -mindepth 1 -type d | wc -l
# Esperado: 14

# Buscar skills huérfanas (sin trigger en el router)
comm -23 <(ls skills-matrix/ | grep -v "^00-" | sort) \
         <(grep -oP "(?<=^\| \`).+(?=\` \|)" AGENTS.md | sort -u)
# Si hay diferencias, esas skills existen pero no se auto-cargan.

# Skills sin frontmatter YAML válido
for d in skills-matrix/*/; do
  head -1 "$d/SKILL.md" 2>/dev/null | grep -q "^---" || echo "MALO: $d"
done
# Esperado: 0 outputs (todas tienen frontmatter)

# Regenerar INDEX.md
node scripts/regen-index.mjs
```

---

## 🤖 Prompt para tu agente: explorar este catálogo

Si quieres que **tu propio agente** (opencode, Claude Code, Cline, Cursor) entienda este catálogo, copia esta carpeta a tu workspace y pégale:

> **Copy-paste prompt (3 líneas):**
> ```
> Explore ~/path/to/My.OC.skills-matrix/. Read AGENTS.md (router) and the first SKILL.md from each of the 8 domains.
> Build a mental map: which skill covers which domain, and which triggers activate which.
> If you find a gap (a domain with no skill, or a stale trigger), propose a new SKILL.md
> following the 9-section template in skills-matrix/00-standard-skill-template/SKILL.md,
> and cache any third-party API research in engram with prefix "ctx7:".
> ```

**Por qué funciona:**

1. *"Read AGENTS.md (router) and the first SKILL.md from each of the 8 domains"* — muestra la estructura macro y un ejemplo representativo de cada categoría, sin cargar las 251 skills en el context.
2. *"Build a mental map: which skill covers which domain, and which triggers activate which"* — fuerza al agente a entender las **dos dimensiones** del catálogo: dominios (qué cubre) y triggers (cuándo se activa). Esto es exactamente la decisión que opencode toma cada vez que recibe una consulta.
3. *"propose a new SKILL.md following the 9-section template"* — convierte al agente en colaborador, no solo lector. El output es accionable.

> **Bonus:** si tu agente no es opencode, dile que las skills son Markdown y puede leerlas con su `read` tool nativa. No requiere MCP.

---

## 🔗 Repos relacionados

| Repo | Rol |
|---|---|
| [**My.OpenCode.Conf**](https://github.com/yosoyignicion/My.OpenCode.Conf) | Bundle v2.0.0 — distro portable de opencode con este catálogo + `engram+zerotoken` + `second-termux-v2` |
| **Este repo** | Mirror standalone de las 250 skills + 14 design skills, útil si solo quieres el catálogo |

---

## 📄 Licencia

**MIT** © 2026 ignicion — Ver [LICENSE](./LICENSE).

Catálogo construido sobre:

- [Anthropic's Agent Skills spec](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) — formato y convenciones
- [Context7](https://context7.com) — fuente de docs oficiales para validar APIs
- [opencode](https://opencode.ai) — runtime de skills
- 250 skills curadas + 14 design skills — autoría mixta (comunidad open-source + curación personal)

Cada skill individual es `SKILL.md` autocontenido — no requiere compilación, build step, ni dependencias. Es puro Markdown.

---

## 🤝 Contribuciones

¿Falta un skill? ¿Encontraste un error? ¿Mejoraste un ejemplo?

1. Haz fork
2. Crea tu rama (`git checkout -b feat/mi-skill`)
3. Sigue el template de 9 secciones
4. Commit con conventional commits
5. Abre un PR

---

*Proyecto generado con ❤️ por opencode — 250 skills, 8 dominios, +65k líneas de conocimiento técnico listo para tu agente.*
