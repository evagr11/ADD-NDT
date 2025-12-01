# PRÁCTICA FINAL

## Tarea 1 - Configurar la conexión 

De cara a la conexión de nuestra BBDD, deberá tener en cuenta: 
- Vamos a externalizar la conexión, por lo que crearemos un archivo “config.xml” con los parámetros de configuración de nuestra conexión separados por etiquetas (url, user, password). Un ejemplo de estructura podría ser la siguiente: 
**config.xml**
```xml
<config> 
  <url>jdbc:postgresql://localhost:5432/bloodbowl</url> 
  <user>postgres</user> 
  <password>1234</password> 
</config>
```
- Deberemos leer dicho archivo .xml de cara a realizar la conexión, por lo que crearemos una clase que gestione la lectura del mismo y cargue la configuración. Se facilita el archivo “ConfiguracionXML.java” como plantilla, debiendo completar los apartados señalados con un TODO en comentarios. 

- Recuerde que es importante cerrar la conexión a nuestra BBDD una vez vayamos a finalizar la ejecución de nuestro programa.

**ConfiguracionXML.java**
```java
package practicafinal4;

/**
 *
 * @author Alumno
 */
//TODO: incluir los import necesarios
import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

public class ConfiguracionXML {

    private static String url;
    private static String usuario;
    private static String password;

    // Bloque estático para cargar la configuración al iniciar la clase sin necesidad de crear un objeto de la clase
    static {
        try {
            // TODO: Establecer la ruta relativa del fichero XML
            File file = new File("config.xml");
            
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document doc = builder.parse(file);
            doc.getDocumentElement().normalize();
            
            factory.setIgnoringElementContentWhitespace(true);
            
            XPath xPath = XPathFactory.newInstance().newXPath();
            String expression = "/config";
            NodeList nodeList = (NodeList) xPath.compile(expression).evaluate(doc, XPathConstants.NODESET);
            

            // TODO: leer el contenido del fichero XML tal y como vimos en el tema 3
            for (int i = 0; i < nodeList.getLength(); i++){
                Node nNode = nodeList.item(i);
                System.out.println("Current Element: " + nNode.getNodeName());
                Element eElement = (Element) nNode;
                url = "URL: " + eElement.getElementsByTagName("url").item(0). getTextContent();
                usuario = "USERNAME: " + eElement.getElementsByTagName("user").item(0). getTextContent();
                password = "PASSWORD: " +eElement.getElementsByTagName("password").item(0). getTextContent();
                
            }
            
        } catch (Exception e) {
            System.err.println("Error al leer el fichero config.xml");
            e.printStackTrace();
        }
    }

    public static String getUrl() {
        return url;
    }

    public static String getUsuario() {
        return usuario;
    }

    public static String getPassword() {
        return password;
    }
}

```
  
 
## Tarea 2 - Cree las tablas de BBDD 

<table>
  <tr>
    <th colspan="3" style="text-align:center;">ENTRENADOR</th>
  </tr>
  <tr>
    <th>Campo</th><th>Tipo</th><th>Descripción</th>
  </tr>
  <tr>
    <td>id</td><td>SERIAL</td><td>Identificador único</td>
  </tr>
  <tr>
    <td>nombre</td><td>VARCHAR(100)</td><td>Nombre del entrenador</td>
  </tr>
  <tr>
    <td>raza</td><td>VARCHAR(50)</td><td>Raza del entrenador: humano, orco, elfo…</td>
  </tr>
  <tr>
    <td>n_partidos</td><td>INT</td><td>Número de partidos disputados</td>
  </tr>
</table>


