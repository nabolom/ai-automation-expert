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

Esta sección tiene dos partes. **Parte A** son los conceptos — léela con calma, idealmente antes del taller. **Parte B** es el taller paso a paso — ahí vas a copiar y pegar. No las mezcles: primero entiende, luego ejecuta.

---

## Parte A — Los conceptos (léelos antes de tocar nada)

### ¿Qué es un eval?

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

### ¿Qué es "el prompt" que vamos a medir?

Es la instrucción que tu sistema le da a Claude para tomar SU decisión — la que quedó como IA en tu `flujo.md` de la Sesión 1. Vive en un archivo de tu proyecto: `proyectos/<tu-proyecto>/prompts.md`. Si aún no lo tienes, el agente te ayuda a crearlo en el **Paso 2**, justo antes de correr `/eval` — lo traduce desde tu diseño de la Sesión 1 (`decisiones.md` y `reglas.md`).

Cada vez que lo cambies, la versión anterior se queda escrita (v0, v1, v2…). Nunca borres una versión: comparar los números entre versiones es justamente el punto.

### Dónde se hace cada cosa (no te pierdas)

Vas a alternar entre dos lugares que se ven parecidos pero no son lo mismo:

| Lugar | Cómo se ve | Qué haces ahí |
|---|---|---|
| **Terminal** | El prompt de tu sistema (`%` o `$`) | Comandos como `ls`, `curl`, `git`, y arrancar `claude` |
| **Claude Code** | Conversación con el agente (entraste con `claude`) | Hablar con el agente y correr `/eval`, `/arrancar`, etc. |

Para salir de Claude Code y volver a la terminal: `Ctrl+C` (o escribe `/exit`). Para volver a entrar: `claude`.

### ¿Por qué los casos van ANTES que el prompt?

Pregunta justa — parecería natural escribir primero el prompt y luego probarlo. El orden es al revés a propósito:

> **La verdad se fija antes de medir.** Si escribes el prompt primero, vas a escribir — sin darte cuenta — casos que tu prompt ya resuelve, y el número saldrá bonito y mentiroso. Si fijas los 10 casos primero (con sus salidas esperadas), el prompt se enfrenta a una realidad que no puede acomodar. Es la misma razón por la que un examen se escribe antes de ver las respuestas del alumno.

Además, tu prompt v0 no nace de la nada: se traduce desde tu `decisiones.md` y `reglas.md` de la Sesión 1. El diseño ya existe; el prompt solo lo pone en palabras. Por eso escribirlo toma 2 minutos en el Paso 2, mientras que los casos — que sí requieren TU conocimiento de la operación — se llevan el Paso 1 completo.

---

## Parte B — El taller, paso a paso

La ruta completa, para que sepas siempre dónde estás:

| Paso | Qué haces | Dónde | Qué queda en disco |
|---|---|---|---|
| **0** | Actualiza tu repo (trae `/eval` y la plantilla) | 📍 Terminal | `.claude/commands/eval.md`, `proyectos/PLANTILLA-evals.md` |
| **1** | Escribe tus 10 casos (la verdad, primero) | 📍 Claude Code | `evals.md` con inputs y salidas esperadas |
| **2** | Escribe tu prompt v0 y corre la línea base | 📍 Claude Code | `prompts.md` (v0) + tu primer número |
| **3** | Diagnostica los fallos, agrega SOLO el contexto pedido | 📍 Claude Code | `prompts.md` (v1, sin borrar v0) |
| **4** | Vuelve a correr y compara v0 vs v1 | 📍 Claude Code | `evals.md` con una columna por versión |

Cada paso cierra con un commit. Al final del taller, tu `git log` cuenta la historia completa: la verdad → el número feo → el porqué → el número mejor.

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

> Primero la verdad, luego el prompt. Si te saltaste el porqué, está en la Parte A.

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

---

### Paso 2 — Escribe tu prompt v0 y corre la línea base 📍 EN CLAUDE CODE

