# 08.- Escenarios multicontenedor

Compose es una herramienta para definir y ejecutar aplicaciones Docker multicontenedor. Con Compose, utilizamos un archivo YAML para configurar los servicios de nuestra aplicación. Luego, con un solo comando, creamos e iniciamos todos los servicios a partir de nuestra configuración.

Compose funciona en todos los entornos: producción, staging, desarrollo, pruebas, así como flujos de trabajo CI (*Continuous Integration*). También dispone de comandos para gestionar todo el ciclo de vida de su aplicación:

- Iniciar, detener y reconstruir servicios
- Ver el estado de los servicios en ejecución
- Transmitir la salida de registro de los servicios en ejecución
- Ejecutar un comando puntual en un servicio

Las características clave de Compose que lo hacen eficaz son:

- Disponer de múltiples entornos aislados en un único host
- Preserva los datos de volumen cuando se crean contenedores
- Sólo recrea los contenedores que han cambiado
- Soporta variables y el movimiento de las composiciones entre entornos