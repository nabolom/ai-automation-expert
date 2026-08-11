# Ejercicio final S5 — Multiagente a juicio

> **Pregunta final:** ¿tu arquitectura con subagentes generó suficiente calidad o velocidad adicional para justificar su uso de tokens?

Este ejercicio no celebra una arquitectura por verse sofisticada. Corre la **misma tarea** dos veces —primero con un solo agente y luego con subagentes— y compara evidencia. Al final tendrás dos tarjetas de veredicto: una para una tarea paralela y otra para una tarea secuencial.

## Qué vas a producir

| Entregable | Qué demuestra |
|---|---|
| Tarjeta A | Si el nest justificó —o no— su costo para una tarea con fuentes independientes |
| Tarjeta B | Si el nest justificó —o no— su costo para una tarea con dependencias secuenciales |
| Brecha de predicción | Qué creías que iba a pasar y qué mostraron los datos |

> **Regla de oro:** misma tarea, mismo modelo del líder, mismo nivel de esfuerzo y mismas herramientas. La única variable que cambia entre las dos corridas es **delegación a subagentes: sí o no**.

---

## Antes de empezar — requisitos del prework

El ejercicio requiere el repositorio complementario público [`nabolom/nest-a-juicio`](https://github.com/nabolom/nest-a-juicio). Incluye las Tareas A y B, seis fuentes fijas por tarea, las definiciones `.claude/agents/fuente-1.md` a `fuente-6.md`, las rúbricas, las tarjetas y las plantillas de captura en `trazas/`.

> No sustituyas las fuentes del ejercicio por búsquedas web en vivo. Si las fuentes cambian entre baseline y nest, cambias más de una variable y la comparación deja de ser justa.

### 1. Confirma tu instalación

En la terminal, corre:

```bash
claude --version
```

Usa una versión reciente de Claude Code. Para este ejercicio, necesitas que `/usage` muestre la atribución de uso a subagentes y que `/clear` reinicie el costo de la sesión. Si no ves ambas cosas en la prueba de humo, avisa **antes** de la sesión.

### 2. Clona el proyecto de ejercicio

Ejecuta:

```bash
git clone https://github.com/nabolom/nest-a-juicio.git
cd nest-a-juicio
```

### 3. Prueba de humo

Abre Claude Code, escribe `hola`, y corre:

```text
/usage
```

Debes ver estadísticas de la sesión, incluyendo tokens y costo estimado. Sal con:

```text
/exit
```

> `/usage` calcula el costo localmente a partir de tokens y precios de lista. Para este ejercicio úsalo como **proxy relativo**, no como factura: compara tokens y costo estimado dentro de la misma cuenta y configuración. [1]

---

## Lo que mides — y lo que no

`/usage` muestra consumo de la sesión y atribuye parte del uso reciente a subagentes, skills, plugins y MCPs. [1] No es un grafo de ejecución: no muestra una traza nodo por nodo, tiempos de espera entre agentes ni el contenido de cada handoff.

| Métrica | Cómo la obtienes | Para qué sirve |
|---|---|---|
| Tokens totales | `/usage` | Métrica primaria de costo relativo |
| Costo estimado | `/usage` | Traducción pedagógica del consumo |
| % atribuido a subagentes | `/usage` | Qué proporción del uso se ejecutó fuera del hilo líder |
| Duración | Cronómetro manual | Cuánto tardó la tarea de punta a punta |
| Calidad | Rúbrica común de la tarea | Si el resultado mejoró de verdad |

> El porcentaje atribuido a subagentes **no es “solo coordinación”**. Incluye el trabajo real que esos subagentes realizaron. El sobrecosto se observa comparando baseline y nest.

---

## Parte 1 — Baseline: un solo agente

El baseline debe impedir técnicamente la delegación. Pedir “no uses subagentes” en el prompt no basta: Claude Code puede delegar de forma automática. La herramienta `Agent` debe estar negada en esta corrida. [2]

### Tarea A — fuentes independientes

1. Desde la terminal, inicia el baseline con la delegación bloqueada:

   ```bash
   claude --disallowedTools Agent
   ```

2. Dentro de Claude Code, corre `/clear`.
3. Abre `tareas/tarea-a-paralela.md`, copia la tarea completa y pégala en Claude Code.
4. Inicia un cronómetro al enviar el prompt. Cuando Claude termine, detén el cronómetro.
5. Corre `/usage` y guarda en tu tarjeta: tokens, costo estimado y porcentaje atribuido a subagentes.
6. Aplica la rúbrica A **sin cambiar sus criterios** y registra el puntaje.
7. Sal con `/exit`.

### Tarea B — dependencias secuenciales

Repite exactamente el mismo procedimiento con `tareas/tarea-b-secuencial.md` y la rúbrica B.

> El resultado esperado del baseline es aproximadamente **0% de subagentes**. Si no es así, no compares todavía: revisa que hayas iniciado con `claude --disallowedTools Agent`.

---

## Parte 2 — Nest: líder + subagentes

Ahora corre las mismas tareas sin bloquear la delegación.

### Tarea A — fuentes independientes

1. Abre una sesión nueva:

   ```bash
   claude
   ```

2. Dentro de Claude Code, corre `/clear`.
3. Copia la Tarea A y agrega al final:

   ```text
   Usa los subagentes fuente-1 a fuente-6 en paralelo: uno por fuente. Después sintetiza los hallazgos usando exactamente el formato de salida pedido por la tarea.
   ```

4. Inicia el cronómetro al enviar el prompt y detenlo cuando termine.
5. Corre `/usage` y guarda tokens, costo estimado y porcentaje atribuido a subagentes.
6. Aplica la **misma** rúbrica A. No premies la salida por venir de un nest.
7. Sal con `/exit`.

### Tarea B — dependencias secuenciales

Repite el proceso con la Tarea B y la rúbrica B. Pide la misma delegación explícita y registra los mismos cinco campos.

> Las definiciones `fuente-1` a `fuente-6` del repositorio complementario deben usar `model: inherit` o el mismo modelo explícito que el líder. Si los subagentes usan otro modelo, ya no estás comparando solo una variable. [2]

---

## Parte 3 — Emite el veredicto

Copia `tarjetas/PLANTILLA-veredicto.md` dos veces: una para A y otra para B. Llénala con los resultados de cada par de corridas.

| Cálculo | Fórmula | Cómo leerlo |
|---|---|---|
| **N× tokens** | `tokens_nest ÷ tokens_baseline` | Cuánto multiplicó el consumo de tokens |
| **N× costo** | `costo_nest ÷ costo_baseline` | Cuánto multiplicó el costo estimado |
| **Δ calidad** | `calidad_nest − calidad_baseline` | Cuánto cambió la calidad con la misma rúbrica |
| **Δ duración** | `duración_nest − duración_baseline` | Si la paralelización ahorró o agregó tiempo |

Tu veredicto solo puede ser uno de estos tres:

| Veredicto | Cuándo aplica |
|---|---|
| **Justifica** | La mejora de calidad o duración compensa el N× adicional para esta tarea |
| **No justifica** | El nest cuesta más y no agrega beneficio suficiente |
| **Evidencia insuficiente** | Algo cambió entre corridas, faltan datos o la rúbrica no permite decidir |

La Tarea A y la Tarea B son **hipótesis**, no resultados predeterminados:

| Tarea | Hipótesis que pones a prueba |
|---|---|
| A — paralela | Un nest podría mejorar cobertura o duración porque las fuentes son independientes |
| B — secuencial | Un nest podría no aportar calidad suficiente para compensar su coordinación y tokens |

Un resultado contrario a la hipótesis no es un error: es evidencia de que mediste en vez de repetir el guion.

---

## Parte 4 — Confronta tu apuesta

Antes de las corridas, escribe una línea: “Creo que A/B [sí/no] justificará el nest porque…”. Al final, compara tu predicción con las dos tarjetas.

> La brecha entre lo que creías y lo que midieron es el hallazgo del día.

---

## Si algo falla en clase

No detengas el juicio. Abre las capturas de referencia del repositorio complementario en `trazas/` y aplica las rúbricas a esas corridas ya capturadas. El trabajo central sigue siendo leer los números y defender un veredicto.

No uses `/usage` como sustituto de una traza detallada: para este ejercicio, la evidencia mínima es la captura de `/usage`, la salida de la tarea, el cronómetro y la rúbrica.

---

## Agent Teams — demo opcional del facilitador

Agent Teams no forma parte de las tarjetas A/B. Es una arquitectura distinta: sus integrantes tienen contextos independientes, comparten una lista de tareas y pueden comunicarse entre sí; los subagentes normales solo reportan al líder. [3]

Si quieres mostrarla, habilítala en una demo separada:

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
claude
```

Pide un equipo para una tarea pequeña y observa el consumo en `/usage`. No intentes concluir de esa pantalla dónde se perdió contexto: úsala para hablar de costo distribuido y de por qué un equipo experimental no debe entrar al experimento principal sin una nueva línea base.

---

## Recordatorio de la aritmética

Cinco agentes al 95% de confiabilidad individual dan aproximadamente 77% de confiabilidad compuesta. Cada subagente agrega costo, superficie de coordinación y un nuevo eslabón en `0.95^n`. La pregunta nunca es “¿puedo usar más agentes?”; es “¿qué gané que justifique agregar este eslabón?”.

## Chuleta de comandos

| Comando | Uso en este ejercicio |
|---|---|
| `claude --disallowedTools Agent` | Abre el baseline e impide delegación a subagentes |
| `claude` | Abre la corrida nest |
| `/clear` | Inicia una sesión nueva y limpia el contador de sesión |
| `/usage` | Consulta tokens, costo estimado y atribución de uso |
| `/exit` | Cierra la sesión antes de la siguiente corrida |

## Referencias

[1]: https://code.claude.com/docs/en/costs "Claude Code Docs — Manage costs effectively"
[2]: https://code.claude.com/docs/en/sub-agents "Claude Code Docs — Create custom subagents"
[3]: https://code.claude.com/docs/en/agent-teams "Claude Code Docs — Orchestrate teams of Claude Code sessions"
