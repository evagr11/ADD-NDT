En este ejercicio se propone aprender aspectos básicos del framework Spring Boot con el objetivo final de conectarnos a una base de datos embebida H2.

### Parte 1. 
Descarga de Spring Boot y configuración del proyecto.
En primer lugar nos dirigimos a la siguiente página web: https://start.spring.io/.

Esta página nos permite configurar:
 - Tipo de proyecto: elegiremos Maven.
 - Lenguaje: Java.
 - Versión de Spring Boot: Dejamos la versión por defecto.
 - Metadatos del proyecto, lo configuramos como en la siguiente imagen (modifique el grupo para incluir su nombre y primer apellido):

En la parte de la derecha podemos elegir y añadir dependencias a nuestro proyecto de forma muy sencilla, en nuestro caso vamos a añadir nuestra base de datos de tipo H2:

Vamos a añadir también el módulo Spring Web para simular el funcionamiento de una aplicación web en este ejercicio:

También añadimos esta dependencia:

A continuación incluya una captura de pantalla de la web anteriormente mencionada con toda la configuración realizada:

   ![configuracionSpring](IMAGENES/CapturaSpring.PNG)

Finalmente hacemos click en “Generate” y se nos descargará el proyecto con toda la configuración realizada.En nuestro archivo pom.xml podemos ver como se nos ha añadido por ejemplo la dependencia de la base de datos H2:


### Parte 2. 
Por defecto Spring Boot como ya hemos mencionado en las diapositivas del tema, genera algunos archivos de configuración de forma automática. Localiza el archivo “Application.properties” e incluye una captura de pantalla de la configuración de la base de datos H2. (En caso de que no se haya generado esta configuración, añádela al archivo).

   ![configuracionSpring2](IMAGENES/CapturaConfiguracionParte2.PNG)

Como vemos en la segunda línea, la base de datos está definida en memoria:
| ``` spring.datasource.url=jdbc:h2:file:∼/test; ``` |
|--------------------------------------------------------------------------------------|

Lo vamos a cambiar para utilizar una base de datos embebida:
| ``` spring.datasource.url=jdbc:h2:mem:testdb; ``` |
|--------------------------------------------------------------------------------------|

### Parte 3. 
En este punto ya tenemos la aplicación configurada y lista para trabajar con una base de datos. Nos vamos a dirigir a la ruta “src/main/resources” y vamos a crear un fichero llamado “data.sql” donde crearemos una primera tabla.
Deberá crear a continuación una tabla (de libre elección, pero con al menos 3 campos y usando dos tipos de datos diferentes). Incluya el contenido del fichero creado.

   ![configuracionBBDD](IMAGENES/CapturaBaseDatos.PNG)

Otra cosa que podemos habilitar en Spring Boot es el acceso a nuestra base de datos o a nuestro panel de gestión de base de datos. Para ello añadimos la siguiente línea a nuestro fichero “application.properties”:
| ``` spring.h2.console.enabled=true ``` |
|--------------------------------------------------------------------------------------|

Incluya el contenido del archivo completo con la nueva configuración añadida:
   ![configuracionBBDDTrue](IMAGENES/CapturaAccesoBBDDTrue.PNG)

De esta forma si ejecutamos nuestra aplicación podemos acceder a la siguiente URL (http://localhost:8080/h2-console/) y acceder al panel de gestión de la base de datos. Pero antes para ejecutar la aplicación debemos realizar una configuración. Hacemos click en “Maven build” y añadimos el siguiente “goal”:

Incluya una captura de pantalla dentro del gestor de base de datos H2 donde se vea la tabla creada:
(Nota: Si encuentra un error relacionado con la existencia de la base de datos deberá crear un fichero de tipo “test.mv.db”)

   ![configuracionBBDDTabla](IMAGENES/CapturaBBDDTabla.PNG)