<table>
  <tr>
    <th colspan="3" style="text-align:center;">JUGADOR</th>
  </tr>
  <tr>
    <th>Campo</th><th>Tipo</th><th>Descripción</th>
  </tr>
  <tr>
    <td>id</td><td>SERIAL</td><td>Identificador único del jugador</td>
  </tr>
  <tr>
    <td>nombre</td><td>VARCHAR(100)</td><td>Nombre del jugador</td>
  </tr>
  <tr>
    <td>posicion</td><td>VARCHAR(50)</td><td>Posición: Corredor, Línea, Blitzer, Big Guy…</td>
  </tr>
  <tr>
    <td>herido</td><td>BOOLEAN</td><td>Valor por defecto FALSE </td>
  </tr>
  <tr>
    <td>entrenador_id</td><td>INT</td><td>Entrenador responsable del jugador </td>
  </tr>
</table>
 
De cara a la gestión de nuestra BBDD, deberá tener en cuenta: 
 
- Deberá, en caso de no existir, crear las tablas al inicio de la ejecución de su programa. Lo más sencillo es realizar una prueba al comienzo de la implementación de su programa, primero probando directamente desde DBeaver para conseguir la sentencia SQL para crear las tablas y a continuación implementarlo a nivel de código. Investigue el uso de “IF NOT EXISTS” a la hora de diseñar la sentencia SQL para crear las tablas. 
 
- El campo entrenador_id de la tabla Jugador se trata de una clave foránea de la tabla Entrenador. Recuerde que la sintaxis para declarar claves foráneas durante la creación de una tabla es:  
 
| FOREIGN KEY (columna_foranea) REFERENCES nombre_tabla_padre (columna_clave_primaria) |
|--------------------------------------------------------------------------------------|

 
- Referente nuevamente al comportamiento de esta clave foránea, establezca el comportamiento de “ON DELETE SET NULL” para que cuando se borre un entrenador de la BBDD, no se eliminen todos los jugadores asociados al mismo. Esto también se establecerá durante la creación de las tablas. 
 
- Inserte varios entrenadores y jugadores manualmente para poder realizar pruebas durante la implementación.

**Codigo de DBeaver**
```SQL
--CREACION DE TABLA
CREATE TABLE IF NOT EXISTS ENTRENADOR (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    raza VARCHAR(50) NOT NULL,
    n_partidos INT NOT NULL
);

CREATE TABLE IF NOT EXISTS JUGADOR (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    posicion VARCHAR(50) NOT NULL,
    herido BOOLEAN,
    entrenador_id INT,
    FOREIGN KEY (entrenador_id) REFERENCES ENTRENADOR(id)
    	ON DELETE SET NULL
);

--INSERTAR DATOS
INSERT INTO ENTRENADOR (nombre, raza, n_partidos)
VALUES 
('Carlos', 'Humano', 50),
('Marta', 'Elfo', 30),
('Luis', 'Orco', 20);

INSERT INTO JUGADOR (nombre, posicion, herido, entrenador_id)
VALUES
('Pedro', 'Delantero', FALSE, 1),
('Ana', 'Defensa', TRUE, 2),
('Jorge', 'Portero', FALSE, 1),
('Lucía', 'Centrocampista', FALSE, 3);
```
 
## Tarea 3 - Clases auxiliares 

Diseñe e implemente en java las clases auxiliares Entrenador y Jugador. Por agilidad, se recomienda utilizar directamente las clases facilitadas en los archivos “Entrenador.java” y “Jugador.java”, adaptándolas en caso de ser necesario. 

**Jugador.java**
```java
package practicafinal4;

public class Jugador {

    private int id;
    private String nombre;
    private String posicion;
    private boolean herido;
    private int entrenadorId;

    // Constructor
    public Jugador(int id, String nombre, String posicion, boolean herido, int entrenadorId) {
        this.id = id;
        this.nombre = nombre;
        this.posicion = posicion;
        this.herido = herido;
        this.entrenadorId = entrenadorId;
    }

    // Getters y Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public String getPosicion() { return posicion; }
    public void setPosicion(String posicion) { this.posicion = posicion; }

    public boolean isHerido() { return herido; }
    public void setHerido(boolean herido) { this.herido = herido; }

    public int getEntrenadorId() { return entrenadorId; }
    public void setEntrenadorId(int entrenadorId) { this.entrenadorId = entrenadorId; }
}

```

