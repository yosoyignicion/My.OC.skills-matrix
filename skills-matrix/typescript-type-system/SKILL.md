---
name: typescript-type-system
description: "TypeScript es un superset de JavaScript que añade tipado estático opcional"
---
# TypeScript Type System

## Semantic Triggers
```
TypeScript advanced types generics infer satisfies, branded types nominal typing domain primitives, discriminated unions exhaustive matching, template literal types mapped types key remapping, conditional types infer utility types, satisfies operator type narrowing
```

---

## 1. Definición Teórica

TypeScript es un superset de JavaScript que añade tipado estático opcional. Su sistema de tipos es estructural (duck typing a nivel de tipos) pero permite emular nominal typing mediante branded types. El sistema incluye tipos condicionales (`T extends U ? X : Y`), tipos mapeados (`{ [K in keyof T]: T[K] }`), tipos de literales de plantilla, e inferencia contextual. El operador `satisfies` (TS 4.9+) permite validar que un valor cumple un tipo sin ensanchar (widen) su tipo inferido. La filosofía central: "tipos en tiempo de compilación, cero overhead en runtime".

---

## 2. Implementación de Referencia

TypeScript 5.8+ con `strict: true` en `tsconfig.json`. Preferir `interface` para APIs públicas (declaration merging, extends) y `type` para uniones, intersecciones y tipos condicionales. Evitar `any` en favor de `unknown` con guards.

### Ejemplo Práctico Avanzado

```typescript
// Branded (nominal) types — previene mezclar tipos del mismo shape
type Brand<T, B extends string> = T & { __brand: B }
type USD = Brand<number, "USD">
type EUR = Brand<number, "EUR">

function add(a: USD, b: USD): USD {
  return (a + b) as USD
}

// Template literal types — type-level string manipulation
type EventName = `on${Capitalize<string>}`
type ColorShade = `${"light" | "dark"}-${number}`
type HexColor = `#${string}`
type CSSValue = `${number}${"px" | "rem" | "em" | "%"}`

// Conditional + infer — extraer tipos de estructuras
type ElementType<T> = T extends (infer U)[] ? U : T
type ReturnOf<T> = T extends (...args: any[]) => infer R ? R : never
type AsyncReturn<T> = T extends Promise<infer R> ? R : T

// Mapped types with key remapping — crear getters/setters
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K]
}

type Setters<T> = {
  [K in keyof T as `set${Capitalize<string & K>}`]: (value: T[K]) => void
}

// Discriminated unions — estado de máquina modelado con tipos
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rect"; width: number; height: number }
  | { kind: "triangle"; base: number; height: number }

function area(s: Shape): number {
  switch (s.kind) {
    case "circle":   return Math.PI * s.radius ** 2
    case "rect":     return s.width * s.height
    case "triangle": return (s.base * s.height) / 2
    default:         return s satisfies never // exhaustive check
  }
}

// satisfies — validar sin widen
const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
  blue: [0, 0, 255],
} satisfies Record<string, string | number[]>

// palette.red → number[] (no string | number[])
palette.red.map(x => x * 2)

// TypedObjectKeys — Object.keys tipado
function typedKeys<T extends Record<string, unknown>>(obj: T): (keyof T)[] {
  return Object.keys(obj) as (keyof T)[]
}
```

**Fuente oficial:** https://www.typescriptlang.org/docs/

### Alternativa de Implementación Específica

**ArkType** para validación en runtime con tipos inferidos automáticamente. ArkType es un validador type-first que infiere el tipo estático de la definición del schema, similar a Zod pero con sintaxis más cercana a TypeScript nativo.

```typescript
import { type } from "arktype"

const User = type({
  name: "string",
  email: "string.email",
  age: "number.integer >= 0",
  role: "'admin' | 'user'",
})

