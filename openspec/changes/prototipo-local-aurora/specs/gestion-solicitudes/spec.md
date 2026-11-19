# Gestión de solicitudes

## Purpose

Da a Comercial Aurora una bandeja para registrar y seguir cada solicitud de servicio, desde su recepción hasta el cierre.

## ADDED Requirements

### Requirement: Radicación validada
El formulario SHALL exigir un cliente del directorio, un canal, un tipo y una descripción de al menos diez caracteres útiles. Una referencia de pedido SHALL ser opcional.

#### Scenario: Radicar una solicitud válida
- **WHEN** se guarda una solicitud de un cliente encontrado por NIT o razón social, con canal de llamada, correo o mensajería y un tipo del catálogo
- **THEN** la bandeja muestra el ticket único e inmutable en formato `AUR-YYYYMM-XXXX`, la fecha y hora de recepción, el agente asignado y el estado Nuevo
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

### Requirement: Bandeja e historial en una sola vista
La interfaz SHALL permitir registrar, buscar, filtrar y consultar solicitudes desde una única vista interna adaptable. El detalle SHALL mostrar el historial cronológico de radicación, cambios de estado y cierre.

#### Scenario: Localizar una solicitud y consultar su historial
- **WHEN** se busca por ticket, NIT o razón social y se aplica un filtro de estado, agente o plazo
- **THEN** la bandeja muestra solo las coincidencias y permite abrir el detalle y su historial sin cambiar de perfil

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

### Requirement: Medición al resolver
Al resolver un caso, la aplicación SHALL conservar la fecha de resolución, las horas corridas transcurridas y el resultado de cumplimiento del plazo.

#### Scenario: Resolver en el límite o después
- **WHEN** se resuelve un caso exactamente a las 48 horas corridas
- **THEN** queda registrado como resuelto dentro del plazo
- **AND** una resolución posterior a ese límite queda registrada fuera del plazo

#### Scenario: Consultar un caso resuelto
- **WHEN** se revisa después un caso resuelto o cerrado
- **THEN** se conserva el resultado original de cumplimiento y ya no se muestran alertas de caso abierto
