# TEMA 7.2: Explorando el mapeo objeto relacional

Enlaces de apoyo: 

- TODO : [Diferencia entre ODBC y JDBC en Java](https://es.differkinome.com/articles/database/difference-between-odbc-and-jdbc.html)

## Clases persistentes
Las **clases persistentes** son aquellas que nos permiten guardar la información de sus objetos directamente en una base de datos. Su objetivo principal es servir de puente **para almacenar los atributos de un objeto** en un sistema de almacenamiento relacional.

### Reglas para conseguir una clase persistente
Para que una clase en Java pueda ser tratada como persistente por un ORM (como Hibernate), debe cumplir obligatoriamente con las siguientes tres reglas:
1. **Constructor por defecto**: La clase debe tener un constructor sin argumentos.
2. **Atributo "id"**: Debe incluir un atributo que funcione como la **clave primaria** en la base de datos.
3. **Encapsulamiento**: Todos los atributos deben ser **privados** y deben definirse sus correspondientes métodos **get() y set()** para permitir el acceso a los datos.

### Implementación mediante Anotaciones
En el código, estas clases se identifican y configuran utilizando anotaciones específicas que le indican al framework cómo debe realizar el mapeo:
- ```@Entity```: Se coloca sobre la definición de la clase para indicar que es una clase persistente.
- ```@Table(name="nombre")```: Especifica el nombre exacto que tendrá la tabla en la base de datos.
- ```@Id```: Identifica cuál de los atributos de la clase es la clave primaria.
- ```@GeneratedValue```: Se utiliza junto al ID para indicar que la clave primaria debe **generarse automáticamente** (frecuentemente usando la estrategia GenerationType.IDENTITY).

Como ejemplo práctico, una clase Pelicula tendría sus atributos (como título, director o duración) mapeados como **columnas de la tabla**, mientras que cada instancia de esa película se guardaría como una fila.

## Configuración del proyecto

### A. Preparación de la Base de Datos (PostgreSQL)
Antes de programar, es necesario tener listo el motor de almacenamiento:
- **Instalación**: Se requiere tener instalado **PostgreSQL**.
- **Creación de usuario y BD**: Se debe crear un usuario (por ejemplo, ```postgres``` con contraseña ```postgres```) y una base de datos específica, que en el ejemplo se denomina ```acceso_a_datos```.
- **Gestión**: Se recomienda el uso de **DBeaver** como sistema gestor de bases de datos para conectarse y visualizar las tablas creadas.

### B. Creación del Proyecto con Spring Initializr
Para generar la estructura base del proyecto, se utiliza la herramienta web start.spring.io:
- **Proyecto y Lenguaje**: Se selecciona un proyecto **Maven** con lenguaje **Java** (versión 17 en el ejemplo).
- **Dependencias fundamentales**:
  - **Spring Data JPA**: Para la persistencia de datos en almacenes SQL usando Hibernate.
  - **Spring Web**: Para construir aplicaciones web y RESTful.
  - **PostgreSQL Driver**: Permite que los programas Java se conecten a la base de datos PostgreSQL.

### C. Configuración en el IDE y ```application.properties```
Una vez descargado el proyecto, se abre en un IDE (como **Eclipse**) y se procede a configurar el archivo de propiedades:
- **Archivo ```application.properties```**: Es donde se definen los parámetros de conexión. Los ajustes típicos incluyen:
  - ```spring.datasource.url```: La dirección de la BD (```jdbc:postgresql://localhost:5432/acceso_a_datos```).
  - ```spring.datasource.username``` y ```password```: Credenciales de acceso.
  - ```spring.jpa.hibernate.ddl-auto=update```: Indica a Hibernate que **actualice automáticamente** el esquema de la base de datos según las clases Java.
  - ```spring.jpa.show-sql=true```: Permite ver las sentencias SQL que Hibernate genera en la consola.

### D. Ejecución del Proyecto
Para poner en marcha la aplicación, se debe configurar la ejecución a través de Maven utilizando el comando (goal) ```spring-boot:run```

## Ejemplo de clase persistente: La clase ```Pelicula```
Para transformar una clase común de Java en una entidad que Hibernate pueda manejar, se siguen estos pasos:
- **Anotaciones de Clase:**
  - **```@Entity```**: Indica que la clase ```Pelicula``` es una clase persistente.
  - **```@Table(name="peliculas")```**: Especifica que la tabla en la base de datos se llamará exactamente "peliculas".

- **Atributos y Mapeo de Columnas**: Los atributos de la clase se convertirán en las **columnas** de la tabla.
  - **Clave Primaria**: Se define un atributo ```id``` (de tipo ```Long```) marcado con **```@Id```**. La anotación **```@GeneratedValue(strategy = GenerationType.IDENTITY)```** se usa para que la base de datos genere este número automáticamente.
  - **Otros atributosv: Se incluyen ```titulo``` (String), ```director``` (String) y ```duracion``` (int).

- **Estructura Interna:**
  - **Constructores**: Es obligatorio crear un **constructor vacío** (por defecto) y es recomendable uno con parámetros para facilitar la creación de objetos.
  - **Encapsulamiento**: Se deben definir todos los métodos **```get()```** y **```set()```** para cada atributo.

### Inserción de datos y la Capa de Servicio
Una vez definida la clase, se utiliza una clase de servicio para gestionar las operaciones con la base de datos:
- **Clase Service**: Se crea ```PeliculaService``` anotada con **```@Service```**. Esta clase contiene la lógica para trabajar con la entidad.
- **SessionFactory**: Mediante la anotación **```@Autowired```**, se obtiene la configuración de la base de datos definida previamente en el archivo ```application.properties```.
- **El Objeto Sesión (Session)**: Es el componente que establece la conexión real.
  - Permite insertar, borrar y leer datos.
  - **Seguridad**: No deben permanecer abiertos mucho tiempo; se deben crear y destruir (cerrar) cada vez que se utilicen.

### Proceso de inserción (```insertarPelicula```)
Para guardar un objeto en la base de datos, el servicio sigue este flujo lógico:
1. **Abrir sesión**: ```sessionFactory.openSession()```.
2. **Iniciar transacción**: Se indica que va a comenzar una operación.
3. **Persistir**: Se utiliza el método **```session.persist(pelicula)```** para guardar el objeto.
4. **Confirmar (Commit)**: Se confirma la operación para que los cambios sean permanentes.
5. **Gestión de errores**: Si algo falla, se realiza un **```rollback()```** para revertir la operación y evitar fallos en la base de datos. Finalmente, la sesión se cierra en un bloque ```finally```.
Si lanzas la aplicación correctamente, Hibernate creará automáticamente la tabla en tu base de datos PostgreSQL, la cual podrás visualizar mediante herramientas como DBeaver.

## Lectura, almacenamiento y modificación de objetos
- **Almacenamiento (```.persist```)**: Para guardar un objeto nuevo en la base de datos, se utiliza el método **```.persist(Entity entity)```**. Este método toma la instancia de la clase Java y la prepara para ser insertada como una nueva fila en su tabla correspondiente.
- **Lectura (```.get```)**: Cuando necesitamos recuperar la información de un registro específico, usamos **```.get(Class, Long id)```**. Este método requiere dos parámetros: el tipo de clase (ej. ```Pelicula.class```) y el **identificador único (ID)** o clave primaria del registro que queremos obtener.
- **Borrado (```.delete```)**: Para eliminar un registro de la base de datos, se emplea el método **```.delete(Entity entity)```**. Al pasarle el objeto que deseamos borrar, Hibernate se encarga de ejecutar la sentencia de eliminación correspondiente en SQL.
- **Actualización (```.merge```)**: Si un objeto ya existe en la base de datos pero sus atributos han cambiado en el código Java, se utiliza **```.merge(Entity entity)```** para sincronizar esos cambios y actualizar el registro en la tabla.

## CriteriaBuilder, CriteriaQuery & Root
Estas clases forman parte de una API que permite **construir consultas mucho más complejas** que las búsquedas simples por ID. Su uso sigue un flujo lógico dividido en varios componentes:
- **CriteriaBuilder**: Es la clase base que permite construir la consulta y definir los criterios de búsqueda. Se obtiene directamente desde el objeto sesión mediante el método ```session.getCriteriaBuilder()```.
- **CriteriaQuery**: Se utiliza para indicar específicamente el tipo de resultado que esperamos obtener. Por ejemplo, al usar ```criteriaBuilder.createQuery(Pelicula.class)```, le estamos indicando a Hibernate que vamos a realizar una consulta sobre la entidad **Pelicula**.
- **Root**: Este componente define la **raíz de la consulta**. Es el equivalente al "FROM" de SQL, ya que indica de qué entidad o tabla se van a extraer los datos inicialmente.
- **Añadir condiciones**: Una vez definida la estructura, se pueden aplicar filtros específicos utilizando métodos del builder. En el ejemplo de la presentación, se utiliza ```criteriaBuilder.lessThan``` para filtrar películas cuya **"duracion"** sea menor a un número determinado de minutos.
- **Ejecución de la consulta**: Finalmente, para obtener los datos, se utiliza ```session.createQuery(criteriaQuery).getResultList()```. Este método ejecuta la lógica construida y devuelve una **lista de objetos** (ej. ```List<Pelicula>```) con los registros que cumplen las condiciones.

Al igual que en las operaciones de inserción o modificación, todo este proceso debe estar envuelto en un bloque **try-catch-finally** para garantizar que, independientemente de si la consulta tiene éxito o falla, la sesión se cierre correctamente al terminar.
