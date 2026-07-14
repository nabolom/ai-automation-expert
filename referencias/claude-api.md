---
type: referencia-api
title: Claude API
description: Precios, rate limits, caching, batch y costos ocultos de la API de Anthropic. Verificado contra fuente oficial.
resource: https://platform.claude.com/docs/en/about-claude/pricing
tags: [claude, api, costos, rate-limits, anthropic]
timestamp: 2026-07-13T00:00:00Z
---

# Claude API — hechos verificados

> **Verificado: 13 julio 2026** contra https://platform.claude.com/docs/en/about-claude/pricing y /api/rate-limits
> Re-verificar cada ~2 meses con `/verificar`. Nunca cites estos números de memoria.

## Precios (USD por millón de tokens)

| Modelo | Input | Output | Cache write 5m | Cache write 1h | Cache hit |
|---|---|---|---|---|---|
| Claude Fable 5 | $10 | $50 | $12.50 | $20 | $1 |
| Claude Opus 4.8 | $5 | $25 | $6.25 | $10 | $0.50 |
| Claude Sonnet 5 | $2 | $10 | $2.50 | $4 | $0.20 |
| Claude Sonnet 4.6 | $3 | $15 | $3.75 | $6 | $0.30 |
| Claude Haiku 4.5 | $1 | $5 | $1.25 | $2 | $0.10 |

**Ojo con Sonnet 5:** ese $2/$10 es precio introductorio **hasta el 31 de agosto de 2026**. A partir del 1 de septiembre sube a $3/$15. Si alguien está modelando costos anuales, este es el número que van a fallar.

**Tokenizador nuevo.** Opus 4.7+, Sonnet 5, Fable 5 y Mythos 5 usan un tokenizador que produce **~30% más tokens para el mismo texto**. Un cálculo de costo hecho con la intuición de Sonnet 4.6 se queda corto por un tercio.

**Contexto de 1M sin sobreprecio** en Fable 5, Opus 4.8/4.7/4.6, Sonnet 5 y Sonnet 4.6. Una petición de 900k tokens se cobra a la misma tarifa por token que una de 9k.

## Los cuatro descuentos, en orden de impacto

1. **Prompt caching.** Un cache hit cuesta el 10% del input normal. Escribir al cache cuesta 1.25x (5 min) o 2x (1 hora). Se paga solo con **una** lectura en el cache de 5 min, o dos en el de 1 hora.
2. **Batch API — 50% de descuento** en input y output. Asíncrono, resultados en ≤24h. Sin diferencia de calidad; solo de tiempo. Si el proceso no es en tiempo real (enriquecer datos, procesar documentos, análisis nocturno), esto es dinero regalado.
3. **Rutear por modelo.** Haiku 4.5 para clasificar, extraer, resumir, rutear. Sonnet para el grueso. Opus solo donde el razonamiento lo justifica. Es una diferencia de 5x.
4. **Recortar contexto.** El output cuesta 5x el input. Baja `max_tokens` cuando la salida debe ser corta.

Los descuentos **se acumulan**: batch + caching juntos.

## Rate limits (tier Start)

| Modelo | RPM | Input tok/min | Output tok/min |
|---|---|---|---|
| Fable 5 | 1,000 | 500,000 | 100,000 |
| Opus 4.x (bucket compartido) | 1,000 | 2,000,000 | 400,000 |
| Sonnet 5 (bucket propio) | 1,000 | 2,000,000 | 400,000 |
| Haiku 4.5 | 1,000 | 2,000,000 | 400,000 |

Los tiers son Start / Build / Scale / Custom. Topes de gasto mensual: Start $500, Build $1,000, Scale $200,000.

### El dato que casi nadie conoce

**Los tokens leídos de cache NO cuentan para el límite ITPM.** Solo cuentan `input_tokens` + `cache_creation_input_tokens`.

Con 2M ITPM y 80% de cache hit, procesas efectivamente 10M tokens de input por minuto. El caching no es solo un descuento: **es un multiplicador de throughput.** (Excepción: Haiku 3.5, ahí sí cuentan.)

`max_tokens` no afecta el límite OTPM — solo cuentan los tokens realmente generados. No hay penalización por poner un `max_tokens` alto.

## Errores y reintentos

429 = límite excedido. La respuesta trae header `retry-after` con los segundos a esperar. **Úsalo**, no inventes un backoff.

También puedes comer 429 por "límites de aceleración" si tu tráfico se dispara de golpe. Sube el volumen gradualmente.

Headers útiles en cada respuesta: `anthropic-ratelimit-requests-remaining`, `anthropic-ratelimit-input-tokens-remaining`, `anthropic-ratelimit-tokens-reset`.

## Herramientas del servidor

- **Web search:** $10 por cada 1,000 búsquedas + tokens del contenido.
- **Web fetch:** sin costo adicional; solo pagas los tokens del contenido. Usa `max_content_tokens` para no tragarte un PDF de 500 kB (~125,000 tokens).
- **Code execution:** 1,550 horas gratis al mes por organización, luego $0.05/hora por contenedor. Gratis si va acompañado de web search o web fetch.

## Costos ocultos que rompen el presupuesto

- **Definiciones de herramientas.** Solo por incluir `tools` se agregan tokens de sistema (~290–500 según modelo, más los esquemas de tus herramientas). En cada llamada.
- **Extended thinking.** Los tokens de razonamiento se cobran como output.
- **El bucle infinito.** Un agente que reintenta sin tope con una API de pago es una factura sin tope. **Pon un límite de gasto en la consola.** No es opcional.

Fuente: https://platform.claude.com/docs/en/about-claude/pricing
