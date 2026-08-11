# Ejercicio final S5 — Multiagente a juicio

> **La pregunta de cierre del curso:** ¿qué tendría que ganar una arquitectura multiagente para merecer los tokens, la coordinación y la superficie de error que agrega?

En la S1 aprendiste a no automatizar un flujo antes de entenderlo. En la S2 aprendiste a no confiar en una decisión antes de medirla. Este ejercicio aplica exactamente la misma disciplina a los subagentes: **no preguntamos si un nest se ve sofisticado; diseñamos la prueba que tendría que ganar para justificar existir.**

No son cuatro demos. Son cuatro corridas que forman dos comparaciones justas.

| Comparación | La hipótesis | Qué nos puede enseñar |
|---|---|---|
| **A: fuentes independientes** | Repartir seis lecturas podría mejorar cobertura o duración. | Cuándo la paralelización compra valor real. |
| **B: cadena secuencial** | Repartir una cadena de reglas podría agregar coordinación sin mejorar la respuesta. | Cuándo la arquitectura es complejidad innecesaria. |

En cada comparación, mantienes fijos el corpus, la tarea, el formato de salida, la rúbrica y el modelo del líder. Solo cambia una variable: **delegación a subagentes: apagada o encendida**.

---

## Qué vas a ejecutar y por qué

El ejercicio ya tiene agentes, tareas y launchers definidos. No copies prompts manualmente: cada script abre Claude Code con la configuración correcta.

```bash
git clone https://github.com/nabolom/nest-a-juicio.git
cd nest-a-juicio
bash scripts/empezar.sh
```

El último comando valida que Claude Code esté instalado, autenticado y que los archivos del experimento existan. Si falla, corrige el setup antes de correr: no quieres confundir un error de instalación con evidencia sobre la arquitectura.

### Las cuatro corridas

| # | Ejecutas en la terminal | Por qué la ejecutas | Qué debe ocurrir |
|---|---|---|---|
| 1 | `bash scripts/correr-baseline-a.sh` | Establece cuánto cuesta y qué calidad obtiene **un solo agente** para seis fuentes independientes. | No hay delegaciones. |
| 2 | `bash scripts/correr-nest-a.sh` | Prueba si seis lectores especializados mejoran A lo suficiente para justificar su costo. | Seis delegaciones + `NEST COMPLETADO: 6/6 fuentes recibidas.` |
| 3 | `bash scripts/correr-baseline-b.sh` | Establece qué tan bien resuelve un solo agente una cadena de reglas conectadas. | No hay delegaciones. |
| 4 | `bash scripts/correr-nest-b.sh` | Prueba si distribuir una cadena de decisión ayuda o agrega coordinación inútil. | Seis delegaciones + `NEST COMPLETADO: 6/6 fuentes recibidas.` |

El baseline abre Claude Code con `--disallowedTools Agent`, por lo que no depende de que el modelo “prometa” no delegar. El nest abre el agente principal `nest-coordinador`, obligado a llamar a `fuente-1` hasta `fuente-6`, esperar los resultados y sintetizar después. [1] [2]

> **Una corrida nest es inválida** si no ves las seis delegaciones y el cierre `NEST COMPLETADO: 6/6 fuentes recibidas.`. Sal con `/exit` y ejecuta otra vez el mismo script; no agregues prompts sueltos.

---

## Qué evidencia guardas después de cada corrida

Cuando Claude termine, antes de escribir `/exit`:

1. Escribe `/usage` y toma una captura.
2. Detén el cronómetro de la tarea.
3. Aplica la rúbrica correspondiente, de 0 a 5.
4. Regresa a la terminal y registra el resultado.

```bash
bash scripts/registrar-corrida.sh baseline-a <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
bash scripts/registrar-corrida.sh nest-a     <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
bash scripts/registrar-corrida.sh baseline-b <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
bash scripts/registrar-corrida.sh nest-b     <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
```

| Evidencia | La pregunta que responde |
|---|---|
| Tokens y costo estimado | ¿Cuánto más trabajo de modelo compró el nest? |
| Duración | ¿La distribución realmente ahorró tiempo? |
| Rúbrica de calidad | ¿La respuesta cumplió más condiciones verificables? |
| % atribuido a subagentes | ¿Qué parte del uso ocurrió fuera del hilo líder? |

El porcentaje atribuido a subagentes no es solo “overhead de coordinación”: incluye su trabajo real. `/usage` es un indicador relativo para comparar una misma configuración, no una factura. [3]

---

## El entregable: dos juicios, no un resumen

Crea una tarjeta por tarea:

```bash
cp tarjetas/PLANTILLA-veredicto.md resultados/veredicto-a.md
cp tarjetas/PLANTILLA-veredicto.md resultados/veredicto-b.md
```

En cada tarjeta compara baseline y nest con N× tokens, N× costo, Δ duración y Δ calidad. Elige un solo veredicto:

| Veredicto | Significa |
|---|---|
| **Justifica** | La mejora de calidad o duración compensa el costo adicional. |
| **No justifica** | El nest cuesta más y no compra suficiente beneficio. |
| **Evidencia insuficiente** | No controlaste una condición o te falta evidencia para decidir. |

Antes de empezar, escribe tu predicción para A y B. Al final, compara esa predicción con lo que midieron. La brecha entre intuición y evidencia es el aprendizaje.

> Un nest que pierde no es un fracaso. Es la demostración de que no convertiste la complejidad en una religión.

## Dónde está el detalle

El repositorio [`nabolom/nest-a-juicio`](https://github.com/nabolom/nest-a-juicio) explica el paso a paso completo, el corpus, los agentes, las tareas, las rúbricas, las tarjetas y cómo recuperarte de un error de setup.

## Referencias

[1]: https://code.claude.com/docs/en/cli-reference "Claude Code Docs — CLI reference"
[2]: https://code.claude.com/docs/en/sub-agents "Claude Code Docs — Create custom subagents"
[3]: https://code.claude.com/docs/en/costs "Claude Code Docs — Manage costs effectively"
