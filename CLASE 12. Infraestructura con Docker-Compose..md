# 12. Infrastructure with Docker-Compose.

Ya hemos creado nuestra infraestructura con un ``docker-compose.yml``, ahora, aprendamos algunos comandos útiles para poder gestionarlos y administrarlos

---
## Visualización de imagen creada por `docker-compose.yml`.

Ejecutemos el comando para visualizar las ***imagenes***:

	Recuerda:

~~~
docker images
~~~

***¿Puedes localizar la imagen creada por nuestro `docker-compose.yml`?

	Es el llamado: mongoapp-curso-docker-chanchito.

---
## Visualización de contenedores creados por `docker-compose.yml`.

Ejecutemos el comando para visualizar TODOS los ***contenedores***:

	Recuerda:

~~~
docker ps -a
~~~

***¿Puedes localizar los contenedores creados por nuestro `docker-compose.yml`?

	Es el llamado: mongoapp-curso-docker-chanchito.
	Es el llamado: mongoapp-curso-docker-monguito.

---
## Ejecutar nuevamente `docker-compose.yml`.

Si volvemos a ejecutar el comando `docker compose up` o `docker compose up` con la intensión de duplicar una nueva infraestructura, lo único que sucederá será que ***docker*** levantará nuevamente la misma infraestructura, utilizando las mismas imágenes y contenedores creados por única vez.

Es decir:

	Volver a ejecutar un archivo docker-compose.yml NO permite DUPLICAR INFRAESTRUCTURAS, sino que las LEVANTARÁ NUEVAMENTE.

---
## Eliminar la infraestructura creada de un `docker-compose.yml`.

Si deseamos eliminar TODA la ***infraestructura*** creada por `docker-compose.yml` podemos ejecutar al comando:

~~~
docker compose down
~~~

o igual:

~~~
docker-compose down
~~~

***Nota:*** Esto ***ELIMINARÁ*** los ***contenedores*** y ***reds*** que haya realizado el ``docker-compose.yml`.

veamos los logs al eliminar:

~~~
[+] Running 3/3                                                              
✔ Container mongoapp-curso-docker-chanchito-1  Removed       10.3s
✔ Container mongoapp-curso-docker-monguito-1   Removed       0.3s
✔ Network mongoapp-curso-docker_default        Removed       0.1s
~~~

Donde se eliminaron, tanto los ***contenedores*** como la red creada por ***docker*** para la ***infraestructura***.

---
## Ejecución de `docker-compose.yml` en segundo plano.

Si al ejecutar un `docker-compose.yml` queremos que este se realice en ***segundo plano*** (Y por consiguiente, devuelva a la terminal y no este en escucha de logs), podemos agregar un parámetro extra al comando:

	-d

Quedando:

~~~
docker compose up -d 
~~~

O tambien:

~~~
docker-compose up -d 
~~~

---

## Detener la infraestructura creada de un `docker-compose.yml`.

Si deseamos ***detener*** o apagar una infraestructura completa, la cual hallamos creado por medio de ``docker-compose.yml``. utilizamos el comando:

~~~
docker compose stop
~~~

O:

~~~
docker-compose stop
~~~

***Nota:*** No confundamos `docker compose stop` con `docker compose down`.

El `docker compose stop` solo apagará la ***infraestructura***, mientras que el `docker compose down` la eliminará.

---
## Levantar/Encender la infraestructura creada de un `docker-compose.yml`.

Para levantar o encender una infraestructura, ya vimos que podemos utilzar el comando:

~~~
docker-compose up

~~~

 O:

~~~
docker-compose up -d
~~~

O cualquiera de sus variantes.

Pero también, y el comando correcto en realidad es:

~~~
docker compose start
~~~

O:

~~~
docker-compose start
~~~

***Nota:*** El `docker-compose up` intenta crear una nueva ***infraestructura***, si encuentra una ya creada, la levanta, mientras que `docker-compose start` intentará levantar la ***infraestructura*** asumiendo que esta ya fue creada.
