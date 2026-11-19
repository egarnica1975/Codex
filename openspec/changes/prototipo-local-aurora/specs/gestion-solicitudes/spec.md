# Gestión de solicitudes

## Purpose

Da a Comercial Aurora una bandeja para registrar y seguir cada solicitud de servicio, desde su recepción hasta el cierre.

## ADDED Requirements

### Requirement: Radicación validada
El formulario SHALL exigir un cliente del directorio, un canal, un tipo y una descripción de al menos diez caracteres útiles. Una referencia de pedido SHALL ser opcional.

#### Scenario: Radicar una solicitud válida
- **WHEN** se guarda una solicitud de un cliente encontrado por NIT o razón social, con canal de llamada, correo o mensajería y un tipo del catálogo
- **THEN** al consultar la bandeja del coordinador se muestra el ticket único e inmutable en formato `AUR-YYYYMM-XXXX`, la fecha y hora de recepción, el agente asignado y el estado Nuevo
- **AND** el historial contiene el evento de radicación

#### Scenario: Rechazar un formulario incompleto
- **WHEN** se intenta guardar sin cliente, canal o tipo válido, o con una descripción demasiado corta
- **THEN** la vista señala los campos que requieren corrección y conserva lo ingresado
- **AND** el intento no genera un ticket ni avanza el turno de asignación

### Requirement: Asignación automática
La aplicación SHALL rotar la asignación de solicitudes nuevas entre cuatro agentes ficticios en orden fijo.

#### Scenario: Completar una ronda
- **WHEN** se registran cinco solicitudes válidas consecutivas
- **THEN** las primeras cuatro se asignan a agentes distintos en el orden de rotación y la quinta al mismo agente que la primera

### Requirement: Bandeja e historial por perfil
La aplicación SHALL ofrecer en una sola página adaptable un selector de perfil para simular Agente o Coordinador y, en el perfil Agente, un selector de los cuatro agentes ficticios. La bandeja del agente SHALL limitar la lista, la búsqueda y el acceso al detalle y al historial a sus solicitudes asignadas. El coordinador SHALL poder consultar todas las solicitudes.

#### Scenario: Localizar una solicitud y consultar su historial
- **WHEN** en la vista del coordinador se busca por ticket, NIT o razón social y se aplica un filtro de estado, agente o plazo
- **THEN** la bandeja muestra las coincidencias y permite abrir el detalle y su historial cronológico de radicación, cambios de estado y cierre

#### Scenario: Cambiar de coordinador a agente
- **GIVEN** el coordinador tiene abierto el detalle de un caso asignado a otro agente
- **WHEN** se cambia al perfil Agente y se selecciona un agente sin ese caso
- **THEN** se retira el detalle y la bandeja muestra solo los casos del agente activo
- **AND** buscar por el identificador del caso anterior no permite volver a abrirlo

#### Scenario: Volver a la bandeja completa
- **WHEN** se cambia de Agente a Coordinador
- **THEN** se pueden consultar los casos de todos los agentes y sus alertas de 24 y 48 horas

### Requirement: Seguimiento del estado
La aplicación SHALL ofrecer los estados Nuevo, En proceso, Pendiente logística, Pendiente ventas, Resuelto y Cerrado. Cada transición SHALL añadir un evento con fecha, estado anterior, estado nuevo y comentario cuando corresponda.

#### Scenario: Resolver y cerrar un caso
- **WHEN** un caso abierto se marca como Resuelto con una respuesta de fondo y luego se marca como Cerrado
- **THEN** quedan registrados ambos cambios y la fecha de resolución original permanece visible

#### Scenario: Intentar cerrar un caso abierto
- **WHEN** se intenta cerrar una solicitud que aún está abierta
- **THEN** la aplicación requiere resolverla primero y conserva su estado e historial

### Requirement: Semáforo de 24 y 48 horas
La bandeja SHALL distinguir con texto y color las solicitudes abiertas que llevan menos de 24 horas, de 24 a menos de 48 horas, y 48 horas o más. El cálculo SHALL usar horas corridas y continuar durante las esperas de logística o ventas.

#### Scenario: Alcanzar el umbral de aviso
- **WHEN** una solicitud abierta alcanza las 24 horas corridas
- **THEN** la bandeja pasa de En plazo a Atención

#### Scenario: Alcanzar el umbral de vencimiento
- **WHEN** una solicitud abierta alcanza las 48 horas corridas
- **THEN** la bandeja muestra Vencida, incluso si está en espera de logística o ventas

### Requirement: Distribución de plazos en cuatro barras
La vista del coordinador SHALL mostrar cuatro barras con la distribución de solicitudes abiertas según las horas corridas desde su recepción. Cada barra SHALL identificar su rango y cantidad con texto y color, incluso cuando la cantidad sea cero. Las solicitudes abiertas de 48 horas o más SHALL figurar en un contador separado de vencidas.

#### Scenario: Clasificar cada solicitud abierta en un solo grupo
- **GIVEN** hay solicitudes abiertas con menos de 12 horas, desde 12 hasta menos de 24, desde 24 hasta menos de 26, y desde 26 hasta menos de 48
- **WHEN** se consulta el gráfico del coordinador
- **THEN** cada solicitud se cuenta una sola vez en la barra que le corresponde, sea la verde de 12 h, la amarilla de 24 h, la naranja de 26 h o la roja de 48 h, respectivamente
- **AND** las solicitudes resueltas y cerradas quedan fuera de las barras y del contador de vencidas

#### Scenario: Cambiar de tramo y registrar el vencimiento
- **WHEN** una solicitud abierta alcanza exactamente 12, 24 o 26 horas
- **THEN** pasa al tramo que comienza en ese umbral y al llegar a las 24 horas recibe la alerta Atención en la bandeja

#### Scenario: Una solicitud alcanza las 48 horas
- **WHEN** una solicitud abierta alcanza exactamente 48 horas
- **THEN** sale de las barras, se suma al contador de vencidas y aparece como Vencida en la bandeja

### Requirement: Medición al resolver
Al resolver un caso, la aplicación SHALL conservar la fecha de resolución, las horas corridas transcurridas y el resultado de cumplimiento del plazo.

#### Scenario: Resolver en el límite o después
- **WHEN** se resuelve un caso exactamente a las 48 horas corridas
- **THEN** queda registrado como resuelto dentro del plazo
- **AND** una resolución posterior a ese límite queda registrada fuera del plazo

#### Scenario: Consultar un caso resuelto
- **WHEN** se revisa después un caso resuelto o cerrado
- **THEN** se conserva el resultado original de cumplimiento y ya no se muestran alertas de caso abierto
