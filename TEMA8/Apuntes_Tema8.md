# TEMA 8: Implementación de APIs en Java

Enlaces de apoyo: 

- TODO : [Diferencia entre ODBC y JDBC en Java](https://es.differkinome.com/articles/database/difference-between-odbc-and-jdbc.html)

## Controllers (Controladores)

En el ecosistema de **Spring Boot**, el controlador es la pieza fundamental para construir una API, ya que actúa como la puerta de entrada a la aplicación.

- **Definición**: Un controlador es una clase de Java cuya función principal es **manejar las peticiones HTTP** que llegan desde los clientes (como un navegador o una aplicación móvil).

- **Identificación**: Para que Spring Boot reconozca una clase como un controlador de API, se debe utilizar la anotación **```@RestController```** justo encima de la definición de la clase.

En esencia, el controlador "escucha" lo que el cliente pide y decide qué parte del código debe ejecutarse para dar una respuesta adecuada.

## RequestMapping
La anotación **```@RequestMapping```** es fundamental en Spring Boot porque permite definir el **recurso específico** que va a gestionar un determinado controlador.

- **Ubicación y sintaxis**: Se coloca justo encima de la definición de la clase, siguiendo el formato ```@RequestMapping("/nombreRecurso")```. Por ejemplo, en un controlador para gestionar películas, se utilizaría ```@RequestMapping("/peliculas")```.

- **Propósito**: Sirve para establecer la ruta base de la URL a la que responderá el controlador.

### Buenas prácticas para nombrar recursos
Para que una API sea profesional y fácil de entender, las fuentes recomiendan seguir estas reglas al definir el nombre del recurso en el ```@RequestMapping```:

1. **Uso de sustantivos**: Los recursos deben ser nombres (ej. peliculas), no verbos.
2. **Letras en minúscula**: Se debe evitar el uso de mayúsculas.
3. **Evitar caracteres especiales**: No se deben incluir símbolos que puedan causar problemas en las URLs.
4. **Separar palabras con guiones ("-")**: Si el nombre del recurso tiene más de una palabra, se deben unir mediante guiones.

## Servicios en controladores
Dentro de los controladores de Spring Boot, es fundamental integrar los servicios para llevar a cabo las operaciones reales sobre los datos.

- **Propósito**: El controlador se encarga de recibir la petición HTTP, pero delega en el servicio la responsabilidad de **insertar, borrar, actualizar y obtener datos** de la base de datos.
- Inyección de dependencias: Para utilizar un servicio dentro de un controlador, se utiliza la anotación **```@Autowired```**. Esto permite que Spring inyecte automáticamente la instancia necesaria del servicio (por ejemplo, ```PeliculaService```) para que el controlador pueda llamar a sus métodos.

Como se observa en el ejemplo de las fuentes, la estructura típica incluye la declaración del servicio como un atributo privado de la clase controlador, marcado con la anotación de inyección mencionada.

## El objeto ResponseEntity
Cuando una aplicación realiza una petición a una API, esta siempre debe devolver una respuesta. En Spring Boot, el objeto ResponseEntity es la herramienta estándar para gestionar este retorno, permitiendo controlar con precisión lo que el cliente recibe.

### Variantes de la respuesta
Todos los métodos de los controladores deben devolver un objeto de este tipo, el cual acepta uno o dos parámetros dependiendo de la información que se desee enviar:
1. **Estado HTTP**: Se devuelve únicamente el código de estado (por ejemplo, para confirmar que una acción se realizó aunque no haya datos que mostrar).
2. **Datos + Estado HTTP**: Se devuelven los datos solicitados (como un objeto o una lista) junto con el código de estado correspondiente.

### El objeto HttpStatus
El estado que devuelve la API depende de la operación realizada y del resultado de la misma. Para ello, se utiliza el objeto HttpStatus, que contiene todos los estados posibles mediante enumerados. Algunos de los más comunes mencionados en las fuentes son:

- **OK (200)**: La operación se realizó correctamente (ej. al obtener o actualizar un dato).
- **CREATED (201)**: Se ha creado con éxito un nuevo recurso en la base de datos.
- **NO_CONTENT (204)**: La operación fue exitosa pero la respuesta no envía contenido.
- **NOT_FOUND (404)**: El recurso solicitado no se ha encontrado en el sistema.

En resumen, el uso de ResponseEntity asegura que el cliente de la API sepa exactamente qué ha ocurrido con su petición a través de los códigos de estado estándar de la web.