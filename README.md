# AI & Automation Expert

Tu experto en construcción de productos de IA y automatizaciones, dentro de Claude Code.

No es un chatbot de curso. Es infraestructura: la clonas una vez y te sirve para la automatización #1 y para la #12, seis meses después de que el curso terminó.

**Curso Productividad con Claude (IA) AVANZADO — Collective Academy, julio–agosto 2026.**

---

## Arranque

### Paso 0 — Instala los prerequisitos

#### macOS

1. **Homebrew** (gestor de paquetes). Si no lo tienes, abre Terminal y pega:

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

### Paso 0.5 — Valida que tienes todo

Antes de seguir, corre estos tres comandos. Los tres deben devolver un número de versión sin errores:

```bash
node --version    # debe decir v18.x o superior
npm --version     # debe decir 9.x o superior
git --version     # debe decir git version 2.x o superior
```

Si alguno dice `command not found`, revisa el paso correspondiente arriba.

---

### Paso 1 — Instala Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

**⚠️ Si te sale un warning de `allow-scripts`** (algo como `npm warn allow-scripts 1 package has install scripts not yet covered...`), significa que npm bloqueó el script de post-instalación por seguridad. Claude Code necesita ese script para funcionar. Corre esto en su lugar:

```bash
npm install -g --allow-scripts=@anthropic-ai/claude-code @anthropic-ai/claude-code
```

Para que no te lo vuelva a pedir en futuras actualizaciones:

```bash
npm config set allow-scripts=@anthropic-ai/claude-code --location=user
```

**Verifica que se instaló correctamente:**

```bash
claude --version
```

Debe devolver un número de versión (ej. `2.1.209`). Si dice `command not found: claude`, npm no tiene su directorio global en tu PATH. Solución:

```bash
# Verifica dónde instaló npm el binario:
npm bin -g

# Agrega ese directorio a tu PATH (en macOS/zsh):
echo 'export PATH="'$(npm bin -g)'":$PATH' >> ~/.zshrc
source ~/.zshrc

# Ahora sí:
claude --version
```

---

### Paso 2 — Crea tu copia del repo

**No hagas fork ni clones directo.** Usa el botón de template para tener tu propio repo con permisos de escritura:

1. Ve a [github.com/nabolom/ai-automation-expert](https://github.com/nabolom/ai-automation-expert)
2. Click en el botón verde **“Use this template”** → **“Create a new repository”**
3. Pon el nombre que quieras (ej. `mi-automatizacion`)
4. Márcalo como **Private** — tus procesos de negocio no tienen por qué ser públicos
5. Click en **“Create repository”**

Ahora clona **tu** repo (el que acabas de crear, no el mío).

**⚠️ Antes de clonar: elige dónde quieres que viva tu proyecto.**

`git clone` crea la carpeta **donde estés parado en la terminal**. Si no sabes dónde estás, corre `pwd` primero. Si quieres tenerlo ordenado, créate una carpeta antes:

```bash
# 1. Verifica dónde estás parado:
pwd

# 2. (Opcional) Crea una carpeta para tus proyectos de IA y entra a ella:
mkdir ~/proyectos-ia
cd ~/proyectos-ia

# 3. Ahora sí, clona tu repo aquí:
git clone https://github.com/TU-USUARIO/TU-REPO.git
cd TU-REPO
claude
```

Si prefieres otro lugar (Escritorio, Documentos, donde sea), simplemente haz `cd` ahí antes de clonar:

```bash
# Ejemplo: clonar en el Escritorio
cd ~/Desktop
git clone https://github.com/TU-USUARIO/TU-REPO.git
cd TU-REPO
claude
```

Al entrar al directorio y correr `claude`, el agente lee automáticamente `CLAUDE.md` y carga toda la doctrina, el mapa del repo y las reglas de comportamiento. No necesitas configurar nada más.

Cuando hagas cambios (prompts, proyectos, referencias nuevas), `git push` funciona porque es **tu** repo.

---

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
13. **Menos pasos, siempre.** `0.95^20 = 0.358`. Veinte pasos al 95% y el sistema funciona el 36% de las veces.
14. **Cuando falle, pregunta qué CAPA está rota.** Casi nunca es el modelo. Ver `referencias/agent-engineering.md`.

¿Sabes hacer las ocho cosas que separan una demo de un sistema? → [`COMPETENCIAS.md`](COMPETENCIAS.md)

---

## Estructura

```
CLAUDE.md          el cerebro
skills/            capacidades — se cargan bajo demanda
patrones/          los siete patrones, con estructura y trampas
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
