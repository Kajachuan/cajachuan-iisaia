# Prompts — TP 2

El registro del proceso, en orden. Tres prompts en una sola conversación. El contrato quedó terminado en el tercero.

---

## 1 — Prompt inicial

```text id="h4r8jk"
Necesito un openapi.yaml (3.1) para una API bancaria de tarjetas de crédito y débito.

recursos:
  Card {
    id,
    card_number,
    card_type,
    status,
    expiration_date,
    account_id
  }

  CreditCard {
    id,
    card_number,
    status,
    expiration_date,
    account_id,
    credit_limit,
    available_credit
  }

  DebitCard {
    id,
    card_number,
    status,
    expiration_date,
    account_id,
    available_balance
  }

valores posibles:
  card_type: CREDIT | DEBIT
  status: ACTIVE | BLOCKED | EXPIRED

endpoints:
  GET    /cards                         → 200 lista de tarjetas
  GET    /cards/{cardId}                → 200 tarjeta / 404 si no existe
  POST   /cards                         → 201 / 400 si faltan datos obligatorios
  PATCH  /cards/{cardId}/status         → 200 / 400 si el estado no es válido / 404 si no existe

Para crear una tarjeta, el cliente debe indicar el tipo de tarjeta y la cuenta
asociada, pero no debe enviar el id de la tarjeta porque lo genera el servidor.

Las tarjetas de crédito y débito tienen campos diferentes.
No quiero un único schema con todos los campos opcionales: modelá correctamente
las diferencias entre ambos tipos.

El número de tarjeta que devuelve la API no debe exponer el PAN completo.
En las respuestas debe devolverse enmascarado, por ejemplo:

**** **** **** 1234

Definí schemas de entrada y salida separados cuando corresponda.
```

**Qué buscaba:** fijar desde el comienzo el dominio general de la API y, principalmente, que crédito y débito no terminaran modelados como una única estructura llena de campos opcionales. También dejé explícita la diferencia entre los datos que envía el cliente y los que genera o devuelve el servidor.

Agregué el enmascaramiento del número de tarjeta porque, tratándose de una API bancaria, no quería que el contrato expusiera el PAN completo en las respuestas. La primera versión debería resolver los cuatro endpoints, los estados posibles y la separación entre tarjetas de crédito y débito.

---

## 2 — Agregar filtros y paginación

```text
Modificá GET /cards para que no devuelva todas las tarjetas sin restricciones.

Agregá filtros opcionales coherentes con el dominio:

- account_id
- card_type: CREDIT | DEBIT
- status: ACTIVE | BLOCKED | EXPIRED
- last_four_digits
- expiration_from
- expiration_to

También agregá paginación con:

- page
- page_size

page_size debe tener un máximo de 100 y un valor por defecto de 20.

La respuesta ya no debe ser solamente un array. Quiero una respuesta paginada con:

{
  items,
  page,
  page_size,
  total_items,
  total_pages
}

Permití combinar los filtros entre sí.

No permitas buscar por el número completo de tarjeta.
last_four_digits debe aceptar exactamente 4 dígitos.

Documentá los parámetros, sus tipos, restricciones y ejemplos en el OpenAPI.
No cambies el comportamiento de los demás endpoints.
```

**Qué buscaba:** evitar un `GET /cards` que potencialmente intentara devolver todas las tarjetas del banco y hacer que la búsqueda fuera utilizable en un escenario real. Elegí filtros sobre atributos razonables para localizar conjuntos de tarjetas sin exponer información sensible.

También agregué paginación porque los filtros por sí solos no garantizan resultados pequeños. El límite máximo de `page_size` evita que el cliente pueda pedir volúmenes arbitrariamente grandes.

Para búsquedas relacionadas con el número de tarjeta, pedí únicamente `last_four_digits` y prohibí explícitamente utilizar el PAN completo como criterio de consulta.

---

## Conversación completa

Una sola conversación, sin reiniciar el hilo. El yaml final tiene 4 endpoints repartidos en 3 paths.