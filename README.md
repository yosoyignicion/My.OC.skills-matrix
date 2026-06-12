# Skills Matrix + Good Agents

> **228 skills técnicos** organizados en 8 dominios, con router semántico integrado para agentes de IA. Carga bajo demanda, sin inflar el contexto del LLM.

---

## 📦 ¿Qué es esto?

Una **matriz de habilidades** (skills matrix) que tu agente de IA (opencode, Claude Code, Cline, etc.) puede consultar **bajo demanda** para resolver problemas técnicos específicos. Cada skill es un archivo `SKILL.md` con 9 secciones que cubren desde definición teórica hasta ejemplos prácticos, trade-offs, FAQ y cheatsheet.

En lugar de tener todo el conocimiento del agente en un solo prompt gigante, los skills se cargan **solo cuando se necesitan**, basándose en *triggers semánticos* que el agente detecta en tu consulta.

---

## 🏗️ Arquitectura

```
opencode.jsonc  ← skills-matrix registrado en `skills.paths`
      │
~/.config/opencode/AGENTS.md  ← Router semántico global (553 líneas)
      │
skills-matrix/                ← 228 skills
├── 00-standard-skill-template/
│   └── SKILL.md              ← Template de 9 secciones
├── io-multiplexing-iouring/
│   └── SKILL.md
├── react-ui-development/
│   └── SKILL.md
├── ... (228 skills)
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

## 🧠 228 Skills en 8 Dominios

### 01 — Sistemas Bajo Nivel (33 skills)
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

### 02 — Arquitectura y Diseño (37 skills)
Patrones de diseño, DDD, CQRS, Event Sourcing, arquitecturas, SOLID, API design.

```
api-gateway-bff-patterns          api-versioning-evolution-strategies  asynchronous-messaging-patterns
bulkhead-circuit-breaker          caching-patterns-write-through       clean-architecture-principles
command-pattern-undo-redo         concurrency-patterns-pipelines       configuration-management
data-mapper-active-record         data-serialization-formats           ddd-tactical-patterns
dependency-injection-inversion    design-systems-atomic                domain-events-dispatching
error-handling-patterns           event-driven-cqrs                    event-sourcing-eventstore
gof-behavioral-patterns           gof-creational-patterns              gof-structural-patterns
hexagonal-architecture            idempotency-keys-processing          micro-frontends-routing
microservices-decomposition       modular-monolithic-design            multi-tenant-data-isolation
outbox-inbox-patterns             pipeline-filter-architecture          plugins-and-extensibility
reactive-programming-extensions   rest-api-design                      saga-orchestration-choreography
solid-deep-dive                   state-machine-workflows              state-management-patterns
structured-logging-patterns
```

### 03 — Sistemas Distribuidos (36 skills)
Protocolos de red, consenso distribuido, streaming, colas, caching, Service Mesh.

```
api-idempotency-distributed       backpressure-flow-control            cap-theorem-tradeoffs
cdn-edge-caching-georouting       change-data-capture-cdc              consistent-hashing-topologies
crdts-conflict-free-replicated    data-encryption-in-transit-mtls      data-lakehouses-parquet-iceberg
database-replication-lag          database-sharding-partitioning       distributed-cache-redis-cluster
distributed-consensus-raft        distributed-locking-redlock          distributed-queues-rabbitmq
distributed-tracing-context       distributed-transactions-2pc-3pc     dns-routing-anycast-latency
fault-injection-chaos-engineering gossip-protocols-membership          graphql-federation-gateways
grpc-protobuf                     http-client-patterns                 http3-quic
hybrid-logical-clocks             load-balancing-algorithms-l4-l7      message-brokers-kafka-internals
network-partitions-split-brain    pacelc-theorem-implications          rate-limiting-algorithms
saga-pattern-distributed          service-mesh-envoy-sidecars          session-management
vector-clocks-lamport-timestamps  websockets-sse-realtime              zero-trust-network-architectures
```

### 04 — DevOps Platform (34 skills)
Kubernetes, GitOps, CI/CD, monitoring, IaC, service mesh, secret management.

```
artifact-registries-security      autoscaling-hpa-vpa-keda             bare-metal-vs-virtualization
blue-green-deployment             chaos-mesh-reliability-testing       cicd-declarative-pipelines
configmap-secrets-hot-reloading   container-internals-namespaces       container-orchestration-k8s
container-runtimes-containerd     cost-optimization-finops             ebpf-networking-cilium
git-workflows-conventional        gitops-declarative-reconciliation    immutable-infrastructure-packer
infrastructure-as-code-terraform  kubernetes-operators-controllers     load-testing-k6-distributed
log-aggregation-loki-es           mesh-data-planes-control-planes      monitoring-prometheus-metrics
monorepo-management               network-policies-segmentation        opentelemetry-distributed-tracing
package-management-helm-kustomize policy-as-code-opa-rego              progressive-delivery-canary
second-termux-processes           secret-management-vault              serverless-knative-cold-starts
service-discovery-dns-consul      slas-slis-slos-error-budgets         storage-classes-pv-pvc-csi
```

### 05 — IA Agéntica y Datos (39 skills)
Agentes LLM, RAG, fine-tuning, vector DBs, memoria episódica, MCP, subagentes.

```
advanced-graph-rag                agent-benchmarking-evaluation        agent-human-in-the-loop-hitl
agent-memory-persistence          agentic-multiloop-orchestration      ai-agent-state-recovery
autoprompting-engineering         bridge-mcp-engram-sync               context-token-budgeting
context7-mcp-docs                 curator-loop-hermes                  embeddings-similarity-metrics
engram-memory-system              guardrails-nemo-llamaguard           hybrid-search-sparse-dense
knowledge-graphs-neo4j            llm-fine-tuning-lora-qlora           llm-inference-engines-vllm
llm-integration-patterns          mcp-tools-protocol                   multi-agent-collaboration
nlp-pipeline-processing           ocs-identity-charter                 openclaw-isolation
plugins-extensibility-agent       predict-failure-risk                 prompt-compression-routing
quantization-gguf-awq-gptq        reinforcement-learning-human-feedback
retrieval-reranking-models        self-reflection-corrective-agents    semantic-chunking-embedding
streaming-llm-outputs-sse         structured-outputs-json-schema       subagents-parallelism
synthetic-data-generation         tool-use-function-calling            vector-db-indexing-hnsw
zero-token-optimization
```

### 06 — Seguridad SDLC (23 skills)
Threat modeling, OWASP, OAuth2/OIDC, cifrado, SAST/DAST, SBOM, hardening.

```
auth-jwt-oauth-detailed           compliance-auditing-frameworks       compliance-soc2-iso27001
cryptography-symmetric-asymmetric data-masking-anonymization           defensive-security-hardening
dynamic-application-security-testing-dast
fuzzing-security-boundaries       identity-access-management-rbac-abac incident-response-forensics
integration-testing-wiremock-testcontainers
key-management-kms-rotation       mutation-testing-pitest-stryker      oauth2-oidc-flows
owasp-top-10-mitigation           property-based-testing               rate-limiting-abuse-prevention
securing-cicd-pipelines           software-bill-of-materials-sbom      static-application-security-testing-sast
threat-modeling-stride            vulnerability-scanning-dependency-check
zero-trust-architecture-sdlc
```

### 07 — Frontend Web Fullstack (12 skills)
React, Next.js, Tailwind, TypeScript, testing, Flutter, React Native, desktop.

```
a11y-accessibility-wcag           electron-desktop-apps               flutter-dart-mobile
next-js-app-router                playwright-e2e-testing              react-native-mobile
react-ui-development              rest-api-integration-client         state-management-frontend
tailwind-css-utility              tauri-rust-desktop                  typescript-type-system
```

### 08 — Ingeniería y Herramientas (15 skills)
Bash, Python, pytest, FastAPI, asyncio, PostgreSQL, Prisma, Redis, Docker, SVG, SQLAlchemy.

```
async-python-concurrency          background-jobs-queues              bash-scripting-advanced
docker-compose-watch              dotenv-environment-vars             fastapi-rest-development
postgresql-advanced               prisma-orm-database                 pytest-testing-quality
python-packaging-pyproject        redis-caching-patterns              sqlite-sqlalchemy-persistence
svg-converter-rasterization       svg-generation-programmatic         typer-cli-applications
```

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
git clone https://github.com/tuusuario/skills-matrix-good-agents.git
cd skills-matrix-good-agents
```

### 2. Configurar opencode

Añadir a `~/.config/opencode/opencode.jsonc`:

```jsonc
{
  "skills": {
    "paths": ["/ruta/completa/skills-matrix"]
  },
  "instructions": ["~/.config/opencode/AGENTS.md"]
}
```

### 3. Instalar router global

```bash
cp AGENTS.md ~/.config/opencode/AGENTS.md
```

### 4. ¡Reiniciar opencode!

Los cambios de configuración se cargan al iniciar. Una vez dentro, el agente tendrá acceso a los 228 skills.

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
- **Carga bajo demanda**: no se leen los 228 SKILL.md hasta que se necesitan
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

## 📄 Licencia

MIT — Ver [LICENSE](./LICENSE) para más detalles.

---

## 🤝 Contribuciones

¿Falta un skill? ¿Encontraste un error? ¿Mejoraste un ejemplo?

1. Haz fork
2. Crea tu rama (`git checkout -b feat/mi-skill`)
3. Sigue el template de 9 secciones
4. Commit con conventional commits
5. Abre un PR

---

*Proyecto generado con ❤️ por opencode — 228 skills, 8 dominios, ~64.8k líneas de conocimiento técnico listo para tu agente.*
