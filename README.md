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

## Sesión 2 — Evals: mide tu decisión de IA, desde cero

En la Sesión 1 diseñaste tu sistema y quedaron una o dos decisiones en manos de la IA. La Sesión 2 responde una sola pregunta: **¿cuánto acierta esa decisión?** No "se ve bien". Un número.

### Primero: ¿qué es un eval?

Un eval es **un caso de prueba con dos cosas. Nada más:**

| Parte | Qué es | Ejemplo |
|---|---|---|
| **Input** | Lo que entra a tu sistema | Correo: «su producto llegó roto» |
| **Salida esperada** | Lo que DEBERÍA salir, escrito por ti antes de correr nada | `categoría: queja` |

No es una herramienta cara ni un framework. Con diez de estos tienes con qué medir.

**La regla que lo vuelve real:** la salida esperada tiene que poder verificarse **sin preguntarle al modelo si acertó**. Un campo concreto: una categoría, un sí/no, un valor.

| ✅ Salida esperada bien escrita | ❌ Mal escrita (no verificable) |
|---|---|
| `categoría: queja` | "que clasifique bien el correo" |
| `resultado: escalar` | "que se dé cuenta de que falta la OC" |
| `monto: 14200` | "una respuesta razonable" |

Si para saber si acertó tendrías que opinar, el caso está mal escrito.

### ¿Y qué es "el prompt" que vamos a medir?

Es la instrucción que tu sistema le da a Claude para tomar SU decisión — la que quedó como IA en tu `flujo.md` de la Sesión 1. Vive en un archivo de tu proyecto: `proyectos/<tu-proyecto>/prompts.md`. Si aún no lo tienes, el agente te ayuda a crearlo en el Paso 1.

Cada vez que lo cambies, la versión anterior se queda escrita (v0, v1, v2…). Nunca borres una versión: comparar los números entre versiones es justamente el punto.

### Dónde se hace cada cosa (no te pierdas)

Vas a alternar entre dos lugares que se ven parecidos pero no son lo mismo:

| Lugar | Cómo se ve | Qué haces ahí |
|---|---|---|
| **Terminal** | El prompt de tu sistema (`%` o `$`) | Comandos como `ls`, `curl`, `git`, y arrancar `claude` |
| **Claude Code** | Conversación con el agente (entraste con `claude`) | Hablar con el agente y correr `/eval`, `/arrancar`, etc. |

Para salir de Claude Code y volver a la terminal: `Ctrl+C` (o escribe `/exit`). Para volver a entrar: `claude`.

---

### Paso 0 — Trae el comando `/eval` y la plantilla a TU repo 📍 EN LA TERMINAL

Si creaste tu repo desde esta plantilla **antes** de la Sesión 2, tu copia no tiene `/eval` ni la plantilla de evals (los repos creados desde template NO reciben actualizaciones automáticas — son una foto del día que los creaste).

Abre la terminal y entra a tu repo (si no sabes dónde quedó, es la carpeta donde clonaste; corre `pwd` para ver dónde estás):

```bash
cd ~/proyectos-ia/TU-REPO   # ajusta la ruta a donde clonaste el tuyo
```

Verifica si ya tienes el comando:

```bash
ls .claude/commands/
```

Si en la lista aparece `eval.md`, ya está: salta al Paso 1. Si NO aparece, copia y pega estas dos líneas (descargan el comando y la plantilla directo de este repo):

```bash
curl -o .claude/commands/eval.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/.claude/commands/eval.md
curl -o proyectos/PLANTILLA-evals.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-evals.md
```

Confirma que llegaron y guárdalos en tu repo:

```bash
ls .claude/commands/          # debe aparecer eval.md
ls proyectos/                 # debe aparecer PLANTILLA-evals.md

git add .claude/commands/eval.md proyectos/PLANTILLA-evals.md
git commit -m "sesion 2: comando /eval y plantilla de evals"
git push
```

**Importante:** si tenías Claude Code abierto, ciérralo (`Ctrl+C`) y vuelve a entrar (`claude`). Los comandos nuevos solo se detectan al arrancar.

