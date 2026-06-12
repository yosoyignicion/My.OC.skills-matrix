---
name: second-termux-processes
description: "Second-termux gestiona procesos en segundo plano via MCP tools (bg_start, bg_stop, bg_status, bg_logs, bg_list, bg_wait)"
---
# Second-Termux Processes (Bgx)

## Semantic Triggers
```
second-termux, background process manager, bgx smart routing, setsid detach, mcp background tools, token zero policy, process lifecycle, long running command, bash background, second-termux mcp
```

---

## 1. Definición Teórica

Second-termux gestiona procesos en segundo plano via MCP tools (bg_start, bg_stop, bg_status, bg_logs, bg_list, bg_wait). Los comandos se desprenden via setsid (start_new_session=True) para sobrevivir reinicios del agente. El script bgx enruta TODAS las llamadas bash tool a través de second-termux automáticamente. El directorio de estado `~/.local/share/second-termux/` persiste entre sesiones. Ciclo de vida: bg_start → bg_status/bg_logs polling → bg_stop o bg_wait → bg_cleanup.

---

## 2. Implementación de Referencia

Second-termux MCP server con bgx wrapper. El agente nunca ejecuta bash directamente — siempre a través de bgx.

### Ejemplo Práctico Avanzado

```
# Ciclo de vida completo de un proceso background

# 1. Iniciar proceso
$ bgx "npm run build"            # bgx_wraps → bg_start → setsid
✨ bgx_build_X7k ✓ 0 (2.35s)     # exit code + elapsed time

# 2. Verificar estado
$ bg_status                      # o bg_list
[
  { "id": "bgx_build_X7k", "status": "completed", "exit_code": 0, "elapsed": 2.35 }
]

# 3. Si falló, obtener logs
$ bg_logs bgx_build_X7k
> build: my-app@1.0.0 /app
> tsc --noEmit && vite build
✓ built in 2.35s

# 4. Proceso long-running
$ bgx "kubectl port-forward svc/api 8000:8000"
✨ bgx_portfwd_A3b ✓ 0 (0.10s)

$ bg_status bgx_portfwd_A3b
{ "id": "bgx_portfwd_A3b", "status": "running", "pid": 12345, "elapsed": 45.2 }

# 5. Detener proceso
$ bg_stop bgx_portfwd_A3b
✓ stopped bgx_portfwd_A3b (SIGTERM → SIGKILL after 5s)

# 6. Limpiar
$ bg_cleanup
✓ cleaned 3 stale process files
```
```
# MCP tools JSON-RPC equivalentes:
# bg_start → { "name": "bg_start", "arguments": { "name": "build", "command": "npm run build", "cwd": "/app" } }
# bg_status → { "name": "bg_status", "arguments": { "name": "build" } }
# bg_stop → { "name": "bg_stop", "arguments": { "name": "build", "force": false } }
# bg_logs → { "name": "bg_logs", "arguments": { "name": "build", "tail": 50 } }
# bg_list → { "name": "bg_list" }
# bg_wait → { "name": "bg_wait", "arguments": { "name": "build", "timeout": 30 } }
```

**Fuente oficial:** https://github.com/anomalyco/second-termux

### Alternativa de Implementación Específica

tmux + bash background con nohup para entornos sin MCP. Menos integrado pero funcional: `nohup long-running-command &> output.log &`.

---

## 3. Trade-offs y Decisiones de Arquitectura

| Aspecto | Recomendación |
|---|---|
| **Cuándo usar** | Comandos largos que exceden timeout del LLM, procesos que deben sobrevivir al agente |
| **Cuándo evitar** | Comandos rápidos (<5s), comandos que requieren interacción TTY |
| **Alternativas** | tmux (sesiones persistentes), nohup + setsid (manual), systemd-run (user services) |
| **Coste/Complejidad** | Bajo. bgx wrapper es transparente. Zero overhead en comandos rápidos |

---

## 4. Preguntas Frecuentes (FAQ)

### Caso: bgx no encuentra el comando

**¿Qué ocasionó el error?**
bgx no hereda el PATH completo del shell interactivo, solo el PATH del proceso MCP.

**¿Cómo se solucionó?**
```bash
# Usar PATH absoluto o verificar que bgx tenga acceso al PATH del usuario
bgx "export PATH=$PATH:/usr/local/bin && npm run build"
# O configurar bgx con --env PATH=$PATH
```

**¿Por qué funciona esta técnica?**
bgx se ejecuta en un entorno mínimo. Exportar PATH explícitamente asegura que los binarios sean encontrados.

