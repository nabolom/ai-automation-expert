---
name: claude-api
description: Construir directamente contra la API de Claude. Úsala cuando se escriba código que llama a Claude, se elija modelo, se implemente tool use, prompt caching, batch processing o salida estructurada, o cuando haya que decidir entre la API y un nodo visual.
---

# Claude API

**Lee `referencias/claude-api.md` antes de citar precios, modelos o límites. Nunca de memoria.**

## Elegir modelo

| Tarea | Modelo |
|---|---|
| Clasificar, extraer, rutear, resumir | **Haiku 4.5** |
| El grueso de la producción | **Sonnet 5** |
| Razonamiento complejo, coding agéntico | **Opus 4.8** |
| Máxima capacidad disponible | **Fable 5** |

Empieza por el más barato que resuelva la tarea. Sube solo si los evals te obligan. Casi nadie necesita Opus para clasificar correos, y casi todo el mundo lo intenta.

## Salida estructurada

Dos caminos:

**Tool use con esquema** (más confiable): defines una herramienta cuyo input schema *es* la estructura que quieres, y fuerzas `tool_choice` a esa herramienta. El modelo tiene que llenar el esquema.

**Pedir JSON y validar**: menos ceremonia, pero **valida siempre**. Si no valida, reintenta una vez con el error de validación en el prompt.

Nunca dejes pasar una salida sin validar.

## Prompt caching — el que más rinde

Cachea lo que se repite entre llamadas: system prompt, definiciones de herramientas, documentos de contexto largos.

- Cache hit = 10% del precio del input.
- **Los tokens de cache no cuentan para el rate limit ITPM.** Es un multiplicador de throughput, no solo un descuento.
- Se paga solo con una lectura (cache de 5 min).

Si tu sistema manda el mismo system prompt de 3,000 tokens en cada llamada y no lo estás cacheando, estás pagando de más y limitándote sin razón.

## Batch API

50% de descuento, resultados en ≤24h, sin diferencia de calidad. **Solo cambia el tiempo.**

Si el proceso no es en tiempo real —enriquecer registros, procesar documentos, análisis nocturno, generar contenido en cola— y no está en batch, está tirando la mitad del dinero.

## Tool use

Recuerda: **las definiciones de herramientas se cobran en cada llamada.** Los nombres, descripciones y esquemas van en el input todas las veces. 20 herramientas con esquemas largos es un impuesto permanente.

Herramientas del servidor con costo propio: web search ($10 / 1,000 búsquedas), code execution (1,550 h gratis/mes, luego $0.05/h). Web fetch no cuesta extra más allá de los tokens — pero usa `max_content_tokens` o un PDF grande te mete 125,000 tokens de golpe.

## Errores

- **429** → respeta el header `retry-after`. No inventes el backoff.
- **4xx** → no reintentes. No se arregla insistiendo.
- **5xx** → reintento con backoff, tope 3.
- **Pon un tope de gasto en la consola.** Un agente en loop con una API de pago es una factura sin techo.

## ¿API o nodo visual?

Si cabe en n8n, hazlo en n8n. La API directa es para cuando necesitas control que el nodo no te da: caching fino, batch, streaming, tool use complejo, o lógica que no se dibuja.
