# Ejercicio final S5 — Multiagente a juicio

> **Pregunta final:** ¿tu arquitectura con subagentes generó suficiente calidad o velocidad adicional para justificar los tokens que consumió?

Este ejercicio compara el mismo trabajo dos veces: primero con un solo agente y después con un líder que **debe** coordinar seis subagentes. No copies prompts ni intentes adivinar cómo invocarlos: el repositorio del ejercicio abre cada corrida con el modo correcto.

## Tu único punto de partida

Clona el repositorio ejecutable y corre el preflight:

```bash
git clone https://github.com/nabolom/nest-a-juicio.git
cd nest-a-juicio
bash scripts/empezar.sh
```

Si `empezar.sh` no termina en `OK`, no sigas. Te dirá si falta Claude Code, autenticación o un archivo del ejercicio. Si te pide autenticarte, ejecuta:

```bash
claude auth login
```

> El corpus del ejercicio es fijo y vive dentro del repo. No uses web, MCP ni archivos externos: si cambias las fuentes, ya no estás comparando una sola variable.

---

## Las cuatro corridas — copia solo estos comandos

Ejecuta un comando, espera a que Claude Code abra y termine la tarea, registra la evidencia y después ejecuta el siguiente. **No necesitas abrir archivos ni pegar el prompt de una tarea.**

| # | En la terminal corre | Debes ver |
|---|---|---|
| 1 | `bash scripts/correr-baseline-a.sh` | Tarea A resuelta por un agente. No hay delegaciones. |
| 2 | `bash scripts/correr-nest-a.sh` | Seis delegaciones y `NEST COMPLETADO: 6/6 fuentes recibidas.` |
| 3 | `bash scripts/correr-baseline-b.sh` | Tarea B resuelta por un agente. No hay delegaciones. |
| 4 | `bash scripts/correr-nest-b.sh` | Seis delegaciones y `NEST COMPLETADO: 6/6 fuentes recibidas.` |

El baseline se abre con `--disallowedTools Agent`, que bloquea técnicamente cualquier delegación. El nest se abre con el agente `nest-coordinador`, que está obligado a llamar a `fuente-1` hasta `fuente-6`, esperar los seis resultados y solo entonces sintetizar. [1] [2]

> **Corrida nest inválida:** si no ves seis delegaciones en el transcript y la línea final exacta `NEST COMPLETADO: 6/6 fuentes recibidas.`, escribe `/exit` y vuelve a correr el script nest correspondiente. No intentes arreglarla pegando instrucciones adicionales.

---

## Después de cada corrida

Cuando Claude termine, pero antes de salir:

1. Dentro de Claude Code escribe `/usage`.
2. Toma una captura de pantalla de esa salida.
3. Inicia/detén un cronómetro alrededor de la ejecución de la tarea.
4. Escribe `/exit` para regresar a la terminal.
5. Registra los seis datos que obtuviste; reemplaza cada valor entre `< >`.

```bash
bash scripts/registrar-corrida.sh baseline-a <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
bash scripts/registrar-corrida.sh nest-a     <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
bash scripts/registrar-corrida.sh baseline-b <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
bash scripts/registrar-corrida.sh nest-b     <tokens> <costo_usd> <pct_subagentes> <segundos> <calidad_0a5>
```

`/usage` muestra tokens, costo estimado y atribución de uso a subagentes. Ese porcentaje representa trabajo realizado por subagentes; **no equivale exclusivamente a coordinación**. Úsalo como medida relativa dentro de la misma configuración, no como una factura. [3]

La calidad se califica con la rúbrica de cinco checks que ya está en el repo:

- Tarea A: [`rubricas/rubrica-a.md`](https://github.com/nabolom/nest-a-juicio/blob/main/rubricas/rubrica-a.md)
- Tarea B: [`rubricas/rubrica-b.md`](https://github.com/nabolom/nest-a-juicio/blob/main/rubricas/rubrica-b.md)

---

## Tu entregable

Crea una tarjeta por tarea:

```bash
cp tarjetas/PLANTILLA-veredicto.md resultados/veredicto-a.md
cp tarjetas/PLANTILLA-veredicto.md resultados/veredicto-b.md
```

En cada tarjeta compara baseline y nest con cuatro números: N× tokens, N× costo estimado, Δ duración y Δ calidad. Tu veredicto solo puede ser uno de estos:

| Veredicto | Cuándo aplica |
|---|---|
| **Justifica** | La mejora de calidad o duración compensa el costo adicional. |
| **No justifica** | El nest consume más y no agrega beneficio suficiente. |
| **Evidencia insuficiente** | Alguna condición cambió, faltan datos o la rúbrica no permite decidir. |

Un nest que pierde es un buen resultado: demuestra que mediste en vez de opinar.

## Si algo falla

| Problema | Qué haces |
|---|---|
| `claude: command not found` | Instala Claude Code y abre una terminal nueva. |
| No has iniciado sesión | Corre `claude auth login` y vuelve a `bash scripts/empezar.sh`. |
| El nest no mostró seis delegaciones | Sal con `/exit` y vuelve a ejecutar el script nest; no copies un prompt manual. |
| No sabes leer `/usage` | Toma captura, registra lo que veas y llévala al debrief. |

Para el detalle técnico —corpus, agentes, tareas, rúbricas y scripts— consulta el [README de `nest-a-juicio`](https://github.com/nabolom/nest-a-juicio).

## Referencias

[1]: https://code.claude.com/docs/en/cli-reference "Claude Code Docs — CLI reference"
[2]: https://code.claude.com/docs/en/sub-agents "Claude Code Docs — Create custom subagents"
[3]: https://code.claude.com/docs/en/costs "Claude Code Docs — Manage costs effectively"
