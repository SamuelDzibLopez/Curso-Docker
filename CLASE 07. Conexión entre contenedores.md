# 07. Containers conecction.

Ya aprendimos como funcionan los ***contenedores***. ahora intentemos realizar una pequeña ***aplicación***.

Para ello, descarguemos el siguiente ***repositorio*** de ***git hub***:

[Aplicación para el curso gratuito de docker (github.com)](https://github.com/nschurmann/mongoapp-curso-docker)

Y eliminemos nuestro ***contenedor*** creado de ***monguito***.

- ***Detener*** el contenedor `monguito`:

~~~
docker stop monguito
~~~

- ***Eliminar*** el contenedor `monguito`:

~~~
docker rm monguito
~~~

- ***Eliminar*** la imagen de `mongo`

~~~
docker image rm monguito
~~~

---
## Creación de contenedor Mongo con variables de entorno

Vamos a crear un nuevo ***contenedor*** de ***mongo***.

Para ello, configuraremos algunas ***variables de entorno*** para su funcionamiento, tales como el ***usuario*** y ***contraseña***.

Podemos ver las ***especiaciones*** en [mongo - Official Image | Docker Hub.](https://hub.docker.com/_/mongo) en el apartado de ***variables de entorno***.

Donde coloca un ejemplo:

~~~
docker run -d --network some-network --name some-mongo -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret mongo
~~~

Donde:

- `MONGO_INITDB_ROOT_USERNAME` es la ***variable de entorno*** que recibe el usuario.
- `MONGO_INITDB_ROOT_PASSWORD` es la ***variable de entorno*** que recibe la contraseña.

---
Para ello, utilizaremos el `create`:

~~~
docker create -p27017:27017 --name monguito -e MONGO_INITDB_ROOT_USERNAME=nico -e MONGO_INITDB_ROOT_PASSWORD=password mongo
~~~

Colocando como ***usuario*** `nico` y ***contraseña*** `password`.

***¡Listo!, una vez creado, inicialicemos el contenedor.***

~~~
docker start monguito
~~~

Y podemos ver si se encuentra en ejecución:

~~~
docker ps
~~~






