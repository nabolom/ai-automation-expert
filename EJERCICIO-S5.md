# Ejercicio final S5 — Nest vs. Swarm

> **El objetivo no es usar más agentes. Es elegir la menor arquitectura que permite que la información circule como el caso lo necesita.**

En la S5 no mediremos costo ni tokens. `/usage` muestra información de la sesión y también barras de uso del plan, lo que no es una comparación limpia para cada alumno. En cambio, vamos a observar **cómo se comunica el trabajo** y decidir si el caso necesita un líder que sintetice o agentes que se hablen entre sí.

El repositorio ejecutable es [`nabolom/nest-a-juicio`](https://github.com/nabolom/nest-a-juicio).

---

## 1. Primero entiende el caso

Horizonte quiere lanzar **Cobranza Pro** el 15 de octubre. Producto tiene el sistema listo, Ventas prometió acceso a 40 clientes, Riesgo prohíbe activar automáticamente a 12 clientes regulados y Operaciones solo puede hacer 15 inscripciones manuales en una semana.

Antes de abrir Claude Code, la pregunta es:

> ¿Basta con que cada área reporte su información a un líder, o Riesgo, Ventas y Operaciones necesitan hablarse directamente para negociar el plan?

No hay una respuesta correcta todavía. Esa es la decisión que vas a investigar.

---

## 2. Después entiende los dos patrones

| Patrón | Flujo | Cuándo tiene sentido |
|---|---|---|
| **Nest** | Los especialistas reportan al líder. El líder sintetiza y decide. | Cuando las piezas pueden investigarse por separado y el líder puede recomponerlas. |
| **Swarm** | Los agentes comparten tareas y pueden enviarse mensajes directos entre pares. | Cuando los especialistas necesitan cuestionar, negociar o coordinar entre sí. |

```text
NEST                              SWARM
producto ─────┐                   producto <──> ventas
ventas ───────┼──> líder          riesgo   <──> operaciones
riesgo ───────┤       │              \_______ líder _______/
operaciones ──┘       └──> decisión
```

En Claude Code, el Swarm se implementa como un **Agent Team**, una función experimental que crea sesiones separadas con lista de tareas compartida y comunicación entre compañeros. [1]

---

## 3. Ahora sí: escribe tu hipótesis

Completa esta frase antes de ejecutar:

> “Para Cobranza Pro elegiría [Nest / Swarm] porque los especialistas [sí / no] necesitan comunicarse directamente para resolver las tensiones entre Ventas, Riesgo y Operaciones.”

Tu meta no es acertar. Tu meta es tener una idea explícita que puedas confirmar o cambiar con evidencia.

---

## 4. Todos ejecutan el Nest

Clona y valida el ejercicio:

```bash
git clone https://github.com/nabolom/nest-a-juicio.git
cd nest-a-juicio
bash scripts/empezar.sh
```

Después corre exactamente este comando:

```bash
bash scripts/correr-nest.sh
```

El coordinador abrirá cuatro especialistas: **Producto, Ventas, Riesgo y Operaciones**. Cada uno lee el caso y reporta al líder. El líder toma los cuatro reportes y construye una decisión de lanzamiento con un plan de 72 horas.

**Qué debes observar:** cuatro delegaciones y este cierre:

```text
NEST COMPLETADO: 4/4 reportes recibidos.
```

Guarda una captura del transcript y escribe `/exit`. No necesitas usar `/usage`.

---

## 5. El facilitador demuestra el Swarm

Tu facilitador correrá el mismo caso como un Agent Team. En la demostración, Riesgo, Ventas y Operaciones pueden comunicarse directamente y usar una lista de tareas compartida.

Observa estas tres preguntas:

1. ¿Qué información tuvo que ir directamente de un especialista a otro?
2. ¿Ese intercambio cambió algo que un líder no habría podido resolver con cuatro reportes?
3. ¿La lista de tareas compartida eliminó una confusión real o añadió complejidad?

Agent Teams es experimental. Si no aparece, el facilitador usará el plan B del repo para explicar la diferencia sin simular resultados. [1]

---

## 6. Entrega una decisión arquitectónica

Copia y llena esta tarjeta:

```bash
cp tarjetas/DECISION-ARQUITECTURA.md mi-decision-arquitectonica.md
```

Tu veredicto puede ser **un solo agente**, **Nest** o **Swarm**. Debes defenderlo con evidencia del caso, del transcript del Nest y de la demo.

> Si decides que el Nest basta, no perdiste: evitaste complejidad que este caso no necesitaba.

Para el paso a paso completo y la guía de facilitación: [README de `nest-a-juicio`](https://github.com/nabolom/nest-a-juicio).

## Referencias

[1]: https://code.claude.com/docs/en/agent-teams "Claude Code Docs — Agent teams"
