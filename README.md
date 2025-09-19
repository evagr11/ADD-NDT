# ADD-NDT

Repositorio de la asignatura Acceso a Datos en New Digital Talent en el ciclo de DAM

# Evaluación inicial (no calificable)

## Programación (Java)

1. Explica brevemente con tus palabras qué diferencia hay entre una clase y un objeto.
   · **Clase** : Es como una plantilla que define los atributos de algo
   · **Objeto** : Es una copia de la plantilla co valores
2. ¿Sabrías definir el concepto de **excepción** en el ámbito de la programación? Busque la sintaxis de una excepción en Java.
   
3. ¿Sabrías definir el concepto de **herencia** en el ámbito de la programación?¿Qué utilidad tiene la herencia? Busque la sintaxis para indicar que una clase hereda de otra en Java.
   Es un metodo en el que una clase puede heredar atributos de otra clase, esto nos permite reutilizar código
4. Escribe una clase Persona con los atributos nombre y edad, un constructor y un método que devuelva si la persona es mayor de edad.
  public class Person {
    public String name;
    public int age;

   public Person(String name, int age) {
        this.name = name;
        this.age = age;
   }

   public boolean Adult() {
        return age >= 18;
    }
   }

## Base de Datos (SQL)

1. ¿Por qué es interesante utilizar una base de datos frente a un documento de texto plano o un documento de Excel?
   Utilizar una base de datos ns puede aportar una mayor facilidad de busqueda y facilidad a la hora de añadir nuevos datos. Y a diferencia de un doocumento nos permite relaciones entre datos
2. ¿Sabrías definir los conceptos de **clave primaria** (primary key) y **clave foránea** (foreign key)?
   **Primary Key** : Nos permite identificar un dato de forma unica, esta clave no puede repetirse ni ser nula 
   **Foreign Key** : Hace que podamos enlazar un campo de una tabla con otra, a traves de su primary key 
3. Escribe la sentencia SQL para crear una tabla Alumnos con los campos:
  - id (entero, clave primaria)
  - nombre (texto, obligatorio)
  - edad (entero, opcional)
    ```sql
    CREATE TABLE Students {
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INT
    ```
4. Escribe la sentencia SQL para filtrar los alumnos mayores de edad de nuestra base de datos.
   ```sql
    SELECT * FROM Students WHERE age >= 18
    ```
   

