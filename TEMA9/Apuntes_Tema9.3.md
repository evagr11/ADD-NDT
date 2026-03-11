# TEMA 9.3: MongoDB Atlas y Java (SpringBoot)

## 1. Creación de un proyecto SpringBoot + MongoDB

- **Herramienta de inicio**  
    - Para crear la estructura base del proyecto se utiliza la web **Spring Initializr** (start.spring.io).

- **Dependencia clave**  
    - Es fundamental añadir la dependencia **Spring Data MongoDB**.

- **Propósito de la dependencia**  
    - Permite que la aplicación almacene datos en documentos flexibles tipo **JSON**.  
    - Los campos pueden variar entre documentos y la estructura puede evolucionar con el tiempo sin las restricciones de un esquema rígido.

## 2. Creación de usuario y clúster en MongoDB Atlas

- **Acceso a la plataforma**  
    - La creación del usuario y la gestión del entorno se realizan a través de la web oficial de **MongoDB Atlas**.

- **Asignación del Clúster**  
    - Durante el proceso de creación de la cuenta, el sistema asigna automáticamente un **clúster** para alojar la base de datos.

- **Obtención del String de Conexión**  
    - Una vez configurados el usuario y el clúster, la plataforma proporciona un **string de conexión**.

- **Finalidad**  
    - Esta cadena de texto es la pieza clave que permite vincular la base de datos en la nube con la aplicación de **SpringBoot**, permitiendo la comunicación entre ambos sistemas.

## 3. Conexión con MongoDB Atlas

- **Configuración en el proyecto**  
    - Para establecer el vínculo, se debe añadir el **string de conexión** a la propiedad  
      `spring.data.mongodb.uri` dentro del archivo **application.properties** de SpringBoot.

- **Estructura de la cadena de conexión**  
    - El formato estándar proporcionado por el proveedor es:  
      ```
      mongodb+srv://<usuario>:<contraseña>@<cluster>.mongodb.net/<nombre BaseDatos>?retryWrites=true&w=majority
      ```

- **Identificación y Seguridad**  
    - Esta cadena funciona como identificador de la base de datos y como método de autenticación para reconocer al usuario que intenta acceder.

- **Ejecución**  
    - Una vez configurado este parámetro, al lanzar la aplicación, esta se conectará automáticamente a la base de datos alojada en la nube.

## 4. Anotación @Document

- **Propósito**  
    - Esta anotación se coloca a nivel de clase para indicar que dicha clase representa un **documento** que se va a persistir en la base de datos.

- **Equivalencia**  
    - Es el concepto homólogo a la anotación **@Entity** utilizada en bases de datos relacionales.

- **Definición del modelo**  
    - Al usarla, estamos definiendo un **modelo de datos** para una colección de documentos.

- **Personalización de la colección**  
    - Si se usa simplemente como `@Document`, MongoDB asume que el nombre de la colección coincide con el nombre de la clase.  
    - Permite especificar un nombre distinto para la colección mediante el parámetro `collection`, por ejemplo:  
      ```java
      @Document(collection = "usuarios")
      ```

### 4.1 Anotación @Id

- **Propósito**  
    - Esta anotación indica qué atributo de la clase funcionará como la **clave primaria** del documento.

- **Compatibilidad**  
    - Es la misma anotación que se utiliza habitualmente en las bases de datos relacionales.

- **Generación automática**  
    - En MongoDB Atlas, las claves primarias se generan automáticamente sin necesidad de especificar un modo de generación (como *identity* o *sequence* en SQL).

- **Opcionalidad**  
    - A diferencia de las bases de datos relacionales, en MongoDB tener un campo marcado explícitamente con `@Id` es **opcional**.

### 4.2 Anotación @Field

- **Propósito**  
    - Se utiliza para indicar explícitamente el nombre con el que queremos que MongoDB guarde un campo específico del documento en la base de datos.

- **Funcionamiento**  
    - Si **no** se usa la anotación, MongoDB guardará el campo utilizando exactamente el mismo nombre que tiene el atributo en la clase Java.  
    - Si **sí** se usa, podemos mapear un nombre de variable en Java a un nombre distinto en la colección de la base de datos.

- **Ejemplos prácticos**

    - **Con @Field**  
      ```java
      @Field("email")
      String emailAddress;
      ```
      El dato se almacenará en la base de datos bajo la clave `"email"`.

    - **Sin @Field**  
      ```java
      String emailAddress;
      ```
      El dato se guardará bajo la clave `"emailAddress"`.

### 4.3 Reglas de las clases documento

- **Privacidad de atributos**  
    - Todos los atributos de la clase deben ser **privados**.

- **Encapsulamiento**  
    - Es obligatorio que los atributos cuenten con sus respectivos **métodos getters y setters**.

- **Constructores**  
    - Se debe incluir un **constructor con parámetros** para facilitar la creación de objetos.  
    - Es imprescindible contar con un **constructor sin parámetros** (constructor vacío) para que los frameworks puedan instanciar la clase internamente.

- **Uso del ID**  
    - A diferencia de las bases de datos relacionales, en MongoDB es **opcional** tener un campo marcado explícitamente con la anotación `@Id`.

## 5. Repositorios: MongoRepository

- **Definición**  
    - Es una interfaz que proporciona métodos de acceso a la base de datos MongoDB para realizar operaciones **CRUD** (crear, leer, actualizar y eliminar) de manera sencilla.

- **Propósito**  
    - Simplifica la interacción con la base de datos y evita la necesidad de escribir consultas manuales.

- **Implementación**  
    - Se debe crear una interfaz que extienda de `MongoRepository<Entidad, TipoId>`.  
    - Ejemplo:  
      ```java
      public interface UserRepository extends MongoRepository<User, String> {}
      ```

- **Uso en el código**  
    - Para utilizar el repositorio en la aplicación, se emplea la anotación **@Autowired** para realizar la inyección de dependencia.

---

### Métodos incluidos por defecto

Al heredar de `MongoRepository`, se obtienen automáticamente numerosos métodos ya implementados, como:

- `count()` : Devuelve el total de entidades en la base de datos.  
- `save()` : Guarda un documento nuevo o actualiza uno existente.  
- `findById(id)` : Busca un documento por su clave primaria.  
- `findAll()` : Recupera todos los documentos de la colección.  
- `delete()` / `deleteAll()` : Elimina documentos de la base de datos.

---

### Consultas personalizadas (Keywords)

A pesar de los métodos por defecto, es posible añadir métodos propios utilizando **palabras clave (keywords)**.  
Spring Boot interpreta el nombre del método y genera la consulta automáticamente.

#### 1. Palabras clave para operaciones

- `findBy...` : Para búsquedas.  
- `deleteBy...` : Para borrados.  
- `countBy...` : Para contar registros que cumplan una condición.

**Nota sobre actualizaciones**  
- No existe una keyword para actualizar.  
- Para modificar un documento existente, se usa `.save()` sobre un objeto que ya tenga un ID, reemplazando el documento anterior.

#### 2. Palabras clave para condiciones

Se pueden combinar para filtrar datos:

- **Lógicas** : `And`, `Or`  
- **Comparativas** : `Between`, `GreaterThan`, `LessThan`  
- **De contenido** : `Containing`, `Null`, `NotNull`, `Empty`  
- **Booleanas** : `True`, `False`, `IsFalse`

---

### Ejemplos de métodos personalizados

```java
List<User> findByAgeGreaterThan(int age);

List<User> findByAgeBetween(int min, int max);

void deleteByEmailAddressContaining(String keyword);
