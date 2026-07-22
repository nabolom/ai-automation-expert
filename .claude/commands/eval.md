---
description: Corre los evals del proyecto contra el prompt actual y te da la tasa de acierto
---

Un sistema sin evals "funciona" — hasta que alguien te pregunta cuánto acierta y no tienes un número. Carga la skill `errores-y-evals`.

Tu objetivo: correr los casos de `evals.md` contra el prompt actual y devolver **un número, no una opinión**.

## Qué hacer

**0. Rescate: si falta un prerequisito, constrúyelo — no te bloquees.** Antes de correr necesitas dos cosas: un `evals.md` con casos llenos y un `prompts.md` con al menos un v0. Si falta alguna, NO te detengas a reportarlo: entra en modo rescate, en este orden:

- **Falta `prompts.md`:** el prompt v0 casi siempre ya existe disperso en `proyecto.md`, `reglas.md` o `flujo.md` (la instrucción de la decisión de IA, el formato de salida). Ensámblalo tú, márcalo como v0, escríbelo en `proyectos/<nombre>/prompts.md` y muestra lo que armaste. El v0 NO tiene que ser bueno — tiene que existir para poder medirlo. Un v0 mediocre medido vale más que un v1 perfecto imaginario.
- **Falta `decisiones.md`:** reconstrúyelo desde `reglas.md` y `flujo.md`, clasificando cada paso como if / IA / humano, y confirma con el humano cuál es la decisión de IA a evaluar.
- **Faltan los casos (tabla vacía o incompleta):** NO los inventes tú, pero tampoco pidas los 10 de golpe — eso paraliza. Guía de uno en uno con la receta de `proyectos/GUIA-escribir-evals.md`: propone TÚ un borrador de input realista basado en el proyecto, y que el humano lo corrija y fije la salida esperada. El borrador del input puede ser tuyo; **la salida esperada es siempre del humano.** Con 6 casos fijados (4 típicos, 1 límite, 1 adversarial) ya puedes correr una primera medición honesta — dilo y ofrece correr con lo que hay, marcando la suite como incompleta.

**1. Ubica el proyecto.** Busca en `proyectos/`. Si hay varios, pregunta cuál. Lee su `evals.md` y su `prompts.md` — usa la versión más reciente del prompt (si hay v1, corre v1; si solo hay v0, corre v0). Si el proyecto no tiene `prompts.md`, dilo y ofrece escribir un v0 mínimo desde `decisiones.md`/`reglas.md` antes de correr (paso 0). **Nunca corras contra un prompt vacío o inexistente** — una tasa medida contra nada es un número inventado.

**2. Corre cada caso.** Pasa cada `input` de `evals.md` por el prompt actual, tal como lo haría el sistema en producción. Saca la salida real.

**3. Compara contra la verdad, sin preguntarte a ti mismo.** La salida esperada ya está escrita en `evals.md`, fijada por un humano. Compara el campo `resultado` —no "se ve bien", el campo— contra esa verdad. Marca ✅ o ❌. **Nunca regeneres la salida esperada ni te preguntes si acertaste: eso invalida la medición.**

**4. Llena y cuenta.** Agrega una columna con la versión del prompt (`v0`, `v1`…) y sus ✅/❌. No sobrescribas la corrida anterior: el valor está en el diff entre versiones. Calcula la tasa = aciertos / total.

**5. Diagnostica los fallos, pero no los arregles.** Por cada ❌, di en qué nivel de la escalera cae, en orden: regla ambigua → falta ejemplo límite → era un `if` → el modelo. Nómbralo. La decisión de qué cambiar es del humano, no tuya.

## Reglas

**Verificación externa, siempre.** La verdad vive en `evals.md`, no en tu juicio. Si para saber si un caso acertó tendrías que opinar, el eval está mal escrito — dilo, no lo adivines.

**No inventes salidas esperadas. Nunca.** La salida esperada la fija el humano — esa es la línea que no cruzas. Los borradores de input sí puedes proponerlos (modo rescate), siempre que el humano los valide como realistas. Si `evals.md` tiene menos de 10 casos, o le faltan los de límite y adversariales, puedes correr igual si el humano lo pide — pero marca la tasa como parcial (`6/6 casos, suite incompleta`) y déjalo escrito en el archivo.

**Un prompt, una columna.** Cada versión del prompt deja su propia columna. Correr `/eval` después de cada cambio no es opcional: es la única forma de cachar una regresión antes de que la cache producción.

**Condición de salida.** Los casos puntuados y la tasa escrita en `evals.md`. Ahí acabas — ni sigues optimizando ni te pones a arreglar prompts.

## Escribe

Actualiza `proyectos/<nombre>/evals.md`: la columna de resultados de esta corrida y la tasa. No toques la salida esperada de ninguna fila.

## Antes de cerrar

Di la tasa en una línea. Y señala el eval que más **duele** —el fallo cuyo costo en producción es más alto— para orientar por dónde atacar primero. Recuérdale: si toca el prompt, vuelve a correr `/eval`.