Primero necesitas el prompt que vas a medir. Si aún no tienes `prompts.md`, pídeselo al agente — que lo arme desde tu diseño de la Sesión 1, **mínimo, sin adornos**:

```
Escribe la versión v0 del prompt para la decisión que estoy evaluando, usando mi
decisiones.md y mi reglas.md. Que sea mínima: solo la instrucción y las reglas,
sin contexto de negocio ni ejemplos. Guárdala en proyectos/<tu-proyecto>/prompts.md como v0.
```

Pelón a propósito: quieres medir el punto de partida más crudo, para que la mejora del contexto se note después. Con el v0 en su lugar, mídelo:

```
/eval
```

El agente toma tus 10 casos, los corre contra tu prompt v0, compara cada salida contra la verdad que TÚ escribiste, y te da la tasa: `6/10`, `7/10`, lo que salga. **Ese número feo es tu línea base — tu punto de partida medido.** No lo maquilles.

Guárdalo con un commit 📍 EN LA TERMINAL (o pídele al agente que lo haga):

```bash
git add proyectos/
git commit -m "eval: corrida v0 - 6/10"
git push
```

---

### Paso 3 — Diagnostica y agrega SOLO el contexto que los fallos pidieron 📍 EN CLAUDE CODE

Por cada caso que falló (❌), el agente te dice en qué nivel de la escalera cae, en este orden:

| Nivel | Pregunta | Ejemplo de arreglo |
|---|---|---|
| 1 | ¿La regla está escrita ambigua? | Reescribir "si coincide" → "si la diferencia es menor a $0.50" |
| 2 | ¿Le falta un dato? | Una lista de alias de proveedores, un catálogo, una tolerancia |
| 3 | ¿Era un `if` disfrazado? | Sacarlo del prompt: eso se resuelve con código, no con IA |
| 4 | ¿Es el modelo? | **Casi nunca es el modelo.** Es el último escalón, no el primero |

La disciplina: agrega únicamente el contexto que un eval reprobado pidió. Nada especulativo, ni un documento más. El agente actualiza tu `prompts.md` con la versión nueva (v1) **sin borrar la v0**.

---

### Paso 4 — Vuelve a correr y compara 📍 EN CLAUDE CODE

```
/eval
```

Cada versión del prompt deja su propia columna en tu `evals.md`. Ver el número subir de v0 a v1 — con el porqué escrito al lado — es tu evidencia de que el sistema mejoró. Commit y push otra vez.

**Regla permanente desde hoy: cada vez que toques un prompt, corre `/eval`.** Arreglas un caso y sin querer rompes otros tres; sin la suite, te enteras en producción. Los prompts se rompen igual que el código, pero fallan más callados.

---

## Sesión 3 — Loops: el sistema que corre solo y para solo

En la Sesión 1 diseñaste el sistema. En la Sesión 2 mediste su decisión de IA. La Sesión 3 cierra el núcleo: **convertir el paso donde HOY el humano es el bucle** — revisar, corregir, volver a revisar — en un loop que trabaja, se verifica y se detiene sin que nadie lo esté viendo.

La idea central viene de la doctrina (`CLAUDE.md` §11): un loop sin condición de parada verificable no es automatización — es una fuga de presupuesto con un optimista adentro. Todo loop necesita **cuatro paradas**:

| # | Parada | La pregunta que responde |
|---|---|---|
| 1 | **Éxito** | ¿Cómo sabe que terminó BIEN — sin preguntarle al modelo? |
| 2 | **Presupuesto** | ¿Cuántas vueltas / cuánto dinero antes de rendirse? |
| 3 | **No-progreso** | ¿Cómo detecta que da vueltas sin avanzar? |
| 4 | **Escalamiento** | ¿Qué casos van a un humano ANTES de agotar presupuesto? |

Falta una y el loop es un riesgo (§11). Las cuatro con números concretos, o el loop no se escribe.

---

### Paso 0 — Trae `/loop` y su plantilla a TU repo 📍 EN LA TERMINAL

