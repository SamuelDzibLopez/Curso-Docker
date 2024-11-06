# 09. Connection between containers.

Ya sabemos que los contenedores son elementos que ofrecen un solo servicio, por lo que se necesita un conjunto de estos para el funcionamiento de una ***aplicación***.

Por ejemplo, supongamos que tenemos una aplicación ***cliente-servidor*** que se conecta a una ***BD***, necesitaríamos un ***contenedor*** para cada uno:

	- front-end
	- back-end
	- base de datos

Pero, existen un problema:

	Los contenedores son aplicaciones independientes, que no se conocen entre si, a menos que se encuetren en una RED DE CONTENEDORES, la cual debe ser ESTABLECIDA.

Lo que significa, que si nosotros hubiéramos ***creado nuestra imagen*** y ***contenedor*** la clase anterior, no hubiéramos podido conectarnos al contenedor de ***BD*** mongo, al cual llamamos ``monguito`.

## visualizar redes de contenedores

Para visualizar las ***Redes de contenedores*** existentes, utilizamos el comando.

~~~
docker network ls
~~~

***Nota:*** Podremos observar que ya existen algunas redes creadas. estas son útiles para el funcionamiento de ***Docker***, por lo que no deberíamos eliminarlas o modificarlas.

---

## Crear una red de contenedores

Para crear una ***red de contenedores*** podemos utilizar el ***comando***:

~~~
docker network create <nombre-red>
~~~

Donde `<nombre-red>` es el nombre que deseamos darle a nuestra ***red***.

Si la ***red*** fue creada exitosamente, devolvera el ***ID***.

Vamos a crear una ***nueva red***:

~~~
docker network create mired
~~~

Y visualizamos de nuevo:

~~~
docker network ls
~~~

Deberíamos poder encontrar nuestra red.

---
## Eliminar una red de contenedores

Para eliminar una ***red de contenedores***, se utiliza en comando:

~~~
docker network rm <nombre-red>
~~~

Eliminemos la red `mired` ***creada***:

~~~
docker network rm mired
~~~

Si visualizamos, nuestra red ya no existe

Creémosla de nuevo, la necesitaremos:

~~~
docker network create mired
~~~

---

***Nota:*** La manera en la cual los ***contenedores*** se ***comunican entre sí*** (dentro de su red interna) es mediante el ***nombre*** del contenedor.

Esto quiere decir, que al ***codificar***, el nombre de nuestro dominio no seria `localhost`, sino el nombre del contenedor a utilizar.

	Si debes modificar tus conexiones a tus archivos, no dudes en realizarlo.

