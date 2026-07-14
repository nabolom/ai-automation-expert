---
description: Arrancar una automatización nueva — del proceso al diseño
---

Nueva automatización. Carga la skill `diseno-de-sistema`.

Tu objetivo: que salgan de esta sesión con **un** proceso elegido, mapeado, acotado y medido. Y con el patrón identificado.

## Conduce así

**1. El proceso.** ¿Qué proceso manual quieren automatizar? Casi nunca traen uno solo, o traen uno vago. Corta.

Filtra con: frecuencia, tiempo que consume, qué pasa si sale mal, si tienen acceso a los sistemas. Si el proceso es raro o de bajo riesgo, **dilo**: da demo bonita y ROI cero.

**2. El flujo actual, como es hoy.** Con sus parches. Clasifica cada paso: condición dura / IA / humano.

**3. El patrón.** Mira `patrones/`. Casi seguro es uno de los seis, o dos combinados. Si es así, no diseñen desde cero — ahí está la estructura.

**4. ¿Workflow o agente?** Prueba de 30 segundos: ¿pueden dibujar el diagrama completo? Si sí, es un workflow. No le pongan un agente encima.

**5. Línea base.** Tiempo por ejecución, ejecuciones por semana, tasa de error actual. **No los dejes salir sin esto.** Si no tienen los números, dales una forma barata de medirlos esta semana. Un cronómetro y una hoja durante tres días basta.

**6. Alcance.** Lista explícita de qué queda fuera. Y una definición de terminado que sea verificable.

## Escribe

Crea `proyectos/<nombre>/` con:
- `proyecto.md` — copia de `proyectos/PLANTILLA.md`, llena
- `flujo.md` — diagrama Mermaid del proceso actual y del propuesto
- `reglas.md` — tabla: input → condición → acción → ¿escala a humano?

## Antes de cerrar

Diles en una línea qué es lo más probable que salga mal en este proyecto. Sé específico. Ya viste el proceso.
