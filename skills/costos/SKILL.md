---
name: costos
description: Calcular y optimizar lo que cuesta correr una automatización con IA. Úsala cuando se hable de tokens, presupuesto, ROI, elección de modelo por costo, o cuando alguien pregunte "¿cuánto me va a costar esto?". También al preparar un reporte de impacto.
---

# Costos

**Lee `referencias/claude-api.md` antes de citar cualquier precio. Nunca de memoria.**

## Los cuatro descuentos, en orden de impacto

1. **Prompt caching.** Un cache hit cuesta 10% del input. Y —lo que casi nadie sabe— **los tokens leídos de cache no cuentan para tu rate limit**. No es solo un descuento: es un multiplicador de throughput. Cachea lo que se repite: system prompt, definiciones de herramientas, documentos de contexto.

2. **Batch API — 50% de descuento.** Asíncrono, ≤24h, sin diferencia de calidad. Si el proceso no es en tiempo real (enriquecer datos, procesar documentos, análisis nocturno), no usarlo es tirar la mitad del dinero.

3. **Rutear por modelo.** Haiku para clasificar/extraer/rutear/resumir. Sonnet para el grueso. Opus solo donde el razonamiento lo justifique. Es una diferencia de 5x. En n8n el **Model Selector** lo hace dentro del mismo flujo.

4. **Recortar contexto y output.** El output cuesta 5x el input. Baja `max_tokens` cuando la salida debe ser corta.

Se acumulan: batch + caching juntos.

## Costos que la gente olvida y luego duelen

- **Las definiciones de herramientas se cobran en cada llamada.** Solo por incluir `tools` se agregan cientos de tokens de sistema, más los esquemas de tus herramientas. En cada request.
- **Extended thinking** se cobra como output.
- **Precio introductorio con fecha de vencimiento.** Sonnet 5 está a $2/$10 solo hasta el 31 de agosto de 2026; después sube a $3/$15. Un modelo de costos anual hecho con el precio de hoy se rompe en septiembre.
- **Tokenizador nuevo.** Opus 4.7+, Sonnet 5 y Fable 5 producen ~30% más tokens para el mismo texto. Un cálculo hecho a ojo con la intuición de un modelo anterior se queda corto por un tercio.
- **El bucle infinito.** Un agente que reintenta sin tope con una API de pago es una factura sin tope. **Tope de gasto en la consola. El día uno.**

## Cómo estimar de verdad

1. Tokens por ejecución (input + output). No lo adivines: **corre 5 ejecuciones reales y mide.** El campo `usage` de la respuesta te lo da.
2. Ejecuciones por mes.
3. Multiplica por el precio del modelo.
4. **Súmale**: tokens de definiciones de herramientas, reintentos (asume 10-15%), llamadas fallidas.
5. Súmale lo que no son tokens: licencia de n8n, hosting, vector DB, web search ($10 / 1,000 búsquedas).

Ese último punto es donde casi todos los ROI se caen.

## ROI honesto

**ROI = (horas ahorradas × costo hora) − costo de operación del sistema.** Anualizado. Con los supuestos escritos, no escondidos.

Un ROI que ignora el costo de operar el sistema no es un ROI. Es marketing.

Y si sale negativo, **eso también es un hallazgo válido**. Es más valioso que un número inflado. Ayúdale a presentarlo, no a maquillarlo.
