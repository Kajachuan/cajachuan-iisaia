# Prompts — TP 1

El registro del proceso, en orden. Dos prompts en una sola conversación de Gemini Canvas. El artefacto quedó terminado en el segundo.

---

## 1 — Prompt inicial

```
Construí una experiencia de registro deliberadamente frustrante siguiendo el concepto de “Bad UI”.
La aplicación tiene dos pantallas consecutivas:

Formulario de registro.
Lectura y aceptación de términos y condiciones.

Estructura general:
- Una sola página que simula la navegación entre ambas pantallas sin recargar.
- Un <header> con el título “Registro de usuario” y un indicador de progreso:
  “Paso 1 de 2: Datos personales”.
  “Paso 2 de 2: Términos y condiciones”.
- Un <main> que muestra únicamente la pantalla correspondiente.
- Un <footer> con una advertencia irónica, por ejemplo: “Este proceso fue diseñado pensando en su paciencia”.

Pantalla 1: datos personales:
El formulario solicita:
- Nombre de usuario.
- Contraseña.
- Fecha de nacimiento.
- País de nacimiento.

Nombre de usuario:
- Es el único campo normal del formulario.
- Usar un <input type="text">.
- Es obligatorio y debe tener al menos 3 caracteres.
- Mostrar el valor ingresado y los mensajes de validación habituales.

Contraseña en código Morse:
La contraseña no puede escribirse con el teclado de manera convencional.
- Mostrar una zona grande titulada “Transmisor Morse”.
- La contraseña se ingresa manteniendo presionado:
  * El botón principal del mouse sobre esa zona.
  * O la barra espaciadora.
- Una pulsación de hasta 250 ms representa un punto (.).
- Una pulsación de más de 250 ms representa una raya (-).
- Después de 700 ms sin nuevas pulsaciones, interpretar la secuencia como una letra.
- Admitir las letras A-Z y los números 0-9 según el código Morse internacional.
- Si la secuencia no existe, mostrar un error y descartar esa letra.
- La barra espaciadora no debe desplazar la página mientras se utiliza el transmisor.
- Mostrar:
  * La señal actual en Morse.
  * Un indicador visual de cuánto tiempo lleva presionado.
  * La cantidad de caracteres de la contraseña.
  * La contraseña convertida, pero ocultando sus caracteres con •.
  * Un botón “Borrar última señal”.
  * Un botón “Borrar último carácter”.
  * Una tabla desplegable de referencia con el alfabeto Morse.
- La contraseña debe tener al menos 6 caracteres.

Fecha de nacimiento estilo tragamonedas:
La fecha no se escribe ni se elige con un calendario.
- Crear una máquina tragamonedas con tres rodillos:
  * Día: valores del 01 al 31.
  * Mes: nombres de enero a diciembre.
  * Año: desde el año actual hasta 1900.
- Incluir una palanca o botón “Girar”.
- Al presionarlo, los tres rodillos deben comenzar a cambiar rápidamente sus valores.
- Cada rodillo tiene su propio botón “DETENER”.
- El usuario debe detener por separado el día, el mes y el año.
- Los valores finales forman la fecha seleccionada.
- Si la combinación no representa una fecha válida, por ejemplo 31 de febrero, mostrar un error y obligar a volver a girar los tres rodillos.
- No permitir fechas futuras.

País de nacimiento mediante un mapa:
- No usar un <select> ni un campo de texto.
- Mostrar un mapa mundial interactivo construido con elementos SVG dentro del HTML.
- Cada país o región seleccionable debe ser un elemento del DOM.
- Al pasar el mouse sobre un país, mostrar su nombre.
- Al hacer click, marcarlo como seleccionado y mostrar debajo: “País seleccionado: [nombre]”.
- Incluir suficientes países identificables para que el mapa resulte funcional, incluyendo Argentina y el resto de Sudamérica.
- Los países pequeños pueden representarse mediante puntos o zonas ampliadas.
- No permitir avanzar sin seleccionar un país.

Continuar:
- Incluir un botón “Continuar a términos y condiciones”.
- Solo debe estar habilitado cuando:
  * El nombre de usuario sea válido.
  * La contraseña tenga al menos 6 caracteres.
  * La fecha sea válida.
  * Se haya seleccionado un país.
- Al presionarlo, ocultar la pantalla del formulario y mostrar la pantalla de términos y condiciones.
- Conservar en memoria todos los datos ingresados.

Pantalla 2: términos y condiciones:
- Mostrar un contenedor de lectura con altura limitada y scroll propio.
- Los términos deben ser extensos: incluir al menos 15 secciones y suficiente texto para requerir varios desplazamientos completos.
- El contenido puede ser humorístico, pero debe parecer un contrato real (Uso del servicio, Tratamiento de datos, Responsabilidades, Seguridad, Cookies, Comunicaciones, Suspensión de la cuenta, Propiedad intelectual, Modificaciones, Limitaciones de responsabilidad).
- Mostrar arriba del documento:
  * Tiempo de lectura.
  * Porcentaje recorrido.
  * Velocidad actual de desplazamiento.
  * Estado: “Leyendo”, “Demasiado rápido” o “Lectura completada”.

Validación de lectura:
- El botón “Aceptar y confirmar registro” comienza deshabilitado.
- Registrar el tiempo y la posición del scroll dentro del contenedor de términos.
- Considerar que el usuario está desplazándose demasiado rápido si sucede cualquiera de estas condiciones:
  * La velocidad supera los 900 píxeles por segundo.
  * Avanza más de una altura completa del contenedor en menos de 500 ms.
  * Llega al final en menos de 35 segundos.
  * Arrastra directamente la barra de scroll hasta una posición muy avanzada.
- Si se detecta una lectura demasiado rápida:
  * Mostrar un cartel modal: “Detectamos que no está leyendo los términos y condiciones.”
  * Incluir un único botón: “Volver a leer”.
  * Al cerrarlo: volver el contenedor al inicio, reiniciar el tiempo y el porcentaje recorrido, y mantener deshabilitado el botón de aceptación.
- Para considerar la lectura válida:
  * Deben haber transcurrido al menos 35 segundos.
  * El usuario debe haber recorrido progresivamente todo el documento.
  * Debe haber llegado al final sin superar los límites de velocidad.
  * No debe ser posible habilitar el botón modificando solamente la posición final del scroll.
- Al cumplirse las condiciones: cambiar el estado a “Lectura completada” y habilitar “Aceptar y confirmar registro”.
- Incluir un botón “Volver al formulario” que conserve los datos cargados.

Confirmación:
- Al aceptar los términos: reemplazar el contenido principal por una pantalla de confirmación.
- Mostrar “Registro completado”.
- Mostrar un resumen con: nombre de usuario, fecha de nacimiento, país y contraseña oculta con puntos.
- Incluir un botón “Registrar otra persona” que reinicie completamente la aplicación.

Estilo:
- Estética intencionalmente incómoda y anticuada.
- Fondo gris claro, bordes duros y gruesos, tipografía monoespaciada, cero redondeo.
- Botones grandes con apariencia de interfaz de los años noventa y sombras duras.
- Mensajes de error rojos y exagerados.

Estado:
- Mantener en memoria: currentStep, username, morseSequence, password, birthDate, selectedCountry, termsStartTime, lastScrollPosition, lastScrollTime, maximumReadPosition, readingInvalidated, termsAccepted.

Constraints:
- Un solo archivo HTML con CSS en <style> y JS en <script>.
- Vanilla JavaScript, sin frameworks ni dependencias externas.
- No usar <canvas>.
- Mapa SVG con regiones en el DOM.
- No enviar ni almacenar datos reales: todo en memoria.
- Entregar código completo sin omitir partes.
```

