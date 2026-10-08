# Extensión gemini-kit

Herramientas de trabajo que valen para cualquier proyecto. Ninguna trae un proyecto memorizado: leen el archivo de contexto del proyecto activo (`GEMINI.md`; si no hay, `AGENTS.md` o `CLAUDE.md`).

- **Consejo de asesores:** skill `council`, o el comando `/council <pregunta>` (`/council full <pregunta>` para los 5). Úsalo solo cuando el usuario lo pida o dentro del cierre de sesión; nunca por iniciativa propia.
- **Cierre de sesión:** skill `cerrar-sesion`, que se activa cuando el usuario dice "cerramos sesión" o una variante, o con `/cerrar-sesion`. Con "cierra directo" se salta el consejo.
- **Revisores (subagentes):** `code-reviewer` para código y `supabase-reviewer` para migraciones, RLS, permisos, Storage y Edge Functions. Solo revisan y reportan; nunca modifican nada.
- **Asesores del consejo (subagentes):** `council-simplifier`, `council-skeptic`, `council-user-advocate`, `council-pragmatist` y `council-archivist`. Los convoca la skill `council`, no se llaman sueltos.
