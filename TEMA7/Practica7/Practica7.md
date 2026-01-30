# Practica 7

En esta práctica vamos a trabajar con las siguientes tecnologías:

// TODO : insertar foto apuntes

## Parte 1: PostgreSQL (base de datos)
Acceda a la siguiente página: https://www.postgresql.org/download/ y descargue PostgreSQL.
Una vez descargado podemos probar si funciona ejecutando el siguiente comando en una terminal (git bash):

| ``` psql postgres ``` |
|-----------------------|

A continuación vamos a crear un usuario y su contraseña para acceder a nuestra base de datos, utilizaremos el siguiente comando:

| ``` postgres=# CREATE USER postgres WITH PASSWORD 'postgres'; ``` |
|-----------------------|

Por último vamos a crear una base de datos con nombre acceso_a_datos utilizando el siguiente comando:

| ``` postgres=# CREATE DATABASE acceso_a_datos; ``` |
|-----------------------|

![ConfiguracionPostgresSQL](IMAGENES/CapturaParte1P7.PNG)

## Parte 2: DBeaver (gestor de base de datos)
Vamos a instalar el gestor de base de datos DBeaver, el cual es openSource y permite conectarnos a muchos tipos diferentes de bases de datos. Es un programa muy ligero y eficiente.
Para instalar DBeaver deberemos acceder a la siguiente página: https://dbeaver.io/.
A continuación podemos probar a conectar la base de datos previamente creada con DBeaver.

Para ello debemos dirigirnos a la opción de añadir una conexión con una base de datos. Dentro del menú que aparece debemos indicar que nos queremos conectar a una base de datos de tipo PostgreSQL:

// TODO : insertar foto apuntes

Tras esto debemos rellenar información acerca nuestra base de datos que previamente hemos configurado, **sustituyendo los datos necesarios por los que esté utilizando en su proyecto**:

// TODO : insertar foto apuntes

![ConfiguracionDBeaver](IMAGENES/CapturaConfiguracionDBeaverP7.PNG)

Tras realizar esta configuración podemos observar que aparece nuestra base de datos conectada en DBeaver. En este punto ya podríamos realizar consultas sobre nuestra base de datos.

![ConexionBBDD](IMAGENES/CapturaConexionBBDDP7.PNG)

## Parte 3: Spring Boot (Framework Java) + Hibernate
Como ya sabemos para inicializar un proyecto con Sprint Boot, nos dirigimos a la siguiente página: https://start.spring.io/ y aplicamos en este caso la siguiente configuración, **sustituyendo los datos necesarios por los que esté utilizando en su proyecto**:

![ConfiguracionSpringBoot](IMAGENES/CapturaConfiguracionSpringBootP7.PNG)

Puede observar que estamos instalando una dependencia diferente respecto a la práctica anterior: un driver para nuestra base de datos PostgreSQL. Además ahora podemos observar que la dependencia Spring Data JPA incluye Hibernate.

## Parte 4: Java + Maven (lenguaje + gestor de dependencias)
Rellene en el archivo application.properties con los siguientes campos, sustituyendo los datos necesarios por los que esté utilizando en su proyecto:

![ConfiguracionAppProperties](IMAGENES/CapturaConfiguracionAppPropertiesP7.PNG)

Para lanzar una aplicación Spring Boot deberá añadir la siguiente configuración, recuerde que este paso cambiará si usa Ant o Gradle para su proyecto:

![Funcionando](IMAGENES/CapturaFuncionandoP7.PNG)

