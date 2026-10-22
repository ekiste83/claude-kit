---
name: cerrar-sesion
description: Cierre de una sesión de trabajo. Usar cuando el usuario diga "cerramos sesión", "cerrar sesión", "cierra la sesión" o una variante. Cumple las reglas de cierre del proyecto, convoca al consejo sobre los cambios antes del commit, actualiza el registro de estado del proyecto y commitea solo tras su OK. Con "cierra directo" o "cerramos sin consejo" se salta el consejo.
---

# Cerrar sesión

Vale para cualquier proyecto. Si el proyecto tiene reglas propias de cierre (en su `CLAUDE.md` o en `docs/workflows.md`), **mandan y se suman** a este flujo.

## 1. Lo que exige el proyecto

Lee sus reglas de cierre y cúmplelas antes de seguir. Las habituales:
- **Build verde** si se tocó el frontend.
- **Comprobación de tipos** de lo que se haya cambiado.
- **Copia de la base de datos** si hubo cambios destructivos o migraciones.

Si algo falla, se arregla antes del consejo, o se le dice al usuario.

## 2. Consejo

Sáltalo si el usuario dijo "cierra directo" o "cerramos sin consejo".

- **Si los cambios son triviales** (una errata, un comentario, una línea sin riesgo), pregunta antes: "¿Convoco al consejo o cerramos directo?"
- **Si no, avisa en una línea y convócalo:** "Antes de cerrar, convoco al consejo sobre lo que llevamos. (Di «cierra directo» si prefieres saltarlo.)"
- **Usa la skill `council` en modo completo (los 5 asesores):** pásale la pregunta con `full ` delante. Un error en el cierre sale caro (ya está commiteado o en producción), y el archivero cruza los cambios con lo decidido antes. En el brief, resume qué se ha hecho en la sesión, los archivos tocados y las decisiones tomadas. La pregunta es: *"Antes de cerrar la sesión, evaluad si los cambios de hoy deberían entrar tal cual o hay algo que matizar o arreglar antes del commit."*
- **Si se tocó código de forma importante**, pasa también `kit:code-reviewer`, o el revisor propio del proyecto si tiene uno en `.claude/agents/`. **Si se tocó la base de datos** (migraciones, políticas, funciones SQL, Edge Functions), pasa `kit:supabase-reviewer`. Lánzalos en paralelo con el consejo.
- **Presenta la síntesis** y pregunta si procede, si ajusta algo o si quiere ver el informe completo de algún asesor.
- **Si aparece algo crítico**, no cierres hasta resolverlo o hasta que el usuario decida conscientemente posponerlo. Si lo pospone, apúntalo como deuda (`DT-*` en `CLAUDE.local.md` si el proyecto usa ese patrón).

## 3. Registro de estado (tras su OK)

Actualiza el registro de estado del proyecto **según su convención**: por ejemplo, reescribir "Estado actual" en `docs/bitacora.md`, o la sección de la sesión en `CLAUDE.local.md`. Deja escrito:
- **qué se ha hecho;**
- **qué queda pendiente;**
- **qué toca en la siguiente sesión.**

Si el proyecto no tiene convención, pregunta antes de crear un archivo nuevo.

## 4. Commit y push (tras su OK)

- **Solo los archivos de esta sesión y de este repo:** `git add <archivos>`. Nunca `git add -A` ni `git add .` a ciegas, y nunca archivos de otro repo.
- **Sigue el flujo del proyecto** (rama, PR, formato del mensaje). Si no está claro, pregunta.
- **Nada de `--force`.**
