# Tareas de implementación

## 1. Vista y datos de ejemplo

- [x] 1.1 Crear `aurora.html` con formulario, bandeja y detalle adaptable. Verificar que se abre desde el sistema de archivos con doble clic, funciona sin conexión y permite usar el formulario con teclado.
- [x] 1.2 Incorporar cuatro agentes ficticios, un directorio inicial y solicitudes con distintos plazos. Verificar que al abrir el archivo aparecen los tres niveles de atención y un caso resuelto con historial.

## 2. Radicación, seguimiento y plazo

- [x] 2.1 Implementar búsqueda de clientes, formulario validado, consecutivo mensual y rotación. Verificar los tres canales, cinco registros consecutivos y un intento inválido que no consume número ni turno.
- [x] 2.2 Implementar búsqueda y filtros en la bandeja, detalle y cambios de estado con historial. Verificar la consulta por ticket, NIT o razón social, la conservación de los eventos y el rechazo del cierre sin resolver.
- [x] 2.3 Calcular horas corridas, alertas visuales y cumplimiento al resolver. Verificar los límites de 24 y 48 horas con un reloj controlado, el estado en espera y el resultado fijo tras resolver.

## 3. Archivos JSON

- [x] 3.1 Incorporar la importación del directorio y mostrar un ejemplo JSON en la interfaz. Verificar el alta por NIT, la actualización de una coincidencia y el rechazo completo de un archivo inválido o duplicado.
- [x] 3.2 Implementar exportación, restauración y aviso de cambios pendientes. Verificar que un respaldo recupere los datos y continúe la asignación y el consecutivo, y que la restauración cancelada o inválida preserve la sesión.

## 4. Aceptación en navegador

- [x] 4.1 Probar el recorrido completo con el HTML local en un navegador instalado, en tamaños de escritorio y móvil. Abrir sin conexión, probar las alertas con datos ficticios, importar clientes, radicar y resolver un caso, exportar, volver a abrir y restaurar el historial. Entregar el HTML descargable para repetir el recorrido en el navegador del usuario.

## 5. Perfiles simulados y gráfico de plazos

- [x] 5.1 Incorporar los selectores de perfil y agente. Verificar que cada agente puede consultar solo sus casos, que al cambiar de perfil se retira un detalle fuera de alcance y que el coordinador recupera la bandeja completa con las alertas de 24 y 48 horas.
- [x] 5.2 Incorporar el gráfico de cuatro barras y el contador de vencidas para el coordinador. Probar con un reloj controlado los límites exactos de 12, 24, 26 y 48 horas, las barras vacías y la exclusión de solicitudes resueltas y cerradas.
- [x] 5.3 Probar la nueva interacción con el archivo HTML local en tamaños de escritorio y móvil. Verificar el cambio de perfil, la lectura de rangos y cantidades y la actualización del gráfico al resolver o restaurar solicitudes.
