# Datos locales

## Purpose

Permite evaluar el flujo con datos ficticios y conservar el trabajo de una sesión mediante archivos que el usuario controla.

## ADDED Requirements

### Requirement: Apertura local y demostración
El prototipo SHALL funcionar desde un único archivo HTML abierto con doble clic en un navegador instalado. Al iniciar una sesión nueva, SHALL incluir clientes y solicitudes ficticios.

#### Scenario: Probar desde el primer inicio
- **WHEN** se abre el archivo local sin conexión en una sesión nueva
- **THEN** se pueden usar el formulario, la bandeja y el historial
- **AND** hay casos abiertos de ejemplo para En plazo, Atención y Vencida, y un caso resuelto con historial

### Requirement: Importación del directorio de clientes
La aplicación SHALL importar un archivo JSON con un arreglo de clientes que tengan NIT y razón social como textos. SHALL agregar clientes por NIT, actualizar la razón social de las coincidencias y mantener los tickets existentes.

#### Scenario: Importar clientes válidos
- **WHEN** se importa un directorio con un NIT nuevo y uno ya conocido
- **THEN** el formulario permite seleccionar el cliente nuevo y muestra la razón social actualizada del cliente conocido

#### Scenario: Detectar un directorio inválido
- **WHEN** el archivo tiene una estructura inválida, carece de campos requeridos o repite NIT tras la normalización
- **THEN** la aplicación explica el error y conserva el directorio y las solicitudes vigentes

### Requirement: Copia y recuperación de sesión
La aplicación SHALL exportar a JSON los clientes, solicitudes, agentes, historial y datos necesarios para continuar la secuencia de tickets y el turno de asignación. SHALL validar un respaldo completo antes de ofrecer reemplazar la sesión activa.

#### Scenario: Continuar con un respaldo válido
- **WHEN** en una nueva sesión se selecciona un respaldo válido y se confirma su restauración
- **THEN** se recuperan los tickets, estados e historial con sus fechas originales y los nuevos registros continúan la numeración y la rotación
- **AND** las alertas reflejan la hora actual

#### Scenario: Conservar la sesión ante un error o cancelación
- **WHEN** se elige un respaldo incorrecto o se cancela el reemplazo
- **THEN** la sesión vigente conserva sus datos sin cambios
