# Ejercicio final S5 — Multiagente a juicio

> **No preguntamos “¿puedo usar subagentes?”. Preguntamos “¿qué tendría que ganar este diseño para merecer su costo y su complejidad?”.**

Este ejercicio cierra el curso aplicando la misma disciplina de las sesiones anteriores a una decisión de arquitectura. Primero entiendes el caso. Después entiendes las dos arquitecturas. Solo entonces haces una predicción, ejecutas una comparación justa y emites un juicio.

El repositorio ejecutable es [`nabolom/nest-a-juicio`](https://github.com/nabolom/nest-a-juicio).

---

## 1. Antes de predecir: conoce la situación

No abras Claude Code todavía. El ejercicio tiene dos casos sintéticos:

| Caso | La situación | La diferencia estructural |
|---|---|---|
| **A — Cobranza Pro** | Horizonte debe decidir si lanzar un módulo usando seis notas de áreas distintas. | Las seis fuentes son independientes: producto, ventas, soporte, finanzas, legal y operaciones pueden leerse por separado. |
| **B — Factura con OC faltante** | Debes decidir qué hacer con una factura de $54,500 sin una OC verificable. | Las seis fuentes son una cadena de reglas: cada una modifica cómo entiendes la siguiente. |

Esto no prueba que A “deba” usar agentes ni que B “deba” evitarlos. Solo te da una razón estructural para evaluar ambas posibilidades.

---

## 2. Entiende qué comparas

| Arquitectura | Qué hace | La pregunta que responde |
|---|---|---|
| **Baseline: un solo agente** | Un agente lee las seis fuentes, razona y responde en el mismo contexto. | ¿Qué calidad, tiempo y consumo necesitas si no agregas coordinación? |
| **Nest: líder + seis lectores** | `nest-coordinador` manda una fuente a cada `fuente-1`…`fuente-6`, recibe sus seis hallazgos y sintetiza. | ¿La distribución compra suficiente cobertura o velocidad para justificar su costo? |

El corpus, las tareas, la rúbrica, el formato de salida y el modelo del líder se mantienen fijos. Solo cambia la posibilidad de delegar. Sin ese control, no podrías atribuir el resultado a la arquitectura.

---

## 3. Ahora sí: declara tu hipótesis

Después de conocer casos y arquitecturas, escribe dos frases:

1. **A:** “Dado que las fuentes son independientes, creo que el nest [sí/no] justificará su costo porque…”
2. **B:** “Dado que las reglas forman una cadena, creo que el nest [sí/no] justificará su costo porque…”

No hay una respuesta esperada. La predicción existe para que al final puedas comparar tu intuición con los datos.

---

## 4. Ejecuta el experimento

Clona el repo y corre el preflight:

```bash
git clone https://github.com/nabolom/nest-a-juicio.git
cd nest-a-juicio
bash scripts/empezar.sh
```

Si `empezar.sh` falla, resuelve el setup antes de continuar. Un problema de instalación no es evidencia sobre multiagente.

Después, ejecuta cada par completo:

| Orden | En la terminal ejecutas | Por qué | Corrida válida cuando… |
|---|---|---|---|
| A1 | `bash scripts/correr-baseline-a.sh` | Establece cuánto logra un agente único para seis fuentes independientes. | No aparecen delegaciones. |
| A2 | `bash scripts/correr-nest-a.sh` | Prueba si los seis lectores mejoran cobertura o duración frente al baseline. | Hay seis delegaciones y `NEST COMPLETADO: 6/6 fuentes recibidas.` |
| B1 | `bash scripts/correr-baseline-b.sh` | Establece qué logra un agente único con toda la cadena en su contexto. | No aparecen delegaciones. |
| B2 | `bash scripts/correr-nest-b.sh` | Prueba si distribuir la cadena aporta valor o añade fricción. | Hay seis delegaciones y `NEST COMPLETADO: 6/6 fuentes recibidas.` |

El baseline bloquea técnicamente la herramienta `Agent`. El nest obliga al coordinador a usar los seis lectores. No copies prompts ni agregues instrucciones: los scripts ya cargan la condición experimental correcta. [1] [2]

---

## 5. Convierte cada corrida en evidencia

Después de cada respuesta, antes de escribir `/exit`:

1. Escribe `/usage` y toma una captura.
2. Detén el cronómetro.
3. Aplica la rúbrica de 0–5 del caso correspondiente.
4. Registra los resultados con `scripts/registrar-corrida.sh`.

La evidencia responde cuatro preguntas:

| Dato | Pregunta |
|---|---|
| Tokens y costo estimado | ¿Cuánto consumo adicional compró el nest? |
| Duración | ¿El diseño distribuido ganó o perdió tiempo? |
| Rúbrica | ¿La respuesta mejoró de forma verificable? |
| % atribuido a subagentes | ¿Qué parte del uso ocurrió fuera del hilo líder? |

El porcentaje de `/usage` es trabajo atribuido a subagentes, no únicamente coordinación. Es una medida relativa dentro de la misma configuración, no una factura. [3]

---

## 6. El entregable: dos juicios

Completa una tarjeta para A y otra para B. En cada una compara N× tokens, N× costo, Δ duración y Δ calidad.

| Veredicto | Qué significa |
|---|---|
| **Justifica** | La calidad o velocidad ganada compensa el costo adicional. |
| **No justifica** | El nest gasta más y no compra beneficio suficiente. |
| **Evidencia insuficiente** | Cambió una condición o faltan datos para decidir con honestidad. |

Un nest que pierde no es un fallo: es la prueba de que evitaste pagar complejidad innecesaria en producción.

Para el paso a paso completo, los archivos de los agentes, las tareas, rúbricas, scripts y recuperación de errores, sigue el [README de `nest-a-juicio`](https://github.com/nabolom/nest-a-juicio).

## Referencias

[1]: https://code.claude.com/docs/en/cli-reference "Claude Code Docs — CLI reference"
[2]: https://code.claude.com/docs/en/sub-agents "Claude Code Docs — Create custom subagents"
[3]: https://code.claude.com/docs/en/costs "Claude Code Docs — Manage costs effectively"
