---
type: log
title: Historial de verificación
description: Registro cronológico de cambios en el bundle de referencias.
tags: [log, historial, verificacion]
timestamp: 2026-07-13T00:00:00Z
---

# Log

## 2026-07-13 — Verificación inicial

Bundle creado. Todas las referencias verificadas contra fuente oficial.

**Hallazgos:**

- **Sonnet 5 está en precio introductorio** ($2/$10 por MTok) **hasta el 31 de agosto de 2026.** Sube a $3/$15 el 1 de septiembre. Cualquier modelo de costos anual hecho con el precio de hoy queda 50% abajo.
- **Tokenizador nuevo** en Opus 4.7+, Sonnet 5 y Fable 5: ~30% más tokens para el mismo texto. Las estimaciones hechas con la intuición de modelos anteriores se quedan cortas por un tercio.
- **Los tokens leídos de cache no cuentan para el rate limit ITPM.** El caching no es solo descuento: es multiplicador de throughput.
- **n8n tiene nodos nativos de Guardrails y Evaluation**, y human-in-the-loop en tool calls. Casi ningún tutorial los menciona.
- **MCP:** estable `2025-11-25`; hay RC `2026-07-28` que aún puede cambiar. No construir contra el RC.
- **LangChain:** `AgentExecutor` deprecado (mantenimiento hasta dic 2026). Los tutoriales que lo usan son de 2024.

**Nota metodológica:** tres de los blogs mejor rankeados de "Claude API pricing 2026" tenían datos viejos o incorrectos —incluyendo el primer resultado de Google— presentando modelos retirados como actuales. **Siempre fuente oficial.**

Bundle convertido a **OKF v0.1** el mismo día.