Igual que en la Sesión 2: tu repo es una foto del día que lo creaste, y `/loop` se publicó después. Parado en tu repo (`pwd` para ubicarte, `cd` a tu carpeta), verifica:

```bash
ls .claude/commands/
```

Si aparece `loop.md`, salta al Paso 1. Si NO, copia y pega estas dos líneas (cada una completa):

```bash
curl -o .claude/commands/loop.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/.claude/commands/loop.md
curl -o proyectos/PLANTILLA-loop.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-loop.md
```

Confirma y guarda en tu repo:

```bash
ls .claude/commands/          # debe aparecer loop.md
ls proyectos/                 # debe aparecer PLANTILLA-loop.md

git add .claude/commands/loop.md proyectos/PLANTILLA-loop.md
git commit -m "sesion 3: comando /loop y plantilla de loops"
git push
```

**Importante:** si Claude Code estaba abierto, ciérralo (`Ctrl+C`) y vuelve a entrar (`claude`) — los comandos nuevos solo se detectan al arrancar.

---

### Paso 1 — Diagnostica antes de construir 📍 EN CLAUDE CODE

Antes de escribir un loop, nombra la capa donde tu sistema falla hoy. Un loop mal ubicado automatiza el paso equivocado.

Copia las dos plantillas a tu proyecto:

```
cp proyectos/PLANTILLA-diagnostico.md proyectos/<tu-proyecto>/diagnostico.md
cp proyectos/PLANTILLA-harness.md proyectos/<tu-proyecto>/harness.md
```

Si no las tienes todavía, tráelas igual que `/loop`:

```
curl -o proyectos/PLANTILLA-diagnostico.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-diagnostico.md
curl -o proyectos/PLANTILLA-harness.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-harness.md
```

**`diagnostico.md`** — toma los casos que fallaron en tu `evals.md` y clasifícalos por capa. La columna que cuesta es *evidencia*: «se siente como el modelo» no es evidencia; «le di el dato explícito y aun así falló» sí lo es. Nadie escribe «modelo» sin descartar las otras tres capas por escrito.

**`harness.md`** — inventario de las cuatro capas de tu sistema: qué existe, dónde vive, qué falta. Cada capa termina con al menos un hueco nombrado. Si no le falta nada a ninguna, tu sistema nunca ha corrido solo.

Las cuatro capas se diagnostican **de abajo hacia arriba** — harness, framework, contexto, modelo — porque lo barato y probable se revisa primero. El marco completo está en `referencias/harness.md`.

```
git add proyectos/
git commit -m "sesion 3: diagnostico por capas e inventario de harness"
git push
```

---

### Paso 2 — Corre `/loop` en tu proyecto 📍 EN CLAUDE CODE

**Prerequisitos:** `/loop` lee tu `flujo.md`, tu `evals.md` y tu `alcance.md`. Si falta alguno, el comando se detiene y te lo dice — las Sesiones 1 y 2 son el suelo de esta. Si tu suite de evals quedó incompleta, ciérrala primero (la [guía de evals](proyectos/GUIA-escribir-evals.md) sigue ahí).

```
/loop
```

El comando trabaja en cinco fases, y te va a exigir números — no te aceptes a ti mismo respuestas cualitativas:

| Fase | Qué pasa | Tu papel |
|---|---|---|
| 1 | Identifica EL paso candidato de tu `flujo.md` (uno solo) | Confirmar o corregir |
| 2 | Te exige las cuatro paradas con valores concretos | Dar números: "máx 5 iteraciones o $8 MXN", no "las que necesite" |
| 3 | Escribe la especificación en `proyectos/<tu-proyecto>/loop.md` | Revisar que verificación y trabajo sean pasos SEPARADOS |
| 4 | Corre el loop UNA vez contra un caso real de tus evals | Mirar la traza que queda en `trazas/` |
| 5 | Te pregunta: ¿salió por donde esperabas? | Aplicar UN ajuste y volver a correr |

---

