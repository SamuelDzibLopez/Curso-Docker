# 08. Preparing your application to generate a container

Ya tenemos nuestro ***codigo node.js*** funcionando con normalidad, sin embargo, este funciona dentro de nuestro servidor, deseamos que funcione en un contenedor, para poder llevarlo a un repositorio y ser independiente.

Para ello, necesitamos generar nuestra ***aplicación*** de ***node.js*** en una ***imagen*** para generar un contenedor.

---
## Creando archivo Dockerfile de código node.js

A continuación, queremos crear nuestro ***contendor*** con nuestra ***aplicación***.

	RECUERDA: Todas las imagenes creadas, necesitan generarse con otras imagenes.
### Crear archivo Dockerfile

Una vez ya obtenido el código, podemos visualizar un archivo llamado ***Dockerfile***:

	- Dockerfile

***Nota:*** Este archivo funciona como un tipo de Script para poder generar una imagen y es muy importante. Si no existe debe crearse.

Este archivo `Dockerfile` debería contener el siguiente ***código***:

~~~
FROM node:18
RUN mkdir -p /home/app
COPY . /home/app
EXPOSE 3000
CMD ["node", "/home/app/index.js"]
~~~

Explicaremos cada una de las líneas:

	- Las palabras en MAYUSCULAS (FROM, RUN, COPY, EXPOSE y CMD) son palabras claves y reservadas.

	Linea 1: Acompañado de `FROM` se coloca para DEFINIR el nombre de la IMAGEN a utilizar, acompañado de su TAG (En el caso node y la versón con etiqueta 18).
	Linea 2: Acompañado de `RUN mkdir` se coloca para ejecutar un comando en el contenedor para CREAR un directorio. El parámetro `-p` asegura que los directorios intermedios se creen si no existen (En el caso se creara app, y si no existe home, tambien).
	Linea 3: Acompañado de `COPY` se coloca la dirección del directorio principal donde se encuentra el codigo fuente a generar y COPIAR al momento de GENERAR el contenedor (El `.` significa que se debe copiar TODOS los archivos dentro de /home/app).
	Linea 4: Acompañado de `EXPOSE` DEFINE el puerto de salida/entrada a exponer para conectarse desde el sevidor (En el caso, correra en el purto 3000, como lo haria una web).
	Linea 5: Acompañado de `CMD`, se COLOCA EN UN ARRAY, el COMANDO que permite ejecutar nuestra aplicación (Se coloca el comando y los argumentos).

---

***¡Perfecto!, Con este archivo, podríamos generar una imagen de Docker***.

Lo cual nos serviría para crear un contenedor listo para su uso.

Pero aun no lo haremos, tenemos que aprender un tema antes, el cual son las ***redes de contenedores***.