**Entrenador.java**
```java
package practicafinal4;

public class Entrenador {

    private int id;
    private String nombre;
    private String raza;
    private int n_partidos;

    // Constructor
    public Entrenador(int id, String nombre, String raza, int n_partidos) { 
        this.id = id;
        this.nombre = nombre;
        this.raza = raza;
        this.n_partidos = n_partidos;
    }


    // Getters y Setters
    public int getId() {
        return id;
    }
    public void setId(int id) { this.id = id; }

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public String getRaza() { return raza; }
    public void setRaza(String raza) { this.raza = raza; }

    public int getPartidos() { return n_partidos; }
    public void setPartidos(int n_partidos) { this.n_partidos = n_partidos; }
}

```

## Tarea 4 - Clases DAO 

Vamos a implementar clases DAO (Data Access Object), o dicho en otras palabras, son clases cuyo propósito es aislar la interacción con las bases de datos, y proporcionar una interfaz clara para realizar operaciones de CRUD (Create, Read, Update, Delete). Esto nos facilitará mantener y reutilizar el código, además de modificar lo relacionado con la BBDD sin tocar el resto de código. 

Antes de comenzar a implementar estas dos clases (JugadorDAO y EntrenadorDAO), se debe leer y entender el ejemplo disponible en el “ejemploDAO.java” y leer las funcionalidades solicitadas en el siguiente apartado. 

**JugadorDAO.java**
```java
package practicafinal4;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

public class JugadorDAO {
    
    private Connection connection;
    
    public JugadorDAO(Connection connection) {
        this.connection = connection;
    }
    
    // 1. Añadir jugador
    public void add(Jugador jugador) {
        String sql = "INSERT INTO JUGADOR (nombre, posicion, herido, entrenador_id) VALUES (?, ?, ?, ?)";
        //Evitar inyeccion SQL
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1, jugador.getNombre());
            statement.setString(2, jugador.getPosicion());
            statement.setBoolean(3, jugador.isHerido());
            statement.setInt(4, jugador.getEntrenadorId());
            statement.executeUpdate();
        } catch (SQLException sqle) {
            System.out.println("No se ha podido conectar con el servidor de base de datos. Comprueba que los datos son correctos y que el servidor se ha iniciado");
            sqle.printStackTrace();
        }
    }
    
    // 2. Eliminar jugador
    public void delete(String nombre) {
        String sql = "DELETE FROM JUGADOR WHERE nombre = ?";
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1, nombre);
            statement.executeUpdate();
    
        } catch (SQLException sqle) {
            System.out.println("No se ha podido conectar con el servidor de base de datos. Comprueba que los datos son correctos y que el servidor se ha iniciado");
            sqle.printStackTrace();
        }
    }
    
    // 4. Listar jugadores de un entrenador
    public ArrayList<Jugador> findByEntrenador(int entrenadorId) {
        String sql = "SELECT * FROM JUGADOR WHERE entrenador_id = ?";
        ArrayList<Jugador> jugadores = new ArrayList<>();
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setInt(1, entrenadorId);
            ResultSet resultSet = statement.executeQuery();
            while (resultSet.next()) {
                Jugador jugador = new Jugador(
                resultSet.getInt("id"),
                resultSet.getString("nombre"),
                resultSet.getString("posicion"),
                resultSet.getBoolean("herido"),
                resultSet.getInt("entrenador_id")
                );
                jugadores.add(jugador);
            }
        } catch (SQLException sqle) {
            System.out.println("Error al listar jugadores");
            sqle.printStackTrace();
        }
        return jugadores;
    }
    
    // 5. Lesionar jugador
    public void lesionar(int id) {
        String sql = "UPDATE JUGADOR SET herido = TRUE WHERE id = ?";
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setInt(1, id);
            statement.executeUpdate();
        } catch (SQLException sqle) {
            System.out.println("Error al lesionar jugador");
            sqle.printStackTrace();
        }
    }
    
}
```

