# 13. Volumes.

Durante todo este tiempo que hemos creado ***múltiples*** infraestructuras de ***redes de contenedores*** en ***docker*** tal vez te percataste de un problema:

	Cada vez que eliminamos la infraestructura y creamos una nueva, los datos son borrados.

Esto sucede porque al crear un contenedor, los datos de estos son almacenados en el mismo, dentro de su propio sistema de archivos, no en la ***maquina servidor***.

Si deseamos que al eliminar un ***contenedor*** o ***infraestructura*** pueda almacenar datos aun cuando este se eliminado, debemos crearle un ***VOLUMEN***.

Los ***volúmenes*** permiten:

	- Almacenar datos en nuestro sistema anfitrión. Util para "data" almacenada en BDs.
	- Modificar directamente codigo dentro de los contenedores ya creados al momento de desarrollar. Para no tener la necesidad de crear/build nuevas imagenes y contenedores por cada modificación.

Existen ***3 tipos diferentes de volúmenes***:

	- Anonimo: Este tipo de volumen se crea solo definiendo la ruta. Estos no pueden ser referenciados si deseamos utilizarlo en otro contenedor.
	- Anfitrion o host: Permite definir que directorio colocar y en que directorio colocarse.
	- Nombrado: Similar al anonimo, pero con la posibilidad de poder referenciar este volumen en otros contenedores.

---
## Creación de volúmenes en ``docker-compose.yml``.

¿Recuerdas nuestro archivo ***``docker-compose.yml``***?, el cual contenía:

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

***Nota:*** Ya entendimos que significa.

***Agreguémosle*** nuevas configuraciones.

Esto para al momento de ejecutar el `docker-compose.yml` nuevamente, se cree la ***infraestructura*** ya con los volúmenes:

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

volumes:
  mongo-data:
~~~

Lo único que colocamos fue al final la siguiente configuración:

~~~
volumes:
  mongo-data:
~~~

Creando un volumen llamado `mongo-data`. El cual utilizaremos para enlazar y almacenar los datos de la ***BD*** de mongo de nuestra pequeña ***app***.

***Nota:*** Esto define el nombre de los volúmenes a crear para la ***infraestructura***. Podemos crear tantos volúmenes como necesitemos.

Expliquemos un poco el texto:

| `Linea`: | `Configuracion:` |                                                                                        `Explicación:`                                                                                        | `Identacion` |
| :------: | :--------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ------------ |
|  ``17``  |   ``volumes:``   | `Este atributo permite la definición de los volumenes que seran creados para la infraestructura, este no lleva identaciones y se coloca al final de toda la configuración acompañado de ":"` | `0`          |
|  ``18``  | ``mongo-data:``  |                                                  `Esta es la definición de el volumen a crear, colocado con una identación y al final ":".`                                                  | `1`          |

***Nota:*** Esto ***crea*** y ***define*** los ***volúmenes***, sin embargo, aun necesitamos definir y ***enlazarlos con los contenedores que deseemos***.

---
## Enlace de volúmenes a contenedores en `docker-compose.yml`.

Una vez definidos los contenedores que necesitaremos para la infraestructura, podemos enlazarlos a los contenedores:

***Nota:*** Solo podemos enlazar volúmenes definidos.

En este caso, enlazaremos nuestro volumen `mongo-data` creado al contendor de `monguito`.

Modificando el `docker-compose.yml` a la siguiente manera:

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
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
~~~


Donde agregamos a las configuraciones del contenedor `monguito`:

~~~
    volumes:
      - mongo-data:/data/db
~~~

Donde:

| `Linea`: |     `Configuracion:`      |                                                                                                                                                      `Explicación:`                                                                                                                                                      | `Identacion` |
| :------: | :-----------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ------------ |
|  ``16``  |       ``volumes:``        |                                                                                    `Esta configuración permite definir los volumenes a enlazar con el contenedor al que pertenece el atributo. Con identación y finalizando con ":"`                                                                                     | `2`          |
|  ``16``  | ``- mongo-data:/data/db`` | `Definicion del nombre del volumen (ya creado) y la ruta del contendor donde se almacenan los datos. Donde "mongo-data" es el nombre del volumen y "/data/db" la ruta donde mongo almacena sus datos. No olvidemos la identación, el inicio con "-" y el enlace con ":". Podemos definir tantos volumenes como deseemos` | `2`          |
|          |                           |                                                                                                                                                                                                                                                                                                                          |              |

	Mongo almacena sus datos en el directorio "/data/db", por lo que es necesario colocar este directorio, este puede variar dependiendo del gestor de BD utilizado.

