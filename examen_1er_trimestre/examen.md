# Codigos examen practico

## config.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<config> 
    <url>jdbc:postgresql://localhost:5433/ndt08371367b</url> 
    <user>postgres</user> 
    <password>admin</password> 
</config> 
```
## ConfiguracionXML.java
```java
package examen_1er_trimestre;

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
    
    static {
        try {
            // Crea un objeto File que apunta al fichero config.xml
            File file = new File("config.xml");

            // Obtiene una instancia de la fábrica de constructores de documentos XML
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            // Crea el parser DOM (DocumentBuilder) a partir de la fábrica
            DocumentBuilder builder = factory.newDocumentBuilder();
            // Parsea el fichero XML y lo carga en memoria como un objeto Document
            Document doc = builder.parse(file);
            // Normaliza la estructura del documento (elimina nodos vacíos, etc.)
            doc.getDocumentElement().normalize();

            // Configura la fábrica para ignorar espacios en blanco entre elementos
            factory.setIgnoringElementContentWhitespace(true);

            // Crea un objeto XPath para realizar consultas sobre el XML
            XPath xPath = XPathFactory.newInstance().newXPath();
            // Define la expresión XPath para seleccionar el nodo raíz <config>
            String expression = "/config";
            // Compila y evalúa la expresión XPath sobre el documento, obteniendo los nodos <config>
            NodeList nodeList = (NodeList) xPath.compile(expression).evaluate(doc, XPathConstants.NODESET);

            // Recorre todos los nodos encontrados (en este caso, los <config>)
            for (int i = 0; i < nodeList.getLength(); i++){
                // Obtiene el nodo actual
                Node nNode = nodeList.item(i);
                // Imprime el nombre del nodo actual (debería ser "config")
                System.out.println("Current Element: " + nNode.getNodeName());
                // Convierte el nodo a tipo Element para acceder a sus hijos
                Element eElement = (Element) nNode;
                // Extrae el texto del primer elemento <url> y lo asigna a la variable url
                url = eElement.getElementsByTagName("url").item(0).getTextContent();
                usuario = eElement.getElementsByTagName("user").item(0).getTextContent();
                password = eElement.getElementsByTagName("password").item(0).getTextContent();
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

## BASE DE DATOS
``` sql
CREATE TABLE ALUMNOS (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    curso INT NOT NULL
);

INSERT INTO ALUMNOS (nombre, curso)
VALUES 
('Andres', 2),
('Isra', 1),
('Nico', 2),
('Eva', 1),
('Angel', 2),
('Ana', 2),
('Adrian', 1);
```

## Alumno.java
```java
package examen_1er_trimestre;

public class Alumno {
    
    private int id;
    private String nombre;
    private int curso;

    public Alumno(int id, String nombre, int curso) {
        this.id = id;
        this.nombre = nombre;
        this.curso = curso;
    }

    public int getId() { return id; }

    public String getNombre() { return nombre; }

    public int getCurso() { return curso; }

    public void setId(int id) { this.id = id; }

    public void setNombre(String nombre) { this.nombre = nombre; }

    public void setCurso(int curso) { this.curso = curso; }

}

```

## AlumnoDAO.java
```java
package examen_1er_trimestre;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

public class AlumnoDAO {
    
    private Connection connection;
    
    public AlumnoDAO(Connection connection) {
        this.connection = connection;
    }
    
    // 1. Listar todos los datos de todos los usuarios
    public ArrayList<Alumno> findAll() {
        String sql = "SELECT * FROM ALUMNOS ORDER BY id";
        ArrayList<Alumno> alumnos = new ArrayList<>();
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            ResultSet resultSet = statement.executeQuery();
            while (resultSet.next()) {
                Alumno alumno = new Alumno(
                    resultSet.getInt("id"),
                    resultSet.getString("nombre"),
                    resultSet.getInt("curso")
                );
                alumnos.add(alumno);
            }
        } catch (SQLException sqle) {
            System.out.println("Error al listar todos los usuarios");
            sqle.printStackTrace();
        }

        return alumnos;
    }
    
    
    // 2. Añadir un nuevo usuario
    public void insertarAlumno(Alumno alumno) {
        String sql = "INSERT INTO ALUMNOS (nombre, curso) VALUES (?, ?)";
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1, alumno.getNombre());
            statement.setInt(2, alumno.getCurso());
            statement.executeUpdate();
        } catch (SQLException sqle) {
            System.out.println("Error al añadir un nuevo usuario");
            sqle.printStackTrace();
        }
    }
    
    // 3. Eliminar un usuario proporcionando su ID
    public void eliminarAlumno(int id) {
        String sql = "DELETE FROM ALUMNOS WHERE id = ?";
        try {
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setInt(1, id);
            statement.executeUpdate();
    
        } catch (SQLException sqle) {
            System.out.println("Error al eliminar usuario");
            sqle.printStackTrace();
        }
    }
    
    // 4. Listar alumnos de un curso especifico
    public ArrayList<Alumno> findByCurso(int curso) {
        String sql = "SELECT * FROM ALUMNOS WHERE curso = ?";
        // Creo una lista vacía donde se almacenarán los jugadores encontrados
        ArrayList<Alumno> alumnos = new ArrayList<>();
        try {
            // Preparo la sentencia SQL para poder asignar valores dinámicamente
            PreparedStatement statement = connection.prepareStatement(sql);
    
            // Asigno el valor del entrenadorId recibido como argumento al parámetro (?)
            statement.setInt(1, curso);
    
            // Ejecuto la consulta SELECT y obtengo el resultado en un ResultSet
            ResultSet resultSet = statement.executeQuery();
    
            // Recorro todas las filas devueltas por la consulta
            while (resultSet.next()) {
                Alumno alumno = new Alumno(
                    resultSet.getInt("id"),
                    resultSet.getString("nombre"),
                    resultSet.getInt("curso")
                );
                // Añade el jugador creado a la lista
                alumnos.add(alumno);
            }
        } catch (SQLException sqle) {
            System.out.println("Error al listar los alumnos del curso");
            sqle.printStackTrace();
        }
        return alumnos;
    }
    
}

```

## Main.java
```java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package examen_1er_trimestre;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.Scanner;

public class Main {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        Connection conn = null;
        Scanner sc = new Scanner(System.in);

        try {
            conn = DriverManager.getConnection(
                ConfiguracionXML.getUrl(),
                ConfiguracionXML.getUsuario(),
                ConfiguracionXML.getPassword()
            );
            
            AlumnoDAO alumnoDAO = new AlumnoDAO(conn);
            
            int opcion;
            do {
                mostrarMenu();
                opcion = sc.nextInt();
                sc.nextLine();

                switch (opcion) {
                    case 1: 
                        ArrayList<Alumno> alumnos = alumnoDAO.findAll();
                        System.out.println("------ LISTA DE ALUMNOS ------");
                        for (int i = 0; i < alumnos.size(); i++) {
                            Alumno alu = alumnos.get(i);
                            System.out.println(alu.getId() + " - " + alu.getNombre() +
                                " Curso: " + alu.getCurso());
                        }
                        break;
                        
                    case 2:
                        System.out.print("Nombre: ");
                        String nombreAlumno = sc.nextLine();
                        System.out.println("Recuarda que el alumno solo puede estar cursando 1 o 2 ");
                        System.out.print("Curso: ");
                        int curso = sc.nextInt();
                        if (curso != 1 && curso != 2) {
                            System.out.println("El curso no existe. Alumno no guardado, intentelo de nuevo");
                            break;
                        }
                        sc.nextLine();
                        Alumno a = new Alumno(0, nombreAlumno, curso);
                        alumnoDAO.insertarAlumno(a);
                        System.out.println("------ ALUMNO ANIADIDO CORRECTAMENTE ------");
                        break;
                    case 3:
                        System.out.print("Id del alumno que quieras eliminar: ");
                        int idDelA = sc.nextInt();
                        alumnoDAO.eliminarAlumno(idDelA);
                        System.out.println("------ ALUMNO ELIMINADO CORRECTAMENTE ------");
                        break;
                    case 4:
                        System.out.println("Recuarda que los alumnos solo pueden estar cursando 1 o 2 ");
                        System.out.print("Numero de curso: ");
                        int numCurso = sc.nextInt();
                        if (numCurso != 1 && numCurso != 2) {
                            System.out.println("El curso no existe. Intentelo de nuevo");
                            break;
                        }
                        sc.nextLine();
                        System.out.println("------ LISTA DE ALUMNOS DE " + numCurso + " ------");
                        ArrayList<Alumno> alumnosCurso = alumnoDAO.findByCurso(numCurso);
                        for (int i = 0; i < alumnosCurso.size(); i++) {
                            Alumno alum = alumnosCurso.get(i);
                            System.out.println(alum.getId() + " - " + alum.getNombre());
                        }
                        break;
                    case 0:
                        System.out.println("Saliendo...");
                        break;
                    default:
                        System.out.println("Opcion no valida");
                }

            } while (opcion != 0);
            
        } catch (SQLException sqle) {
            System.out.println("Error de conexion con la base de datos");
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
        System.out.println("===============================================================");
        System.out.println("                       QUE QUIERES HACER?                      ");
        System.out.println("===============================================================");
        System.out.println("1. Listar todos los datos de todos los usuarios");
        System.out.println("2. Aniadir un nuevo usuario");
        System.out.println("3. Eliminar un usuario proporcionando su ID");
        System.out.println("4. Listar unicamente el nombre de los usuarios de un curso especifico");
        System.out.println("0. Salir");
        System.out.print("Seleccione una opcion: ");
    }
    
    
}

```