**Qué intentaba lograr:** levantar el flujo completo de la aplicación en dos pantallas gobernadas por una máquina de estados en memoria. Establecer la mecánica del telégrafo Morse por medición de intervalos (`keyup` y `keydown` contra 250 ms y 700 ms), los rodillos asíncronos para la fecha, el SVG interactivo y el algoritmo de monitoreo de velocidad angular/lineal de scroll en los términos.

**Qué devolvió:** una primera versión totalmente funcional en un solo archivo con estética brutalista noventera (`#c0c0c0`, biseles duros, tipografía Courier). El telégrafo y la tragamonedas operaron con precisión milimétrica, y la auditoría de lectura castigó correctamente los scrolls veloces devolviendo al usuario al tope del documento.

**Qué hice con eso:** evalué la experiencia y detecté el punto débil: el mapa SVG. Intentar resolver la geografía mundial con polígonos manuales en SVG simplificado volvía la selección excesivamente imprecisa para países de menor superficie y no transmitía una mecánica de juego deliberada, sino un defecto gráfico. Decidí rehacer esa sección por completo.

---

## 2 — Reemplazo del mapa por la máquina de garra arcade

```
Modificá la implementación anterior del formulario de registro. El mapa mundial no funciona correctamente, por lo que debe eliminarse por completo y reemplazarse por una nueva mecánica Bad UI para elegir el país de nacimiento.

Conservá sin cambios:
- El campo normal para el nombre de usuario.
- La contraseña ingresada mediante código Morse.
- La fecha de nacimiento estilo tragamonedas.
- La pantalla de términos y condiciones.
- La detección de scrolling demasiado rápido.
- La pantalla final de confirmación.
- El estilo anticuado y deliberadamente incómodo.

Nuevo selector de país: máquina de garra de pasaportes:
El país de nacimiento debe seleccionarse mediante una máquina de garra inspirada en las máquinas arcade.

Estructura:
- Mostrar una máquina titulada “Aduana internacional”.
- Dentro debe haber un depósito virtual mucho más grande que el área visible.
- El depósito contiene una tarjeta o valija por cada país.
- Cada valija debe mostrar:
  * El emoji de la bandera.
  * El nombre del país en español.
  * Su código ISO de dos letras.
- Incluir los 195 países reconocidos habitualmente, no una selección reducida.
- Los países deben almacenarse en un array de JavaScript dentro del mismo HTML.
- No descargar banderas ni datos desde internet: generar las banderas mediante emojis construidos a partir del código ISO.
- Distribuir las valijas aleatoriamente en una grilla al cargar la página.

Área visible:
- La máquina solamente permite ver una parte pequeña del depósito (grilla de 5 columnas por 4 filas).
- El depósito completo puede tener aproximadamente 15 columnas y las filas necesarias para contener todos los países.
- El resto de las valijas queda fuera del área visible.
- La cámara debe seguir automáticamente a la garra cuando esta se mueve.
- Mostrar en una esquina las coordenadas actuales de la garra: “Fila: X | Columna: Y”.
- No incluir un buscador, un <select> ni una lista convencional.

Controles de la garra:
- Debajo de la máquina, mostrar un panel de control con botones grandes: ↑, ←, BAJAR GARRA, →, ↓.
- La garra comienza sobre una posición aleatoria.
- Cada botón de dirección mueve la garra exactamente una celda (también permitir flechas del teclado).
- La garra no puede salir de los límites de la grilla.
- Durante el movimiento, los botones quedan temporalmente bloqueados.
- Cada movimiento debe tener una animación mecánica de aproximadamente 180 ms.
- Reproducir los sonidos mediante efectos visuales escritos (“CLANK”, “BRRRR”, “ERROR ADUANERO”). Sin archivos de audio.

Radar aduanero:
- Incluir un botón “Consultar radar”.
- Al presionarlo: mostrar durante 3 segundos una lista con los países presentes en las filas cercanas a la garra.
- Informar únicamente el nombre del país y si está arriba, abajo, a la izquierda o a la derecha (sin posición exacta).
- El radar entra en “recalibración” durante 5 segundos con cuenta regresiva.

Captura de una valija:
- Al presionar “BAJAR GARRA”: bloquear controles, animar descenso (aprox. 700 ms), tomar valija y subirla.
- Mostrar cartel de inspección: “La garra capturó: [bandera] [nombre del país]. ¿Este es su país de nacimiento?”
- Botón “Sí, nací ahí”: guardar en selectedCountry, mostrar “País de nacimiento declarado: [bandera] [nombre]”, deshabilitar la máquina y mostrar botón “Solicitar reapertura de frontera”.
- Botón “No, devolver valija”: animar caída, devolverla a una celda vacía al azar, mover la garra a otra posición y mostrar por 2 segundos “Equipaje devuelto. La aduana ha reorganizado parcialmente el depósito.”

Probabilidad de fallo absurda:
- La garra debe tener un 15% de probabilidad de fallar al intentar capturar una valija.
- Al fallar: no se selecciona, muestra “La garra perdió el pasaporte por motivos administrativos”, sacude visualmente la máquina y rehabilita controles tras 1 segundo (la valija permanece para reintento).

Estado necesario:
- countries, countryGrid, clawRow, clawColumn, clawMoving, capturedCountry, selectedCountry, radarCooldown.

Integración con el formulario:
- Botón “Continuar a términos y condiciones” requiere país confirmado.
- Mantener la selección al navegar entre pantallas.
- En el reinicio, volver a mezclar valijas y reubicar la garra al azar.

Estilo visual:
- Arcade años noventa combinado con puesto de aduana antiguo.
- La garra y valijas construidas puramente con DOM y CSS.
- Carteles: “DOCUMENTOS”, “SOLO PERSONAL AUTORIZADO”, “NO GOLPEAR EL VIDRIO”, “LA ADUANA NO SE RESPONSABILIZA POR PAÍSES EXTRAVIADOS”.

Constraints:
- Modificar el único HTML existente, sin <canvas>, sin mapas vectoriales/geográficos y sin red.
- Entregar archivo HTML completo y corregido.
```