### Paso 3 — Lee la traza como diagnóstico 📍 EN CLAUDE CODE

La traza dice **por cuál de las cuatro paradas salió** el loop, y cada salida es un diagnóstico distinto:

| Salió por… | Significa |
|---|---|
| Presupuesto | Tu condición de éxito probablemente es inalcanzable, no estricta |
| No-progreso en la primera revisión | Tu verificación no está midiendo nada útil |
| Éxito en la primera vuelta | No necesitabas un loop — necesitabas un prompt |
| Escalamiento con un caso que SÍ estaba en alcance | Tu disparador está mal calibrado |

Un ajuste a la vez — el que más mueva la aguja. Corre de nuevo. Y commit después de cada corrida, porque la traza es evidencia: es lo que hace que la Sesión 4 pueda comparar contra algo en vez de empezar de cero.

```bash
git add proyectos/
git commit -m "loop: primera traza - salio por <parada>"
git push
```

---

## Sesión 4 — Gobernanza: qué pasa cuando se rompe a las 3 AM

En la Sesión 1 diseñaste el sistema. En la 2 mediste su decisión de IA. En la 3 lo hiciste **correr solo**. La Sesión 4 hace la pregunta que decide si eso vive en producción o se cae el martes: **¿qué pasa cuando falla y nadie está viendo?**

Un loop que corre solo, sin gobernanza, no es un sistema — es un riesgo con buena demo. Manda el correo antes de que alguien lo apruebe. Gasta sin techo. Y cuando falla, la falla es invisible. Hoy le pones los guardrails que le faltan, y —por primera vez— sacas su **tasa de acierto real**: no la de escritorio de la Sesión 2, sino la del sistema corriendo end-to-end.

La idea central viene de la doctrina (`CLAUDE.md` §3, §7, §13) y de lo que Anthropic aprendió conteniendo a Claude en producción (`referencias/gobernanza.md`): **el límite duro va en el entorno, no en el prompt.** El modelo es probabilístico; nunca es la última línea de defensa.

Dos hechos que lo aterrizan, ambos verificados:

- La telemetría de Claude Code mostró que los usuarios aprobaban **~93%** de los prompts de permiso — sin leerlos. El human-in-the-loop en cada paso **no escala**: se vuelve un clic reflejo. Resérvalo para lo irreversible.
- En un red-team interno (feb 2026), un prompt malicioso logró que el agente exfiltrara credenciales **24 de 25 veces**. La instrucción llegó *tecleada por el usuario*, así que no había nada anómalo que un clasificador atrapara. Lo único que la habría detenido: el entorno — egress y límites de archivo.

Gobernar no es enumerar cada acción mala. Es **acotar lo que el sistema puede alcanzar**. Cuatro palancas, y hoy configuras las cuatro:

| # | Palanca | La pregunta que responde |
|---|---|---|
| 1 | **Permisos mínimos** | ¿Qué es lo mínimo que necesita tocar — y por qué no más? |
| 2 | **Tope de gasto** | ¿Cuánto puede gastar antes de que alguien lo detenga? |
| 3 | **Human-in-the-loop** | ¿Qué acción irreversible NO se dispara sin un humano? |
| 4 | **Observabilidad** | Cuando falle, ¿cómo me entero — hoy, no en el reporte del mes? |

---

### Paso 0 — Trae `PLANTILLA-gobernanza.md` a TU repo 📍 EN LA TERMINAL

`/auditar` ya lo tienes (es comando base, viene desde que creaste tu repo). Lo único nuevo de esta sesión es la plantilla. Parado en tu repo (`pwd` para ubicarte, `cd` a tu carpeta):

```bash
ls proyectos/     # ¿ya aparece PLANTILLA-gobernanza.md?
```

Si aparece, salta al Paso 1. Si NO, tráela:

```bash
curl -o proyectos/PLANTILLA-gobernanza.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-gobernanza.md
```

Confirma que `/auditar` sí está y guarda la plantilla:

