---
name: owasp-top-10-mitigation
description: "OWASP Top 10 (2021) clasifica los riesgos de seguridad web más críticos: A01 Broken Access Control, A02 Cryptographic Failures, A03 Injection, A04 Insecure Design, A05 Security Misconfiguration, A0..."
---
# owasp-top-10-mitigation

## Semantic Triggers
```
OWASP Top 10 2021 mitigation strategies, SQL injection prevention with parameterized queries, XSS defense with CSP and output encoding, broken access control and least privilege, security misconfiguration and hardening checklists, SSRF mitigation with allowlists and URL validation
```

---

## 1. Definición Teórica

OWASP Top 10 (2021) clasifica los riesgos de seguridad web más críticos: A01 Broken Access Control, A02 Cryptographic Failures, A03 Injection, A04 Insecure Design, A05 Security Misconfiguration, A06 Vulnerable Components, A07 Authentication Failures, A08 Integrity Failures, A09 Logging/Monitoring, A10 SSRF. Cada categoría tiene patrones de prevención específicos. Es la referencia estándar para equipos de desarrollo que implementan programas de seguridad.

---

## 2. Implementación de Referencia

**OWASP ASVS (Application Security Verification Standard)** v4.0 proporciona un checklist detallado de requisitos de seguridad por nivel (L1-L3). La implementación se basa en frameworks modernos con protecciones integradas.

### Ejemplo Práctico Avanzado

```python
# A01: Broken Access Control — ownership check
async def get_invoice(invoice_id: int, current_user: User = Depends(get_user)):
    invoice = await db.fetch_one("SELECT * FROM invoices WHERE id = :id", {"id": invoice_id})
    if not invoice:
        raise HTTPException(404)
    if invoice.user_id != current_user.id and "admin" not in current_user.roles:
        raise HTTPException(403, "Access denied")
    return invoice

# A03: Injection — parameterized queries
async def search_user(email: str):
    return await db.fetch_all(
        "SELECT * FROM users WHERE email = :email",
        {"email": email}  # NEVER f-string interpolation
    )

# A10: SSRF mitigation
from urllib.parse import urlparse

ALLOWED_DOMAINS = {"api.internal.example.com", "cdn.example.com"}

def validate_redirect_url(url: str) -> bool:
    parsed = urlparse(url)
    if parsed.hostname not in ALLOWED_DOMAINS:
        raise ValueError(f"URL {url} not allowed")
    if parsed.scheme not in ("https",):
        raise ValueError("Only HTTPS allowed")
    return True
```

**Fuente oficial:** https://owasp.org/www-project-top-ten/

### Alternativa de Implementación Específica

**OWASP Cheat Sheet Series**: Guías específicas por categoría (SQL Injection Prevention, XSS Prevention, CSRF Prevention, etc.). Disponibles como markdown para integración en repositorios internos.

---

## 3. Trade-offs y Decisiones de Arquitectura

| Aspecto | Recomendación |
|---|---|
| **Cuándo usar** | Como checklist base para cualquier aplicación web expuesta a internet |
| **Cuándo evitar** | Aplicaciones internas sin datos sensibles (aunque aplicar A01-A03 siempre es recomendable) |
| **Alternativas** | CWE Top 25 (más granular, orientado a debilidades), SANS Top 25, ASVS (verificación detallada) |
| **Coste/Complejidad** | Bajo. Son principios universales. Implementarlos desde el inicio es barato; retrofit es caro |

---

## 4. Preguntas Frecuentes (FAQ)

### Caso: SQL injection en ORM "seguro"

**¿Qué ocasionó el error?**
Uso de `Model.objects.raw("SELECT * FROM users WHERE name = '" + name + "'")` en Django ORM, bypassing las protecciones del ORM.

**¿Cómo se solucionó?**
Reemplazar con `Model.objects.raw("SELECT * FROM users WHERE name = %s", [name])` o mejor, usar el QuerySet builder.

**¿Por qué funciona esta técnica?**
Los raw queries con parámetros usan prepared statements del driver, separando SQL de datos. CVE-2019-15043 (Django) demostró que incluso ORMs pueden tener SQLi si se usan incorrectamente.

### Caso: CSP bypass por JSONP injection

**¿Qué ocasionó el error?**
Una CSP con `script-src 'self' https://trusted.cdn.com` permitía JSONP endpoints que devolvían JavaScript ejecutable.

