# BASE DE DATOS

## Previa
### Parte previa 1.
Investigue cómo instalar e instale PostgreSQL en su ordenador. Tras haber instalado PostgreSQL demuestre que puede acceder a través de la terminal a sus servicios haciendo uso del comando *“psql postgres”* y **realice una captura de pantalla**.

![funcionaSQL](IMAGENES/fuincionaSQL.PNG)

### Parte previa 2.
Acceda a postgreSQL haciendo uso del comando “psql postgres” y cree un usuario con este nombre y contraseña “postgres” tal y como se muestra a continuación:

| ``` postgras=# CREATE USER postgres WITH PASSWORD nombre; ``` |
|--------------------------------------------------------------------------------------|

A continuación cree una base de datos llamada “tema4” haciendo uso del siguiente comando en la interfaz de PostgresSQL:

| ``` postgras=# CREATE DATABASE nombredb; ``` |
|--------------------------------------------------------------------------------------|

En este punto, haciendo uso de la terminal y comandos similares, podríamos crear tablas y realizar consultas pero es bastante tedioso lidiar con una terminal. Por ello vamos a utilizar una interfaz gráfica que nos permita manipular y consultar nuestras bases de datos. Para ello nos descargaremos DBeaver: https://dbeaver.io/

Investigue cómo conectarse a la base de datos “tema4” desde DBeaver e **incluya una captura de pantalla** que demuestre la conexión. 

![ConfiguracionBBDDSQL](IMAGENES/ConfiguracionBBDDSQL.PNG)

A continuación deberá crear una tabla con la siguiente sentencia y añadir un par de datos de ejemplo (use sus datos personales como ejemplo). 
``` sql
CREATE TABLE Usuarios (
    ID SERIAL PRIMARY KEY,
    USERNAME VARCHAR(100) NOT NULL,
    PASSWORD VARCHAR(100) NOT NULL,
    NOMBRE VARCHAR(100)
);
```
Demuestre con una consulta SELECT la creación de la tabla y **realice una captura de pantalla**.

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

Para realizar la conexión deberá utilizar el siguiente código:

```java
private static final String URL = "jdbc:postgresql://localhost:5433/nombredb;
private static final String USER = "nombre";
private static final String PASSWORD = "password";

Connection conn = null;
conn = DriverManager.getConnection(URL, USER, PASSWORD);
```

![Conexion](IMAGENES/Conexion.PNG)


### Parte 3.
Una vez establecida la conexión podemos utilizar el siguiente código para mostrar la información de la BDD:

```java
        Statement stmt = null;
        ResultSet rs = null;
        
        stmt = (Statement) conn.createStatement();
        String query = "SELECT * FROM Usuario";
        rs = ((java.sql.Statement) stmt).executeQuery(query);
        
        while (rs.next()){
            int ID = rs.getInt("ID");
            String USERNAME = rs.getString("USERNAME");
            String PASSWORD = rs.getString("PASSWORD");
            String NOMBRE = rs.getString("NOMBRE");
            System.out.println("ID: " + ID + 
                                ", Username: " + USERNAME +
                                ", Password: " + PASSWORD +
                                ", Nombre: " + NOMBRE);
        }
```

Explique con sus palabras que hace el código anterior línea a línea:

Realice una captura de pantalla el resultado obtenido por consola:

![ResultadoConsola](IMAGENES/ResultadoConsola.PNG)

Incluya el código completo:
```java
package tema4_aadd;

import java.sql.Statement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.ResultSet;

/**
 * Archivo para mostrar por consola todos los usuarios de la base de datos
 * @author Eva Gallardo Romero
 * @version 1.0
 */
public class Main {

    private static final String URL = "jdbc:postgresql://localhost:5433/tema4"; // URL de conexión JDBC a PostgreSQL (host, puerto y base de datos)
    private static final String USER = "postgres"; // Usuario de la base de datos
    private static final String PASSWORD = "admin"; // Contraseña del usuario de la base de datos
        
    public static void main(String[] args) throws SQLException {
        Connection conn = null; // Declara la referencia a la conexión e inicializa a null
        try{
            conn = DriverManager.getConnection(URL, USER, PASSWORD); // Obtiene la conexión usando la URL, usuario y contraseña
            System.out.println("Conexion establecida");
        }catch (SQLException e){
            System.out.println("Conexion fallida");
            System.out.println("Error: " + e.toString());
        }
        
        Statement stmt = null; // Declara la referencia al Statement para ejecutar consultas
        ResultSet rs = null; // Declara la referencia al ResultSet para almacenar resultados
        
        stmt = (Statement) conn.createStatement(); // Crea un Statement a partir de la conexión para ejecutar SQL
        String query = "SELECT * FROM Usuario"; // Define la consulta SQL para seleccionar todas las filas de la tabla Usuario
        rs = ((java.sql.Statement) stmt).executeQuery(query); // Ejecuta la consulta y asigna el resultado al ResultSet
        
        while (rs.next()){ // Itera por cada fila del ResultSet mientras existan resultados
            // Obtienen el resultado de la fila/columna 
            int ID = rs.getInt("ID"); 
            String USERNAME = rs.getString("USERNAME");
            String PASSWORD = rs.getString("PASSWORD");
            String NOMBRE = rs.getString("NOMBRE");
            // Hace el print de los resultados obtenidos
            System.out.println("ID: " + ID +  
                                ", Username: " + USERNAME + 
                                ", Password: " + PASSWORD + 
                                ", Nombre: " + NOMBRE); 
        }
    }
}
```
