# BASE DE DATOS

## Previa
### Parte previa 1.
Investigue cómo instalar e instale PostgreSQL en su ordenador. Tras haber instalado PostgreSQL demuestre que puede acceder a través de la terminal a sus servicios haciendo uso del comando *“psql postgres”* y **realice una captura de pantalla**.

![funcionaSQL](IMAGENES/fuincionaSQL.PNG)

### Parte previa 2.
Acceda a postgreSQL haciendo uso del comando “psql postgres” y cree un usuario con este nombre y contraseña “postgres” tal y como se muestra a continuación:

A continuación cree una base de datos llamada “tema4” haciendo uso del siguiente comando en la interfaz de PostgresSQL:

En este punto, haciendo uso de la terminal y comandos similares, podríamos crear tablas y realizar consultas pero es bastante tedioso lidiar con una terminal. Por ello vamos a utilizar una interfaz gráfica que nos permita manipular y consultar nuestras bases de datos. Para ello nos descargaremos DBeaver: https://dbeaver.io/

Investigue cómo conectarse a la base de datos “tema4” desde DBeaver e **incluya una captura de pantalla** que demuestre la conexión. 

![ConfiguracionBBDDSQL](IMAGENES/ConfiguracionBBDDSQL.PNG)

A continuación deberá crear una tabla con la siguiente sentencia y añadir un par de datos de ejemplo (use sus datos personales como ejemplo). Demuestre con una consulta SELECT la creación de la tabla y **realice una captura de pantalla**.

![SelectBBDD](IMAGENES/SelectBBDD.PNG)

## Practica
### Parte 1.
Deberá crear un proyecto de tipo Maven e incluir la dependencia especificado en el siguiente enlace (Driver JDBC PostgreSQL):

https://mvnrepository.com/artifact/org.postgresql/postgresql/42.7.4 

**Realice una captura de pantalla** con la dependencia añadida.

![libreriaEnNetBeans](IMAGENES/libreriaEnNetBeans.PNG)

### Parte 2.
El objetivo ahora es establecer una conexión entre un programa escrito en Java y nuestra base de datos haciendo uso del conector añadido con la dependencia anterior.

El programa deberá conectarse a la base de datos e imprimir si la conexión ha sido o no exitosa. **Realice una captura de pantalla** de la salida de su programa e incluya el código utilizado.
![Conexion](IMAGENES/Conexion.PNG)

Para realizar la conexión deberá utilizar el siguiente código:

### Parte 3.
Una vez establecida la conexión podemos utilizar el siguiente código para mostrar la información de la BDD:

Explique con sus palabras que hace el código anterior línea a línea:

Realice una captura de pantalla el resultado obtenido por consola:

![ResultadoConsola](IMAGENES/ResultadoConsola.PNG)

Incluya el código completo:
