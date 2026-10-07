# kit — consejo de asesores y revisores para Claude Code

Herramientas de trabajo con Claude Code que valen para cualquier proyecto. Nada de lo que hay aquí es de un proyecto concreto: cada pieza lee el `CLAUDE.md` del proyecto en el que se esté trabajando.

## Qué incluye

| Pieza | Tipo | Para qué |
|---|---|---|
| `/council` (`/kit:council`) | skill | Consejo de asesores sobre una decisión. `/council <pregunta>` convoca a 3 según el tipo de pregunta; `/council full <pregunta>`, a los 5. |
| `kit:council-simplifier` | agente | "¿Esto se puede quitar? ¿Es la pregunta correcta?" |
| `kit:council-skeptic` | agente | "¿Qué pasa cuando esto falla?" En código y en planes. |
| `kit:council-user-advocate` | agente | Defiende a quien va a usar o leer el resultado. |
| `kit:council-pragmatist` | agente | "Hecho > perfecto", con freno en dinero, irreversible y legal. |
| `kit:council-archivist` | agente | "¿Esto ya lo decidimos?" Lee decisiones, bitácora y memoria. |
| `kit:cerrar-sesion` | skill | Se activa con "cerramos sesión": reglas de cierre del proyecto, consejo, registro de estado y commit tras el OK. "Cierra directo" se salta el consejo. |
| `kit:code-reviewer` | agente | Revisión de código (TypeScript, React, Edge Functions) con las reglas del proyecto y sin ruido. Si el proyecto tiene revisor propio en `.claude/agents/`, se usa ese. |
| `kit:supabase-reviewer` | agente | Migraciones, RLS, vistas, funciones SQL, permisos, Storage y Edge Functions: fugas de datos y cambios destructivos. |

## Instalación

Hace falta tener Claude Code y `git`. No hace falta cuenta de GitHub. En una terminal:

```bash
claude plugin marketplace add https://github.com/ekiste83/claude-kit.git
claude plugin install kit@kit
```

Después, abrir una sesión nueva de Claude Code y escribir `/council` seguido de la pregunta.

**Para recibir las mejoras:**

```bash
claude plugin marketplace update kit && claude plugin update kit@kit
```

y abrir una sesión nueva.

## Para sacarle partido

El consejo y los revisores leen el `CLAUDE.md` del proyecto. Cuanto mejor diga qué es el proyecto, quién lo usa, el stack y sus reglas innegociables ("NUNCA…", "SIEMPRE…"), mejores opiniones dan. Si además hay un `docs/decisions.md` con las decisiones y su fecha, el archivista lo usa para avisar cuando se reabre algo ya decidido.

## Cómo modificarlo (mantenimiento)

1. Editar los archivos del repo.
2. Comprobar: `claude plugin validate .claude-plugin/plugin.json` (el único aviso esperado es que no hay `version`).
3. Commit. El plugin no lleva `version` a propósito: cada commit cuenta como versión nueva.
4. `claude plugin update kit@kit` y push a GitHub.
5. Abrir una sesión nueva (o `/reload-plugins`) para usar la versión nueva.

## Origen

Lo creó Llorenç Fanés. El consejo nació en mayo de 2026 y en octubre se rehízo para que sirva a cualquier proyecto, con tres ideas tomadas del consejo de ECC. Los revisores toman de ECC la disciplina anti-ruido y parte de sus comprobaciones. Licencia y créditos en `THIRD_PARTY_NOTICES.md`.
