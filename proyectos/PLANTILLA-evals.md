---
type: suite-de-evals
proyecto: [nombre]
decision: [la decisión de IA que estás evaluando]
reparto: 5-tipicos-3-limite-2-adversariales
estado: sin-correr
---

# Evals — [tu decisión]

<!--
CÓMO USAR ESTA PLANTILLA
1. Copia este archivo a proyectos/<tu-proyecto>/evals.md
2. Llena las 10 filas con TUS casos reales (de tu operación, no inventados).
3. Cuando esté lleno, corre /eval. NO llenes resultados a mano: eso lo hace /eval.
   Tu trabajo aquí es el input y la salida esperada. Nada más.
-->

Toma la decisión más difícil de tu `decisiones.md`. Estos 10 casos la ponen a prueba.

**Reglas al llenar (no las negocies):**
- **Reparto obligatorio: 5 típicos · 3 de límite · 2 adversariales.** Si los 10 son del camino feliz, tu número va a mentir hacia arriba.
  - *Típico:* lo que pasa casi siempre.
  - *Límite:* raro pero real, sacado de tu operación (el redondeo, el caso fronterizo, el "a veces son las dos").
  - *Adversarial:* diseñado para romperla — input vacío, en otro idioma, un intento de manipular al agente, un dato que no está.
- **La salida esperada la fijas tú, y tiene que verificarse SIN volver a preguntarle al modelo.** Un campo concreto: una categoría, un sí/no, un valor. Si para saber si acertó tendrías que opinar, el caso está mal escrito.
- **Si un caso lo resuelve un `if`, no es un eval de IA.** Anótalo aparte y sácalo de la tabla.

## Casos

| # | Tipo | Input (tu caso real) | Salida esperada |
|---|---|---|---|
| E1 | típico |  |  |
| E2 | típico |  |  |
| E3 | típico |  |  |
| E4 | típico |  |  |
| E5 | típico |  |  |
| E6 | límite |  |  |
| E7 | límite |  |  |
| E8 | límite |  |  |
| E9 | adversarial |  |  |
| E10 | adversarial |  |  |

<!--
Después de correr /eval, este archivo va a tener columnas de resultados (v0, v1…)
y una tasa de acierto. El diff entre versiones es la historia de tu sistema.
Corre /eval cada vez que toques el prompt.
-->