**EntrenadorDAO.java**
```java
package practicafinal4;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

public class EntrenadorDAO {
        private Connection connection;
    
    public EntrenadorDAO(Connection connection) {
        this.connection = connection;
    }
    
    // 1. Añadir nuevo entrenador
    public void insertarEntrenador(Entrenador entrenador) {
        String sql = "INSERT INTO ENTRENADOR (nombre, raza, n_partidos) VALUES (?, ?, ?)";
        //Evitar inyeccion SQL
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1, entrenador.getNombre());
            statement.setString(2, entrenador.getRaza());
            statement.setInt(3, entrenador.getPartidos());
            statement.executeUpdate();
        } catch (SQLException sqle) {
            System.out.println("No se ha podido conectar con el servidor de base de datos. Comprueba que los datos son correctos y que el servidor se ha iniciado");
            sqle.printStackTrace();
        }
    }
    
    // 2. Eliminar entrenador
    public void eliminarEntrenador(String nombre) {
        String sql = "DELETE FROM ENTRENADOR WHERE nombre = ?";
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1, nombre);
            statement.executeUpdate();
    
        } catch (SQLException sqle) {
            System.out.println("No se ha podido conectar con el servidor de base de datos. Comprueba que los datos son correctos y que el servidor se ha iniciado");
            sqle.printStackTrace();
        }
    }
    
    // 3. Listar todos los entrenadores
    public ArrayList<Entrenador> findAll() {
        String sql = "SELECT * FROM ENTRENADOR ORDER BY nombre";
        ArrayList<Entrenador> ENTRENADOR = new ArrayList<>();
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            ResultSet resultSet = statement.executeQuery();
            while (resultSet.next()) {
                Entrenador entrenador = new Entrenador(
                resultSet.getInt("id"),
                resultSet.getString("nombre"),
                resultSet.getString("raza"),
                resultSet.getInt("n_partidos")
                );
                ENTRENADOR.add(entrenador);
            }
        } catch (SQLException sqle) {
            System.out.println("No se ha podido conectar con el servidor de base de datos. Comprueba que los datos son correctos y que el servidor se ha iniciado");
            sqle.printStackTrace();
        }

        return ENTRENADOR;
    }
    
}
```


## Tarea 5 - Clase Main como menú 

Debe unificar el uso de las diversas clases que hemos estado desarrollando previamente. 

Las funcionalidades solicitadas serán: 
1. Añadir un nuevo entrenador o jugador. 
2. Eliminar un entrenador o jugador. 
3. Listar todos los entrenadores. 
4. Listar todos los jugadores asociados a un entrenador. 
5. Lesionar a un jugador (modifica el valor de herido para que sea TRUE). 
6. OPCIONAL: Jugar un partido (suma 1 al n_partidos de dos entrenadores).

