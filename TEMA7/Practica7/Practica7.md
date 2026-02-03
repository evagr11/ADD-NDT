# Practica 7

> **Nota:**  
> - **Comprobar que hay en el puerto 8080** -> ``` netstat -ano | findstr :8080 ```  
> - **Cortar proceso** -> ``` taskkill /PID <PID> /F ```
---
## Ejercicio guiado.
En esta práctica vamos a trabajar con las siguientes tecnologías:

![Imagen1](IMAGENES/Imagen1.PNG)

### Parte 1: PostgreSQL (base de datos)
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

### Parte 2: DBeaver (gestor de base de datos)
Vamos a instalar el gestor de base de datos DBeaver, el cual es openSource y permite conectarnos a muchos tipos diferentes de bases de datos. Es un programa muy ligero y eficiente.
Para instalar DBeaver deberemos acceder a la siguiente página: https://dbeaver.io/.
A continuación podemos probar a conectar la base de datos previamente creada con DBeaver.

Para ello debemos dirigirnos a la opción de añadir una conexión con una base de datos. Dentro del menú que aparece debemos indicar que nos queremos conectar a una base de datos de tipo PostgreSQL:

![Imagen2](IMAGENES/Imagen2.PNG)

Tras esto debemos rellenar información acerca nuestra base de datos que previamente hemos configurado, **sustituyendo los datos necesarios por los que esté utilizando en su proyecto**:

![Imagen3](IMAGENES/Imagen3.PNG)

![ConfiguracionDBeaver](IMAGENES/CapturaConfiguracionDBeaverP7.PNG)

Tras realizar esta configuración podemos observar que aparece nuestra base de datos conectada en DBeaver. En este punto ya podríamos realizar consultas sobre nuestra base de datos.

![ConexionBBDD](IMAGENES/CapturaConexionBBDDP7.PNG)

### Parte 3: Spring Boot (Framework Java) + Hibernate
Como ya sabemos para inicializar un proyecto con Sprint Boot, nos dirigimos a la siguiente página: https://start.spring.io/ y aplicamos en este caso la siguiente configuración, **sustituyendo los datos necesarios por los que esté utilizando en su proyecto**:

![ConfiguracionSpringBoot](IMAGENES/CapturaConfiguracionSpringBootP7.PNG)

Puede observar que estamos instalando una dependencia diferente respecto a la práctica anterior: un driver para nuestra base de datos PostgreSQL. Además ahora podemos observar que la dependencia Spring Data JPA incluye Hibernate.

### Parte 4: Java + Maven (lenguaje + gestor de dependencias)
Rellene en el archivo application.properties con los siguientes campos, sustituyendo los datos necesarios por los que esté utilizando en su proyecto:

![ConfiguracionAppProperties](IMAGENES/CapturaConfiguracionAppPropertiesP7.PNG)

Para lanzar una aplicación Spring Boot deberá añadir la siguiente configuración, recuerde que este paso cambiará si usa Ant o Gradle para su proyecto:

![Funcionando](IMAGENES/CapturaFuncionandoP7.PNG)

## Ejercicio

### Parte 1: Creación de las clases persistentes.
Piense en dos conceptos que estén relacionados y represente dichos conceptos como **clases persistentes**. Al ser dos conceptos relacionados un atributo de una clase hará referencia a otro atributo de la otra clase, **deberá investigar cómo implementar dicha relación**.

Ejemplo conceptual de relación: Una película tendrá de atributos id, nombre, duración, etc... Una sesión de cine tendrá los atributos id, hora, id_película donde id_película hace referencia al id de la película de la otra clase persistente.
//TODO: Captura ![.](IMAGENES/.PNG)

Una vez creadas las **clases persistentes**, lance el programa a continuación para que se creen las dos clases persistentes anteriores y haciendo uso de DBeaver muestre que las dos tablas anteriores se han creado:
//TODO: Captura ![.](IMAGENES/.PNG)

### Parte 2: Creación de las clases service.
Como hemos visto en clase, una vez que tenemos las clases persistentes necesitamos crearnos **clases “service”** que son clases que nos permiten trabajar con clases persistentes, manipularlas y realizar operaciones CRUD (insertar, leer, borrar, actualizar datos).

Deberá crear dos servicios, uno para cada clase persistente mostrada anteriormente donde se puedan realizar las siguientes operaciones (un método para cada una de las siguientes operaciones):

- Insertar (debe devolver el id del objeto insertado).
- Borrar dado un objeto (parámetro).
- Actualizar un objeto dado un id y un atributo.
(En mi ejemplo podría ser actualizar la hora de una sesión dado su id por parámetro así como la
nueva hora).
- Obtener un objeto dado un id (parámetro).
- Obtener uno o varios objetos dado un atributo diferente del id (parámetro).

(En mi ejemplo podríamos tener un método que obtenga las sesiones que empiecen a una determinada hora).
//TODO: Captura ![.](IMAGENES/.PNG)

### Parte 3: Utilización de todo lo creado hasta ahora.
A continuación vamos a probar todas las operaciones que hemos programado mediante objetos persistentes y servicios y verificaremos en base de datos dichas operaciones.

#### Parte 3.1. Operaciones de la clase no relacionada
Deberá realizar en el método main( ) las siguientes operaciones sobre la clase no relacionada (la
captura mostrada a continuación trabaja sobre mi ejemplo para que sea más claro. Debéis
aplicarlo sobre vuestro caso y poner comentarios similares):
![Referencia1](IMAGENES/Ref1P7.PNG)

#### Parte 3.2. Operaciones de la clase relacionada
Deberá realizar en el método main( ) las siguientes operaciones sobre la clase relacionada (la
captura mostrada a continuación trabaja sobre mi ejemplo para que sea más claro. Debéis
aplicarlo sobre vuestro caso y poner comentarios similares):
![Referencia2](IMAGENES/Ref2P7.PNG)

//TODO: Captura ![.](IMAGENES/.PNG)
