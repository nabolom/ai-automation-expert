# Ejercicio final S5 — Nest vs. Swarm

> **El objetivo no es usar más agentes. Es elegir la menor arquitectura que permite que la información circule como el caso lo necesita.**

## 1. Primero descarga el ejercicio

Abre tu **Terminal** y copia estas tres líneas, una sola vez y en este orden:

```bash
git clone https://github.com/nabolom/nest-a-juicio.git
cd nest-a-juicio
bash scripts/empezar.sh
```

`git clone` descarga el ejercicio; `cd` entra a la carpeta; `bash scripts/empezar.sh` valida que Claude Code esté listo. Este último comando **no ejecuta agentes todavía**.

Si aparece `OK`, continúa. Si pide autenticación, ejecuta `claude auth login` y vuelve a correr `bash scripts/empezar.sh`.

---

## 2. Después lee el caso en el README

Abre el archivo **README.md** que está en tu carpeta `nest-a-juicio` o léelo directamente en GitHub: https://github.com/nabolom/nest-a-juicio

Ahí encontrarás el caso completo de **Cobranza Pro**: Producto está listo, Ventas prometió acceso a 40 clientes, Riesgo bloquea activación automática para 12 clientes regulados y Operaciones solo alcanza 15 inscripciones manuales en una semana.

No necesitas escribir `cat` ni abrir archivos desde la Terminal para entender el caso. El README lo presenta en lenguaje normal antes de pedirte ejecutar nada.

---

## 3. Entiende los dos patrones y haz una predicción

| Patrón | Qué hace |
|---|---|
| **Nest** | Cuatro especialistas reportan a un líder. El líder integra y decide. |
| **Swarm** | Los agentes comparten tareas y pueden enviarse mensajes directos entre sí. |

Después de leer el caso, responde:

> “Para Cobranza Pro elegiría [Nest / Swarm] porque los especialistas [sí / no] necesitan comunicarse directamente para resolver las tensiones entre Ventas, Riesgo y Operaciones.”

---

## 4. Ejecuta el Nest — todos los alumnos

En la Terminal, dentro de la carpeta `nest-a-juicio`, ejecuta:

```bash
bash scripts/correr-nest.sh
```

Verás cuatro delegaciones: Producto, Ventas, Riesgo y Operaciones. El líder recibirá esos reportes, decidirá si lanzar, hacer un piloto o posponer y producirá un plan de 72 horas.

La corrida es válida cuando aparece:

```text
NEST COMPLETADO: 4/4 reportes recibidos.
```

Guarda una captura del transcript y escribe `/exit`. No necesitas usar `/usage` ni calcular costos.

---

## 5. Observa el Swarm — demo del facilitador

Tu facilitador demostrará el mismo caso con un Agent Team. Observa si la comunicación directa entre Riesgo, Ventas y Operaciones resuelve algo que un líder no podría resolver con cuatro reportes claros.

Agent Teams es experimental. Si no aparece, el facilitador usará el plan B del repositorio; no se simulan resultados.

---

## 6. Entrega tu decisión arquitectónica

En la Terminal, copia la tarjeta:

```bash
cp tarjetas/DECISION-ARQUITECTURA.md mi-decision-arquitectonica.md
```

Completa la tarjeta con evidencia del Nest y de la demo Swarm. Tu conclusión puede ser **un solo agente**, **Nest** o **Swarm**.

> Si eliges Nest, no es una respuesta menor: significa que evitaste complejidad innecesaria.

Para el recorrido completo, los diagramas, el caso, la tarjeta y la guía de facilitación, consulta el [README de `nest-a-juicio`](https://github.com/nabolom/nest-a-juicio).

## Referencias

[1]: https://code.claude.com/docs/en/agent-teams "Claude Code Docs — Agent teams"
