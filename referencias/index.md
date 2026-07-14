---
type: index
title: Referencias verificadas
description: Bundle OKF con los hechos duros del stack. Verificados contra fuente oficial y fechados. Ninguno se cita de memoria.
tags: [index, referencias, hechos-verificados]
timestamp: 2026-07-13T00:00:00Z
---

# Referencias

Bundle conforme a **Open Knowledge Format v0.1**. Cada archivo es un concepto; el frontmatter es consultable; los links cruzados forman el grafo.

Esto es lo que hace que este repo valga más que un asistente a secas: **hechos verificados contra fuente oficial, con fecha.** Un LLM alucina precios y rate limits con total confianza. Estos archivos no.

**Regla: ningún hecho duro se cita de memoria. Se lee de aquí.**

## Conceptos

| Concepto | Tipo | Qué contiene |
|---|---|---|
| [Claude API](claude-api.md) | referencia-api | Precios, rate limits, caching, batch, costos ocultos |
| [n8n](n8n.md) | referencia-plataforma | Nodos, versiones, trampas, los nodos de IA infrautilizados |
| [MCP](mcp.md) | referencia-protocolo | Versiones del spec, primitivas, cuándo es sobreingeniería |
| [LangChain / LangGraph](langchain.md) | referencia-framework | Versiones, lo deprecado, cuándo usar cuál |
| [Harness](harness.md) | referencia-patron | Agentes de larga duración: fallas y soluciones verificadas |
| [Agent Engineering](agent-engineering.md) | referencia-campo | Estado del campo: adopción, horizonte autónomo, error compuesto, las 7 capas |
| [OKF](okf.md) | especificacion | El formato de este bundle |
| [Cómo actualizar](COMO-ACTUALIZAR.md) | runbook | Procedimiento de re-verificación |

## Estado

**Última verificación completa: 13 de julio de 2026.**

Se pudren. Corre `/verificar` cada dos meses, o antes de cualquier decisión de costos que importe.

Ver [log.md](log.md) para el historial.
