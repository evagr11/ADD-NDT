# 📑 Índice de contenidos del repositorio ADD-NDT

## 🟦 TEMA 1
- **Ejercicio_Final1 (Biblioteca con directorios y ficheros)**

  ➝ `ADD-NDT/TEMA1/Ejercicio_Final1.md`

  - Creación de directorios y subdirectorios.

  - Creación de archivos `catalogo.txt`.

  - Escritura/lectura con bytes (Novela).

  - Escritura/lectura con caracteres (Poesía).

  - Escritura/lectura con acceso aleatorio (Ciencia).

  - Listado de contenido de la biblioteca.


- **Ejercicios Tema 1 (cine_granada y días de la semana)**

   ➝ `ADD-NDT/TEMA1/Ejercicio_Tema1.md`

  - Ejercicio 1: Crear directorio `cine_granada`.

  - Ejercicio 2: Crear directorios de días de la semana.

  - Ejercicio 3: Mover directorios dentro de `cine_granada`.

  - Ejercicio 4: Comprobaciones de existencia.

  - Ejercicio 5: Mostrar ruta absoluta al crear.

  - Ejercicio 6: Listar archivos creados.

  - Ejercicio 7: Crear archivo `sesiones.txt` en cada día.

  - Ejercicio 8: Escritura/lectura con **bytes** (Lunes).

  - Ejercicio 9: Escritura/lectura con **caracteres** (Martes).

  - Ejercicio 10: Escritura/lectura con **acceso aleatorio** (Miércoles).

- **Práctica Final 1 (Reviews de películas)**
  ➝ `ADD-NDT/TEMA1/PRactica_Final1.md` + archivo `# REVIEWS DE PELÍCULAS.txt`

  - Menú interactivo (crear/eliminar usuario, añadir/mostrar reviews).

  - Gestión de usuarios (`users.txt`).

  - Gestión de reviews (`reviews/Nombre-ID.txt`).

  - Clases `User` y `Review`.

## 🟦 TEMA 2
- **Preguntas sobre flujos de datos**
   ➝ `ADD-NDT/TEMA2/Preguntas.md`

  - `PushbackReader`: métodos `read` y `unread`.

  - `PipedInputStream` y `PipedOutputStream`.

  - `StreamTokenizer` y `LineNumberReader`.

  - `DataInputStream`: orden de lectura y EOFException.

## 🟦 TEMA 3
- **Práctica 1 (Parseador DOM básico)**
   ➝ `ADD-NDT/TEMA3/Practica.md`

  - Creación de `DocumentBuilderFactory` y `DocumentBuilder`.

  - Lectura de XML con DOM.

  - Procesamiento con XPath (`/class/student`).

  - Recorrido de nodos y extracción de atributos/elementos.

- **Práctica Final DOM (Gestor de productos)**
   ➝ `ADD-NDT/TEMA3/PracticaFinalDOM.md`

  - Menú interactivo con opciones:

    1. Mostrar productos.

    2. Añadir producto.

    3. Incrementar precios por categoría.

  - Uso de `Transformer` para guardar cambios.

  - Observaciones sobre mejoras.

- **Códigos completos (DOMManager, GestorProductosXML, productos.xml)**
   ➝ `ADD-NDT/TEMA3/PracticaFinalDOM/CodigosCompletos.md`

  - Implementación detallada y comentada de todas las funciones.

  - XML de ejemplo con productos.

## 🟦 TEMA 4
- **Práctica 4 (Base de datos con PostgreSQL)**
   ➝ `ADD-NDT/TEMA4/Practica4/Practica4.md`

  - Instalación y conexión a PostgreSQL.

  - Creación de usuario y base de datos `tema4`.

  - Creación de tabla `Usuarios`.

  - Conexión desde Java con JDBC.

  - Ejecución de consultas `SELECT`.

  - Código completo en Java para listar usuarios.

## 🟦 EXAMEN 1ER TRIMESTRE

- **Recopilación de archivos del Examen Práctico**  
  ➝ `ADD-NDT/examen_1er_trimestre/`

  - **Configuración mediante XML**  
    - Implementación en `ConfiguracionXML.java` para la lectura y procesamiento del archivo `config.xml` utilizando conectores **DOM** y **XPath**.

  - **Persistencia y Patrón DAO**  
    - Uso de `AlumnoDAO.java` para gestionar la comunicación con la base de datos relacional.  
    - Empleo de **JDBC** (`PreparedStatement`, `ResultSet`) para realizar operaciones seguras.

  - **Lógica de Aplicación y Control**  
    - Clase `Main.java` como punto de entrada.  
    - Orquestación de:
      - Carga de configuración.
      - Conexión mediante `DriverManager`.
      - Interacción con el usuario mediante `Scanner`.

  - **Modelo de Datos**  
    - Definición de la entidad `Alumno.java` para representar los objetos de negocio dentro de la aplicación.


