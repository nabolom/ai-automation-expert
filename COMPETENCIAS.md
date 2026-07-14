---
type: autodiagnostico
title: Las 8 competencias de Agent Engineering
description: Autodiagnóstico. Ocho cosas que separan a quien construye demos de quien construye sistemas.
tags: [competencias, autodiagnostico, agent-engineering]
timestamp: 2026-07-13T00:00:00Z
---

# ¿Sabes hacer estas ocho cosas?

Las herramientas cambian cada seis meses. Estas competencias no.

Marca honestamente. Lo que no marques, ahí está la skill que te toca.

---

**1. Especificación de objetivos** — ☐
Convertir un deseo vago en un criterio de éxito **verificable por algo que no sea el modelo**.

> *La prueba:* tu definición de "terminado" la puede verificar un tercero sin preguntarle al agente.
> → `skills/diseno-de-sistema`

**2. Juicio de delegación** — ☐
Decidir qué se automatiza, qué se supervisa y qué sigue siendo humano. `if` vs IA. Workflow vs agente.

> *La prueba:* miras un flujo y clasificas cada paso en 30 segundos.
> → `skills/diseno-de-sistema`

**3. Context Engineering** — ☐
Diseñar qué información ve el agente, cuándo y con qué permisos. No "más documentos": los correctos.

> *La prueba:* sabes qué NO metes al contexto, y por qué.
> → `skills/okf`, `skills/harness-y-loops`

**4. Evaluación** — ☐
Escribir casos de prueba **antes** de construir. Poder decir un número, no una opinión.

> *La prueba:* puedes decir tu tasa de acierto y sobre cuántos casos.
> → `skills/errores-y-evals`

**5. Harness literacy** — ☐
Distinguir modelo, framework, harness y contexto. Saber **cuál está roto** cuando algo falla.

> *La prueba:* cuando falla, tu primera pregunta no es "¿cambio a Opus?".
> → `skills/harness-y-loops`, `referencias/agent-engineering.md`

**6. Loop Engineering** — ☐
Diseñar ciclos que ejecutan, verifican y paran solos. Dejar de ser tú el bucle.

> *La prueba:* tu loop tiene las cuatro paradas: éxito verificable, presupuesto, no-progreso, escalamiento.
> → `skills/harness-y-loops`, `patrones/07-loop-agentico.md`

**7. Gobernanza** — ☐
Permisos mínimos, límites de gasto, trazabilidad, escalamiento. **Guardrails en la capa de acción, no de output.**

> *La prueba:* si filtras la respuesta, el agente ya mandó el correo. ¿Dónde están tus guardrails?
> → `skills/seguridad-datos`

**8. Mejora continua** — ☐
Usar fallas y trazas para ajustar el sistema, no para repetir prompts a ciegas.

> *La prueba:* cuando la precisión es mala, revisas la ambigüedad de tus reglas antes que el modelo.
> → `skills/errores-y-evals`

---

## La frase que las ordena

> **La productividad no viene de pedirle más al modelo. Viene de diseñar el entorno donde el modelo puede trabajar:** contexto correcto, herramientas adecuadas, memoria durable, criterios de éxito claros y límites seguros.

## El dato que las justifica

Claude Opus 4.6 tiene un horizonte autónomo de **12 horas al 50% de confiabilidad** — y de **70 minutos al 80%**.

Esa brecha no la cierra el modelo. **La cierra el sistema que construyes alrededor.**

(Ver `referencias/agent-engineering.md`. Y corre `/verificar` — ese número tiene fecha.)
