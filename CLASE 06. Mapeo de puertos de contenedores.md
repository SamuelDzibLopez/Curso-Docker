# 06.  Container Port Mapping.

En clases anteriores, hemos aprendido como ***crear***, ***inicializar***, ***detener*** y ***eliminar*** contenedores.

Pero cuando ***inicializamos*** un contenedor como lo hemos aprendido hasta ahora, con:

~~~
docker start <ID>
~~~

El contenedor se encuentra ***ejecutándose***, pero no tiene una conexión de salida en el ***contenedor***.

	Para poder acceder a un CONTENEDOR, se necesita realizar un MAPEO DE PUERTOS entre la MAQUINA FISICA y el CONTENEDOR.

Tomémoslo de la siguiente manera:

	Necesitamos que el PUERTO de ACCESO-SALIDA del CONTENEDOR, este relacionado con un PUERTO de ACCESO-SALIDA de la maquina/servidor.

Expliquemos mejor con la siguiente imagen:

![[resources/ports-c-docker.png]]

Donde tenemos ***3 contenedores***:

	- Cada uno de los contenedores tiene establecido como puerto de Salida/Entrada (del contenedor) el 80, lo que significa que los 3 contenedores corren un sitio web cada uno.
	
	- Estos puertos estan mapeados de la siguiente manera:

		- El primer contenedor tiene mapeado su puerto local 80 al puerto 80 del host (servidor o maquina), de tal manera, que si el usuario accede a la web al sitio en el puerto 80, visualizará la web de este contenedor.

		- El segundo contenedor tiene mapeado su puerto local 80 al puerto 81 del host (servidor o maquina), de tal manera, que si el usuario accede a la web al sitio pero en el puerto 81, visualizará la web de este respectivo contenedor.

		- El tercer contenedor tiene mapeado su puerto local 80 al puerto 82 del host (servidor o maquina), de tal manera, que si el usuario accede a la web al sitio pero en el puerto 82, visualizará la web de este respectivo contenedor.

Esto permite, tanto la ***seguridad*** de los ***contenedores***, como también ***previene*** posibles ***colisiones*** de ***puertos*** en el host, pudiendo, como en ejemplo, poder correr diferentes versiones del mismo servicio y modificar su puerto de manera sencilla.

---

recordemos que teníamos creado un contenedor, al cual llamamos `monguito`.

Vamos a eliminarlo, para crear un nuevo, el cual tenga las configuraciones.

 ~~~
 docker rm monguito
 ~~~

---
## Creando contenedor con puertos mapeados

Para crear un ***contenedor*** con los puertos mapeados, se utiliza el comando de `docker create`, pero con parametros extra:

~~~
docker create -p<puerto-servidor>:<puerto-contenedor> --name <nombre> <imagen>
~~~

Lo único extra es: `p<puerto-servidor>:<puerto-contenedor>`.

Donde:

	- El primer puerto es el puerto de Salida/Entrada de nuestro Equipo/Servidor.
	- El segundo puerto es el puerto de Salida/Entrada de nuestro Contenedor.

Ahora, vamos a crear un contenedor nuevo basándonos en la imagen de ***mongo***, con los puertos mapeados:

Para ello, vamos a ejecutar el comando:

~~~
docker create -p27017:27017 --name monguito mongo
~~~

De esta manera, hemos creado un ***contenedor*** que tenga los ***puertos mapeados***.

***Nota:*** ***Mongo*** utiliza por defecto el puerto ***27017***, por eso utilizamos esos.

---
## Visualizar mapeo de puertos en contenedores

Activemos el ***contenedor monguito***. con: `docker start monguito` y ejecutemos el ***comando***: `docker ps`.

En el ***apartado*** de `ports` de el ***contenedor*** de nombre `monguito` debería aparecernos algo similar a:

$$0.0.0.0:27017->27017/tcp, :::27017->27017/tcp$$

