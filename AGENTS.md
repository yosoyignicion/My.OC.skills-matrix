---
name: Skills-Matrix-Sematic-Router
version: "2.0.0"
spec_version: "2026.1"
capabilities:
  - 01-sistemas-bajo-nivel
  - 02-arquitectura-diseno
  - 03-sistemas-distribuidos
  - 04-devops-platform
  - 05-ia-agentica-datos
  - 06-seguridad-sdlc
  - 07-frontend-web-fullstack
  - 08-ingenieria-herramientas
---

# Agent Manifest & Semantic Router Specification

Manifiesto unificado de identidad, directivas y enrutamiento semantico para el Agente de IA.

---

## 1. Zero-Token Master Policy

*   Respuestas <=10 palabras o JSON directo.
*   Sin explicaciones, saludos ni despedidas.
*   Usa `engram_search` antes de preguntar.
*   No repitas info ya en memorias Engram.

---

## 2. System Guardrails & Encoding Protocols

### Encoding Protocol
*   **ENCODING:** Todas las operaciones de escritura/edicion en UTF-8.
*   **VALIDATE:** `iconv -f utf-8 -t utf-8` antes de guardar no-ASCII.
*   **FIX:** `iconv -f utf-8 -t utf-8//IGNORE` si invalido.

### Noise Filter
*   **STRIP:** Remover caracteres de control no imprimibles antes de persistir.
*   **RETURN:** Solo UTF-8 limpio para KV Cache Efficiency.

---

## 3. Registro de Habilidades (Skill Registry)

Catalogo completo de **228 habilidades** en `skills-matrix/`. Cada skill es un archivo `SKILL.md` con 9 secciones (definicion, implementacion, trade-offs, FAQ, vector IA, terminal/GUI, cheatsheet, relacionados, metadatos).

### Resumen por Dominio

| Dominio | Skills | Total Lineas | Tokens Estimados | Prioridad Media |
| :--- | ---: | ---: | ---: | :--- |
| **Sistemas Bajo Nivel** | 33 | 10,302 | ~184 | Alta |
| **Arquitectura y Diseno** | 37 | 12,776 | ~802 | Alta |
| **Sistemas Distribuidos** | 36 | 10,145 | ~987 | Alta |
| **DevOps Platform** | 33 | 8,993 | ~389 | Alta |
| **IA Agentica y Datos** | 39 | 9,891 | ~1133 | Alta |
| **Seguridad SDLC** | 23 | 5,644 | ~630 | Alta |
| **Frontend Web Fullstack** | 12 | 4,182 | ~887 | Alta |
| **Ingenieria y Herramientas** | 15 | 3,550 | ~673 | Alta |

### Tabla de Enrutamiento Semantico

Cada entrada usa 2 lineas: la primera con ID, prioridad, tokens, lineas y triggers semanticos; la segunda con la ruta al SKILL.md.

