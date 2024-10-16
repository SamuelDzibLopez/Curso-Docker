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