```bash
ls .claude/commands/          # debe aparecer auditar.md
ls proyectos/                 # debe aparecer PLANTILLA-gobernanza.md

git add proyectos/PLANTILLA-gobernanza.md
git commit -m "sesion 4: plantilla de gobernanza"
git push
```

Si `auditar.md` NO aparece (creaste tu repo muy temprano), tráelo igual:

```bash
curl -o .claude/commands/auditar.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/.claude/commands/auditar.md
```

**Importante:** si Claude Code estaba abierto, ciérralo (`Ctrl+C`) y vuelve a entrar (`claude`).

---

### Paso 1 — Corre `/auditar`: qué parte de tu sistema es teatro 📍 EN CLAUDE CODE

No empieces tapando huecos al azar. Primero deja que la auditoría te diga **cuáles son**. Este comando no está para ayudarte — está para encontrar lo que un cliente encontraría primero.

```
/auditar
```

Lee todo tu `proyectos/<tu-proyecto>/` y te devuelve máximo cinco hallazgos, ordenados por lo que más te va a doler: dónde falta tope de gasto, qué acción irreversible corre sin humano, dónde no hay forma de enterarte de una falla, dónde pagas IA por algo que era un `if`. Queda escrito en `proyectos/<tu-proyecto>/auditoria-<fecha>.md`.

**Esa lista es tu plan de trabajo del resto de la sesión.** Cada hueco que encontró, lo cierras en los pasos que siguen.

```bash
git add proyectos/
git commit -m "sesion 4: auditoria - N huecos de gobernanza"
git push
```

---

### Paso 2 — Permisos mínimos y tope de gasto 📍 EN CLAUDE CODE

Copia la plantilla a tu proyecto y llena las dos primeras secciones:

```
cp proyectos/PLANTILLA-gobernanza.md proyectos/<tu-proyecto>/gobernanza.md
```

**Permisos mínimos (§1).** Por cada credencial o recurso que toca tu sistema: ¿es el mínimo que le permite trabajar? Solo-lectura donde se pueda. Un agente con acceso de solo-lectura se puede soltar mucho más ancho que uno que escribe a prod. Y la regla de la allowlist: cada dominio o API que permites no es un destino, es un **grant de capacidad** — anota qué puede hacer con cada uno.

**Tope de gasto (§2).** Un loop con API de pago es una factura sin techo hasta que le pones un número. Tres límites: gasto en la consola de tu proveedor, tokens por corrida, e iteraciones por corrida. Este último ya lo tienes: es la parada de *presupuesto* de tu `loop.md` de la Sesión 3 — cópiala aquí con el mismo número.

> El monto real y dónde se configura en la consola: **ábrela y verifícalo ahora**, no lo pongas de memoria. Ese número en tu commit es el entregable.

```bash
git add proyectos/
git commit -m "sesion 4: permisos minimos y tope de gasto"
git push
```

---

### Paso 3 — Human-in-the-loop y Error Trigger 📍 EN CLAUDE CODE

Aquí cierras las dos secciones que más pesan en la auditoría.

**Human-in-the-loop (§3).** No en cada paso — eso es el clic reflejo del 93%. Solo antes de lo **irreversible**: mandar, cobrar, borrar, publicar. Por cada una: ¿requiere aprobación?, ¿quién aprueba?, ¿por qué canal?, ¿qué ve antes de decir que sí? Esto extiende la tabla de escalamiento que ya escribiste en `loop.md` — no la dupliques, complétala.

**Error Trigger + observabilidad (§4).** Si no te enteras de que falló, no lo manejas. Por cada modo de falla (API caída, LLM fuera de esquema, presupuesto tocado, no-progreso): ¿cómo te enteras, por qué canal, con qué umbral? El mínimo viable es un disparador de error que mande una alerta a un canal que sí revisas.

```bash
git add proyectos/
git commit -m "sesion 4: human-in-the-loop y error trigger configurados"
git push
```

---

### Paso 4 — La tasa de acierto REAL 📍 EN CLAUDE CODE