## 🟦 TEMA 6

- **Práctica 5 (Spring Boot y BBDD H2)**  
  ➝ `ADD-NDT/TEMA6/Practica5.md`

  - Configuración inicial con **Spring Initializr** (Maven + Java).
  - Gestión de dependencias: **H2 Database**, **Spring Data JPA**, **Spring Web**.
  - Configuración del archivo `application.properties` para base de datos embebida y habilitación de la **consola H2**.
  - Creación de esquemas de datos mediante el fichero `data.sql`.
  - Acceso y administración de tablas desde la consola web **h2-console**.

- **Ejercicio 5 (Gestión de procesos y persistencia)**  
  ➝ `ADD-NDT/TEMA6/Ejercicio5.md`

  - Notas técnicas sobre la comprobación del puerto **8080** (`netstat`) y finalización de procesos por **PID** (`taskkill`).
  - Definición detallada de tablas de la base de datos:
    - **Usuarios**: `id` (SERIAL), `nombre` (VARCHAR).
    - **Productos**: `id`, `nombre`, `precio` (DECIMAL).
    - **Pedido**: relación con usuarios y productos mediante **claves foráneas**.
  - Implementación de clases Java para la lógica y la interfaz:
    - `MenuPrincipal`
    - `AnadirUsuario`
    - `AnadirProducto`
    - `AnadirPedido`
    - `ConexionBD`

## 🟦 TEMA 7

- **Apuntes del Tema 7.1 (ORM e Hibernate)**  
  ➝ `ADD-NDT/TEMA7/Apuntes_Tema7.1.md`

  - Introducción al **Mapeo Objeto‑Relacional (ORM)** como puente entre la POO y las bases de datos relacionales.
  - Equivalencias fundamentales: **Clases ↔ Tablas**, **Objetos ↔ Filas**, **Atributos ↔ Columnas**.
  - Comparativa de herramientas ORM: **Hibernate**, **iBatis**, **EBean**.
  - Componentes clave de Hibernate:
    - Objetos de persistencia.
    - Archivos `.properties`.
    - Lenguaje **HQL** (Hibernate Query Language).

- **Apuntes del Tema 7.2 (Clases Persistentes y Operaciones)**  
  ➝ `ADD-NDT/TEMA7/Apuntes_Tema7.2.md`

  - Reglas para clases persistentes:
    - Constructor por defecto.
    - Atributo **ID**.
    - Encapsulamiento mediante getters/setters.
  - Uso de anotaciones JPA: **@Entity**, **@Table**, **@Id**, **@GeneratedValue**.
  - Configuración de PostgreSQL y parámetros en `application.properties`:
    - URL de conexión.
    - Credenciales.
    - Propiedad `ddl-auto`.
  - Gestión de la persistencia mediante la **capa de Servicio** y el objeto **Session**.
  - Operaciones CRUD fundamentales:
    - `.persist()`
    - `.get()`
    - `.delete()`
    - `.merge()`
  - Consultas avanzadas con:
    - `CriteriaBuilder`
    - `CriteriaQuery`
    - `Root`

- **Práctica 7 (Ejercicios de Relaciones y CRUD)**  
  ➝ `ADD-NDT/TEMA7/Practica7.md`

  - Instalación y configuración de **PostgreSQL** y el gestor **DBeaver**.
  - Creación de proyectos con Spring Initializr incluyendo **Spring Data JPA**.
  - Implementación de relaciones entre clases (ejemplo: **Autor ↔ Libro**).
  - Desarrollo de métodos en la clase *Service* para:
    - Inserción.
    - Borrado.
    - Actualización.
    - Búsqueda.

- **Guía de Eclipse y Spring Boot**  
  ➝ `ADD-NDT/TEMA7/ApuntesEclipse.md`

  - Pasos para la creación de proyectos **Maven** y su ajuste para Spring Boot.
  - Configuración del **JDK 17**.
  - Solución de errores de versión del JRE en el IDE.

## 🟦 TEMA 8

- **Apuntes del Tema 8 (Implementación de APIs en Java)**  
  ➝ `ADD-NDT/TEMA8/Apuntes_Tema8.md`

  - Introducción a los controladores como puerta de entrada de la aplicación mediante la anotación **@RestController**.
  - Definición de rutas de recursos con **@RequestMapping** y buenas prácticas de nomenclatura (sustantivos, minúsculas, guiones).
  - Inyección de servicios en el controlador usando **@Autowired** para delegar la lógica de persistencia.
  - Uso de **ResponseEntity** y **HttpStatus** para personalizar respuestas HTTP (200 OK, 201 Created, 204 No Content, 404 Not Found).

