Docker Compose es una herramienta que se utiliza para definir y ejecutar aplicaciones Docker de **múltiples contenedores.** Con Docker Compose, se puede definir una aplicación en un archivo YAML y luego ejecutarla con un solo comando. Docker Compose también permite la creación de redes personalizadas y volúmenes de datos para los contenedores.

Docker Compose **es útil para simplificar la gestión de aplicaciones Docker complejas.** Por ejemplo, si una aplicación consta de varios contenedores que deben comunicarse entre sí, Docker Compose puede definir fácilmente las relaciones entre los contenedores y las redes que los conectan. También puede definir volúmenes de datos para los contenedores, lo que permite el almacenamiento persistente de datos.

Presentamos a continuación un ejemplo de un archivo YAML de Docker Compose para una aplicación Django:

```yaml
version: '3'

services:
	db:
		image: postgres
	environment:
		POSTGRES_DB: django_db
		POSTGRES_USER: django_user
		POSTGRES_PASSWORD: django_password

	web:
		build: .
			command: python manage.py runserver 0.0.0.0:8000
		volumes:
		-  .:/code

ports:
	- "8000:8000"

depends_on:
	- db
```

En este ejemplo, se definen dos servicios: `db` y `web`. El servicio `db` utiliza la imagen de `postgres` y se definen las variables de entorno para la base de datos. El servicio `web` se construye a partir del directorio actual y se ejecuta el comando para iniciar el servidor Django. También se define un volumen para el código fuente y se expone en el puerto 8000. El servicio `web` depende del servicio `db`.