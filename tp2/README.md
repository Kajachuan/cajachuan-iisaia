# TP 2 — API de tarjetas de crédito y débito

Un `openapi.yaml` que describe una API bancaria para consultar y administrar tarjetas de crédito y débito. El contrato contempla distintos tipos de tarjeta, estados, búsqueda mediante filtros y paginación. Cuatro operaciones distribuidas en tres paths, sin nada implementado: el entregable es el contrato.

## Cómo se lee

Pegar el contenido de [openapi.yaml](openapi.yaml) en [editor.swagger.io](https://editor.swagger.io). Aparece la documentación navegable del lado derecho, con cada endpoint desplegable, sus parámetros, schemas de entrada y salida y posibles códigos de respuesta.

## Qué me propuse construir

Quería modelar una API sencilla pero cercana a un dominio bancario real. Elegí tarjetas de crédito y débito porque permiten trabajar con recursos que comparten información básica pero que, al mismo tiempo, tienen diferencias importantes que el contrato tiene que poder expresar.

Una tarjeta tiene información común como su identificador, estado, fecha de vencimiento y cuenta asociada, pero una tarjeta de crédito además maneja conceptos como límite y crédito disponible, mientras que una tarjeta de débito tiene otras características.

También quería evitar que el contrato quedara limitado a un `GET /cards` que devolviera indiscriminadamente todas las tarjetas. En un banco eso sería inviable por volumen, por lo que en un segundo paso agregué filtros y paginación.

Salió en dos prompts, dentro de una sola conversación.

## Decisiones que tomé yo

**Separar las tarjetas de crédito y débito en schemas distintos.** Aunque ambos recursos representan tarjetas, no tienen exactamente los mismos campos. No quise resolverlo con un único schema lleno de propiedades opcionales porque eso permitiría combinaciones que conceptualmente no tienen sentido, como una tarjeta de débito con `credit_limit`. La intención fue que el contrato expresara explícitamente las diferencias del dominio.

**Separar schemas de entrada y de salida.** El cliente no tiene que enviar todos los datos que finalmente forman parte del recurso. En particular, el `id` de la tarjeta es generado por el servidor. Por eso la representación utilizada para crear una tarjeta no debería ser exactamente la misma que se devuelve después de crearla o consultarla.

**No exponer el número completo de tarjeta en las respuestas.** El contrato devuelve el número de tarjeta enmascarado, por ejemplo `**** **** **** 1234`. No necesito que un consumidor de esta API reciba el PAN completo para identificar visualmente una tarjeta, y exponerlo agregaría información sensible innecesaria.

**No agregué un endpoint de borrado de tarjetas.** Una tarjeta bancaria no debería desaparecer del sistema como si nunca hubiera existido. Incluso cuando deja de poder utilizarse, sigue teniendo importancia para el historial del banco y puede estar relacionada con operaciones realizadas anteriormente. Por eso no incluí un `DELETE /cards/{cardId}`. El ciclo de vida de una tarjeta se representa mediante su estado: puede bloquearse o vencer y, si en el futuro se incorpora una baja definitiva, debería modelarse también como un cambio de estado y no como un borrado físico del recurso.

**Usar un endpoint específico para modificar el estado.** En vez de permitir modificar libremente todos los datos de una tarjeta, agregué `PATCH /cards/{cardId}/status`. La operación expresa de manera explícita qué parte del recurso puede cambiar a través de ese endpoint y evita convertir un `PATCH /cards/{cardId}` genérico en una puerta para modificar propiedades que no deberían ser editables.

**Agregar filtros al `GET /cards`.** Una consulta que intentara devolver todas las tarjetas de un banco no sería razonable. Incorporé filtros por `account_id`, tipo de tarjeta, estado, últimos cuatro dígitos y rango de vencimiento para poder acotar la búsqueda según datos relevantes del dominio.

**No permitir búsquedas por PAN completo.** Para búsquedas relacionadas con el número de tarjeta solamente permití `last_four_digits`, que debe contener exactamente cuatro dígitos. El número completo no es necesario como parámetro de búsqueda para este contrato y prefiero no convertirlo en un dato que viaje regularmente en una URL.

**Agregar paginación además de los filtros.** Los filtros reducen la cantidad de resultados, pero no garantizan que el conjunto resultante sea pequeño. Por eso `GET /cards` también recibe `page` y `page_size`. El tamaño por defecto es 20 y el máximo permitido es 100, evitando que el caller pueda solicitar una cantidad arbitrariamente grande de registros.

**Devolver metadatos de paginación.** Una vez agregada la paginación, la respuesta dejó de ser simplemente un array de tarjetas. Ahora incluye `items`, `page`, `page_size`, `total_items` y `total_pages`. De esta manera el consumidor sabe dónde se encuentra dentro del conjunto de resultados y cuántas páginas existen.

## Qué salió mal y cómo lo corregí

La primera versión tenía un problema de diseño que no era incorrecto sintácticamente, pero sí poco viable para el dominio: `GET /cards` devolvía una lista de tarjetas sin ningún mecanismo para limitar la búsqueda.

En una API pequeña de ejemplo eso puede parecer suficiente, pero llevado al escenario que estaba modelando implicaría potencialmente consultar todas las tarjetas del banco. El problema no estaba en OpenAPI sino en el contrato que había pedido: el primer prompt definía qué recursos quería consultar, pero no había pensado cómo se localizarían realmente esos recursos cuando la cantidad de datos fuera grande.

Lo corregí en el segundo prompt agregando filtros opcionales por cuenta, tipo, estado, últimos cuatro dígitos y fecha de vencimiento. También agregué paginación porque filtrar no significa necesariamente obtener pocos resultados.

Ese cambio obligó además a modificar la forma de la respuesta. En lugar de devolver directamente un array, `GET /cards` pasó a devolver los resultados dentro de `items` junto con los metadatos necesarios para recorrer las distintas páginas.

Otra decisión que apareció al pensar la búsqueda fue no permitir que el PAN completo se utilizara como filtro. Para los casos en los que sea necesario reconocer una tarjeta, el contrato permite trabajar con los últimos cuatro dígitos. Esto también me obligó a especificar una validación concreta: `last_four_digits` debe tener exactamente cuatro caracteres numéricos.

La regla que me llevo de esta corrección es que diseñar una operación de lectura no significa solamente definir qué recurso devuelve. También hay que pensar cuántos recursos podría devolver, cómo los busca el consumidor y qué límites necesita el contrato para seguir siendo razonable cuando aumenta el volumen de datos.

## Prompts

El registro completo está en [prompts.md](prompts.md).

El primer prompt definió el dominio, los tres paths y las cuatro operaciones principales, además de la separación entre tarjetas de crédito y débito, los schemas de entrada y salida y el enmascaramiento del número de tarjeta.

El segundo corrigió la principal limitación de la consulta general: agregó filtros y paginación a `GET /cards` sin crear nuevos paths ni modificar el comportamiento de las demás operaciones.

El contrato final tiene **4 operaciones distribuidas en 3 paths**:

- `GET /cards`
- `POST /cards`
- `GET /cards/{cardId}`
- `PATCH /cards/{cardId}/status`

Los filtros y la paginación forman parte de `GET /cards`, por lo que no agregan nuevos endpoints al contrato.