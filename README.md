# AI & Automation Expert

Tu experto en construcción de productos de IA y automatizaciones, dentro de Claude Code.

No es un chatbot de curso. Es infraestructura: la clonas una vez y te sirve para la automatización #1 y para la #12, seis meses después de que el curso terminó.

**Curso Productividad con Claude (IA) AVANZADO — Collective Academy, julio–agosto 2026.**

---

## Arranque

### Paso 0 — Instala los prerequisitos

#### macOS

1. **Homebrew** (gestor de paquetes). Si no lo tienes:

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Node.js** (incluye npm — no hace falta instalar npm por separado):

   ```bash
   brew install node
   ```

3. **Git** — normalmente ya viene con macOS o con las Command Line Tools de Xcode. Si falta:

   ```bash
   brew install git
   ```

   O alternativamente:

   ```bash
   xcode-select --install
   ```

#### Windows

1. **Node.js LTS** — descárgalo directo desde [nodejs.org](https://nodejs.org/) (el instalador trae npm incluido). Homebrew **no aplica** en Windows.

2. **Git** — descárgalo desde [git-scm.com](https://git-scm.com/download/win) si no lo tienes.

3. Claude Code corre nativo en Windows (PowerShell o cmd). Si prefieres un entorno tipo Linux, puedes usar WSL.

---

### Paso 1 — Instala Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

### Paso 2 — Clona este repo y entra

```bash
git clone https://github.com/nabolom/ai-automation-expert.git mi-automatizacion
cd mi-automatizacion
claude
```

### Paso 3 — Arranca tu primera automatización

Dentro de Claude Code, escribe:

```
/arrancar
```

El agente te va a conducir paso a paso: elegir un proceso, mapearlo, clasificar cada paso, identificar el patrón, medir la línea base y definir el alcance. Al terminar tendrás los archivos de tu proyecto en `proyectos/`.

---

## Por qué este repo existe

Un asistente de IA genérico ya sabe qué es un webhook. Lo que **no** tiene es criterio, hechos frescos y patrones probados. Y peor: **alucina precios, rate limits y nombres de nodos con total confianza.**

Tres cosas viven aquí, y solo tres:

**1. Doctrina.** Opiniones, no análisis balanceados. Preguntas "¿agente o workflow?" y obtienes una respuesta, con el criterio para decidirlo tú la próxima vez.

**2. Hechos verificados y fechados.** Precios, rate limits, versiones, nombres exactos de nodos — contra fuente oficial, con fecha. Cuando el asistente y la referencia se contradigan, **gana la referencia**.

`referencias/` es un bundle conforme a **Open Knowledge Format v0.1** (el spec que Google Cloud publicó en junio de 2026 para formalizar el patrón "LLM wiki"). Portable, interoperable, y crece solo: el agente puede escribir conceptos nuevos cuando aprende algo.

**3. Patrones ejecutables.** Siete formas. Casi toda automatización de negocio es una de ellas.

---

## La doctrina, en corto

1. **Si puedes escribirlo como un `if`, que sea un `if`.** La IA es cara, lenta y no determinista.
2. **¿Puedes dibujar el diagrama completo?** Entonces es un workflow, no un agente.
3. **Diseña la falla, no el éxito.** Nadie está viendo a las 3 AM.
4. **Salida estructurada o no hay sistema.** JSON con esquema, validado.
5. **Los prompts son código.** Versiónalos, corre evals cuando cambien.
6. **Un sistema completo a medias > medio sistema completo.**
7. **El agente debe saber cuándo no sabe.** Escalamiento explícito.
8. **Mide el proceso antes de automatizarlo.** No se recupera después.
9. **Lo que no está en disco no existe.**
10. **El harness importa más que el modelo.** Antes de subir de modelo, arregla el andamio.
11. **La condición de parada debe ser verificable sin el modelo.** Un modelo siempre cree que avanzó.
12. **El conocimiento vive en archivos, no en prompts.** Un concepto por archivo, versionado junto al código (OKF).

---

## Estructura

```
CLAUDE.md          el cerebro
skills/            capacidades — se cargan bajo demanda
patrones/          los seis patrones, con estructura y trampas
referencias/       hechos verificados y FECHADOS ← lo más valioso
proyectos/         tus automatizaciones
```

## Comandos

| Comando | Qué hace |
|---|---|
| `/arrancar` | Nueva automatización: del proceso al diseño |
| `/auditar` | Revisión crítica. Te dice qué parte de tu sistema es teatro |
| `/verificar` | Re-verifica las referencias contra fuente oficial |

Fuera de los comandos, habla normal. Pregúntale, pídele código, discútele decisiones. Tiene el contexto.

---

## Los siete patrones

| # | Patrón | Pieza clave |
|---|---|---|
| 01 | Clasificar y rutear | Text Classifier — **no un agente** |
| 02 | Extraer documento → JSON | Information Extractor |
| 03 | Human-in-the-loop | Chat node / HIL en tool calls |
| 04 | RAG interno | Vector store + citas obligatorias |
| 05 | Watcher | Estado + umbral de relevancia |
| 06 | Enriquecer datos | Batch API (50% menos) |
| 07 | Loop agéntico de larga duración | Harness: progreso + verificación externa |

---

## Advertencia sobre los hechos

Los archivos de `referencias/` están verificados al **13 de julio de 2026**.

Se pudren. Corre `/verificar` cada dos meses, o antes de cualquier decisión de costos que importe.

Al armar este repo, tres de los blogs mejor rankeados de "Claude API pricing 2026" tenían datos viejos o mal — incluyendo el primer resultado de Google. **Siempre la fuente oficial.**

---

## Seguridad

Nunca subas credenciales. `.gitignore` ya bloquea `.env`.

Si tu proceso toca datos sensibles de tu empresa, lee `skills/seguridad-datos/SKILL.md` antes de conectar nada.
