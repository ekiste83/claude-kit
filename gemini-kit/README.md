# gemini-kit — consejo de asesores y revisores para Gemini CLI

Versión para **Gemini CLI** del kit de Claude Code que está en la raíz de este repo. Hace lo mismo: nada de lo que hay aquí es de un proyecto concreto, y cada pieza lee el archivo de contexto del proyecto en el que se esté trabajando (`GEMINI.md`; si no hay, `AGENTS.md` o `CLAUDE.md`).

## Qué incluye

| Pieza | Tipo | Para qué |
|---|---|---|
| `/council <pregunta>` | comando + skill | Consejo de asesores sobre una decisión. Convoca a 3 según el tipo de pregunta; `/council full <pregunta>`, a los 5. |
| `council-simplifier` | subagente | "¿Esto se puede quitar? ¿Es la pregunta correcta?" |
| `council-skeptic` | subagente | "¿Qué pasa cuando esto falla?" En código y en planes. |
| `council-user-advocate` | subagente | Defiende a quien va a usar o leer el resultado. |
| `council-pragmatist` | subagente | "Hecho > perfecto", con freno en dinero, irreversible y legal. |
| `council-archivist` | subagente | "¿Esto ya lo decidimos?" Lee decisiones, bitácora y la memoria global de Gemini. |
| `/cerrar-sesion` o "cerramos sesión" | comando + skill | Reglas de cierre del proyecto, consejo, registro de estado y commit tras el OK. "Cierra directo" se salta el consejo. |
| `code-reviewer` | subagente | Revisión de código (TypeScript, React, Edge Functions) con las reglas del proyecto y sin ruido. Si el proyecto tiene revisor propio en `.gemini/agents/`, se usa ese. |
| `supabase-reviewer` | subagente | Migraciones, RLS, vistas, funciones SQL, permisos, Storage y Edge Functions: fugas de datos y cambios destructivos. |

## Requisitos

- **Gemini CLI** con una cuenta que lo pueda usar. Desde el 18 de junio de 2026, Google solo mantiene Gemini CLI para cuentas de empresa (Gemini Code Assist Standard o Enterprise, o un proyecto de Google Cloud); las cuentas personales y gratuitas pasaron a Antigravity CLI (ver más abajo).
- `git` y Node.js 20 o superior, que Gemini CLI ya necesita.
- Probado con Gemini CLI 0.63.0.

## Instalación

En una terminal:

```bash
git clone https://github.com/ekiste83/claude-kit.git ~/claude-kit
gemini extensions link ~/claude-kit/gemini-kit
```

`link` deja la extensión apuntando a esa carpeta, así que no hace falta reinstalar al actualizar. Para comprobar que está: `gemini extensions list` debe mostrar `gemini-kit` con sus dos skills. Dentro de Gemini CLI, `/skills list` enseña las skills y `/commands list`, los comandos.

**Para recibir las mejoras:** `cd ~/claude-kit && git pull`, y abrir una sesión nueva de Gemini CLI.

**Si te han pasado un zip** en lugar del enlace al repo: descomprímelo en una carpeta que no vayas a mover (por ejemplo, `~/gemini-kit`) y ejecuta `gemini extensions link ~/gemini-kit`. Para actualizar, sustituye esa carpeta por la del zip nuevo y abre una sesión nueva.

## Uso

- `/council ¿Migramos el login a magic links o lo dejamos como está?`
- `/council full ¿Subimos el precio del plan básico?`
- Al terminar: "cerramos sesión" (o `/cerrar-sesion`). Con "cierra directo" se salta el consejo.
- Revisión suelta: `@code-reviewer revisa los cambios sin commitear` o `@supabase-reviewer revisa la última migración`.

**Qué te va a pedir confirmar:** la primera vez que se activa una skill, Gemini CLI pregunta si la dejas cargar. Los revisores y el archivista usan la terminal para cosas como `git diff` o leer `~/.gemini/GEMINI.md`, y cada comando pide confirmación como cualquier otro. Los demás asesores solo leen archivos.

## Para sacarle partido

El consejo y los revisores leen el `GEMINI.md` del proyecto. Cuanto mejor diga qué es el proyecto, quién lo usa, el stack y sus reglas innegociables ("NUNCA…", "SIEMPRE…"), mejores opiniones dan. Si además hay un `docs/decisions.md` con las decisiones y su fecha, el archivista lo usa para avisar cuando se reabre algo ya decidido. `/init` dentro de Gemini CLI genera un primer `GEMINI.md` a partir del proyecto.

## Si tu cuenta usa Antigravity CLI

Google dice que Antigravity CLI (`agy`) conserva skills, subagentes y extensiones (allí, "plugins"), y que `agy plugin import gemini` convierte las extensiones instaladas en `~/.gemini/extensions/`. **No está probado con esta extensión.** Si lo usas, instálala primero como arriba y después ejecuta `agy plugin import gemini`. Ojo: en Antigravity los subagentes funcionan en segundo plano, así que el consejo puede tardar más en presentar la síntesis.

## Mantenimiento

Este kit es una copia adaptada de los archivos de la raíz del repo (`agents/` y `skills/`). Las diferencias son de formato y rutas: `tools` como lista con los nombres de Gemini (`read_file`, `grep_search`…), los nombres de los subagentes sin el prefijo `kit:`, `GEMINI.md` en lugar de `CLAUDE.md` y `.gemini/` en lugar de `.claude/`. **Si cambias un asesor o un revisor en un lado, cámbialo también en el otro.**

Para comprobar: `gemini extensions validate gemini-kit`. Ese comando no revisa los subagentes; si uno tiene un error de formato, Gemini CLI lo avisa al arrancar ("Agent loading error").

## Origen

Lo creó Llorenç Fanés a partir de su kit para Claude Code. Licencia y créditos en `THIRD_PARTY_NOTICES.md`.
