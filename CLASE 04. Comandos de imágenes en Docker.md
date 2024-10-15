# 04. Docker images commands.

Para estas clases, se recomienda ***instalar*** un ***servidor*** o un ***sistema*** con ***docker***, en caso de un ***servidor***, se recomienda virtualizar por practicidad ***Ubuntu server***:

[Descargar Ubuntu server](https://ubuntu.com/download/server)

Recuerda, antes de instalar:

	Configura 2 Adaptadores:
		- NAT
		- Adaptador solo anfitrion

Al instalar:

	Instalar OpenSSH y Docker

***Nota:*** No olvides tu ***usuario*** y ***contraseña***.

---
## Conectarse por SSH.

Una vez listo y encendido nuestro ***servidor***, deberemos conectarnos por ***SSH*** desde nuestra maquina.

~~~
ssh <usuario>@<ip-servidor>
~~~

***Nota:*** ***¡Listo!***, una vez colocada nuestra contraseña, tendremos acceso.

Una vez dentro del servidor, podremos ejecutar ***docker***, siempre y cuando, lo hubiéramos instalado de manera correcta. Para comprobarlo ejecutaremos el ***comando***:

~~~
docker
~~~

***Nota:*** Esto debería mostrarnos más información de ***docker***.

---
## Visualizar imágenes.

Recordemos que ya aprendimos que son las ***imágenes***.

Con ayuda de un comando, podemos visualizar todas las ***imágenes descargadas*** dentro de nuestro servidor:

~~~
docker images
~~~

Esto devolverá información de las imágenes, en forma de una tabla:

	- REPOSITORY: Es el nombre del repositorio desde donde se descargo la imagen.
	- TAG: Es el nombre de la etiqueta (versión) de la imagen. latest significa la ultima version existente en su descarga.
	- IMAGE ID: Es el nombre que se le da a la imagen, si no se define, se nombra con un ID.
	- CREATED: Fecha regresiva desde que esa imagen fue creada.
	- SIZE: Tamaño de la imagen.

***Nota:*** En nuestro caso, si es nuevo, no deberíamos tener ninguna ***imagen descargada***.

---
## Descargar imagen.

Aprendamos a descargar una imagen.

Para ello se utiliza el comando:

~~~
docker pull <imagen>
~~~

Descargaremos ***node.js***, nos basaremos de la pagina de ***docker hub***:

[node - Official Image | Docker Hub](https://hub.docker.com/_/node)

Para ello, ejecutaremos el siguiente comando:

~~~
docker pull node
~~~

***Nota:*** Este comando ***descargará*** la ***ultima versión*** (latest) de la imagen oficial de ***node.js***, debido a que no especificamos la ***tag*** o versión. ¡Esperemos a que termine de descargarse!

Recuerda, podemos ***visualizar*** nuestras imágenes descargadas con:

~~~
docker images
~~~

---
## Descargar imagen especifica (con tag)

Si deseamos descargar una ***versión*** en ***especifico***, podremos buscarla en ***tags*** (desde el repositorio de una ***imagen*** en ***docker hub***), y colocar su tag en el ***comando:

~~~
docker pull node:<tag>
~~~

En nuestro ejemplo, descargaremos:

~~~
docker pull node:18
~~~

***Nota:*** Los `:` permiten descargar una ***versión*** en especifico, para ello se utilzan las ***tags*** de los repositorios.

Ejecutamos de nuevo el comando `docker images` y podremos visualizar ***2 imágenes*** de versiones de ***node***.

	- Una con tag: latest
	- Una con tag: 18

Ejecutemos nuevamente el comando, pero ahora descarguemos ***node versión 16***.

~~~
docker pull node:16
~~~

Perfecto, ahora ya tenemos también la ***imagen*** de ***node***, pero en versión 16.

¡Listo!, hemos aprendido a descargar imágenes, tanto ***versiones especificas*** como las ***ultimas actualizadas***.

Ahora, intenta ***descargar*** la imagen de ***mysql***.

~~~
docker pull mysql
~~~

o si no

~~~
docker pull --platform linux/x86_64 mysql
~~~

Verifica que se ***descargo*** de manera correcta, ¿Recuerdas el ***comando?***

---
## Eliminar imagen

Si en algún momento deseamos ***eliminar*** una ***imagen***, podemos utiliza el ***comando***:

~~~
docker image rm <nombre>:<tag>
~~~

Cambiando:

	<nombre>: Por el nombre de la imagen o mombre del repositorio.
	<tag>: La etiqueta si se tienen diferentes versiones de la imagen.

Vamos a ***eliminar*** las ***imagenes*** de ***node***, de las versiones ***16*** y ***18***:

~~~
docker image rm node:18
~~~

~~~
docker image rm node:16
~~~

Verifiquemos con el ***comando*** para ***visualizar imágenes***.

Eliminemos también ***MySQL***.

~~~
docker image rm mysql
~~~

y ***node***:

~~~
docker image rm node
~~~

---