**Main.java**
```java
package practicafinal4;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Connection conn = null;
        Scanner sc = new Scanner(System.in);

        try {
            conn = DriverManager.getConnection(
                ConfiguracionXML.getUrl(),
                ConfiguracionXML.getUsuario(),
                ConfiguracionXML.getPassword()
            );

            EntrenadorDAO entrenadorDAO = new EntrenadorDAO(conn);
            JugadorDAO jugadorDAO = new JugadorDAO(conn);

            int opcion;
            do {
                mostrarMenu();
                System.out.print("Seleccione una opción: ");
                opcion = sc.nextInt();
                sc.nextLine();

                switch (opcion) {
                    case 1: // Submenú añadir
                        System.out.println("¿Qué desea añadir?");
                        System.out.println("1. Entrenador");
                        System.out.println("2. Jugador");
                        int subAdd = sc.nextInt();
                        sc.nextLine();
                        if (subAdd == 1) {
                            System.out.print("Nombre: ");
                            String nombreE = sc.nextLine();
                            System.out.print("Raza: ");
                            String raza = sc.nextLine();
                            System.out.print("Número de partidos: ");
                            int partidos = sc.nextInt();
                            sc.nextLine();
                            Entrenador e = new Entrenador(0, nombreE, raza, partidos);
                            entrenadorDAO.insertarEntrenador(e);
                        } else if (subAdd == 2) {
                            System.out.print("Nombre: ");
                            String nombreJ = sc.nextLine();
                            System.out.print("Posición: ");
                            String posicion = sc.nextLine();
                            System.out.print("¿Está herido? (true/false): ");
                            boolean herido = sc.nextBoolean();
                            System.out.print("ID del entrenador: ");
                            int entrenadorId = sc.nextInt();
                            sc.nextLine();
                            Jugador j = new Jugador(0, nombreJ, posicion, herido, entrenadorId);
                            jugadorDAO.add(j);
                        }
                        break;

                    case 2: // Submenú eliminar
                        System.out.println("¿Qué desea eliminar?");
                        System.out.println("1. Entrenador");
                        System.out.println("2. Jugador");
                        int subDel = sc.nextInt();
                        sc.nextLine();
                        if (subDel == 1) {
                            System.out.print("Nombre del entrenador: ");
                            String nombreDelE = sc.nextLine();
                            entrenadorDAO.eliminarEntrenador(nombreDelE);
                        } else if (subDel == 2) {
                            System.out.print("Nombre del jugador: ");
                            String nombreDelJ = sc.nextLine();
                            jugadorDAO.delete(nombreDelJ);
                        }
                        break;

                    case 3: // Listar entrenadores
                        ArrayList<Entrenador> entrenadores = entrenadorDAO.findAll();
                        for (int i = 0; i < entrenadores.size(); i++) {
                            Entrenador ent = entrenadores.get(i);
                            System.out.println(ent.getId() + " - " + ent.getNombre() +
                                " (" + ent.getRaza() + ") Partidos: " + ent.getPartidos());
                        }
                        break;

                    case 4: // Listar jugadores de un entrenador
                        System.out.print("ID del entrenador: ");
                        int idEnt = sc.nextInt();
                        sc.nextLine();
                        ArrayList<Jugador> jugadores = jugadorDAO.findByEntrenador(idEnt);
                        for (int i = 0; i < jugadores.size(); i++) {
                            Jugador jug = jugadores.get(i);
                            System.out.println(jug.getId() + " - " + jug.getNombre() +
                                " (" + jug.getPosicion() + ") Herido: " + jug.isHerido());
                        }
                        break;

                    case 5: // Lesionar jugador
                        System.out.print("ID del jugador a lesionar: ");
                        int idJug = sc.nextInt();
                        sc.nextLine();
                        jugadorDAO.lesionar(idJug);
                        break;

                    case 0:
                        System.out.println("Saliendo...");
                        break;

                    default:
                        System.out.println("Opción no válida");
                }

            } while (opcion != 0);

        } catch (SQLException sqle) {
            System.out.println("Error de conexión con la base de datos");
            sqle.printStackTrace();
        } finally {
            try {
                if (conn != null) conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            sc.close();
        }
    }

    public static void mostrarMenu() {
        System.out.println("\n--- MENÚ BLOOD BOWL ---");
        System.out.println("1. Añadir (entrenador/jugador)");
        System.out.println("2. Eliminar (entrenador/jugador)");
        System.out.println("3. Listar entrenadores");
        System.out.println("4. Listar jugadores de un entrenador");
        System.out.println("5. Lesionar jugador");
        System.out.println("6. Jugar partido (opcional)");
        System.out.println("0. Salir");
        System.out.print("Seleccione una opción: ");
    }
}

```
