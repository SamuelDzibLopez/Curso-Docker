# 05. Docker containers commands.

	Para poder crear un contenedor, necesitamos al menos una IMAGEN.

Ya aprendimos como ***descargar imágenes*** de ***Docker***, ahora aprenderemos como generar un ***contenedor*** tomando esa imagen. Veamos:

Primero, descargaremos la ultima versión de la ***imagen*** de ***mongo***:

~~~
docker pull mongo
~~~

***Nota:*** No olvides verificar que la ***imagen*** se descargue correctamente. (`docker images`).

---
## Crear contenedor

Ya que tenemos una ***imagen***, podemos utilizarla para ***crear*** nuestro propio ***contenedor***.

Para ello se utiliza el comando:

~~~
docker create <imagen>
~~~

Utilizando la imagen de `mongo`, escribiremos el comando:

~~~
docker create mongo
~~~

***¡Listo!***, esto nos devolverá una ***cadena de texto***, esta es el ***ID*** del contenedor creado.

***Nota:*** El comando `docker create` es una simplificación del comando:

~~~
docker container create <imagen>
~~~

El ***contenedor*** esta creado, pero no esta siendo ejecuta.

---
## Ejecutar contenedor

Una vez creado un ***contenedor***, puede ser ***ejecutado*** para utilizarlo, para ello utilizaremos el comando:

~~~
docker start <ID>
~~~

***Nota:*** El ***ID*** es la ***cadena de texto*** que ***Docker*** nos devolvió al crear el ***contenedor***.

Debes colocar el tuyo propio.

Si al ***ejecutar*** el comando, nos devuleve el ***ID*** de nuevo, significa que ***Docker*** encontró el contenedor y ahora se encuentra encendido.

---
## Visualizar contenedores encendidos

Se supone que hemos ***encendido*** nuestro ***contenedor***, ¿pero como lo visualizamos de una mejor forma?

Para ello, tenemos el comando:

~~~
docker ps
~~~

Este ***comando***, devolverá una ***tabla***, donde estarán listados los ***contenedores*** que se encuentren en ***ejecución***.

La tabla contiene:

	- CONTAINER ID: Este es un ID de identificación más corto, para poder ubicarlo en comandos.
	- IMAGE: Nombre de la imagen base del contenedor.
	- COMMAND: Este es el comando para poder ejecutarse.
	- CREATED: Fecha regresiva de creacion del contenedor.
	- STATUS: Es el estado en que se encuentra el contenedor.
	- PORTS: Este es el puerto que utiliza el contenedor
	- NAMES: Este es el nombre de nuestro contenedor.

***Nota:*** Recuerda, con `docker ps` solo se mostrarán los ***contenedores*** corriendo.

---
## Detener un contenedor corriendo

Para detener un ***contenedor*** que se encuentre ***corriendo***, debemos ejecutar el comando:

~~~
docker stop <ID>
~~~

Este comando va acompañado del ***ID*** del contenedor, este lo podemos visualizar con `docker ps`.

***Nota:*** Al ejecutarlo, se detendrá (devolviendo el ***ID***, que significa que encontró el ***contenedor***), si ejecutamos de nuevo:

~~~
docker ps
~~~

Ya no se visualizará.

	El contenedor SIGUE CREADO, solo lo APAGAMOS.

---
## Visualizar TODOS los contenedores CREADOS

Si deseamos ***visualizar TODOS*** los ***contenedores*** existentes, tanto ***activos*** como ***inactivos***, basta con hacer una ***pequeña*** modificación al comando:

~~~
docker ps -a
~~~

***Nota:*** Los ***datos*** de la ***tabla*** significan lo mismo.

---
## Eliminar contenedor

Si deseamos ***eliminar*** un ***contenedor***, podemos colocar el ***comando***:

~~~
docker rm <ID>
~~~

Donde `<ID>` representa el ***ID*** del contenedor.

---
## Utilizando nombres de contenedores

Al ejecutar `docker ps` o `docker ps -a` podemos visualizar un ***apartado*** de `NAMES`.

Podemos utilizar este `NAME` para ***identificar*** al ***contenedor*** en lugar de estar utilizando su `ID`.

Tanto para:

- Inicializarlos:

~~~
docker start <name>
~~~~

- Detenerlos:

~~~
docker stop <name>
~~~

- Eliminarlos:

~~~
docker rm <name>
~~~

***Nota:*** El nombre es definido por ***Docker***, pero aprenderemos a definirlo nosotros.

---
## Creando contenedor con nombre personalizado

Para esta acción, utilizamos el ***mismo comando*** que ya aprendimos, pero agregamos un argumento mas.

- Antes:

~~~
docker create <imagen>
~~~

Ahora:

~~~
docker create --name <nombre> <imagen>
~~~

Por lo que, tomando como base la imagen de ***mongo***, crearemos un nuevo ***contenedor*** con nombre de `monguito`.

Para ello:

~~~
docker create --name monguito mongo
~~~

***De esta manera, podemos referenciar nuestro contenedor de manera personalizada.***

Intentemos ***activarlo***:

~~~
docker start monguito
~~~

Podemos visualizar que este ***activo***, ¿Con que comando?