// User.infer → { name: string; email: string; age: number; role: "admin" | "user" }
const result = User({ name: "Alice", email: "alice@example.com", age: 30, role: "admin" })
if (result instanceof type.errors) {
  console.error(result.summary)
}
```

**Fuente oficial:** https://arktype.io/docs

---

## 3. Trade-offs y Decisiones de Arquitectura

| Aspecto | Recomendación |
|---|---|
| **Cuándo usar** | Cualquier proyecto JavaScript de más de 100 líneas; APIs públicas; librerías; equipos grandes |
| **Cuándo evitar** | Prototipos desechables; scripts de una sola ejecución; proyectos donde el equipo no tiene experiencia con TS |
| **Alternativas** | JSDoc (tipado en comentarios, sin compilación); Flow (herramienta similar, menos ecosistema); ArkType/Zod (validación runtime, no tipado estático puro) |
| **Coste/Complejidad** | Medio — la configuración inicial es simple, pero el sistema de tipos avanzado (condicionales, mapped, template literal) tiene una curva de aprendizaje pronunciada. El compilador añade ~2-5s a builds pequeños |

---

## 4. Preguntas Frecuentes (FAQ)

### Caso: `Object.keys()` no infiere tipos específicos

**¿Qué ocasionó el error?**
`Object.keys(obj)` retorna `string[]`, no `(keyof typeof obj)[]`, porque TypeScript modela que los objetos pueden tener propiedades adicionales en runtime.

**¿Cómo se solucionó?**
Usar una función helper con cast:

```typescript
function typedKeys<T extends Record<string, unknown>>(obj: T): (keyof T)[] {
  return Object.keys(obj) as (keyof T)[]
}
```

**¿Por qué funciona esta técnica?**
El cast `as (keyof T)[]` es seguro en la práctica porque estamos iterando sobre las claves conocidas. La alternativa es usar `Object.entries()` que maneja mejor el tipado.

### Caso: Type 'string' is not assignable to type 'never' en discriminated union

**¿Qué ocasionó el error?**
Falta de manejo exhaustivo en un switch sobre una unión discriminada. TypeScript reporta error cuando una rama nueva se añade a la unión pero no se actualiza el switch.

**¿Cómo se solucionó?**
Añadir un default exhaustivo:

```typescript
type Shape = Circle | Rect | Triangle // si se añade Polygon, TS error aquí
function area(s: Shape): number {
  switch (s.kind) {
    case "circle": return ...
    case "rect": return ...
    case "triangle": return ...
    default: return s satisfies never // ❌ Polygon no asignable a never
  }
}
```

**¿Por qué funciona esta técnica?**
`satisfies never` solo compila cuando `s` es de tipo `never`, que solo ocurre si todas las variantes están cubiertas. Si una variante no se maneja, `s` será de ese tipo y no `never`, causando error de compilación.

---

## 5. Vector de IA Agéntica

### Optimización de Contexto

- **Tokens a cargar:** ~900 tokens estimados al invocar este skill
- **Trigger de activación:** "typescript", "type", "generic", "interface" o "type-safe" en la consulta
- **Prioridad de carga:** Alta — TypeScript es ubicuo en frontend moderno
- **Dependencias:** Ninguna directa; complementario a todos los skills de frontend

### Tool Integration

```json
{
  "tool_name": "typescript-type-system",
  "description": "Guía del sistema de tipos de TypeScript: branded types, discriminated unions, conditional types, mapped types, satisfies",
  "triggers": ["typescript", "type", "generic", "interface", "type-safe", "branded type"],
  "context_hint": "Inyectar sección 2 (Implementación) para ejemplos de tipos avanzados. FAQ para errores comunes de tipos.",
  "output_format": "markdown",
  "max_tokens": 2900
}
```

### Prompt Snippet (carga rápida)

```
Cuando el usuario pregunte sobre tipos de TypeScript, carga el skill typescript-type-system
y responde con ejemplos prácticos. Prioriza branded types para domain primitives
y discriminated unions para modelado de estado.
```

---

## 6. Uso en Terminal y GUI/Web

### Terminal (CLI)

```bash
# Verificar tipos
npx tsc --noEmit

# Compilar
npx tsc

# Modo watch
npx tsc --watch

# Generar declaration files (.d.ts)
npx tsc --declaration --emitDeclarationOnly

# tsconfig básico
npx tsc --init

# Comprobar versión y tipos disponibles
npx tsc --version
```

### GUI / Web

- **VS Code:** Intellisense completo — autocompletado, refactors (rename symbol, extract type), diagnostics en tiempo real
- **TypeScript Playground:** https://www.typescriptlang.org/play — probar tipos sin instalar nada
- **Error Lens (extension):** Muestra errores TS inline en el editor
- **Pretty TypeScript Errors (extension):** Errores formateados más legibles
- **Source map:** Los `.map` generados permiten debuggear TS directamente en browser DevTools

### Hotkeys / Atajos

| Acción | Atajo CLI | Atajo GUI |
|---|---|---|
| Ver tipo de símbolo | — | `Ctrl+Shift+Space` / Hover |
| Renombrar símbolo | — | `F2` |
| Ir a definición | — | `F12` / `Ctrl+Click` |
| Fix rápido | — | `Ctrl+.` (Quick Fix) |
| Organizar imports | `npx tsc --noEmit` | `Alt+Shift+O` |

---

## 7. Cheatsheet Rápido

```typescript
// Utility types básicos
Partial<T>, Required<T>, Readonly<T>, Pick<T, K>, Omit<T, K>
Record<K, V>, Exclude<T, U>, Extract<T, U>, NonNullable<T>
ReturnType<T>, Parameters<T>, Awaited<T>

// Branded type
type Brand<T, B> = T & { __brand: B }
type UserId = Brand<string, "UserId">

// Discriminated union
type Result<T, E = Error> =
  | { ok: true; value: T }
  | { ok: false; error: E }

// satisfies
const x = { a: 1, b: "hello" } satisfies Record<string, string | number>
```

---

## 8. Skills Relacionados

| Skill ID | Relación | ¿Cargar junto? |
|---|---|---|
| `07-01-react-ui-development` | Complementario | Sí |
| `07-02-next-js-app-router` | Complementario | Sí |
| `07-05-state-management-frontend` | Complementario | Sí |
| `07-12-rest-api-integration-client` | Complementario | No |

---

## 9. Metadatos del Skill

```yaml
---
id: typescript-type-system
domain: 07-frontend-web-fullstack
version: 1.0.0
created: 2026-06-12
updated: 2026-06-12
author: opencode-agent
status: active
archive_after: 2026-08-11
source: old-skills/opencode-bak-skills/typescript
tags: [typescript, types, generics, type-system, frontend, programming-language]
---
```

---

*Template v1.0 — 9 secciones. Última actualización: 2026-06-12*