**¿Cómo se solucionó?**
Eliminar JSONP endpoints o usar `strict-dynamic` en CSP para eliminar la whitelist de dominios.

**¿Por qué funciona esta técnica?**
`strict-dynamic` rompe la confianza en whitelists de dominios y solo permite scripts que pasan por nonce/hash, eliminando JSONP como vector.

---

## 5. Vector de IA Agéntica

### Optimización de Contexto

- **Tokens a cargar:** ~650 tokens estimados al invocar este skill
- **Trigger de activación:** "OWASP" o "seguridad web" en la consulta del usuario
- **Prioridad de carga:** Alta — base de seguridad para aplicaciones web
- **Dependencias:** `01-threat-modeling-stride`, `08-static-application-security-testing-sast`

### Tool Integration

```json
{
  "tool_name": "owasp-top-10-mitigation",
  "description": "Guía de mitigación para OWASP Top 10 2021 con ejemplos de código y patrones de prevención",
  "triggers": ["OWASP", "SQL injection", "XSS", "SSRF", "broken access control", "security misconfiguration"],
  "context_hint": "Inyectar secciones 1-2 cuando el usuario mencione riesgos de seguridad web específicos",
  "output_format": "markdown",
  "max_tokens": 650
}
```

### Prompt Snippet (carga rápida)

```
Cuando el usuario pregunte sobre OWASP Top 10, carga el skill owasp-top-10-mitigation y responde
con ejemplos de código de mitigación para cada categoría relevante.
```

---

## 6. Uso en Terminal y GUI/Web

### Terminal (CLI)

```bash
# Scan for OWASP Top 10 with ZAP
docker run -v $(pwd):/zap/wrk:rw -t ghcr.io/zaproxy/zaproxy \
  zap-baseline.py -t https://staging.example.com -r owasp-report.html

# Check OWASP dependencies
npm audit --audit-level=high

# Run OWASP Dependency-Check
dependency-check --scan . --format HTML --out ./reports
```

### GUI / Web

- **OWASP ZAP Desktop:** Interfaz gráfica para escaneo interactivo con reportes OWASP Top 10
- **OWASP Dependency-Track:** Dashboard de vulnerabilidades en dependencias con métricas
- **DefectDojo:** Gestión de hallazgos de seguridad con reportes OWASP Top 10

### Hotkeys / Atajos

| Acción | Atajo CLI | Atajo GUI |
|---|---|---|
| Escaneo rápido ZAP | `zap-baseline.py -t URL -r report.html` | ZAP → Quick Scan |
| Dependency check | `dependency-check --scan . --format HTML` | Dependency-Track → Upload BOM |

---

## 7. Cheatsheet Rápido

```python
# OWASP Top 10 quick reference
rules = {
    "A01: Access Control": "Deny by default, ownership checks",
    "A02: Crypto": "bcrypt/argon2, TLS 1.3, no MD5/SHA-1",
    "A03: Injection": "Parameterized queries, ORM, input validation",
    "A04: Design": "Threat modeling, rate limiting",
    "A05: Misconfig": "Hardened templates, automated scanning",
    "A06: Components": "SBOM, npm audit, Dependabot",
    "A07: Auth": "MFA, account lockout, session rotation",
    "A08: Integrity": "Signed pipelines, SBOM attestation",
    "A09: Logging": "Centralized, tamper-proof, no secrets",
    "A10: SSRF": "URL allowlist, no redirects, segmentation",
}
```

---

## 8. Skills Relacionados

| Skill ID | Relación | ¿Cargar junto? |
|---|---|---|
| `01-threat-modeling-stride` | Complementario — threat modeling identifica riesgos OWASP | Sí |
| `08-static-application-security-testing-sast` | Dependiente — SAST automatiza detección de OWASP Top 10 | Sí |
| `07-dynamic-application-security-testing-dast` | Dependiente — DAST valúa runtime OWASP Top 10 | Sí |

---

## 9. Metadatos del Skill

```yaml
---
id: 04-owasp-top-10-mitigation
domain: 06-seguridad-sdlc
version: 1.0.0
created: 2026-06-12
updated: 2026-06-12
author: opencode-agent
status: active
archive_after: 2026-08-11
source: oficial
tags: [owasp, top-10, sql-injection, xss, ssrf, access-control, web-security]
---
```

---

*Template v1.0 — 9 secciones. Última actualización: 2026-06-12*
