# TEMA 7.1: El mapeo objeto relacional

Enlaces de apoyo: 

- TODO : [Diferencia entre ODBC y JDBC en Java](https://es.differkinome.com/articles/database/difference-between-odbc-and-jdbc.html)

## Mapeo objeto relacional (ORM)

### ¿Qué es el ORM?
El **Mapeo Objeto Relacional** es una técnica o **framework** diseñado para solucionar la complejidad que surge al intentar **guardar objetos de un lenguaje de programación en una base de datos relacional**. Debido a que los lenguajes como Java utilizan Programación Orientada a Objetos (POO) y las bases de datos utilizan un modelo relacional, existen diferencias estructurales que hacen que la comunicación directa sea difícil.

La principal ventaja operativa es que, en lugar de que el programador tenga que escribir manualmente todas las consultas SQL, el ORM permite **manipular la base de datos directamente a través de los objetos** definidos en el código del programa.

**Equivalencias del Mapeo**
Para que este sistema funcione, el ORM establece un "puente" de equivalencias entre el mundo de Java y el de las Bases de Datos:
- **Clases ↔ Tablas**: Cada clase definida en Java (como una plantilla) se corresponde con una tabla completa en la base de datos.
- **Objetos ↔ Filas**: Cada instancia u objeto específico creado a partir de esa clase se guarda como una fila (registro) individual en la tabla.
- **Atributos ↔ Columnas**: Las propiedades de la clase (como el nombre o el ID) se convierten en las columnas que definen los datos de la tabla.

**Ejemplo Práctico de la Presentación**
Las fuentes ilustran este concepto con el siguiente ejemplo:
1. **En Java**: Tienes una clase llamada ```Usuario``` con los atributos ```id``` y ```nombre```. Al ejecutar el código, creas un objeto: ```Usuario Juan = new Usuario (1, "Juan")```.
2. **En la Base de Datos**: El ORM se encarga de que exista una tabla llamada usuarios. El objeto "Juan" se inserta automáticamente como una **fila** donde la columna ```id``` tiene el valor 1 y la columna ```nombre``` tiene el valor **"Juan"**.

## Ventajas y desventajas de los ORMs
El uso de un ORM supone un cambio significativo en la forma de trabajar con los datos, lo cual conlleva los siguientes aspectos positivos y negativos:
| VENTAJAS | DESVENTAJAS |
|----------|-------------|
| **Rapidez en el desarrollo**: Al automatizar la relación entre el código y la base de datos, el proceso de creación de software se vuelve mucho más ágil. | **Menor rendimiento**: Debido a que el mapeo es automático, el rendimiento puede ser inferior comparado con consultas SQL escritas y optimizadas a mano. |
| **Desarrollo orientado a objetos**: Permite que el programador trabaje en todo momento con la lógica de objetos de Java, manteniendo la coherencia del paradigma. | **Curva de aprendizaje**: Implementar y dominar correctamente un framework de ORM requiere un tiempo de aprendizaje inicial por parte del desarrollador. |
| **Abstracción**: El desarrollador se beneficia de no tener que escribir o utilizar sentencias SQL de forma directa para las operaciones comunes. | |
| **Mantenimiento sencillo**: La estructura del código es más limpia, lo que facilita realizar cambios o correcciones en el futuro. | |

## Herramientas ORM
| EBEAN | IBATIS | HIBERNATE |
|-------|--------|-----------|
| **Eficiencia**: Es valorado por ser un framework eficiente. | **Origen**: Fue desarrollado por la fundación Apache. | **Popularidad**: Es la herramienta más popular y utilizada en el ecosistema Java. |
| **Versatilidad**: Permite combinar consultas ORM junto a consultas SQL tradicionales. | **Simplicidad**: Cuenta con una curva de aprendizaje sencilla que facilita un desarrollo rápido. | **Facilidad**: Ofrece un aprendizaje y desarrollo sencillo. |
| **Compatibilidad**: Ofrece soporte para bases de datos como H2, PostgreSQL y MySQL. | **Limitación**: Únicamente permite realizar consultas ORM. | **Integral**: Permite ejecutar tanto sentencias ORM como SQL. |
|  |  | **HQL**: Introduce su propio lenguaje de consultas (Hibernate Query Language) que mejora la sintaxis de SQL. |

## Hibernate
**Hibernate** es la herramienta de mapeo objeto relacional **más popular y utilizada** actualmente,. Se distingue por ser muy completa, permitiendo el uso tanto de sentencias ORM como de SQL, incluyendo su propio lenguaje llamado **HQL** (Hibernate Query Language), que mejora la sintaxis y portabilidad de SQL,.

En el funcionamiento de Hibernate, destacan tres componentes fundamentales que conectan la aplicación con la base de datos:
- **Objetos de persistencia**: Son los objetos que Hibernate utiliza para manipular la base de datos. La ventaja principal es que permiten trabajar directamente con objetos en Java, y estas manipulaciones se traducen automáticamente en sentencias SQL. Gracias a esto, el desarrollador no necesita preocuparse por escribir las consultas manualmente.
- **Archivo .PROPERTIES**: Es el archivo de configuración de Hibernate. En él se definen las propiedades críticas para la conexión, como la URL de la base de datos, el usuario y la contraseña.
- **Mapeo XML / Anotaciones**: El mapeo XML se encarga de definir la relación exacta entre las clases de Java y las tablas de la base de datos. No obstante, las fuentes señalan que hoy en día es más común utilizar anotaciones directamente en el código para realizar esta función,.

En resumen, Hibernate actúa como un puente que permite a la aplicación manejar clases y objetos mientras el framework se encarga de la comunicación con la base de datos.
