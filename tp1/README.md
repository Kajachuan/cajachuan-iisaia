# TP 1 — Registro hostil
Un formulario de registro en dos pasos donde cada dato exige una mecánica deliberadamente incómoda: contraseña por código Morse, fecha en tragamonedas, país mediante una máquina de garra arcade sobre 195 valijas y un contrato de términos auditado por velocidad de lectura. Funciona de punta a punta y completarlo demanda una paciencia absurda, que era exactamente el objetivo.

## Cómo se ejecuta
Doble click en index.html. Un solo archivo, sin dependencias ni librerías externas.

## Qué me propuse construir
Una experiencia de Bad UI que no rompiera la funcionalidad, sino que trasladara mecánicas físicas y anacrónicas al navegador para entorpecer cada interacción básica: el teclado se anula para la clave, el calendario se vuelve azaroso, la lista desplegable se convierte en un depósito aduanero navegable celda a celda y el scroll de términos castiga a quien intente saltearlo. Salió en dos prompts iterativos dentro de una misma conversación.

## Decisiones que tomé yo
**DOM puro en vez de Canvas o mapas vectoriales pesados.** La máquina de garra mueve un viewport sobre una grilla de más de 200 celdas HTML reales con banderas generadas por código de puntos regionales. Todo el estado (coordenadas, celdas ocupadas y posiciones relativas) es inspeccionable directamente desde las herramientas de desarrollo sin capas opacas de renderizado.

**La máquina de garra en lugar del mapa SVG.** La primera aproximación con polígonos geográficos resultaba imprecisa y poco interactiva. La sustitución por una grilla oculta de 15 columnas con cámara móvil replica la torpeza de un arcade noventero: obliga a desplazarse a ciegas o depender de un sonar con enfriamiento temporal para ubicar el país buscado.

**Margen de fallo administrativo (15%).** Incluso alineando la garra sobre la valija correcta, existe una probabilidad fija de que el mecanismo suelte el documento. Introduce la frustración clásica de las máquinas de feria sin bloquear de forma definitiva el avance, ya que la valija permanece en su lugar para un reintento.

**Auditoría estricta de lectura en términos y condiciones.** No alcanza con llegar al final del contenedor. El validador cruza tres variables en tiempo real: velocidad de desplazamiento (máximo 900 px/s), progresión geométrica acumulada y un tiempo mínimo de permanencia de 35 segundos. Si detecta un arrastre brusco de la barra, devuelve el scroll a cero y reinicia el cronómetro.

**Persistencia entre pantallas sin recarga.** Si el usuario supera el formulario, llega a los términos y decide retroceder mediante "Volver al formulario", el estado en memoria preserva intactos el usuario, la clave Morse acumulada, la fecha detenida en los rodillos y el país confirmado.

## Qué salió mal y cómo lo corregí
El primer intento dependía de un mapa SVG interactivo con países delimitados manualmente. La interacción era tosca: la escala no permitía seleccionar naciones pequeñas con facilidad, los límites territoriales eran difíciles de calibrar en pantalla y la experiencia general se sentía como un bug de interfaz más que como una mecánica lúdica deliberada.

La corrección consistió en descartar el mapa por completo en el segundo prompt y plantear la "Aduana internacional": un depósito matricial de 195 países identificables con emojis dinámicos e indicadores de proximidad. Esto transformó un problema de puntero en una mecánica de exploración espacial intencionalmente engorrosa.

A nivel de especificación, fue indispensable explicitar el bloqueo de atajos de navegación (como evitar que la barra espaciadora hiciera scroll de página mientras se transmitía Morse) y blindar la máquina de garra para que el teclado (Arrow keys) no interfiriera con los controles si el foco estaba puesto sobre otros campos.

## Prompts
El registro completo está en [prompts.md](prompts.md). Los dos hitos centrales fueron:

1. **Prompt inicial:** Estructuró la arquitectura de la aplicación en memoria, el manipulador temporal del telégrafo Morse, la sincronización de rodillos independientes para la fecha y el algoritmo de penalización por scroll acelerado en el contrato legal.

2. **Prompt de refactorización aduanera:** Eliminó el SVG geográfico e introdujo la máquina de garra de 195 países con viewport restringido de 5x4, sistema de sonar por cuadrantes y el ciclo de captura, confirmación o extravío administrativo.