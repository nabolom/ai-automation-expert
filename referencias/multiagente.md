# Referencias · Sistemas multiagente
> Hechos verificados para citar **de aquí, no de memoria**. Fechados contra fuente. Verificado 11 ago 2026.

## El debate fundacional (dos voces, misma semana, jun 2025)
- **Anthropic — "How we built our multi-agent research system"** · anthropic.com/engineering/multi-agent-research-system · jun 2025.
  - Un sistema multiagente (lead Claude Opus 4 + subagentes Claude Sonnet 4) superó al agente único Opus 4 por **90.2%** en su eval interna de investigación.
  - Funciona **principalmente porque gasta suficientes tokens** para resolver el problema. En su análisis, tres factores explicaron el **95%** de la varianza de desempeño en BrowseComp; el uso de tokens es el dominante.
  - Costo: alrededor de **15×** los tokens de una interacción de chat normal.
  - Brilla en tareas **breadth-first**: direcciones múltiples e independientes en paralelo. Mal ajuste cuando los agentes deben compartir contexto o hay muchas dependencias entre ellos.
- **Cognition — "Don't Build Multi-Agents"** (Walden Yan) · jun 2025.
  - Subagentes en paralelo toman decisiones independientes → salidas en conflicto (ejemplo: clon de Flappy Bird con fondo y personaje de estilos distintos).
  - Regla: **comparte trazas completas, no mensajes sueltos.** Acuñó el uso de "context engineering" para este problema.
- Reconciliación: no es ideología, es **fit a la forma de la tarea**. Anthropic = investigación breadth-first (subtareas independientes). Cognition = escribir código (cada decisión condiciona la siguiente).

## La economía (evidencia 2026)
- A **presupuesto de tokens igual**, un solo agente iguala o supera al multiagente en razonamiento (estudios 2026: Stanford; Tran & Kiela; OneFlow). Buena parte de lo que parece "coordinación ganando" es "más tokens ganando".
- Multiplicadores de costo reportados: **2–5×** típico; hasta **15×** (Anthropic); hasta **4–220×** en tokens (UIUC).
- Caso ilustrativo (servicio a cliente): **$47,000/mes** multiagente al 94.3% vs **$22,700/mes** agente único al 92.2% — el doble de costo por ~2 puntos.
- Microsoft (Azure SRE) construyó hacia multiagente y **revirtió** al ver que los handoffs bajaban la confiabilidad.

## La aritmética del error compuesto, a nivel de agente
- 1 agente → 95% · 3 → 86% · 5 → **77%** · 8 → 66% (asumiendo 95% por agente). Cada subagente es otro `× 0.95`.

## Modos de falla nuevos (taxonomía Microsoft, jun 2026)
- **Escalamiento de confianza entre agentes:** en cadenas de delegación, un agente comprometido afirma identidad falsa o infla permisos ante un orquestador que no los verifica (el "confused deputy", inducido por lenguaje natural).
- **Ataque visual a agentes de computer-use (CUA):** instrucciones adversarias escondidas en imágenes o en UI fuera del viewport visible.

## Observabilidad (el juez)
- **LangSmith sobre LangGraph:** la traza **es** el grafo de ejecución —nodo por nodo, diffs de estado, desglose de model + tool calls, replay contra otros modelos. Vista unificada de costo a lo largo del workflow (modelo, recuperación, herramientas, APIs externas), latencia P50/P99. Es la forma de emitir un veredicto costo-beneficio con datos, no con vibra.

## Cuándo SÍ / cuándo NO
- **SÍ:** subtareas independientes · paralelizables · multi-dominio · no caben en un contexto.
- **NO:** secuencial · dependiente · comparte contexto · cabe en un solo agente bien contextualizado.
