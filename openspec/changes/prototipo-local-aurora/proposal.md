# Propuesta de prototipo local para Comercial Aurora

## Why

La especificación de Comercial Aurora estima que se pierden 60 de unas 600 solicitudes mensuales y que el 30 % queda sin responsable. Un prototipo local permitirá comprobar si la asignación automática, las vistas por perfil y las alertas visuales ayudan al equipo a dar seguimiento a los casos antes de invertir en un sistema compartido.

## What Changes

- Entregar un único archivo HTML en español, con CSS y JavaScript integrados, que se pueda abrir con doble clic y probar sin conexión en un navegador instalado.
- Permitir radicar solicitudes recibidas por llamada, correo o mensajería, buscar al cliente por NIT o razón social, generar un identificador y asignar un responsable entre cuatro agentes ficticios por rotación.
- Reunir en una sola página el formulario, la bandeja y el historial. Simular las vistas de agente y coordinador con un selector de perfil. Cada agente consulta sus casos y el coordinador consulta la bandeja completa, con avisos a las 24 horas y vencimientos a las 48 horas corridas.
- Mostrar en la vista de coordinador cuatro barras de distribución por antigüedad, con cortes de 12, 24, 26 y 48 horas, y un contador de casos abiertos vencidos. Registrar el cumplimiento del plazo al resolver.
- Incluir solicitudes y clientes ficticios desde el inicio. Cargar un directorio adicional en JSON y exportar o recuperar la sesión completa en un archivo JSON.
- Considerar el prototipo aceptado cuando se pueda abrir el HTML, radicar y resolver un caso, alternar entre los dos perfiles, comprobar las alertas y el gráfico con ejemplos, y recuperar la sesión a partir del archivo exportado.

## Capabilities

### New Capabilities

- `gestion-solicitudes` — Radicación, asignación, seguimiento, historial y control visual de plazos.
- `datos-locales` — Demostración, importación del directorio JSON y respaldo y recuperación de la sesión.

### Modified Capabilities

## Impact

El entregable será `aurora.html`. La experiencia usa datos en memoria durante la sesión y archivos JSON para conservarlos. El directorio de esta entrega se importa en JSON, según la última definición de alcance. La aplicación está pensada para una sesión local. La operación concurrente, autenticación, base de datos central, ERP, ingesta automática, notificaciones externas y portal de clientes quedan para etapas posteriores. Las cifras del documento describen la operación actual y sus metas, y sirven como contexto para el prototipo.