---

### Paso 1 — Escribe tus 10 casos ANTES de correr nada 📍 EN CLAUDE CODE

El camino fácil: copia la plantilla a tu proyecto y llénala con ayuda del agente. Dentro de Claude Code, pégale esto (ajusta el nombre de tu proyecto):

```
Copia proyectos/PLANTILLA-evals.md a proyectos/<tu-proyecto>/evals.md y ayúdame
a llenarla. La decisión que quiero evaluar es la más difícil de mi decisiones.md.
Hazme preguntas sobre mi operación para construir los casos — no los inventes tú.
```

El reparto de los 10 casos no es negociable:

| Cuántos | Tipo | Qué son |
|---|---|---|
| 5 | Típicos | El caso normal de todos los días |
| 3 | Límite | Raros pero reales, sacados de TU operación (el redondeo, el caso fronterizo) |
| 2 | Adversariales | Diseñados para romperla: input vacío, datos que faltan, un intento de manipulación |

> Si los 10 casos fueran del camino feliz, el número mentiría hacia arriba. Los casos límite y adversariales son los que te dicen la verdad.

Tú pones los casos reales y decides la salida esperada de cada uno. El agente arma la tabla. **No dejes que el agente invente los casos: un número construido con casos inventados es ficción.**

**¿Se te dificulta escribir los casos?** Es normal — es la parte más difícil de la sesión. Lee [`proyectos/GUIA-escribir-evals.md`](proyectos/GUIA-escribir-evals.md): tiene recetas por tipo de decisión (clasificar, extraer, decidir, redactar) con tablas completas para calcar, el truco de "no escribas casos, recuerda casos", y un prompt para llenar la tabla con el agente de uno en uno. Si vas contra reloj: 6 casos reales (4 típicos, 1 límite, 1 adversarial) bastan para una primera medición honesta.

### Paso 2 — Corre la línea base 📍 EN CLAUDE CODE

Escribe exactamente esto:

```
/eval
```

El agente toma tus 10 casos, los corre contra tu prompt actual, compara cada salida contra la verdad que TÚ escribiste, y te da la tasa: `6/10`, `7/10`, lo que salga. **Ese número feo es tu línea base — tu punto de partida medido.** No lo maquilles.

Guárdalo con un commit 📍 EN LA TERMINAL (o pídele al agente que lo haga):

```bash
git add proyectos/
git commit -m "eval: corrida v0 - 6/10"
git push
```

### Paso 3 — Diagnostica y agrega SOLO el contexto que los fallos pidieron 📍 EN CLAUDE CODE

Por cada caso que falló (❌), el agente te dice en qué nivel de la escalera cae, en este orden:

| Nivel | Pregunta | Ejemplo de arreglo |
|---|---|---|
| 1 | ¿La regla está escrita ambigua? | Reescribir "si coincide" → "si la diferencia es menor a $0.50" |
| 2 | ¿Le falta un dato? | Una lista de alias de proveedores, un catálogo, una tolerancia |
| 3 | ¿Era un `if` disfrazado? | Sacarlo del prompt: eso se resuelve con código, no con IA |
| 4 | ¿Es el modelo? | **Casi nunca es el modelo.** Es el último escalón, no el primero |

La disciplina: agrega únicamente el contexto que un eval reprobado pidió. Nada especulativo, ni un documento más. El agente actualiza tu `prompts.md` con la versión nueva (v1) **sin borrar la v0**.

### Paso 4 — Vuelve a correr y compara 📍 EN CLAUDE CODE

```
/eval
```

Cada versión del prompt deja su propia columna en tu `evals.md`. Ver el número subir de v0 a v1 — con el porqué escrito al lado — es tu evidencia de que el sistema mejoró. Commit y push otra vez.

**Regla permanente desde hoy: cada vez que toques un prompt, corre `/eval`.** Arreglas un caso y sin querer rompes otros tres; sin la suite, te enteras en producción. Los prompts se rompen igual que el código, pero fallan más callados.

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
| `/eval` | Corre tus evals contra el prompt actual y te da la tasa de acierto |

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
