# Diseño del prototipo local

## Context

El repositorio contiene la configuración de OpenSpec. El archivo adjunto describe una arquitectura hospedada para el sistema A05. El alcance acordado para esta entrega es una sesión de demostración en el navegador. La motivación y el alcance funcional están en [la propuesta](proposal.md).

## Goals / Non-Goals

El diseño debe permitir distribuir un HTML autocontenido, probarlo sin conexión y recuperar una sesión mediante archivos. El selector simula perfiles dentro de la sesión local. La identidad verificada, la auditoría a prueba de alteraciones y la coordinación entre dispositivos corresponden a un despliegue posterior.

## Decisions

- **Distribución.** `aurora.html` integra HTML5, CSS3 y JavaScript sin bibliotecas ni peticiones de red. La aplicación usa controles nativos del navegador para leer archivos seleccionados y descargar los respaldos.
- **Vistas por perfil.** El formulario, la bandeja y el detalle comparten la misma página. Al elegir Agente, el agente activo delimita el conjunto de casos antes de aplicar la búsqueda o los filtros. La selección se invalida si el detalle abierto deja de pertenecer a la vista. Coordinador inicia con la bandeja completa y puede aplicar los filtros de consulta.
- **Gráfico de plazos.** Construir las barras con HTML y CSS, sin bibliotecas ni conexión. Calcular las cantidades a partir de todos los casos abiertos, independientemente de los filtros de la bandeja. Usar horas sin redondear en los cortes de 12, 24, 26 y 48, y mostrar los rangos y conteos como texto. Actualizar la distribución con el mismo reloj que las alertas y al cambiar el conjunto de casos.
- **Modelo en memoria.** Objetos y arreglos representan clientes, agentes, tickets y eventos. Las altas válidas actualizan el ticket, el turno y el historial en una sola operación. Los tipos de solicitud son consulta de stock, estado de pedido, inconformidad de envío, facturación o cobro y devolución. Se conserva la razón social que tenía el ticket al radicarse, aunque se actualice el directorio.
- **Identificadores y reloj.** Guardar fechas en ISO 8601 usando el reloj del dispositivo, mostrar la hora en `America/Bogota` y usar esa zona para el mes del ticket. La secuencia mensual de cuatro dígitos continúa tras restaurar. Si se agota el rango mensual, la aplicación informa el límite y conserva los datos. Calcular el SLA comparando instantes sin redondear y actualizar el semáforo mientras la página esté activa y al volver a ella.
- **Estados e historial.** Los casos abiertos pueden pasar a En proceso, Pendiente logística, Pendiente ventas o Resuelto. Resuelto puede pasar a Cerrado. La resolución requiere una respuesta y fija por escrito el cumplimiento. Los eventos se agregan cronológicamente y se muestran sin controles de edición.
- **Contratos JSON.** El directorio es un arreglo de objetos con `nit` y `razonSocial`, ambos textos. Se recortan espacios exteriores y se rechazan claves vacías o repetidas dentro del archivo. El respaldo es un objeto versionado que contiene el directorio, las entidades de la sesión y el estado de consecutivos y rotación. Validar relaciones, identificadores únicos, fechas y estados antes de reemplazar la sesión. Mostrar los textos importados como texto literal.
- **Datos de demostración.** Generar las fechas de ejemplo a partir de la hora de apertura para que el usuario pueda comprobar de inmediato las tres condiciones de plazo. Incluir un ejemplo del directorio JSON en la propia aplicación para probar la importación sin preparar otro archivo.

## Risks / Trade-offs

- **Pérdida al cerrar o recargar.** Explicar en la interfaz que guardar equivale a descargar un respaldo y señalar los cambios pendientes de exportación.
- **Hora del equipo incorrecta.** Documentar qué reloj controla el SLA. Usar un reloj controlado durante las pruebas de los límites exactos.
- **Copias divergentes.** Restaurar reemplaza la sesión completa previa confirmación. Los respaldos se usan para continuar una sesión y los directorios para actualizar clientes.
- **Auditoría local.** El historial sirve para recorrer el flujo. Un archivo JSON exportado puede editarse fuera de la aplicación.

## Migration Plan

Distribuir el HTML para abrirlo con doble clic. Comprobar en el navegador que la sesión funciona sin conexión y que un respaldo válido se recupera al volver a abrir el archivo. Para reemplazar el prototipo por una nueva versión durante las pruebas, exportar primero y validar la restauración con el archivo nuevo.
