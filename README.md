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

No hace falta cuenta de GitHub ni terminal.

1. En la app de Claude (o en claude.ai), ir a **Customize > Plugins**.
2. **Add > Add marketplace** y pegar `https://github.com/ekiste83/claude-kit`.
3. En la lista aparece el plugin **kit**: abrirlo y pulsar **Add**.

Queda guardado en tu cuenta. Úsalo en **Cowork** o en la pestaña **Code**: el consejo necesita agentes, y el chat normal no los ejecuta. En Code, si ya tenías una sesión abierta, escribe `/reload-plugins` o abre una nueva. Luego escribe `/council` seguido de la pregunta.

**Para recibir las mejoras:** en **Customize > Plugins**, abrir el marketplace y pulsar **Check for updates**, o activar **Sync automatically** para que lleguen solas.

**Desde la terminal** (alternativa, solo para Claude Code en ese equipo):

```bash
claude plugin marketplace add https://github.com/ekiste83/claude-kit.git
claude plugin install kit@kit
```

## Versión para Gemini CLI

La carpeta `gemini-kit/` tiene el mismo kit adaptado a Gemini CLI, para quien trabaja con Google en lugar de Claude. Instalación y uso en `gemini-kit/README.md`. Es una copia: si cambias un asesor, un revisor o una skill aquí, cámbialo también allí.

## Para sacarle partido

El consejo y los revisores leen el `CLAUDE.md` del proyecto. Cuanto mejor diga qué es el proyecto, quién lo usa, el stack y sus reglas innegociables ("NUNCA…", "SIEMPRE…"), mejores opiniones dan. Si además hay un `docs/decisions.md` con las decisiones y su fecha, el archivista lo usa para avisar cuando se reabre algo ya decidido.

## Cómo modificarlo (mantenimiento)

1. Editar los archivos del repo.
2. Comprobar: `claude plugin validate .claude-plugin/plugin.json` (el único aviso esperado es que no hay `version`).
3. Si el cambio toca `agents/` o `skills/`, llevarlo también a `gemini-kit/` y comprobar con `gemini extensions validate gemini-kit`.
4. Commit. El plugin no lleva `version` a propósito: cada commit cuenta como versión nueva.
5. `claude plugin update kit@kit` y push a GitHub.
6. Abrir una sesión nueva (o `/reload-plugins`) para usar la versión nueva.

## Origen

Lo creó Llorenç Fanés. El consejo nació en mayo de 2026 y en octubre se rehízo para que sirva a cualquier proyecto, con tres ideas tomadas del consejo de ECC. Los revisores toman de ECC la disciplina anti-ruido y parte de sus comprobaciones. Licencia y créditos en `THIRD_PARTY_NOTICES.md`.
