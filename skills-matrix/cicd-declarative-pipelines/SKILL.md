---
name: cicd-declarative-pipelines
description: "Los pipelines CI/CD declarativos se definen como código YAML dentro del repositorio (.github/workflows/ o .gitlab-ci.yml)"
---
# CI/CD Declarative Pipelines

## Semantic Triggers
```
ci cd pipeline, github actions, gitlab ci, declarative workflow, matrix build test, pipeline cache artifacts, oidc cloud auth, composite action reuse
```

---

## 1. Definición Teórica

Los pipelines CI/CD declarativos se definen como código YAML dentro del repositorio (.github/workflows/ o .gitlab-ci.yml). La estrategia matrix ejecuta jobs en múltiples combinaciones (OS, versiones de lenguaje) con fail-fast: false para máxima señal. OIDC permite autenticación cloud sin credenciales estáticas. Composite actions encapsulan pasos reutilizables entre workflows.

---

## 2. Implementación de Referencia

GitHub Actions es la plataforma líder con su ecosistema de marketplace. GitLab CI y CircleCI ofrecen modelos similares con ejecutores autogestionados.

### Ejemplo Práctico Avanzado

```yaml
name: CI/CD Pipeline
on:
  push: { branches: [main] }
  pull_request: { branches: [main] }
  workflow_dispatch: {}
env:
  NODE_VERSION: "22"
  PNPM_VERSION: "10"

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]
      fail-fast: false
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
        with: { version: "${{ env.PNPM_VERSION }}" }
      - uses: actions/setup-node@v4
        with: { node-version: "${{ matrix.node-version }}", cache: "pnpm" }
      - run: pnpm install --frozen-lockfile
      - run: pnpm lint && pnpm typecheck
      - run: pnpm test -- --coverage
      - uses: actions/upload-artifact@v4
        if: failure()
        with: { name: test-results-${{ matrix.node-version }}, path: test-results/ }

  deploy:
    needs: [test]
    if: github.ref == 'refs/heads/main'
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - uses: docker/build-push-action@v6
        with:
          push: true
          tags: ghcr.io/${{ github.repository }}:${{ github.sha }}
```

**Fuente oficial:** https://docs.github.com/en/actions

### Alternativa de Implementación Específica

GitLab CI con runners autogestionados para equipos que necesitan ejecutores on-premise con GPU/aceleración hardware y pipelines multi-stage más complejos.

---

## 3. Trade-offs y Decisiones de Arquitectura

| Aspecto | Recomendación |
|---|---|
| **Cuándo usar** | Automatización de build, test, deploy con integración nativa al repositorio |
| **Cuándo evitar** | Workflows extremadamente complejos (>100 jobs), pipelines que requieren ejecutores muy especializados |
| **Alternativas** | CircleCI (caché configurable), Jenkins (customizable), Argo Workflows (K8s nativo) |
| **Coste/Complejidad** | Bajo-medio. GitHub Actions incluye minutos gratuitos. Costo escala con paralelismo y tiempo de ejecución |

---

## 4. Preguntas Frecuentes (FAQ)

### Caso: Cache no restaurada entre runs

**¿Qué ocasionó el error?**
El hash del lockfile cambió por un `pnpm install` sin `--frozen-lockfile`, invalidando el cache key.

**¿Cómo se solucionó?**
```yaml
- run: pnpm install --frozen-lockfile  # ← siempre usar frozen-lockfile en CI
```
Y se agregó fallback cache:
```yaml
key: pnpm-${{ hashFiles('pnpm-lock.yaml') }}
restore-keys: pnpm-
```

**¿Por qué funciona esta técnica?**
`hashFiles('pnpm-lock.yaml')` asegura que el cache se invalide solo cuando las dependencias cambien.

### Caso: Secrets expuestos en logs

**¿Qué ocasionó el error?**
Usar `${{ env.SECRET }}` en lugar de `${{ secrets.SECRET }}` imprimió el valor en los logs.

**¿Cómo se solucionó?**
Siempre usar `${{ secrets.X }}` que GitHub auto-masca. Nunca pasar secrets via `env:` para debug.

**¿Por qué funciona esta técnica?**
GitHub Actions auto-detecta y masca valores de `secrets.*` en logs.

---

## 5. Vector de IA Agéntica

### Optimización de Contexto

- **Tokens a cargar:** ~370 tokens al invocar este skill
- **Trigger de activación:** ci/cd, github actions, pipeline, gitlab ci, workflow, automation
- **Prioridad de carga:** Alta — skill central para automatización
- **Dependencias:** `31-git-workflows-conventional`, `07-progressive-delivery-canary`

### Tool Integration

```json
{
  "tool_name": "cicd-declarative-pipelines",
  "description": "Creación y debugging de pipelines CI/CD declarativos con GitHub Actions, GitLab CI y matrix builds",
  "triggers": ["ci/cd", "github actions", "pipeline", "gitlab ci", "matrix build"],
  "context_hint": "Activar cuando se hable de automatización de build/test/deploy",
  "output_format": "markdown",
  "max_tokens": 1850
}
```

### Prompt Snippet (carga rápida)

```
Cuando el usuario pregunte sobre CI/CD o pipelines declarativos, carga el skill
cicd-declarative-pipelines. Proporciona ejemplos de matrix strategy, caching OIDC,
composite actions, y patrones de seguridad.
```

---

## 6. Uso en Terminal y GUI/Web

### Terminal (CLI)

```bash
# GitHub CLI
gh workflow list
gh workflow run ci.yml --ref main
gh run list --limit 10
gh run watch <run-id>
gh run download <run-id> -n test-results

# Act locally
act --job test -s GITHUB_TOKEN=xxx
act pull_request --verbose

# Cache management
gh actions-cache list
gh actions-cache delete <key>
```

### GUI / Web

- **GitHub Actions UI**: Dashboard de workflows, logs en vivo, artefactos descargables, matriz de resultados
- **GitLab CI UI**: Pipeline DAG visual, job traces, environments con deploy status
- **Netlify/Vercel**: Preview deployments por PR
- **VS Code**: GitHub Actions extension para ver workflows y logs locales

### Hotkeys / Atajos

| Acción | Atajo CLI | Atajo GUI |
|---|---|---|
| Ver workflows | `gh workflow list` | Actions tab |
| Re-run failed | `gh run rerun <id> --failed` | Re-run failed jobs |
| Cancel run | `gh run cancel <id>` | Cancel button |

---

## 7. Cheatsheet Rápido

```yaml
# Mínimo CI workflow
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [18, 20]
      fail-fast: false
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: "${{ matrix.node }}", cache: "npm" }
      - run: npm ci
      - run: npm test
```

---

## 8. Skills Relacionados

| Skill ID | Relación | ¿Cargar junto? |
|---|---|---|
| `31-git-workflows-conventional` | complementario (conventional commits + CI) | Sí |
| `07-progressive-delivery-canary` | complementario (CI + canary deploy) | Sí |
| `25-load-testing-k6-distributed` | complementario (load test en CI) | No |
| `22-artifact-registries-security` | complementario (registro de artefactos) | Sí |

---

## 9. Metadatos del Skill

```yaml
---
id: cicd-declarative-pipelines
domain: 04-devops-platform
version: 1.0.0
created: 2026-06-12
updated: 2026-06-12
author: opencode-agent
status: active
archive_after: 2026-08-11
source: old-skills/opencode-bak-skills/github-actions
tags: [ci-cd, github-actions, gitlab-ci, pipelines, matrix-build, oidc, automation]
---
```

---

*Template v1.0 — 9 secciones. Última actualización: 2026-06-12*
