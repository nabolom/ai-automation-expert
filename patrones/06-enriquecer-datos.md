# Patrón 06 — Enriquecer datos

**Tomar un registro incompleto, completarlo con fuentes externas o IA, y escribirlo de vuelta.** Leads, contactos, productos, empresas.

## Estructura

```
Trigger (nuevo registro / lote programado)
   ↓
Loop Over Items                ← procesa en lotes, NO todo de golpe
   ↓
Enriquecer:
   ├─ APIs externas (Clearbit, Hunter, LinkedIn, scrape)
   └─ LLM (normalizar, categorizar, resumir, inferir)
   ↓
Validar                        ← ¿el dato es plausible?
   ↓
├─ Confianza alta  → escribir en CRM/DB
└─ Confianza baja  → cola de revisión humana
   ↓
Registrar qué se enriqueció y de dónde salió
```

## Batch API: aquí es donde vive el dinero

Este patrón casi nunca es urgente. Enriquecer 5,000 leads puede tardar 6 horas y a nadie le importa.

**Batch API = 50% de descuento.** Sin diferencia de calidad, solo de tiempo. Si estás enriqueciendo en tiempo real un lote de registros, estás tirando la mitad del presupuesto por nada.

Ver `referencias/claude-api.md`.

## Procedencia: el campo que salva el proyecto

Por cada campo enriquecido, guarda **de dónde salió** y **cuándo**.

```json
{
  "industria": "fintech",
  "industria_fuente": "llm_inferido",
  "industria_confianza": "media",
  "industria_fecha": "2026-07-13"
}
```

Sin esto, en tres meses nadie sabe qué datos del CRM son reales y cuáles se los inventó un modelo. Y una vez que se mezclan, **no se pueden separar**. El CRM queda envenenado y no hay vuelta atrás.

Esto no es opcional. Es la diferencia entre enriquecer una base de datos y contaminarla.

## Nunca sobrescribas un dato humano

Si el campo ya tiene un valor puesto por una persona, **no lo pises**. Escribe en un campo paralelo o mándalo a revisión.

Un enriquecedor que sobrescribe datos verificados es una máquina de destruir información, y el daño se descubre semanas después.

## Idempotencia

El workflow se va a correr dos veces sobre el mismo registro. Garantizado: un reintento, un trigger duplicado, alguien que le da al botón.

Diseña para que correrlo dos veces no haga daño. Marca los registros procesados. Verifica antes de escribir.

## Costo

Haiku para normalizar y categorizar. Batch. **Y pon un tope de gasto**: un enriquecedor con un bug de loop sobre 50,000 registros es una factura que no quieres explicar.