**Qué intentaba lograr:** reemplazar el componente problemático por una mecánica arcade que convirtiera la selección de país en un desafío de exploración espacial. Para que sea justa dentro de lo absurdo, introduje el radar direccional, y para reforzar el concepto de Bad UI agregué el 15% de fallo por "trámite administrativo", la animación por pasos mecánicos y los efectos de sonido por carteles textuales ("CLANK", "BRRRR").

**Por qué está escrito así:** el prompt detalla exhaustivamente las dos ramas de resolución del modal de inspección ("Sí, nací ahí" y "No, devolver valija"). Si no se especifica el comportamiento ante el rechazo (devolver la valija a una celda vacía al azar y alejar la garra), los modelos tienden a dejar la valija flotando o a vaciar la celda sin reubicar el país, volviéndolo inalcanzable. También se exigió calcular las banderas con emojis nativos derivados de los puntos de código ISO (`127397 + charCode`), eliminando cualquier dependencia de red.

**Qué devolvió:** el código completo integrado y refactorizado. Generó la matriz de 15x14 celdas en el DOM, sincronizó el movimiento por cámara virtual manteniendo la garra dentro del viewport, implementó las colisiones de borde, la animación de descenso/aprehensión con CSS y el cooldown del sonar aduanero sin romper la persistencia con la pantalla de términos ni el reinicio global.

---

## Conversación completa

Una sola conversación de Gemini Canvas, sin reiniciar el hilo. El artefacto final tiene 1.942 líneas en un único archivo ejecutable.