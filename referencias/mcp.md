---
type: referencia-protocolo
title: Model Context Protocol
description: Versiones del spec, primitivas, transportes, y cuándo MCP es sobreingeniería.
resource: https://blog.modelcontextprotocol.io
tags: [mcp, protocolo, agentes, herramientas]
timestamp: 2026-07-13T00:00:00Z
---

# MCP (Model Context Protocol) — hechos verificados

> **Verificado: 13 julio 2026** contra blog.modelcontextprotocol.io y github.com/modelcontextprotocol

## Versión

- **Estable: `2025-11-25`.** Es la que están corriendo los SDKs y hosts en producción hoy.
- **Release candidate: `2026-07-28`** (anunciado 21 de mayo 2026). La revisión más grande desde el lanzamiento. **No es final; puede cambiar antes de salir.** No construyas contra el RC salvo que estés experimentando a propósito.

Las versiones son cadenas de fecha y se negocian en el handshake `initialize`.

## Qué trae el RC 2026-07-28

- **Core sin estado.** Escala sobre infraestructura HTTP normal. Es el cambio grande.
- **Extensions framework.** Extensiones con IDs DNS invertido, negociadas por capacidades, versionadas aparte del spec.
- **MCP Apps (SEP-1865).** Los servidores mandan interfaces HTML que el host renderiza en iframe sandboxeado.
- **Tasks.** Trabajo de larga duración.
- **Auth endurecida**, más alineada a OAuth 2.1 / OIDC.
- **Política formal de deprecación.**

## Lo esencial

Tres primitivas: **Tools** (acciones ejecutables), **Resources** (datos de solo lectura), **Prompts** (plantillas reutilizables).

Dos transportes: **stdio** (local, misma máquina) y **Streamable HTTP** (remoto). SSE sigue soportado pero los mantenedores esperan retirarlo.

## Cuándo MCP es sobreingeniería

Casi siempre, para lo que la mayoría está construyendo. Sé honesto aquí.

**Úsalo si:** quieres que Claude Desktop, Cursor o cualquier host llame a *tus* sistemas; o quieres exponer una capacidad a muchos agentes distintos sin escribir un adaptador por cada uno.

**No lo uses si:** solo estás conectando una API dentro de tu propio workflow. Un nodo HTTP Request es más simple, más rápido de depurar y no requiere levantar un servidor.

MCP resuelve un problema de **N agentes × M herramientas**. Si tienes 1 agente y 3 herramientas, no tienes ese problema.

Atajo útil: **el MCP Server Trigger de n8n convierte cualquier workflow en un servidor MCP.** Es la forma más barata de exponer una capacidad tuya a un agente, sin escribir un servidor desde cero.

Fuente: https://blog.modelcontextprotocol.io