| | **Sistemas Bajo Nivel** | *Optimizacion a nivel de kernel, hardware y lenguajes de sistemas. io_uring, eBPF, SIMD, NUMA, C++ moderno, Rust, Go, profiling.* |
| `assembly-inline-optimizations` | Baja, ~180tok, 294lns | GCC extended inline assembly constraints, x86-64 assembly calling convention, hand-optimized assembly loops... |
| | | `skills-matrix/assembly-inline-optimizations` |
| `ast-manipulation` | Media, ~180tok, 297lns | abstract syntax tree traversal, clang LibTooling AST matchers, tree-sitter query pattern matching, AST tran... |
| | | `skills-matrix/ast-manipulation` |
| `compilation-linking-loader` | Alta, ~185tok, 287lns | ELF shared library relocation and GOT, PE COFF PE32+ format structure, dynamic linker ld.so resolution, pos... |
| | | `skills-matrix/compilation-linking-loader` |
| `concurrency-actor-model` | Alta, ~200tok, 295lns | actor model message passing concurrency, Erlang/OTP supervision trees, Akka typed actor lifecycle, Orleans ... |
| | | `skills-matrix/concurrency-actor-model` |
| `cpp-audio-development` | Media, ~190tok, 330lns | JUCE audio plugin VST3 AU AAX, real-time audio DSP allocation-free, PortAudio callback audio I/O, biquad fi... |
| | | `skills-matrix/cpp-audio-development` |
| `cpp-graphics-rendering` | Media, ~190tok, 366lns | raylib immediate-mode prototyping game, OpenGL 4.6 DSA direct state access, Vulkan 1.3 render pipeline, Web... |
| | | `skills-matrix/cpp-graphics-rendering` |
| `cpp-memory-safety` | Alta, ~185tok, 318lns | ASan AddressSanitizer heap buffer overflow detection, Valgrind memcheck memory leak detection, smart pointe... |
| | | `skills-matrix/cpp-memory-safety` |
| `cpu-cache-locality-alignment` | Alta, ~170tok, 271lns | cache line false sharing in concurrent data structures, cache-friendly data layout AoS vs SoA, alignment pa... |
| | | `skills-matrix/cpu-cache-locality-alignment` |
| `garbage-collection-algorithms` | Alta, ~190tok, 297lns | generational garbage collection young old generation, tricolor mark-sweep concurrent GC, G1 garbage first h... |
| | | `skills-matrix/garbage-collection-algorithms` |
| `go-systems-production` | Alta, ~190tok, 369lns | goroutine leak detection pprof, errgroup.Group concurrency limit, graceful shutdown signal handling, panic ... |
| | | `skills-matrix/go-systems-production` |
| `hardware-timers-clock-precision` | Alta, ~180tok, 335lns | CLOCK_MONOTONIC vs CLOCK_REALTIME, TSC cycle counter frequency scaling, HPET high precision event timer, ti... |
| | | `skills-matrix/hardware-timers-clock-precision` |
| `hardware-transactional-memory` | Baja, ~175tok, 352lns | Intel TSX restricted transactional memory, hardware lock elision HLE, transactional abort conflict detectio... |
| | | `skills-matrix/hardware-transactional-memory` |
| `instruction-level-parallelism` | Media, ~175tok, 315lns | superscalar pipeline instruction throughput, out-of-order execution and reorder buffer, speculative executi... |
| | | `skills-matrix/instruction-level-parallelism` |
| `io-multiplexing-iouring` | Alta, ~180tok, 257lns | io_uring syscall batching, epoll edge-triggered vs level-triggered, async I/O with kernel submissions, SQ/C... |
| | | `skills-matrix/io-multiplexing-iouring` |
| `io-scheduling-linux` | Media, ~165tok, 268lns | I/O scheduler deadline CFQ vs kyber, blk-mq multi-queue block layer, IOPS vs latency I/O tuning, mq-deadlin... |
| | | `skills-matrix/io-scheduling-linux` |
| `ipc-shared-memory-pipes` | Alta, ~180tok, 290lns | POSIX shared memory shm_open mmap, Unix domain socket SOCK_DGRAM SOCK_STREAM, anonymous pipes pipe fork, me... |
| | | `skills-matrix/ipc-shared-memory-pipes` |
| `jit-compilation-engines` | Media, ~185tok, 313lns | JIT compiler lazy compilation tiered, LLVM ORC JIT API, DynASM LuaJIT assembly, method-based compilation ho... |
| | | `skills-matrix/jit-compilation-engines` |
| `kernel-bypass-dpdk` | Media, ~180tok, 296lns | DPDK poll mode driver packet processing, XDP eXpress Data Path eBPF hook, AF_XDP socket zero-copy, kernel b... |
| | | `skills-matrix/kernel-bypass-dpdk` |
| `linux-ebpf-tracing` | Alta, ~185tok, 291lns | eBPF program attach to kprobe tracepoint, bpftrace one-liner tracing, BCC Python eBPF trace tool, perf even... |
| | | `skills-matrix/linux-ebpf-tracing` |
| `lock-free-data-structures` | Alta, ~190tok, 340lns | lock-free stack with hazard pointers, wait-free queue multi-producer multi-consumer, Compare-And-Swap CAS A... |
| | | `skills-matrix/lock-free-data-structures` |
| `memory-raii-borrowing` | Alta, ~190tok, 289lns | RAII resource acquisition is initialization, Rust borrow checker ownership, C++ smart pointers unique_ptr s... |
| | | `skills-matrix/memory-raii-borrowing` |
| `modern-cpp-development` | Alta, ~200tok, 314lns | modern C++20 CMake target-based build, C++23 std::expected std::print std::mdspan, C++26 static reflection ... |
| | | `skills-matrix/modern-cpp-development` |
| `numa-architectures-tuning` | Alta, ~185tok, 333lns | NUMA memory access latency remote vs local, numactl bind node policy, NUMA-aware memory allocation mbind, N... |
| | | `skills-matrix/numa-architectures-tuning` |
| `performance-profiling-optimization` | Alta, ~190tok, 305lns | perf top sampling profiler production, Valgrind Cachegrind Callgrind cache simulation, Tracy real-time prof... |
| | | `skills-matrix/performance-profiling-optimization` |
| `process-scheduler-namespaces` | Alta, ~185tok, 321lns | CFS completely fair scheduler vruntime, cgroups v2 cpu.weight cpu.max, namespace isolation Linux, PID names... |
| | | `skills-matrix/process-scheduler-namespaces` |
| `qt6-framework` | Alta, ~200tok, 387lns | Qt6 CMake AUTOMOC AUTORCC AUTOUIC, QWidgets desktop application layout, QML QtQuick declarative UI componen... |
| | | `skills-matrix/qt6-framework` |
| `rust-systems-programming` | Alta, ~200tok, 337lns | Rust cargo Tokio async runtime, axum web server Tower middleware, serde serialization derive macros, clap C... |
| | | `skills-matrix/rust-systems-programming` |
| `signals-and-interrupts-handling` | Alta, ~190tok, 340lns | async-signal-safe functions list, signal handler design restrictions, SA_SIGINFO extended signal handling, ... |
| | | `skills-matrix/signals-and-interrupts-handling` |
| `simd-vectorization` | Media, ~185tok, 305lns | AVX-512 vectorized loop optimization, SSE intrinsics manual vectorization, auto-vectorization compiler hint... |
| | | `skills-matrix/simd-vectorization` |
| `system-calls-overhead-tracing` | Alta, ~175tok, 290lns | system call latency measurement, strace syscall count per second, syscall overhead context switch, getpid v... |
| | | `skills-matrix/system-calls-overhead-tracing` |
| `virtual-memory-paging` | Alta, ~175tok, 303lns | page table walk TLB miss mitigation, huge pages 2MB 1GB Linux, mmap MAP_HUGETLB anonymous memory, transpare... |
| | | `skills-matrix/virtual-memory-paging` |
| `webassembly-runtimes-sandboxing` | Media, ~175tok, 322lns | wasmtime runtime sandbox isolation, WASI preview 2 streaming I/O, Component Model cross-language, wasm3 int... |
| | | `skills-matrix/webassembly-runtimes-sandboxing` |
| `zero-copy-serialization` | Media, ~175tok, 275lns | zero-copy deserialization without parsing, FlatBuffers schema definition, Cap'n Proto arena allocation, Apa... |
| | | `skills-matrix/zero-copy-serialization` |
| | **Arquitectura y Diseno** | *Patrones de diseno, DDD, CQRS, Event Sourcing, arquitecturas hexagonal/limpia, GoF, SOLID, API design, estado y logging.* |
| `api-gateway-bff-patterns` | Alta, ~800tok, 326lns | api gateway routing composición, bff backend for frontend específico, gateway aggregation multiple services... |
| | | `skills-matrix/api-gateway-bff-patterns` |
| `api-versioning-evolution-strategies` | Alta, ~780tok, 368lns | api versioning url prefix, api versioning header accept, evolución api backward compatible, api breaking ch... |
| | | `skills-matrix/api-versioning-evolution-strategies` |
| `asynchronous-messaging-patterns` | Alta, ~820tok, 318lns | message broker queue topic, pub sub messaging patrón, asynchronous messaging desacoplamiento, message queue... |
| | | `skills-matrix/asynchronous-messaging-patterns` |
| `bulkhead-circuit-breaker-resilience` | Alta, ~820tok, 470lns | circuit breaker patrón resiliencia, bulkhead aislamiento recursos, resilience patterns fallos, circuit brea... |
| | | `skills-matrix/bulkhead-circuit-breaker-resilience` |
| `caching-patterns-write-through` | Alta, ~820tok, 405lns | cache aside lazy loading patrón, write through cache actualización síncrona, write behind cache escritura a... |
| | | `skills-matrix/caching-patterns-write-through` |
| `clean-architecture-principles` | Alta, ~820tok, 288lns | clean architecture capas dependencias, regla dependencia inward pointing, entities casos uso adaptadores, s... |
| | | `skills-matrix/clean-architecture-principles` |
| `command-pattern-undo-redo` | Media, ~800tok, 451lns | command pattern undo redo, command history deshacer rehacer, command encapsulado operación, memento snapsho... |
| | | `skills-matrix/command-pattern-undo-redo` |
| `concurrency-patterns-pipelines` | Media, ~810tok, 362lns | pipeline pattern stages channels, fan out fan in concurrencia, worker pool paralelismo, circuit breaker con... |
| | | `skills-matrix/concurrency-patterns-pipelines` |
| `configuration-management` | Alta, ~780tok, 373lns | configuration management settings pydantic, environment variables config, config validation startup fail fa... |
| | | `skills-matrix/configuration-management` |
| `data-mapper-active-record` | Alta, ~780tok, 345lns | active record pattern modelo datos, data mapper pattern separación modelo persistencia, orm active record v... |
| | | `skills-matrix/data-mapper-active-record` |
| `data-serialization-formats` | Alta, ~780tok, 352lns | data serialization json yaml toml, protobuf avro parquet comparativa, schema evolution serialization, binar... |
| | | `skills-matrix/data-serialization-formats` |
| `ddd-tactical-patterns` | Alta, ~850tok, 251lns | agregado raíz agregado, value object inmutabilidad, domain service lógica dominio, repository patrón colecc... |
| | | `skills-matrix/ddd-tactical-patterns` |
| `dependency-injection-inversion` | Alta, ~780tok, 324lns | dependency injection contenedor ioc, inversión control constructor injection, service locator anti pattern,... |
| | | `skills-matrix/dependency-injection-inversion` |
| `design-systems-atomic` | Media, ~800tok, 379lns | design system tokens colores tipografía, atomic design atoms molecules organisms, design tokens w3c formato... |
| | | `skills-matrix/design-systems-atomic` |
| `domain-events-dispatching` | Alta, ~800tok, 351lns | domain event dispatch publicación, event handler dominio, domain event after commit, unit of work event dis... |
| | | `skills-matrix/domain-events-dispatching` |
| `error-handling-patterns` | Alta, ~800tok, 383lns | error handling exception jerarquía, result type error handling, structured errors rfc 9457, global error ha... |
| | | `skills-matrix/error-handling-patterns` |
| `event-driven-cqrs` | Alta, ~800tok, 268lns | command query responsibility segregation, event sourcing proyecciones, write model vs read model, event bus... |
| | | `skills-matrix/event-driven-cqrs` |
| `event-sourcing-eventstore` | Alta, ~850tok, 366lns | event sourcing almacenamiento eventos, event store append only, aggregate stream eventos, rebuilding state ... |
| | | `skills-matrix/event-sourcing-eventstore` |
| `gof-behavioral-patterns` | Alta, ~820tok, 304lns | observer patrón eventos notificación, strategy algoritmos intercambiables, command solicitud encapsulada, c... |
| | | `skills-matrix/gof-behavioral-patterns` |
| `gof-creational-patterns` | Alta, ~780tok, 295lns | factory method creación objetos, abstract factory familias objetos, singleton instancia única, builder cons... |
| | | `skills-matrix/gof-creational-patterns` |
| `gof-structural-patterns` | Alta, ~800tok, 286lns | adapter patrón interfaz incompatible, decorator responsabilidades dinámicas, proxy control acceso, facade s... |
| | | `skills-matrix/gof-structural-patterns` |
| `hexagonal-architecture` | Alta, ~850tok, 282lns | puertos adaptadores arquitectura, hexagonal ports adapters, inversión dependencias infraestructura, core do... |
| | | `skills-matrix/hexagonal-architecture` |
| `idempotency-keys-processing` | Alta, ~780tok, 390lns | idempotency key garantía procesamiento único, idempotent consumer deduplicación, idempotency key expire, sa... |
| | | `skills-matrix/idempotency-keys-processing` |
| `micro-frontends-routing` | Media, ~800tok, 319lns | micro frontends composición fragmentos, micro frontend routing shell, web components federated modules, mod... |
| | | `skills-matrix/micro-frontends-routing` |
| `microservices-decomposition` | Alta, ~800tok, 288lns | descomposición microservicios bounded context, strangler fig migración incremental, decomposition by subdom... |
| | | `skills-matrix/microservices-decomposition` |
| `modular-monolithic-design` | Alta, ~790tok, 300lns | modular monolith módulos boundaries, monolito modular vs microservicios, module boundaries contexto acotado... |
| | | `skills-matrix/modular-monolithic-design` |
| `multi-tenant-data-isolation` | Alta, ~800tok, 375lns | multi tenant aislamiento datos, database per tenant isolation, shared database shared schema tenant, multi ... |
| | | `skills-matrix/multi-tenant-data-isolation` |
| `outbox-inbox-patterns` | Alta, ~800tok, 358lns | transactional outbox mensajes fiables, inbox pattern deduplicación eventos, outbox poller publicación mensa... |
| | | `skills-matrix/outbox-inbox-patterns` |
| `pipeline-filter-architecture` | Media, ~790tok, 362lns | pipeline filter arquitectura procesamiento, pipes and filters pattern, pipeline processing stages sequentia... |
| | | `skills-matrix/pipeline-filter-architecture` |
| `plugins-and-extensibility-architectures` | Media, ~800tok, 398lns | plugin architecture extensibilidad, extension point hook system, plugin loader registry, micro kernel plugi... |
| | | `skills-matrix/plugins-and-extensibility-architectures` |
| `reactive-programming-extensions` | Media, ~800tok, 331lns | reactive programming observables streams, rxjs operators pipe chain, reactive extensions rx pattern, observ... |
| | | `skills-matrix/reactive-programming-extensions` |
| `rest-api-design` | Alta, ~800tok, 362lns | rest api diseño endpoints, api paginación offset cursor, rest api status codes, api versioning hateoas, res... |
| | | `skills-matrix/rest-api-design` |
| `saga-orchestration-choreography` | Alta, ~820tok, 342lns | saga orchestration coordinador central, saga choreography eventos descentralizados, compensating transactio... |
| | | `skills-matrix/saga-orchestration-choreography` |
| `solid-deep-dive` | Alta, ~800tok, 263lns | single responsibility principio, open closed extensión sin modificación, liskov substitution subtipos, inte... |
| | | `skills-matrix/solid-deep-dive` |
| `state-machine-workflows` | Alta, ~800tok, 402lns | state machine estados transiciones, workflow engine máquina estados, finite state machine fsm, state machin... |
| | | `skills-matrix/state-machine-workflows` |
| `state-management-patterns` | Alta, ~780tok, 363lns | state management zustand jotai, redux toolkit state management, atomic state derived atoms, state persisten... |
| | | `skills-matrix/state-management-patterns` |
| `structured-logging-patterns` | Alta, ~780tok, 376lns | structured logging json output, correlation id trace logging, contextual logging bind, structured logging b... |
| | | `skills-matrix/structured-logging-patterns` |
| | **Sistemas Distribuidos** | *Protocolos de red, consenso distribuido, streaming, colas, caching, Service Mesh, CDN, Chaos Engineering.* |
| `api-idempotency-in-distributed-networks` | Alta, ~950tok, 304lns | idempotency key based request deduplication, distributed idempotency with redis and database, idempotency k... |
| | | `skills-matrix/api-idempotency-in-distributed-networks` |
| `backpressure-and-flow-control` | Alta, ~1000tok, 301lns | backpressure in distributed async systems, flow control with reactive streams and rx, tcp flow control slid... |
| | | `skills-matrix/backpressure-and-flow-control` |
| `cap-theorem-tradeoffs` | Alta, ~850tok, 225lns | cap theorem consistency availability partition tolerance, cp vs ap system design choices, eventual consiste... |
| | | `skills-matrix/cap-theorem-tradeoffs` |
| `cdn-edge-caching-georouting` | Alta, ~950tok, 242lns | cdn edge caching with cache-control and surrogate-key, geo routing and origin shield for cdn, cdn cache inv... |
| | | `skills-matrix/cdn-edge-caching-georouting` |
| `change-data-capture-cdc` | Alta, ~1050tok, 306lns | change data capture with postgres logical replication, debezium kafka connect for streaming cdc, cdc for ca... |
| | | `skills-matrix/change-data-capture-cdc` |
| `consistent-hashing-topologies` | Media, ~950tok, 256lns | consistent hashing ring and virtual nodes, consistent hashing for distributed caching, load balancing with ... |
| | | `skills-matrix/consistent-hashing-topologies` |
| `crdts-conflict-free-replicated` | Media, ~1050tok, 294lns | crdt state based merge and delta mutations, crdt operation based vs state based replication, g counter and ... |
| | | `skills-matrix/crdts-conflict-free-replicated` |
| `data-encryption-in-transit-mtls` | Alta, ~950tok, 295lns | mutual tls mtls for service to service authentication, tls handshake cipher suite and certificate chain, tl... |
| | | `skills-matrix/data-encryption-in-transit-mtls` |
| `data-lakehouses-parquet-iceberg` | Media, ~1000tok, 257lns | apache iceberg table format and snapshot isolation, parquet columnar storage and row group pruning, iceberg... |
| | | `skills-matrix/data-lakehouses-parquet-iceberg` |
| `database-replication-lag-strategies` | Media, ~900tok, 272lns | read replicas and replication lag handling, read your writes consistency after write, monotonic read consis... |
| | | `skills-matrix/database-replication-lag-strategies` |
| `database-sharding-partitioning` | Alta, ~1000tok, 288lns | database sharding key selection and data distribution, range vs hash based sharding strategies, cross shard... |
| | | `skills-matrix/database-sharding-partitioning` |
| `distributed-cache-redis-cluster` | Alta, ~1100tok, 286lns | redis cluster hash slots and cross slot commands, distributed caching cache aside and write through, redis ... |
| | | `skills-matrix/distributed-cache-redis-cluster` |
| `distributed-consensus-raft` | Alta, ~1050tok, 294lns | raft leader election mechanism, raft log replication and commit index, raft safety guarantees and quorum, r... |
| | | `skills-matrix/distributed-consensus-raft` |
| `distributed-locking-redlock` | Media, ~1050tok, 285lns | distributed lock with redis redlock algorithm, set nx ex vs redlock multi node fencing, distributed lock fe... |
| | | `skills-matrix/distributed-locking-redlock` |
| `distributed-queues-rabbitmq-amqp` | Alta, ~1000tok, 312lns | rabbitmq exchanges queues and bindings, amqp 0-9-1 protocol and message acknowledgments, rabbitmq publisher... |
| | | `skills-matrix/distributed-queues-rabbitmq-amqp` |
| `distributed-tracing-context-propagation` | Alta, ~1000tok, 243lns | distributed tracing with opentelemetry and jaeger, w3c tracecontext and baggage headers, context propagatio... |
| | | `skills-matrix/distributed-tracing-context-propagation` |
| `distributed-transactions-2pc-3pc` | Baja, ~900tok, 285lns | two phase commit prepare and commit protocol, three phase commit timeout and recovery, two phase commit coo... |
| | | `skills-matrix/distributed-transactions-2pc-3pc` |
| `dns-routing-anycast-latency` | Media, ~950tok, 314lns | dns anycast routing for global load balancing, dns latency based routing with geo proximity, dns ttl and re... |
| | | `skills-matrix/dns-routing-anycast-latency` |
| `fault-injection-chaos-engineering` | Media, ~950tok, 337lns | chaos engineering principles steady state and hypothesis, fault injection with litmus chaos and chaos mesh,... |
| | | `skills-matrix/fault-injection-chaos-engineering` |
| `gossip-protocols-membership` | Media, ~950tok, 287lns | gossip protocol for failure detection and membership, swim protocol and lifeguard enhancements, gossip base... |
| | | `skills-matrix/gossip-protocols-membership` |
| `graphql-federation-gateways` | Media, ~1050tok, 269lns | graphql federation with apollo router and rover, graphql subgraph composition and @key directive, graphql e... |
| | | `skills-matrix/graphql-federation-gateways` |
| `grpc-protobuf` | Alta, ~1100tok, 265lns | grpc bidirectional streaming, protobuf schema evolution and wire format, grpc deadline propagation and canc... |
| | | `skills-matrix/grpc-protobuf` |
| `http-client-patterns` | Alta, ~1000tok, 305lns | http client connection pooling and keep alive, http client retry with exponential backoff and jitter, http ... |
| | | `skills-matrix/http-client-patterns` |
| `http3-quic` | Media, ~950tok, 221lns | http3 connection establishment, quic 0-rtt handshake, multiplexed streams without head-of-line blocking, ht... |
| | | `skills-matrix/http3-quic` |
| `hybrid-logical-clocks` | Media, ~900tok, 321lns | hybrid logical clock hlc for distributed systems, hlc combining physical and logical time, hlc to causal or... |
| | | `skills-matrix/hybrid-logical-clocks` |
| `load-balancing-algorithms-l4-l7` | Alta, ~1000tok, 285lns | layer 4 load balancing tcp udp vs layer 7 http, round robin least connections and weighted load balancing, ... |
| | | `skills-matrix/load-balancing-algorithms-l4-l7` |
| `message-brokers-kafka-internals` | Alta, ~1150tok, 260lns | kafka partition assignment and consumer group rebalancing, kafka log compaction and retention policies, kaf... |
| | | `skills-matrix/message-brokers-kafka-internals` |
| `network-partitions-split-brain` | Alta, ~950tok, 309lns | network partition detection and fencing mechanisms, split brain prevention with quorum and lease, split bra... |
| | | `skills-matrix/network-partitions-split-brain` |
| `pacelc-theorem-implications` | Media, ~800tok, 226lns | pacelc tradeoffs if partition else latency consistency, pacelc vs cap theorem differences, dynamo style eve... |
| | | `skills-matrix/pacelc-theorem-implications` |
| `rate-limiting-algorithms` | Media, ~900tok, 227lns | token bucket rate limiting algorithm, sliding window log and counter, fixed window vs sliding window rate l... |
| | | `skills-matrix/rate-limiting-algorithms` |
| `saga-pattern-distributed-coordination` | Alta, ~1050tok, 287lns | saga pattern choreography vs orchestration, compensating transactions and rollback in saga, saga orchestrat... |
| | | `skills-matrix/saga-pattern-distributed-coordination` |
| `service-mesh-envoy-sidecars` | Alta, ~1100tok, 287lns | envoy proxy sidecar injection and xds control plane, istio virtual service and destination rule, service me... |
| | | `skills-matrix/service-mesh-envoy-sidecars` |
| `session-management-stateless-vs-stateful` | Alta, ~1000tok, 316lns | stateless session with jwt and token based auth, stateful session with redis or database storage, session a... |
| | | `skills-matrix/session-management-stateless-vs-stateful` |
| `vector-clocks-lamport-timestamps` | Media, ~950tok, 279lns | vector clock causality tracking and conflict detection, lamport logical clock happens before relation, vect... |
| | | `skills-matrix/vector-clocks-lamport-timestamps` |
| `websockets-sse-realtime` | Media, ~1000tok, 293lns | websocket connection lifecycle and reconnection, server sent events vs websockets use cases, websocket broa... |
| | | `skills-matrix/websockets-sse-realtime` |
| `zero-trust-network-architectures` | Alta, ~1050tok, 312lns | zero trust network access ztna beyondcorp, zero trust principle never trust always verify, zero trust micro... |
| | | `skills-matrix/zero-trust-network-architectures` |
| | **DevOps Platform** | *Kubernetes, GitOps, CI/CD, monitoring, IaC, service mesh, secret management, Git workflows, FinOps.* |
| `artifact-registries-security` | Alta, ~410tok, 272lns | container registry, artifact registry, docker image security, ghcr, ecr, gcr artifact registry, image signi... |
| | | `skills-matrix/artifact-registries-security` |
| `autoscaling-hpa-vpa-keda` | Alta, ~420tok, 303lns | horizontal pod autoscaler, vertical pod autoscaler, keda event driven autoscaling, custom metrics autoscale... |
| | | `skills-matrix/autoscaling-hpa-vpa-keda` |
| `bare-metal-vs-virtualization` | Media, ~400tok, 298lns | bare metal kubernetes, virtualization kvm, kubernetes on bare metal, vm vs container overhead, bare metal v... |
| | | `skills-matrix/bare-metal-vs-virtualization` |
| `blue-green-deployment-strategies` | Media, ~370tok, 265lns | blue green deployment, zero downtime deployment, traffic switch blue green, drain old version, kubernetes b... |
| | | `skills-matrix/blue-green-deployment-strategies` |
| `chaos-mesh-reliability-testing` | Baja, ~400tok, 278lns | chaos mesh, chaos engineering kubernetes, fault injection, pod failure network partition, stress testing, l... |
| | | `skills-matrix/chaos-mesh-reliability-testing` |
| `cicd-declarative-pipelines` | Alta, ~370tok, 251lns | ci cd pipeline, github actions, gitlab ci, declarative workflow, matrix build test, pipeline cache artifact... |
| | | `skills-matrix/cicd-declarative-pipelines` |
| `configmap-secrets-hot-reloading` | Alta, ~400tok, 329lns | kubernetes configmap, kubernetes secrets, hot reload config, configmap update pod reload, reloader stakater... |
| | | `skills-matrix/configmap-secrets-hot-reloading` |
| `container-internals-namespaces` | Alta, ~340tok, 246lns | linux namespaces, cgroups, container isolation, pid namespace, mount namespace, network namespace, user nam... |
| | | `skills-matrix/container-internals-namespaces` |
| `container-orchestration-k8s-scheduling` | Alta, ~380tok, 257lns | kubernetes scheduler, pod scheduling, node affinity, taints tolerations, topology spread constraints, resou... |
| | | `skills-matrix/container-orchestration-k8s-scheduling` |
| `container-runtimes-containerd-cri` | Alta, ~380tok, 255lns | container runtime, containerd, cri o runtime interface, dockershim removal, cri o vs containerd, crictl, ne... |
| | | `skills-matrix/container-runtimes-containerd-cri` |
| `cost-optimization-finops-kubernetes` | Media, ~410tok, 279lns | finops kubernetes, cost optimization k8s, kubecost, request right sizing, spot instance kubernetes, node po... |
| | | `skills-matrix/cost-optimization-finops-kubernetes` |
| `ebpf-based-networking-cilium` | Media, ~390tok, 275lns | ebpf networking, cilium kubernetes, ebpf data plane, kube proxy replacement, cilium network policy, hubble ... |
| | | `skills-matrix/ebpf-based-networking-cilium` |
| `git-workflows-conventional` | Alta, ~380tok, 287lns | git workflows, conventional commits, pre-commit hooks, branching strategy, changelog generation, commit lin... |
| | | `skills-matrix/git-workflows-conventional` |
| `gitops-declarative-reconciliation` | Alta, ~320tok, 237lns | gitops declarativa, reconciliacion estado deseado, git como fuente única verdad, push vs pull deployment, d... |
| | | `skills-matrix/gitops-declarative-reconciliation` |
| `immutable-infrastructure-packer` | Media, ~390tok, 268lns | packer immutable image, image building ami golden, infrastructure immutability, packer provisioner shell an... |
| | | `skills-matrix/immutable-infrastructure-packer` |
| `infrastructure-as-code-terraform` | Alta, ~360tok, 248lns | terraform infrastructure code, hcl declarative infra, terraform plan apply, state management remote backend... |
| | | `skills-matrix/infrastructure-as-code-terraform` |
| `kubernetes-operators-controllers` | Alta, ~360tok, 242lns | kubernetes operator, custom resource definition crd, controller reconciliation, kubebuilder operator sdk, c... |
| | | `skills-matrix/kubernetes-operators-controllers` |
| `load-testing-k6-distributed` | Baja, ~400tok, 274lns | k6 load testing, distributed load test, k6 javascript, performance testing kubernetes, k6 operator, thresho... |
| | | `skills-matrix/load-testing-k6-distributed` |
| `log-aggregation-loki-elasticsearch` | Media, ~360tok, 255lns | log aggregation, grafana loki, elasticsearch, elk stack, structured logging, label based indexing, logql, f... |
| | | `skills-matrix/log-aggregation-loki-elasticsearch` |
| `mesh-data-planes-control-planes` | Alta, ~420tok, 288lns | service mesh, istio control plane, envoy data plane, linkerd, sidecar proxy, mutual tls service mesh, traff... |
| | | `skills-matrix/mesh-data-planes-control-planes` |
| `monitoring-prometheus-metrics` | Alta, ~380tok, 247lns | prometheus metrics, monitoreo kubernetes, promql queries, grafana dashboards, service monitor pod monitor, ... |
| | | `skills-matrix/monitoring-prometheus-metrics` |
| `monorepo-management` | Media, ~400tok, 301lns | monorepo, turborepo, nx, pnpm workspaces, build caching, task pipeline, changesets versioning, ts project r... |
| | | `skills-matrix/monorepo-management` |
| `network-policies-segmentation` | Alta, ~400tok, 301lns | kubernetes network policy, network segmentation, default deny ingress egress, pod selector policy, namespac... |
| | | `skills-matrix/network-policies-segmentation` |
| `opentelemetry-distributed-tracing` | Media, ~380tok, 243lns | opentelemetry, distributed tracing, otel, span trace metric, otlp collector, instrumentacion automatica, co... |
| | | `skills-matrix/opentelemetry-distributed-tracing` |
| `package-management-helm-kustomize` | Alta, ~420tok, 292lns | helm package manager, kustomize overlay, helm chart structure, kustomize variant overlay, helm values inher... |
| | | `skills-matrix/package-management-helm-kustomize` |
| `policy-as-code-opa-rego` | Media, ~430tok, 289lns | policy as code, open policy agent, rego language, opa kubernetes admission, gatekeeper constraint template,... |
| | | `skills-matrix/policy-as-code-opa-rego` |
| `progressive-delivery-canary` | Alta, ~400tok, 257lns | canary deployment, progressive delivery, traffic splitting, feature flags, argo rollouts, flagger, automate... |
| | | `skills-matrix/progressive-delivery-canary` |
| `second-termux-processes` | Alta, ~350tok, 254lns | second-termux, background process manager, bgx smart routing, setsid detach, mcp background tools, token ze... |
| | | `skills-matrix/second-termux-processes` |
| `secret-management-vault-integration` | Alta, ~400tok, 278lns | hashicorp vault, secret management kubernetes, vault agent injector, dynamic secrets, vault transit encrypt... |
| | | `skills-matrix/secret-management-vault-integration` |
| `serverless-knative-cold-starts` | Media, ~400tok, 283lns | knative serving, serverless kubernetes, cold start latency, scale to zero knative, revision autoscaling kna... |
| | | `skills-matrix/serverless-knative-cold-starts` |
| `service-discovery-dns-consul` | Media, ~380tok, 266lns | service discovery kubernetes, consul service mesh, core dns, dns based discovery, consul connect, health ch... |
| | | `skills-matrix/service-discovery-dns-consul` |
| `slas-slis-slos-error-budgets` | Media, ~420tok, 264lns | sli slo sla, error budget, service level indicator, service level objective, availability slo, latency slo,... |
| | | `skills-matrix/slas-slis-slos-error-budgets` |
| `storage-classes-pv-pvc-csi` | Alta, ~420tok, 311lns | persistent volume pv, persistent volume claim pvc, storage class, csi driver, dynamic provisioning, statefu... |
| | | `skills-matrix/storage-classes-pv-pvc-csi` |
| | **IA Agentica y Datos** | *Agentes LLM, RAG, fine-tuning, vector DBs, memoria episodica (Engram), MCP, subagentes, zero-token, guardrails.* |
| `advanced-graph-rag` | Alta, ~1400tok, 206lns | graph rag, knowledge graph retrieval, graph-based rag, graphrag, graph augmented generation, multi-hop retr... |
| | | `skills-matrix/advanced-graph-rag` |
| `agent-benchmarking-evaluation` | Media, ~1100tok, 251lns | agent benchmark, llm evaluation, agent evaluation, benchmark dataset, task oriented evaluation, agent perfo... |
| | | `skills-matrix/agent-benchmarking-evaluation` |
| `agent-human-in-the-loop-hitl` | Alta, ~1100tok, 252lns | human in the loop, hitl agent, human approval, agent intervention, human oversight, human agent collaborati... |
| | | `skills-matrix/agent-human-in-the-loop-hitl` |
| `agent-memory-persistence-episodic` | Alta, ~1200tok, 235lns | episodic memory, agent memory persistence, memory retrieval, conversation memory, long term memory agent, m... |
| | | `skills-matrix/agent-memory-persistence-episodic` |
| `agentic-multiloop-orchestration` | Alta, ~1200tok, 218lns | agentic loop, multi-agent orchestration, cognitive cycle, reflection loop, task decomposition, iterative ex... |
| | | `skills-matrix/agentic-multiloop-orchestration` |
| `ai-agent-state-recovery-checkpoints` | Alta, ~1100tok, 262lns | agent checkpoint, state recovery, agent state persistence, fault tolerant agent, checkpoint restore, long r... |
| | | `skills-matrix/ai-agent-state-recovery-checkpoints` |
| `autoprompting-engineering` | Alta, ~1000tok, 263lns | autoprompting, prompt engineering, system prompt design, chain of thought, persona crafting, self prompting... |
| | | `skills-matrix/autoprompting-engineering` |
| `bridge-mcp-engram-sync` | Media, ~1000tok, 314lns | bridge sync, process to engram, mcp engram bridge, process synchronization, second termux sync, observation... |
| | | `skills-matrix/bridge-mcp-engram-sync` |
| `context-token-budgeting` | Alta, ~800tok, 211lns | token budget, context window management, token allocation, prompt compression, context pruning, token optim... |
| | | `skills-matrix/context-token-budgeting` |
| `context7-mcp-docs` | Alta, ~800tok, 245lns | context7 documentation, library docs fetch, api reference lookup, code example retrieval, framework documen... |
| | | `skills-matrix/context7-mcp-docs` |
| `curator-loop-hermes` | Alta, ~1200tok, 296lns | curator loop hermes, automatic skill creation, command to skill, self improving agent, curator pattern, aut... |
| | | `skills-matrix/curator-loop-hermes` |
| `embeddings-similarity-metrics` | Alta, ~1000tok, 244lns | embedding similarity, cosine similarity, distance metric, dense embedding, embedding model comparison, cros... |
| | | `skills-matrix/embeddings-similarity-metrics` |
| `engram-memory-system` | Alta, ~1400tok, 307lns | engram memory, typed memory sqlite, fts5 memory retrieval, memory importance decay, memory persistence, eng... |
| | | `skills-matrix/engram-memory-system` |
| `guardrails-nemo-llamaguard` | Alta, ~1200tok, 257lns | nemo guardrails, llamaguard, llm safety, content moderation, guardrails configuration, output validation gu... |
| | | `skills-matrix/guardrails-nemo-llamaguard` |
| `hybrid-search-sparse-dense` | Alta, ~1100tok, 252lns | hybrid search, sparse dense retrieval, bm25 dense fusion, reciprocal rank fusion, hybrid vector search, key... |
| | | `skills-matrix/hybrid-search-sparse-dense` |
| `knowledge-graphs-neo4j` | Media, ~1300tok, 245lns | neo4j knowledge graph, graph database rag, cypher query, entity relationship graph, graph extraction llm, k... |
| | | `skills-matrix/knowledge-graphs-neo4j` |
| `llm-fine-tuning-lora-qlora` | Alta, ~1300tok, 232lns | lora finetuning, qlora, parameter efficient fine tuning, peft, llm adaptation, fine tuning strategy, rank s... |
| | | `skills-matrix/llm-fine-tuning-lora-qlora` |
| `llm-inference-engines-vllm` | Alta, ~1300tok, 229lns | vllm, llm inference engine, continuous batching, pagedattention, llm serving, inference optimization, specu... |
| | | `skills-matrix/llm-inference-engines-vllm` |
| `llm-integration-patterns` | Alta, ~1200tok, 270lns | llm integration, openai api, anthropic api, multi provider llm, llm client abstraction, provider fallback, ... |
| | | `skills-matrix/llm-integration-patterns` |
| `mcp-tools-protocol` | Alta, ~1200tok, 269lns | mcp protocol, json rpc tools, mcp tool definition, tool server endpoint, model context protocol, mcp proxy ... |
| | | `skills-matrix/mcp-tools-protocol` |
| `multi-agent-collaboration-protocols` | Alta, ~1300tok, 246lns | multi agent protocol, agent collaboration, agent communication, swarm agent, agent negotiation, agent role ... |
| | | `skills-matrix/multi-agent-collaboration-protocols` |
| `nlp-pipeline-processing` | Alta, ~1000tok, 296lns | nlp pipeline, intent recognition, entity extraction, spacy nlp, natural language command, command parser, d... |
| | | `skills-matrix/nlp-pipeline-processing` |
| `ocs-identity-charter` | Alta, ~1500tok, 278lns | agent identity, system prompt layering, cognitive architecture, identity charter, system prompt pipeline, a... |
| | | `skills-matrix/ocs-identity-charter` |
| `openclaw-isolation` | Alta, ~1300tok, 299lns | openclaw isolation, workspace validation, write protection, plan approval gate, multi file guard, destructi... |
| | | `skills-matrix/openclaw-isolation` |
| `plugins-extensibility-agent` | Media, ~1100tok, 291lns | agent plugin system, plugin protocol json, extensibility agent, script plugin, plugin integration, external... |
| | | `skills-matrix/plugins-extensibility-agent` |
| `predict-failure-risk` | Alta, ~1000tok, 285lns | failure prediction, risk assessment agent, temporal loop detection, command risk scoring, novikov risk, pre... |
| | | `skills-matrix/predict-failure-risk` |
| `prompt-compression-routing` | Alta, ~1100tok, 236lns | prompt compression, llm routing, prompt routing, query classification, prompt optimization, inference routi... |
| | | `skills-matrix/prompt-compression-routing` |
| `quantization-gguf-awq-gptq` | Alta, ~1200tok, 220lns | gguf quantization, awq quantization, gptq quantization, model compression, llm quantization, weight quantiz... |
| | | `skills-matrix/quantization-gguf-awq-gptq` |
| `reinforcement-learning-human-feedback` | Media, ~1300tok, 238lns | rlhf, reinforcement learning human feedback, dpo, ppo alignment, preference optimization, human feedback tr... |
| | | `skills-matrix/reinforcement-learning-human-feedback` |
| `retrieval-reranking-models` | Alta, ~1200tok, 233lns | retrieval reranking, cross encoder reranker, multi stage retrieval, colbert reranking, dense passage retrie... |
| | | `skills-matrix/retrieval-reranking-models` |
| `self-reflection-corrective-agents` | Alta, ~1100tok, 233lns | self reflection agent, self correction llm, reflective agent, corrective feedback loop, self critique, refi... |
| | | `skills-matrix/self-reflection-corrective-agents` |
| `semantic-chunking-embedding-pipelines` | Alta, ~900tok, 248lns | semantic chunking, document chunking strategy, embedding pipeline, chunk overlap, embedding batch processin... |
| | | `skills-matrix/semantic-chunking-embedding-pipelines` |
| `streaming-llm-outputs-sse` | Alta, ~900tok, 233lns | streaming llm, server sent events, sse streaming, token streaming, async llm generation, streaming response... |
| | | `skills-matrix/streaming-llm-outputs-sse` |
| `structured-outputs-json-schema` | Alta, ~1000tok, 239lns | structured output, json schema generation, constrained decoding, json mode llm, outlines grammar, instructo... |
| | | `skills-matrix/structured-outputs-json-schema` |
| `subagents-parallelism` | Alta, ~1200tok, 279lns | subagent delegation, parallel agent execution, goroutine agent, fan out tasks, multi role agent, concurrent... |
| | | `skills-matrix/subagents-parallelism` |
| `synthetic-data-generation-pipelines` | Media, ~1200tok, 265lns | synthetic data generation, data augmentation llm, synthetic dataset, llm data generation, data pipeline gen... |
| | | `skills-matrix/synthetic-data-generation-pipelines` |
| `tool-use-function-calling` | Alta, ~1000tok, 222lns | tool calling, function calling, tool use, json schema tools, tool description, parallel tool calls, tool er... |
| | | `skills-matrix/tool-use-function-calling` |
| `vector-db-indexing-hnsw` | Alta, ~1000tok, 216lns | hnsw index, vector index, approximate nearest neighbor, ivf index, vector database indexing, index build pa... |
| | | `skills-matrix/vector-db-indexing-hnsw` |
| `zero-token-optimization` | Alta, ~1000tok, 276lns | zero token, token minimization, response compression, ultra short answers, token budget optimization, compa... |
| | | `skills-matrix/zero-token-optimization` |
| | **Seguridad SDLC** | *Threat modeling, OWASP, OAuth2/OIDC, cifrado, SAST/DAST, SBOM, cumplimiento, hardening defensivo.* |
| `auth-jwt-oauth-detailed` | Alta, ~700tok, 302lns | JWT access token refresh token rotation, OAuth2 authorization code PKCE S256, OIDC id_token authentication ... |
| | | `skills-matrix/auth-jwt-oauth-detailed` |
| `compliance-auditing-frameworks` | Alta, ~650tok, 267lns | compliance framework SOC 2 ISO 27001 PCI DSS HIPAA FedRAMP, audit evidence collection automated compliance ... |
| | | `skills-matrix/compliance-auditing-frameworks` |
| `compliance-frameworks-soc2-iso27001` | Alta, ~650tok, 242lns | SOC 2 Type II audit trust services criteria, ISO 27001 ISMS implementation and certification, SOC 2 vs ISO ... |
| | | `skills-matrix/compliance-frameworks-soc2-iso27001` |
| `cryptography-symmetric-asymmetric` | Alta, ~650tok, 230lns | AES-256-GCM authenticated encryption, RSA and ECDSA public key signatures, ChaCha20-Poly1305 for mobile/low... |
| | | `skills-matrix/cryptography-symmetric-asymmetric` |
| `data-masking-anonymization` | Alta, ~600tok, 267lns | data masking PII redaction for non-production environments, k-anonymity and l-diversity anonymization techn... |
| | | `skills-matrix/data-masking-anonymization` |
| `defensive-security-hardening` | Alta, ~650tok, 259lns | defensive security hardening Linux Windows containers, firewall iptables nftables UFW configuration, log sa... |
| | | `skills-matrix/defensive-security-hardening` |
| `dynamic-application-security-testing-dast` | Alta, ~600tok, 222lns | DAST scanning for runtime vulnerabilities, OWASP ZAP automated security testing, Burp Suite professional pa... |
| | | `skills-matrix/dynamic-application-security-testing-dast` |
| `fuzzing-security-boundaries` | Media, ~600tok, 257lns | fuzz testing input validation boundaries, libFuzzer and AFL coverage-guided fuzzing, API fuzzing with RESTl... |
| | | `skills-matrix/fuzzing-security-boundaries` |
| `identity-access-management-rbac-abac` | Alta, ~650tok, 256lns | RBAC role-based access control implementation, ABAC attribute-based policy with OPA and Cedar, AWS IAM poli... |
| | | `skills-matrix/identity-access-management-rbac-abac` |
| `incident-response-forensics-logging` | Alta, ~650tok, 276lns | incident response lifecycle NIST 800-61 preparation detection containment eradication recovery, forensic ac... |
| | | `skills-matrix/incident-response-forensics-logging` |
| `integration-testing-wiremock-testcontainers` | Media, ~600tok, 282lns | WireMock HTTP stub server for external API mocking, Testcontainers disposable PostgreSQL Redis Kafka contai... |
| | | `skills-matrix/integration-testing-wiremock-testcontainers` |
| `key-management-kms-rotation` | Alta, ~650tok, 243lns | AWS KMS key rotation and automatic yearly rotation, HashiCorp Vault transit engine for encryption as a serv... |
| | | `skills-matrix/key-management-kms-rotation` |
| `mutation-testing-pitest-stryker` | Media, ~550tok, 240lns | mutation testing with PITest for Java, Stryker mutator for JavaScript and TypeScript, mutation score as qua... |
| | | `skills-matrix/mutation-testing-pitest-stryker` |
| `oauth2-oidc-flows` | Alta, ~700tok, 217lns | OAuth2 authorization code flow with PKCE, OpenID Connect ID token and userinfo endpoint, JWT access token v... |
| | | `skills-matrix/oauth2-oidc-flows` |
| `owasp-top-10-mitigation` | Alta, ~650tok, 214lns | OWASP Top 10 2021 mitigation strategies, SQL injection prevention with parameterized queries, XSS defense w... |
| | | `skills-matrix/owasp-top-10-mitigation` |
| `property-based-testing` | Media, ~600tok, 228lns | property-based testing with Hypothesis or QuickCheck, invariant properties that must hold for all inputs, s... |
| | | `skills-matrix/property-based-testing` |
| `rate-limiting-abuse-prevention` | Alta, ~600tok, 251lns | rate limiting with token bucket and sliding window algorithms, API gateway rate limiting per user and per I... |
| | | `skills-matrix/rate-limiting-abuse-prevention` |
| `securing-cicd-pipelines` | Alta, ~650tok, 248lns | CI/CD pipeline security hardened workflows, supply chain level SLSA attestation requirements, ephemeral cre... |
| | | `skills-matrix/securing-cicd-pipelines` |
| `software-bill-of-materials-sbom` | Alta, ~600tok, 215lns | SPDX and CycloneDX SBOM formats, generating SBOM from package manifests, SBOM diffing and vulnerability cor... |
| | | `skills-matrix/software-bill-of-materials-sbom` |
| `static-application-security-testing-sast` | Alta, ~650tok, 249lns | SAST source code vulnerability scanning, Semgrep custom rules for security patterns, SonarQube security hot... |
| | | `skills-matrix/static-application-security-testing-sast` |
| `threat-modeling-stride` | Alta, ~600tok, 210lns | STRIDE spoofing tampering repudiation information disclosure denial of service elevation of privilege, thre... |
| | | `skills-matrix/threat-modeling-stride` |
| `vulnerability-scanning-dependency-check` | Alta, ~600tok, 249lns | dependency scanning with OWASP Dependency-Check and Trivy, CVE correlation with NVD and OSV databases, auto... |
| | | `skills-matrix/vulnerability-scanning-dependency-check` |
| `zero-trust-architecture-sdlc` | Alta, ~650tok, 220lns | zero trust principles never trust always verify, micro-segmentation and per-request authentication, least p... |
| | | `skills-matrix/zero-trust-architecture-sdlc` |
| | **Frontend Web Fullstack** | *React, Next.js, Tailwind, TypeScript, estado frontend, a11y, testing E2E, Flutter, React Native, Electron, Tauri.* |
| `a11y-accessibility-wcag` | Alta, ~950tok, 376lns | WCAG 2.2 AA AAA POUR principles accessibility, ARIA roles states properties live regions, keyboard navigati... |
| | | `skills-matrix/a11y-accessibility-wcag` |
| `electron-desktop-apps` | Media, ~950tok, 333lns | Electron main process renderer process IPC contextBridge, BrowserWindow preload script context isolation, e... |
| | | `skills-matrix/electron-desktop-apps` |
| `flutter-dart-mobile` | Media, ~900tok, 393lns | Flutter Material Design 3 cross-platform mobile, Dart 3 records pattern matching sealed classes, Riverpod s... |
| | | `skills-matrix/flutter-dart-mobile` |
| `next-js-app-router` | Alta, ~950tok, 325lns | Next.js App Router Server Components Server Actions, data fetching with async components and fetch cache, I... |
| | | `skills-matrix/next-js-app-router` |
| `playwright-e2e-testing` | Alta, ~850tok, 356lns | Playwright E2E testing page objects fixtures, getByRole getByTestId getByText selectors, trace viewer scree... |
| | | `skills-matrix/playwright-e2e-testing` |
| `react-native-mobile` | Alta, ~900tok, 418lns | React Native Expo Router FlatList FlashList, Reanimated 4 animations Gesture Handler, React Navigation nati... |
| | | `skills-matrix/react-native-mobile` |
| `react-ui-development` | Alta, ~850tok, 278lns | React 19 hooks useState useEffect useCallback useMemo, Server Components and client boundaries, compound co... |
| | | `skills-matrix/react-ui-development` |
| `rest-api-integration-client` | Media, ~850tok, 400lns | HTTP client patterns httpx requests, session reuse connection pooling, retry with exponential backoff HTTPS... |
| | | `skills-matrix/rest-api-integration-client` |
| `state-management-frontend` | Alta, ~850tok, 342lns | Zustand store state management React, Jotai atomic state derived atoms, Redux Toolkit createSlice createAsy... |
| | | `skills-matrix/state-management-frontend` |
| `tailwind-css-utility` | Alta, ~800tok, 294lns | Tailwind CSS v4 utility-first responsive design system, @theme directive custom design tokens, dark mode wi... |
| | | `skills-matrix/tailwind-css-utility` |
| `tauri-rust-desktop` | Media, ~900tok, 368lns | Tauri v2 Rust backend web frontend desktop app, tauri::command invoke frontend Rust communication, State Mu... |
| | | `skills-matrix/tauri-rust-desktop` |
| `typescript-type-system` | Alta, ~900tok, 299lns | TypeScript advanced types generics infer satisfies, branded types nominal typing domain primitives, discrim... |
| | | `skills-matrix/typescript-type-system` |
| | **Ingenieria y Herramientas** | *Bash, Python/packaging, pytest, FastAPI, asyncio, Typer, PostgreSQL, Prisma, Redis, Docker Compose, SVG, SQLAlchemy.* |
| `async-python-concurrency` | Alta, ~700tok, 229lns | asyncio TaskGroup structured concurrency gather, anyio library-agnostic async backend trio, asyncio timeout... |
| | | `skills-matrix/async-python-concurrency` |
| `background-jobs-queues` | Alta, ~750tok, 242lns | Celery task queue Redis broker result backend, BullMQ Node.js queue worker, task chaining chain group chord... |
| | | `skills-matrix/background-jobs-queues` |
| `bash-scripting-advanced` | Alta, ~650tok, 230lns | bash strict mode set -euo pipefail, argument parsing getopts while case shift, jq JSON query and transforma... |
| | | `skills-matrix/bash-scripting-advanced` |
| `docker-compose-watch` | Alta, ~600tok, 239lns | Docker Compose watch file sync hot reload, develop watch sync rebuild sync+restart, docker compose up --wat... |
| | | `skills-matrix/docker-compose-watch` |
| `dotenv-environment-vars` | Media, ~600tok, 231lns | dotenv .env file loading Node.js, dotenvx encrypted .env for production, .env.example template committed, .... |
| | | `skills-matrix/dotenv-environment-vars` |
| `fastapi-rest-development` | Alta, ~700tok, 239lns | FastAPI REST API Pydantic v2 validation, APIRouter dependency injection Depends, async lifespan context man... |
| | | `skills-matrix/fastapi-rest-development` |
| `postgresql-advanced` | Alta, ~750tok, 237lns | PostgreSQL upsert INSERT ON CONFLICT, recursive CTE tree traversal query, window functions RANK LAG SUM OVE... |
| | | `skills-matrix/postgresql-advanced` |
| `prisma-orm-database` | Alta, ~700tok, 259lns | Prisma schema model datasource generator, migrations prisma migrate dev deploy, relations one-to-many many-... |
| | | `skills-matrix/prisma-orm-database` |
| `pytest-testing-quality` | Alta, ~650tok, 232lns | pytest fixtures conftest scope, parametrize multiple inputs test cases, mocking with pytest-mock mocker, as... |
| | | `skills-matrix/pytest-testing-quality` |
| `python-packaging-pyproject` | Alta, ~700tok, 235lns | pyproject.toml modern Python packaging, uv fast package manager pip alternative, hatchling build backend, e... |
| | | `skills-matrix/python-packaging-pyproject` |
| `redis-caching-patterns` | Alta, ~700tok, 251lns | Redis cache-aside lazy loading pattern, distributed lock SET NX EX redlock, rate limiter sliding window INC... |
| | | `skills-matrix/redis-caching-patterns` |
| `sqlite-sqlalchemy-persistence` | Alta, ~700tok, 236lns | SQLite SQLAlchemy 2.0 mapped_column ORM, Repository generic CRUD soft delete, TimestampMixin created_at upd... |
| | | `skills-matrix/sqlite-sqlalchemy-persistence` |
| `svg-converter-rasterization` | Media, ~600tok, 219lns | SVG to PNG JPG PDF conversion CairoSVG Pillow, batch convert SVG list parallel ThreadPoolExecutor, format r... |
| | | `skills-matrix/svg-converter-rasterization` |
| `svg-generation-programmatic` | Media, ~650tok, 231lns | SVG XML programmatic generation lxml, svg_builder add_circle add_rect add_text, shape_factory registry disp... |
| | | `skills-matrix/svg-generation-programmatic` |
| `typer-cli-applications` | Alta, ~650tok, 240lns | Typer CLI app subcommands Annotated arguments options, Rich integration rich_markup_mode colorful output, t... |
| | | `skills-matrix/typer-cli-applications` |

