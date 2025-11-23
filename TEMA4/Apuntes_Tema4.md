# TEMA 4: Manejo de Conectores

Enlaces de apoyo: 

- [Diferencia entre ODBC y JDBC en Java](https://es.differkinome.com/articles/database/difference-between-odbc-and-jdbc.html)

## Conector

**Definición**: Un conector es un conjunto de clases y librerías que permiten unir la capa de aplicación con la capa de base de datos.

**Funciones principales**:
- Conectar con la base de datos (BBDD).
- Realizar consultas y operaciones sobre los datos.

### Desfase del objeto relacional
**Problema**: Existe una diferencia entre cómo se representan los datos en la aplicación y en la base de datos:
- **Aplicación (Java)**: trabaja con objetos (datos complejos).
- **Base de datos física**: almacena datos simples (tablas, filas, columnas).

**Solución**:
- Se deben realizar traducciones entre objetos Java y datos de la BBDD.
- Se crean entidades equivalentes en ambas partes para representar lo mismo.

**Ejemplo conceptual**:
- Clase Usuario en Java ↔ Tabla Usuarios en la BBDD.

### Protocolos de acceso a las BBDD
Existen diferentes protocolos para acceder a bases de datos desde aplicaciones:

#### JDBC (Java Database Connectivity)
- Solo para Java.
- Multiplataforma (funciona en distintos sistemas operativos).
- Orientado a objetos.
- Buen rendimiento → recomendado para Java.
- Permite que un mismo código funcione con diferentes motores SQL (Oracle, MySQL, etc.).

#### ODBC (Open Database Connectivity)
- Compatible con C, C++, Java.
- Solo para Windows.
- No orientado a objetos.
- Peor rendimiento en Java → requiere conversiones internas de lenguajes.
- No recomendado para proyectos Java.

## Ideas clave
- Los conectores son la puerta de enlace entre aplicaciones y bases de datos.
- El desfase objeto-relacional obliga a mapear objetos ↔ tablas.
- JDBC es la opción estándar y más eficiente en Java.
- ODBC es más genérico, pero poco eficiente y limitado en entornos Java.