### Caso: Proceso en segundo plano desaparece sin logs

**¿Qué ocasionó el error?**
El proceso se ejecutó pero falló silenciosamente porque la salida estándar no se redirigió.

**¿Cómo se solucionó?**
bgx automáticamente redirige stdout/stderr. Si no se ve salida, usar:
```bash
bg_logs <job-id> --tail 50
grep -i "error" ~/.local/share/second-termux/<job-id>.log
```

**¿Por qué funciona esta técnica?**
bgx captura ambas salidas (stdout+stderr) combinadas. `bg_logs` las recupera del archivo de log persistente.

---

## 5. Vector de IA Agéntica

### Optimización de Contexto

- **Tokens a cargar:** ~350 tokens al invocar este skill
- **Trigger de activación:** second-termux, bgx, background process, long running, detached
- **Prioridad de carga:** Alta — esencial para comandos largos
- **Dependencias:** ninguno

### Tool Integration

```json
{
  "tool_name": "second-termux-processes",
  "description": "Gestión de procesos en segundo plano via bgx/MCP, con setsid detach y logs persistentes",
  "triggers": ["bgx", "background process", "long running", "detached", "second-termux"],
  "context_hint": "Activar automáticamente para cualquier comando que pueda exceder 30s",
  "output_format": "markdown",
  "max_tokens": 1750
}
```

### Prompt Snippet (carga rápida)

```
Cuando el agente necesite ejecutar un comando que pueda tomar >10s, usa siempre bgx.
Nunca ejecutes bash directamente. bgx automáticamente maneja setsid, logs, y timeout.
Usa bg_logs si la salida es necesaria, bg_status para verificar estado.
```

---

## 6. Uso en Terminal y GUI/Web

### Terminal (CLI)

```bash
# Comandos bgx
bgx "npm run dev"
bgx "docker build -t myapp ."
bgx "kubectl apply -f manifests/"
bgx "terraform apply plan.out"

# Gestión de procesos
bg_list
bg_status <job-id>
bg_logs <job-id>
bg_logs <job-id> --tail 100
bg_stop <job-id>
bg_stop <job-id> --force
bg_wait <job-id> --timeout 60
bg_cleanup

# Estado interno
ls -la ~/.local/share/second-termux/
cat ~/.local/share/second-termux/<job-id>.log
cat ~/.local/share/second-termux/<job-id>.json

# Monitoreo
watch -n 1 "bg_list | jq '.[] | select(.status==\"running\") | .id'"
```

### GUI / Web

- No tiene GUI nativa. Toda la interacción es via MCP (data channel) o CLI bgx
- Los logs pueden verse con cualquier editor: `vim ~/.local/share/second-termux/<job-id>.log`
- Integración con code-server/VS Code para monitorear procesos en background

### Hotkeys / Atajos

| Acción | Atajo CLI | Atajo GUI |
|---|---|---|
| List procesos | `bg_list` | MCP → bg_list |
| Ver logs | `bg_logs <id>` | MCP → bg_logs |
| Detener | `bg_stop <id>` | MCP → bg_stop |

---

## 7. Cheatsheet Rápido

```bash
# bgx commands
bgx "npm run build"
bgx "docker compose up -d"
bgx "kubectl rollout status deployment/api"
bgx "terraform plan -out=plan.out"

# Process management
bg_list
bg_status bgx_build_X7k
bg_logs bgx_build_X7k --tail 20
bg_stop bgx_portfwd_A3b
bg_wait bgx_deploy_F9q --timeout 120

# State directory
ls ~/.local/share/second-termux/
```

---

## 8. Skills Relacionados

| Skill ID | Relación | ¿Cargar junto? |
|---|---|---|
| `01-bash-scripting-advanced` | complementario (bash + bgx) | Sí |
| `06-cicd-declarative-pipelines` | complementario (CI + background) | No |
| `12-docker-compose-watch` | complementario (compose + bgx) | No |
| `05-async-python-concurrency` | complementario (async + background) | No |

---

## 9. Metadatos del Skill

```yaml
---
id: second-termux-processes
domain: 04-devops-platform
version: 1.0.0
created: 2026-06-12
updated: 2026-06-12
author: opencode-agent
status: active
archive_after: 2026-08-11
source: old-skills/opencode-bak-skills/second-termux
tags: [second-termux, bgx, background-process, process-management, mcp-tools, setsid]
---
```

---

*Template v1.0 — 9 secciones. Última actualización: 2026-06-12*