Para otras ***BD***:

	mysql -> /var/lib/mysql
	postgres -> /var/lib/postgresql/data

***FORMA FINAL DE*** `docker-compose-yml`:

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
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
~~~

Listo, ***NO*** olvides guardar el ***archivo*** y sus ***cambios***.

---
## Ejecución de `docker-compose.yml` con volumenes.

Para este punto, si tienes la ***infraestructura*** inicializada, puedes apagarla.

¿Recuerdas el comando?

~~~
docker-compose stop
~~~

Esto para poder ejecutar de nuevo el `docker-compose.yml`.

Para ello, ejecutamos:

~~~
docker-compose up
~~~

***Nota:*** Es ***importante*** ejecutar `up` (en cualquiera de sus variantes y `-d`), ***NO*** `start`. 

Al ejecutar, debería mostrar el siguiente log similar en terminal:

```
[+] Running 3/3
 ✔ Volume "mongoapp-curso-docker_mongo-data"    Created             0.0s
 ✔ Container mongoapp-curso-docker-monguito-1   Started             0.1s
 ✔ Container mongoapp-curso-docker-chanchito-1  Started             0.1s
```

Visualizando que:

	- El volumen llamado "mongoapp-curso-docker_mongo-data" fue CREADO.
	- El contenedor llamado "Container mongoapp-curso-docker-monguito-1" fue INICIALIZADO.
	- El contenedor llamado "mongoapp-curso-docker-chanchito-1" fue INICIALIZADO.

***Nota:*** Recordemos el comando `docker-compose up` intenta crear nueva infraestructura, pero si encuentra una ya creada, solo la levanta, y solo crea los cambios.

¡NUESTRO VULUMEN YA HA SIDO CREADO Y ENLAZADO AL CONTENEDOR DEFINIDO!

---
## Verificación de enlace de volumen

Verifiquemos que el enlace se a realizado, para ello, realizaremos algunos pasos:

### 1. Guardemos algunos datos:

	1. Ingresa a <ip-servidor>:3000
	2. Ingresa a <ip-servidor>:3000/crear
		- Esto ingresará un registro
	3. Ingresa nuevamente a <ip-servidor>:3000
	4. Ingresa nuevamente a <ip-servidor>:3000/crear
		- Esto ingresará un segundo registro
	5. Ingresa a <ip-servidor>:3000
		- Para visualizar los 2 registros.

Con esto deberíamos poder ***visualizar*** de manera correcta los ***2 registros***.

### 2. Eliminaremos la infraestructura:

Eliminaremos la ***infraestructura***, incluyendo la ***BD*** ``monguito``. Pero no te preocupes, el volumen creado debería almacenar los datos, aun cuando se elimine el ***contendor***. ***¡Confía!***.

Para ***apagar primero*** la ***infraestructura***:

~~~
docker compose stop
~~~

Y ***eliminar*** la ***infraestructura***:

~~~
docker compose down
~~~

***Nota:*** No debería haber problema de ejecutar `down` sin el `stop`, pero mejor estar seguro.

Verifica que se elimino, correctamente:

~~~
docker ps
~~~

Y:

~~~
docker ps -a
~~~

¡Ya hemos eliminado nuestra infraestructura!.

### 3. Creación de nueva infraestructura con volumen enlazado.

Ahora, crearemos una ***nueva infraestructura***, pero enlazando el el volumen.

Ejecutaremos el `docker-compose.yml`

~~~
docker-compose up -d
~~~

Si visualizamos los logs, podemos ver que no se creo el ***volumen***, esto es porque el volumen se creo antes, solo se enlazo al ***volumen*** al ***contenedor***:

~~~
[+] Running 3/3
 ✔ Network mongoapp-curso-docker_default        Created             0.2s
 ✔ Container mongoapp-curso-docker-monguito-1   Started             0.1s
 ✔ Container mongoapp-curso-docker-chanchito-1  Started             0.1s
~~~

***Nota:*** El ***volumen*** es creado en los ***directorios*** de la ***maquina física/servidor***, por lo que aun borrando el contenedor, estos permanecen a espera de ser utilizados cuando se cree otro.

### 4. Verificación de funcionamiento de volumen.

Ahora solo tenemos que acceder al sitio web:

~~~
<ip-servidor>:3000
~~~

Y podremos visualizar que los datos permanecen.