---

## 4. Protocolo de Carga Bajo Demanda (On-Demand Loading Rules)

1.  **Evaluacion de Entrada:** El agente escanea la consulta y busca coincidencias en los triggers semanticos.
2.  **Carga Localizada:** Identificada una coincidencia, lee solo la ruta del `SKILL.md` asociado.
3.  **Ejecucion:** Aplica los patrones y convenciones del skill al contexto actual, priorizando las secciones de implementacion y cheatsheet.
4.  **Compatibilidad con 00-template:** Si un SKILL.md no tiene la estructura completa de 9 secciones, el agente debe inferir las secciones faltantes desde la documentacion oficial.
5.  **Skills Inactivos:** Si un skill no se usa en 60 dias, se archiva automaticamente.
6.  **Carga en Cadena:** Si un skill declara dependencias en S8 (Skills Relacionados), cargarlos automaticamente si el contexto lo permite.

---

### Leyenda de Prioridades

| Prioridad | Significado | Accion del Agente |
| :--- | :--- | :--- |
| **Alta** | Critico para la tarea | Cargar inmediatamente |
| **Media** | Usado frecuentemente | Cargar bajo demanda segun trigger |
| **Baja** | Especializado o nicho | Cargar solo si el trigger es exacto |

---

*Matrix generada: 228 skills en 8 dominios, ~65,483 lineas totales. Ultima actualizacion: 2026-06-13*
