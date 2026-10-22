---
name: council
description: Convoca al consejo de asesores sobre una pregunta. /council <pregunta> → 3 asesores elegidos según el tipo de pregunta (código, producto o estrategia). /council full <pregunta> → los 5. Cada asesor opina desde su lente, en paralelo, sobre el proyecto activo, y se cierra con una síntesis directa. Usar solo cuando el usuario lo pida o dentro del cierre de sesión (skill cerrar-sesion); no convocarlo por iniciativa propia.
argument-hint: "[full] <pregunta>"
---

Convoca al consejo de asesores para evaluar la siguiente cuestión.

**Argumento recibido:** `$ARGUMENTS`

## Cómo procesar

1. **Lee el argumento:**
   - Si empieza por `full ` (la palabra "full" y un espacio), el modo es **completo** (5 asesores) y la pregunta es lo que viene después.
   - Si no, el modo es **rápido** (3 asesores) y la pregunta es todo el argumento.
   - Si el argumento está vacío o es solo "full", pide al usuario que reformule la pregunta y no lances a nadie. (Si te ha invocado la skill `cerrar-sesion`, la pregunta es la que ella te pasa.)

2. **Prepara el brief para los asesores.** Tiene que bastarse solo: los asesores no ven la conversación. Incluye:
   - La pregunta exacta del usuario.
   - **El proyecto activo y su ruta absoluta**, para que cada asesor lea su `CLAUDE.md`. Si la pregunta no es de un proyecto concreto, dilo.
   - El estado del trabajo si la pregunta apunta a algo en curso: archivos en juego, cambios propuestos, decisión pendiente. 2-4 frases, densas.
   - **No incluyas tu opinión.** Es lo que evita que los asesores se dejen arrastrar.

3. **Escribe tu postura inicial ANTES de lanzar a los asesores.** 1-2 frases: qué harías tú y por qué. Así la síntesis no se limita a repetir lo que digan ellos. La mostrarás en la síntesis.

4. **Elige a los asesores.**
   - Modo **completo**: los 5 (`kit:council-simplifier`, `kit:council-skeptic`, `kit:council-user-advocate`, `kit:council-pragmatist`, `kit:council-archivist`).
   - Modo **rápido**: los 3 que más aportan según el tipo de pregunta:

     | Tipo de pregunta | Asesores |
     |---|---|
     | Código, datos, infraestructura | simplifier, skeptic, pragmatist |
     | Producto, UX, copy, contenido | user-advocate, simplifier, pragmatist |
     | Estrategia, negocio, prioridades, herramientas | pragmatist, skeptic, archivist |
     | Cierre de sesión | no aplica: el cierre (skill `cerrar-sesion`) convoca siempre a los 5 |

     Si dudas entre dos tipos, elige el que más riesgo tenga y di en una línea qué tipo has elegido.

5. **Lanza a los asesores en PARALELO** (todas las llamadas en un único mensaje), con `subagent_type` = `kit:council-<nombre>` y el mismo brief.

6. **Espera todas las respuestas.**

7. **Sintetiza con este formato exacto** (pensado para leerse en el móvil):

```
# 🏛️ Consejo — [3 asesores: tipo | 5 asesores]

## 🎯 Recomendación

[1-3 frases con la decisión propuesta. Directa. Si el consejo está dividido, dilo y explica por qué.]

**Mi postura antes de leerles:** [la del paso 3, en 1 frase] → [se mantiene / cambió por lo que dijo X: 1 frase].

**¿Es la pregunta correcta?:** [solo si algún asesor, normalmente el simplifier, la cuestionó; si no, omite la línea].

## ⚠️ Riesgos si se sigue tal cual

- [El riesgo concreto que más grita, en 1 frase].
- (Omite la sección si no hay riesgos reales).

## 🤝 Acuerdos y 🥊 desacuerdos

- **Coinciden:** [1 frase].
- **Discrepan:** [la tensión principal: quién empuja hacia dónde. Incluye siempre la discrepancia más fuerte, aunque la descartes. Si no hay, omite].

## Veredictos

| Asesor | Veredicto | Punto clave |
|---|---|---|
| [emoji + nombre] | [veredicto] | [1 frase] |

---
💬 *Para el informe completo de un asesor: "dame el informe del [nombre]".*
```

8. **Guarda los informes completos en el contexto de la conversación.** Si el usuario pide "dame el informe del skeptic", lo devuelves tal cual, sin reescribirlo.

## Reglas del orquestador (tú)

- **No edites lo que dijo cada asesor.** Si el skeptic dijo "Rechazar", no lo conviertes en "preocupación".
- **La recomendación final es TUYA.** Tienes que mojarte; "depende de lo que prefieras" no vale. El usuario decide después, con tu opinión y la del consejo delante.
- **Si dos asesores se contradicen de frente**, decide qué pesa más en este caso y justifícalo. Nada de empates artificiales.
- **Si dos asesores coinciden contra tu postura inicial**, tómatelo como una señal real y explica qué haces con ella.
- **Si el consejo aprueba casi entero**, no busques pegas que no hay: "adelante", con el matiz que haga falta.
- **Empieza directamente con `# 🏛️ Consejo`**, sin preámbulos.
- **Idioma:** español.

## Cuándo pasar a modo completo automáticamente

Si la pregunta toca terreno con historia o decisiones de fondo ("precio", "pricing", "rebrand", "público", "target", "stack", "arquitectura", "modelo de negocio", "descartar", "volver a", "reabrir"), pasa a modo completo aunque no se haya escrito "full", y avisa en una línea al principio: *"La pregunta toca decisiones de fondo, convoco a los 5."*

En cualquier otro caso, respeta lo que pidió el usuario.