El número de la Sesión 2 medía **un prompt**, en el escritorio. Ahora tu sistema corre solo, con guardrails. Córrelo end-to-end contra tus 10 evals y mide qué acierta **como sistema**:

```
/eval
```

Va a ser más baja que la de escritorio. Eso es lo correcto y lo honesto — un prompt de 9/10 puesto a correr solo pierde en las junturas: se queda sin contexto, declara terminado antes de tiempo, escala algo que no debía. Ese número real es lo que llevas al Demo Day. Anótalo como la primera traza real en tu `loop.md`.

```bash
git add proyectos/
git commit -m "sesion 4: tasa de acierto real del sistema - X/10"
git push
```

---

### Paso 5 — Mejora continua: arregla UNA cosa con evidencia 📍 EN CLAUDE CODE

Toma **un** eval que falló en la corrida real. No lo arregles a ojo — haz análisis de error: ¿por qué falló exactamente? ¿capa, dato, regla ambigua? (la escalera de la Sesión 2 sigue aplicando). Arregla el prompt, y **vuelve a correr `/eval`**:

```
/eval
```

Compara antes/después. Puede que subas el número — o que arregles ese caso y rompas otros dos. Cualquiera de las dos es la lección: **cada vez que tocas un prompt, corren los evals.** Esa es la disciplina que separa mejorar un sistema de rezarle. Desde hoy no toques un prompt sin correr la suite.

```bash
git add proyectos/
git commit -m "sesion 4: postmortem + fix con evals antes/despues"
git push
```

Al final de la noche tu `git log` cuenta la historia de la robustez: la auditoría → los permisos → los guardrails → el número real → la primera mejora medida. Eso es un sistema, no una demo.

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
| `/loop` | Diseña y ejecuta un loop de trabajo-verificación con cuatro paradas explícitas |

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

## Sesión 5 · Multiagente a juicio — "Pruébame que merecías existir"

Última sesión. La pregunta de la S1 ("¿qué vale la pena automatizar y cómo sabré que funcionó?") vuelve aplicada a la arquitectura: **¿este multiagente valió su sobrecosto, de verdad?** No admiramos multiagente; lo ponemos a juicio contra un solo agente al mismo presupuesto de tokens, y dejamos que la traza dicte el veredicto.

**Pasos:**

1. **Prework.** Lee el debate fundacional —Anthropic *"How we built our multi-agent research system"* (a favor) y Cognition *"Don't Build Multi-Agents"* (en contra)— con una pregunta: ¿tu tarea es paralela/independiente o secuencial/dependiente? Setup único del template `nest-a-juicio` (clona-y-corre + LangSmith). Trae tu baseline de un solo agente y tu tarea candidata. Detalle en `referencias/multiagente.md`.
2. **Audita tu apuesta (Paso 0).** Aplica la prueba de forma a tu tarea candidata. Si es secuencial, ya intuyes el veredicto; igual lo pruebas con datos.
3. **Corre el nest.** Lanza tu tarea en el multiagente jerárquico; deja que LangSmith capture la traza.
4. **Lee la traza.** En LangSmith/LangGraph: tokens por agente, costo total, latencia, trabajo redundante, agentes esperando en fila.
5. **Corre el baseline.** El mismo problema con un solo agente, **mismas herramientas y mismo presupuesto de tokens**.
6. **Emite el veredicto.** ¿El nest ganó lo suficiente para justificar los N× tokens? Escribe el número y adjunta las dos trazas. Un multiagente que pierde contra el baseline es un hallazgo válido —y el más valioso.

**Recuerda la aritmética:** 5 agentes al 95% de confiabilidad individual dan 77% de sistema. Cada subagente que agregas es otro eslabón en tu `0.95^n`.

**Doctrina aplicable** (por nombre, no por número): *workflow vs agente*, *elige el que duele*, *medir en vez de opinar*.

**Entregable:** la traza de tu nest + tu veredicto costo-beneficio contra el baseline.

