# 11. Docker Compose.

Ya conocemos como funciona ***Docker*** y gestiona los contenedores y redes, incluso hemos creado nuestra pequeña infraestructura.

	Pero realizar TODO este PROCESO ha sido COMPLICADO.

Veamos todo lo que hemos realizado para lograr tener una ***infraestructura***:

	- Descargar y Buildear imagenes.
	- Crear una Red.
	- Crear contenedores.
		- Asigninar puertos.
		- Asignar nombres.
		- Asigniar variables de entorno.
		- Especificar la red.
		- Indicar la imagen:etiqueta.

Todo esto por cada contenedor creado y en ***terminal***.

Pero existe una manera de realizar todas estas acciones y automatizarlas por completo, este es:

	docker-compose.yml

***docker-compose*** es un archivo con extensión `.yml`, el cual permite la creación de una estructura personalizada de contenedores y redes, esto solamente al ejecutar el archivo ``docker-compose.yml`.

---

## Creación de archivo `docker-compose.yml`.

Primeramente, necesitamos crear un archivo llamado:

~~~
docker-compose.yml
~~~

***Nota:*** Tiene que llamarse asi.

Dentro de este archivo colocaremos las definiciones de como crear los contenedores:

	- Imágenes de docker.
	- Contenedores. 
	- Asignación de puertos.
	- Variables de entorno.
	- Red.

Veamos un ejemplo de un `docker-compose` para crear una infraestructura de nuestra aplicación, la cual creamos anteriormente.

Pero esta vez, la realizaremos con un `docker-compose`.

Ejemplo:

~~~
version: "3.9"
services:
  chanchito:
    build: .
    ports:
      - "3000:3000"
    links:
      - monguito
  monguito:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=nico
      - MONGO_INITDB_ROOT_PASSWORD=password
~~~

***Nota:*** Es importante que la configuración de este archivo se encuentre correctamente ***identada***, de lo contrario, podrían existir errores en la creación de la infraestructura.

A continuacion, se explica cada parte de la configuracion del archivo `docker compose`:

Primero explicaremos las primeras `2` lineas principales:

| `Linea`: |  `Configuracion:`  |                                                                                             `Explicación:`                                                                                             | `Identacion` |
| :------: | :----------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ------------ |
|  ``1``   | ``version: "3.9"`` | `Esta configuración define la version del compose.yml, en este caso estamos utilizando la version 3.9. Es importante colocar la version entre "". El encabezado y valor se colocan en la misma línea.` | `0`          |
|   `2`    |    `services:`     |              `Este encabezado permite poder definir los nombres de los contenedores que crearemos, por medio de los nombres que deseemos (Esto en lineas posteriores y con identación).`               | `0`          |

A partir de la línea `3` hasta `8`, definimos la configuración de un contenedor el cual llamaremos `chanchito`:

| `Linea`: | `Configuracion:` |                                                                                                                                                       `Explicación:`                                                                                                                                                        | `Identacion` |
| :------: | :--------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ------------ |
|   `3`    |  `  chanchito:`  | ``Este es el nombre con el que deseamos definir nuestro contenedor, puede ser nombredo a gusto ¡No olvidemos la identacion!. Dentro de esta encabezado, colocaremos las configuraciones de cada contenedor que definamos, como sucede mas adelante con "monguito" tambien, el cual es otro contenedor que deseamos crear.`` | `1`          |
|   `4`    |    `build: .`    |                                                 `Este encabezado define el directorio donde se realiza el build al crear el contenedor/imagen, el "." define el mismo directorio donde se encuentra el "Dockerfile" util para la generación de su contenedor y la imagen.`                                                  | `2`          |
|   `5`    |     `ports:`     |                                                                                               `Este encabezado permite la configuración de el mapeo de puertos de la maquina fisica al contenedor en las siguientes lineas.`                                                                                                | `2`          |
|   `6`    | ` - "3000:3000"` |                                                    `Asignación de mapeo, donde el puerto 3000 de la maquina fisica (izquierda) es mapeado al puerto 3000 del contenedor. No olvidemos el "- " y el mapeo entre "". Podemos mapear multiples puertos si los necesitamos.`                                                    | `3`          |
|   `7`    |     `links:`     |                                                                                                                              `Este encaezado permite definir la conexión entre contenedores.`                                                                                                                               | `2`          |
|   `8`    |   `- monguito`   |                                                                       `Se define que el contenedor "chanchito" estará conectado con el de "monguito". Pueden realizarse multiples conexiones. ESTE DATO SE COLOCA SIN COMILLAS, no olvides el "- ".`                                                                        | `3`          |
Esta es toda la configuración del primer contenedor, al cual definimos como `chanchito` y sera nuestra ***app*** que realizamos con ***node.js***.

Ahora veamos la configuración para el segundo contenedor llamado `monguito`.

| `Linea`: |            `Configuracion:`             |                                                                                                                      `Explicación:`                                                                                                                      | `Identacion` |
| :------: | :-------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ------------ |
|   `9`    |              `  monguito:`              |                                                                ``Este es el nombre con el que deseamos definir nuestro contenedor, al igual que "chanchito" y con la misma identacion.``                                                                 | `1`          |
|   `10`   |            ``image: mongo``             |               `El encabezado "image" permite la definición de la imagen con la cual sera creado el contenedor, al ser solo la creación de una imagen pura, no necesitamos un "build", sino una "image", en este caso sera la de "mongo".`                | `2`          |
|   `11`   |                `ports:`                 |                                             `Este encabezado permite la configuración de el mapeo de puertos de la maquina fisica al contenedor en las siguientes lineas. Explicado tambien en la linea 5.`                                              | `2`          |
|   `12`   |           ` - "27017:27017"`            | `Asignación de mapeo, donde el puerto 27017 de la maquina fisica (izquierda) es mapeado al puerto 27017 del contenedor. No olvidemos el "- " y el mapeo entre "". Podemos mapear multiples puertos si los necesitamos. Explicado tambien en la linea 6.` | `3`          |
|   `13`   |             `environment:`              |                                                             `Este encabezado permite definir las "variables de entorno" utiles para crear un contenedor. todo eso en las siguientes líneas.`                                                             | `2`          |
|   `14`   |   `- MONGO_INITDB_ROOT_USERNAME=nico`   |                                  `Definimos que el al crearse el contenedor "monguito", se le configurará la variable de entorno llamada "- MONGO_INITDB_ROOT_USERNAME" con el valor de "nico". ¡No olvidemos el "- "!`                                  | `3`          |
|   `15`   | `- MONGO_INITDB_ROOT_PASSWORD=password` |                                 `Definimos que el al crearse el contenedor "monguito", se le configurará la variable de entorno llamada "MONGO_INITDB_ROOT_PASSWORD" con el valor de "password". ¡No olvidemos el "- "!`                                 | `3`          |

Estas son las ***configuraciones*** de nuestro contenedor a crear llamado `monguito`.

---
## Ejecución de archivo ``docker.compose.yml``.

Una vez configurado nuestro archivo `docker-compose.yml` podemos ejecutarlo:

	Esto nos creará la infraestructura definida de manera automatizada.

Para ello, ejecutaremos el comando:

~~~
docker compose up
~~~

El cual, hace lo mismo que:

~~~
docker-compose up
~~~

***Nota:*** No olvides encontrarte en el mismo directorio que tu archivo.

Esto realizará la:

	- Descarga de las imagenes.
	- Creación de las imagenes.
	- Creacion de red.
	- Configuración de contenedores.
		- Mapeo de puertos.
		- Configuración de variables de entorno.

De una manera automatizada.

***Nota:*** Al ejecutar un `docker-compose-yml` ***docker*** sabrá que todos los ***contenedores*** definidos están en una red. ***creando*** una ***red***.

---
## Visualización de app en ejecución.

Ahora, podemos acceder a nuestra ***app*** desde el navegador, colocando la ***URL***:

~~~
<ip-servidor>:80
~~~

Y crear un nuevo registro en la ***BD***:

~~~
<ip-servidor>:80/crear
~~~


Y regresar para visualizar el nuevo registro:

~~~
<ip-servidor>:80
~~~

Con esto, podemos confirmar que el `docker-compose.yml` fue ejecutado y realizo su trabajo de manera ***exitosa***.

***Nota:*** Si visualizamos la terminal, podremos ver los ***logs*** de las acciones que realizamos:

~~~
listando... chanchitos...
creando...
listando... chanchitos...
~~~

Para detener la ejecucion utilizamos:

	Ctrl + c

En la siguiente clase, aprenderemos como ***gestionar*** la ***infraestructura creada***.