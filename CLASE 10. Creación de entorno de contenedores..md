# 10. Container environment creation.

Perfecto, ahora que ya conocemos que para que entre contenedores puedan relacionarse, necesitan definirse dentro de una red, (Y ya hemos creado una red llamada `mired`), vamos a crear ***imágenes*** y contenedores para una ***aplicación***.

Recuerda, crearemos 2 contenedores:

- 1 ***contenedor*** con node.js que contiene el ***back-end*** e ***interfaz***.
- 1 ***contenedor*** con la BD de ***mongoDB***.

Empecemos creando la imagen de nuestra app realizada en ***node.js***

---
## Creación de imagen de nuestra aplicación node.js con DockerFile.

Donde se encuentra nuestro ***Dockerfile*** de nuestra app de `node.js`, ejecutamos el comando:

~~~
docker build -t <nombre-imagen>:<etiqueta> .
~~~

Este comando significa:

	- Nombre de la imagen: Como deseamos que sea nombreda nuestra imagen.
	- Etiqueta: Como deseamos llamar a esa version de la imagen.
	- El punto significa donde se encuentra en Dockerfile, en este caso, en el mismo directorio.

Como ejemplo, crearemos una ***imagen***:

~~~
docker build -t miapp:1 .
~~~

***Nota:*** Este comando ***NO*** crea contenedores, ***SIMPLEMENTE*** crea la imagen.

Visualicemos la imagen, ***¿Cómo listábamos las imágenes?***.

---
## Creación de contenedor de mongo (en red).

Creemos un contenedor de `mongo`, tal como hemos hecho varias veces, pero agregemosla a la red `mired`:

~~~
docker create -p27017:27017 --name monguito --network mired -e MONGO_INITDB_ROOT_USERNAME=nico -e MONGO_INITDB_ROOT_PASSWORD=password mongo
~~~

Para agregar un contenedor a una red, agregamos el parametro:

~~~
--network <nombre-red>
~~~

En este caso fue:

~~~
--network mired
~~~

---
## Creación de contenedor con imagen personalizada.

Con ayuda de la imagen creada en el paso anterior, podremos crear nuestro contenedor, el cual configuraremos en una ***red de contenedores***, para poder comunicarla con el otro contenedor de `mongo`.

Para ello, utilizaremos el comando `create`.

~~~
docker create -p3000:3000 --name chanchito --network mired miapp:1
~~~

Donde:

~~~
docker create -p<puerto-servidor>:<puerto-contenedor> --name <nombre-contenedor> --network <red> <nombre-imagen>:<etiqueta>
~~~

Ahora podemos visualizar, ambos contenedores:

~~~
docker ps -a
~~~

Si ejecutamos 

~~~
docker ps
~~~

no veremos los contenedores, debemos iniciarlos

***¡Inícialos!***.

***nota:*** Si presentas un problema al inicializar tu contenedor de ***chanchito***, crea uno nuevo con el siguiente contenido en el ***Dockerfile***:

~~~
# Usa una imagen base de Node.js
FROM node:18

# Crea el directorio de trabajo en el contenedor
WORKDIR /home/app

# Copia package.json y package-lock.json al contenedor
COPY package*.json ./

# Instala las dependencias
RUN npm install

# Copia el resto de los archivos de la aplicación
COPY . .

# Expone el puerto que la aplicación usa
EXPOSE 3000

# Comando para ejecutar la aplicación
CMD ["node", "index.js"]

~~~

---
## Visualización de funcionamiento de aplicación.

Una vez revisado que ambos contenedores se encuentren levantados, ***visualicemos*** la ***aplicación***.

1. Para ello, ingresemos a nuestro navegador y coloquemos:

~~~
<ip-servidor>:3000
~~~

Esto deberá dirigirnos al ***endpoint*** principal.

	Este tendra un arreglo vacio, significando que la BD devolvio 0 registros.

2. Ingresemos al ***segundo endpoint***, el cual seria:

~~~
<ip-servidor>:3000/crear
~~~

Este ***endpoint*** permite ingresar a la base de datos un registro.

3. Regresemos al ***endpoint*** principal

~~~
<ip-servidor>:3000
~~~

***Nota:*** Ya tendremos un registro devuelto.

Esta es nuestra simple aplicación.

Para comprobar podemos utilizar los ***logs*** de los contenedores:

~~~
docker logs <nombre-contenedor>
~~~

Veamos:

~~~
docker logs chanchito
~~~

Esto devolverá los ``prints`` del ***back-end***.

---

***¡LISTO!, AHORA YA SABES COMO CONECTAR CONTENEDORES E INFRAESTRUCTURAS EN REDES***.