- **Práctica 8.1 (Construcción de API básica)**  
  ➝ `ADD-NDT/TEMA8/Practica8_parte1.md`

  - Creación de un controlador para gestionar recursos sin claves externas.
  - Implementación de métodos **POST** para inserción de objetos recibiendo JSON mediante **@RequestBody**.
  - Desarrollo de métodos **PUT** para actualizar atributos específicos usando **@PathVariable** y **@RequestParam**.
  - Configuración de métodos **GET** por ID y listados generales, gestionando códigos de estado según la existencia de datos.
  - Creación de filtros personalizados mediante métodos GET basados en condiciones de atributos (ej.: búsqueda por nombre).

- **Práctica 8.2 (APIs con Relaciones y Claves Externas)**  
  ➝ `ADD-NDT/TEMA8/Practica8_parte2.md`

  - Implementación de lógica para controladores con entidades que contienen claves externas.
  - Gestión de inserciones (**POST**) validando la existencia de la relación externa y devolviendo **404** si no se encuentra el recurso vinculado.
  - Métodos **PUT** para actualizar atributos simples y relaciones de claves externas.
  - Implementación del método **DELETE** con validación previa de existencia.
  - Recuperación de datos vinculados mediante **GET**, devolviendo mensajes descriptivos en caso de error.

- **Códigos Completos y Documentación de Pruebas**  
  ➝ `ADD-NDT/TEMA8/`

  - Listado de clases Java finales: **Autor**, **Libro**, sus controladores y servicios de persistencia.
  - Recopilación de capturas de **Postman** mostrando pruebas reales de inserción, actualización, borrado y consultas.
  - Visualización del estado de las tablas *autores* y *libros* en la base de datos mediante **DBeaver**.


## 🟦 TEMA 9

- **Apuntes del Tema 9.1 (Introducción a NoSQL)**  
  ➝ `ADD-NDT/TEMA9/Apuntes_Tema9.1.md`

  - Concepto de bases de datos **NoSQL** y su estructura basada en esquemas flexibles (habitualmente JSON).
  - Definición de **Clúster**: escalabilidad, alta disponibilidad, resiliencia y uso de balanceadores de carga.
  - Comparativa de escalabilidad:
    - **Vertical** (mejorar hardware).
    - **Horizontal** (añadir nodos / clúster), base del enfoque NoSQL.
  - Tipos de bases de datos NoSQL:
    - Clave‑valor.
    - Documentos.
    - Columnas.
    - Grafos.

- **Apuntes del Tema 9.2 (MongoDB y Teorema de CAP)**  
  ➝ `ADD-NDT/TEMA9/Apuntes_Tema9.2.md`

  - Características de MongoDB:
    - Almacenamiento interno en **BSON**.
    - Uso de **replica sets**.
    - Escalabilidad horizontal.
  - Terminología fundamental:
    - **Colección** (tabla).
    - **Documento** (fila).
  - Explicación del **Teorema de CAP**:
    - Consistencia (C).
    - Disponibilidad (A).
    - Tolerancia a particiones (P).
  - Clasificación de bases de datos según CAP (CA, CP, AP), situando a **MongoDB en el modelo CP**.
  - Uso de **MongoDB Atlas** como servicio de base de datos en la nube (SaaS).

- **Apuntes del Tema 9.3 (Integración con Spring Boot)**  
  ➝ `ADD-NDT/TEMA9/Apuntes_Tema9.3.md`

  - Uso de la dependencia **Spring Data MongoDB** para persistencia de documentos flexibles.
  - Configuración del **String de conexión** en el archivo `application.properties`.
  - Mapeo de objetos mediante anotaciones:
    - **@Document** (entidad).
    - **@Id** (clave primaria).
    - **@Field** (nombre de campo personalizado).
  - Implementación de **MongoRepository**:
    - Métodos CRUD automáticos.
    - Consultas personalizadas mediante Keywords (`findBy`, `deleteBy`, etc.).

- **Práctica 9 (Control de Gastos e Ingresos)**  
  ➝ `ADD-NDT/TEMA9/Practica9.md`

  - Configuración completa del entorno en **MongoDB Atlas** (creación de clúster y usuario administrador).
  - Desarrollo de la clase de modelo **Transaction** con:
    - ID.
    - Descripción.
    - Cantidad.
    - Tipo (ingreso/gasto).
    - Fecha.
  - Creación de repositorios con métodos para filtrar transacciones por:
    - Fechas.
    - Descripción (ej.: “Compra”).
    - Rangos de valores.
  - Implementación de **CommandLineRunner** para inicialización y limpieza de datos de prueba en la nube.