Si conoces un poco de redes, entenderás mejor esto, pero, basta con visualizar `27017->27017` para comprenderlo.

El ***puerto*** `27017` de nuestro ***equipo físico*** se encuentra ***mapeado*** al ***puerto*** `27017` del contenedor `monguito`.

***SI NOSOTROS SOLO COLOCAMOS UN PUERTO EN LOS ARGUMENTOS DEL COMANDO, DOCKER DECIDIRA EL PUERTO DEL EQUIPO FISICO***.

Veamos que pasa si:

~~~
docker create -p27017 --name monguito2 mongo
~~~

Creamos otro ***contenedor***, el cual solo tiene definido el ***puerto del contenedor*** (el cual es el ``27017``) y se llamara ``monguito2`.

Lo inicializamos: ``docker start monguito2``

Y visualizamos: ``docker ps``.

El el apartado de ``ports`:

$$0.0.0.0:32768->27017/tcp, :::32768->27017/tcp$$

Vemos que ***Docker*** nos establece un ***mapeado*** en la ***maquina física*** (en este caso fue el puerto `32768`).

***¡Listo!, Ya vimos que pasa si solo definimos un puerto.***

Podemos ***detener*** y ***eliminar*** el ***contenedor*** ``monguito2`.

¿Sabes como verdad?, hazlo.

---
## Visualizar logs de contenedor

También ***Docker*** permite poder visualizar el ***registro de acciones*** de un ***contenedor***.

Para ello, se utiliza el comando:

~~~
docker logs <container>
~~~

***Nota:*** recuerda, `<container>` puede ser el `ID` o el `NAME` de un ***contenedor***.

Utilicemoslo con `monguito`:

~~~
docker logs monguito
~~~

Pero, este ***comando*** solo mostrara los ***logs*** sin actualización continua, es decir, regresará a la ***línea de comandos***.

Si deseamos que la ***línea de comandos*** quede en ***modo escucha***, debemos colocar:

~~~
docker logs --follow <container>
~~~

Veamos la prueba:

~~~
docker logs --follow monguito
~~~

Si tenemos otra ***terminal***, podría sernos útil este comando.

	Para detener la "escucha" y regresar a la línea de comandos, ejecuta: Ctrl+C.

---
## Comando `run` (Descargar, crear, inicializar)

Ya aprendimos a como ***descargar*** una ***imagen***, con ella ***crear*** un ***contenedor*** y también a ***inicializarlo***; todas estas acciones con sus respectivos comandos independientes (`pull`, `create` y `start`). Pero existe un ***comando*** que realiza las ***3 ACCIONES*** en uno solo:

El cual es el ***comando*** `run`. Vamos a ver su sintaxis:

~~~
docker run --name <nombre> -p<puerto-servidor>:<puerto-contenedor> <image>
~~~

o si deseamos ***crear*** desde segundo plano (agregamos `-d`):

~~~
docker run --name <nombre> -p<puerto-servidor>:<puerto-contenedor> -d <image>
~~~

---
Pero antes de aplicarlo:

- Detengamos el contenedor `monguito`.

~~~
docker stop monguito
~~~

- Eliminemos `monguito`.

~~~
docker rm monguito
~~~

- Eliminemos la ***imagen*** de `mongo`.

~~~
docker image rm mongo
~~~

---

***¡Ahora si!, listos para ejecutar el comando `run`***.

Recordemos la sintaxis:

~~~
docker run --name <nombre> -p<puerto-servidor>:<puerto-contenedor> -d <image>
~~~

Si deseamos crear un contenedor de ***mongo***, similar al que borramos:

~~~
docker run --name monguito -p27017:27017 -d mongo
~~~

Verifiquemos que funciono, con el comando para ***visualizar contenedores activos***.

***Nota:*** Al ejecutar el `run`, ***siempre*** se creara un ***nuevo contenedor***, por lo que hay que entender que este comando debe utilizarse de manera ***selectiva***.

