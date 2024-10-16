# 02. Virtual Machines VS  Containers.

	Los CONTENEDORES no son IGUAL a las MAQUINAS VIRTUALES.

Con facilidad, podemos confundir las ***aplicaciones*** que corren dentro de ***contenedores*** con ***maquinas virtuales***, esto porque a simple vista pueden parecer realizar el mismo funcionamiento. pero en el ***interior*** son ***muy diferentes***.

![[resources/VM-vs-C.png]] 

***1. Maquinas virtuales***: Las ***maquinas virtuales*** trabajan con un ***SO independiente*** por cada una, además de su ***SO principal*** , lo que ocasiona el consumo, tanto de datos como de recursos mas ***amplio***, sus ventajas son que estos trabajan de manera independiente ***unos con otros***, si una ***VM*** falla, no afecta a las demás. Las ***maquinas virtuales*** suelen contener dentro de ellos sus servicios unidos y una sola versión de ellos por VM.

***2. Contenedores:*** Los ***contenedores*** trabajan sobre un solo ***SO principal***, lo que ahorra recursos y peso, ocasionando que los ***contenedores*** ocupen un espacio ***reducido***. sus desventajas es que si el ***hipervisor*** obtiene un fallo, todos los contenedores ***fallaran***. Por lo general, cada contenedor solo corre un solo ***servicio*** (MySQL, Node.js, Apache, React.js, etc.), y una ***aplicación*** consta de un ***conjunto unido*** de ***contenedores*** con sus ***servicios***. los ***contenedores*** por si solos, se encuentran separados entre si, por lo que es posible tener diferentes ***versiones*** de un ***servicio*** corriendo en diferentes contenedores, sin problemas.



