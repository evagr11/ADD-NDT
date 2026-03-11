# TEMA 9.2: MongoDB y el teorema de CAP

## 1. MongoDB

- **Definición** : Es una base de datos NoSQL orientada a documentos.

- **Formato de datos** : Aunque los documentos se estructuran externamente en **JSON**, internamente la base de datos los almacena en **BSON** (JSON binario).

- **Modelo flexible** : No requiere un esquema rígido, permitiendo adaptar la estructura de los datos según sea necesario.

- **Escalabilidad** : Está diseñada para la **escalabilidad horizontal**, permitiendo dividir y distribuir los datos entre diferentes nodos para manejar volúmenes masivos de información.

- **Réplica y disponibilidad** : Utiliza un sistema llamado **replica sets**, mediante el cual los datos se duplican automáticamente entre varios servidores para garantizar seguridad y acceso continuo.

- **Multiplataforma** : Compatible con múltiples entornos, pudiendo implementarse tanto en servidores locales como en la nube.

- **Terminología básica**
    - **Colección** : Equivalente a una tabla en el modelo relacional; agrupa elementos relacionados pero sin una estructura fija.  
    - **Documento** : Equivalente a una fila; es la unidad fundamental de almacenamiento que representa un registro individual.

## 2. ¿Cuándo utilizar MongoDB?

El uso de MongoDB es especialmente recomendable en los siguientes tipos de desarrollos y entornos:

- **Datos no estructurados o semi‑estructurados**  
    - Ideal para aplicaciones que manejan información que no encaja en una estructura fija de tablas y columnas.

- **Datos que cambian con frecuencia**  
    - Muy útil cuando el modelo de datos evoluciona rápidamente, aprovechando su esquema flexible.

- **Escalabilidad y disponibilidad críticas**  
    - Recomendado en proyectos donde el sistema no puede permitirse caídas y necesita crecer horizontalmente para soportar más carga.

- **Aplicaciones en tiempo real**  
    - Adecuado para sistemas que requieren procesamiento inmediato de datos.  
    - Ejemplos:  
        - Sistemas de logs y registros de eventos.  
        - Plataformas de chat y mensajería.  
        - Portales de comercio electrónico.  
        - Aplicaciones para el sector automotriz.

## 3. El teorema de CAP

Este teorema sostiene que en los sistemas distribuidos es imposible garantizar simultáneamente al completo las tres propiedades siguientes: **Consistencia**, **Disponibilidad** y **Tolerancia a particiones**.

---

### A. Los tres pilares del teorema

- **Consistencia (C)**  
    - Asegura que todos los nodos del sistema vean los mismos datos en un momento dado.  
    - Una lectura siempre debe devolver el valor más reciente tras una escritura.

- **Disponibilidad (A)**  
    - Garantiza que el sistema siempre responda a las solicitudes de los usuarios.  
    - Cualquier petición debe recibir una respuesta, sin importar el estado de los nodos individuales.

- **Tolerancia a particiones (P)**  
    - El sistema debe seguir funcionando correctamente aunque existan fallos de comunicación o interrupciones de red entre los nodos.

---

### B. Clasificación de las bases de datos según CAP

Como solo se pueden cumplir **dos** de estas condiciones a la vez, las bases de datos se clasifican en tres grupos:

- **CA (Consistencia y Disponibilidad)**  
    - Sacrifica la tolerancia a particiones.  
    - Garantiza datos idénticos y respuesta constante, pero el sistema puede fallar si hay un problema de red.  
    - **Ejemplo** : Bases de datos relacionales tradicionales.

- **CP (Consistencia y Tolerancia a Particiones)**  
    - Sacrifica la disponibilidad.  
    - Ante un fallo de red, el sistema prioriza que el dato sea correcto, aunque eso implique dejar de responder temporalmente.  
    - **Ejemplo** : MongoDB.

- **AP (Disponibilidad y Tolerancia a Particiones)**  
    - Sacrifica la consistencia.  
    - El sistema siempre responde, incluso si hay fallos de comunicación, aunque la información devuelta pueda no estar actualizada.  
    - **Ejemplo** : Bases de datos NoSQL de grafos.

---

### C. Ejemplos prácticos

- **Aplicación Bancaria**  
    - En un modelo **CP**, todos los nodos deben reflejar el saldo exacto tras una transferencia; si la red falla, el sistema se bloquea para evitar errores.  
    - En un modelo **AP**, el usuario siempre verá un saldo, aunque pueda ser incorrecto temporalmente, para asegurar que la aplicación responda.

- **Tienda Online (Stock)**  
    - Si un usuario compra el último artículo en un servidor y la red falla, otro servidor podría no enterarse y permitir una segunda compra del producto agotado.  
    - Esto ocurre cuando se prioriza la disponibilidad (**AP**), aceptando inconsistencias temporales.

## 4. Términos utilizados en MongoDB

Para entender cómo se estructuran los datos en MongoDB, es útil conocer sus tres componentes principales:

- **Base de datos**  
    - Concepto idéntico al del modelo relacional.  
    - Es el contenedor de más alto nivel que agrupa toda la información de una aplicación o proyecto.

- **Colección**  
    - Equivalente a una tabla en las bases de datos SQL.  
    - Consiste en un conjunto de elementos relacionados conceptualmente.  
    - A diferencia de las tablas, su estructura no es fija, permitiendo que los elementos dentro de una misma colección tengan campos diferentes.

- **Documento**  
    - Equivalente a una fila de una tabla.  
    - Es la unidad fundamental de almacenamiento en MongoDB.  
    - Cada documento representa un registro individual dentro de una colección.

## 5. Bases de datos en la nube: MongoDB Atlas

### A. Concepto de Base de Datos en la nube

- **Diferencia con las locales**  
    - Las bases de datos locales se almacenan en el propio ordenador, lo que limita el acceso desde otras redes.

- **Definición**  
    - Son bases de datos que se implementan y acceden a través de internet, siendo alojadas y gestionadas por empresas externas.

---

### B. ¿Qué es MongoDB Atlas?

- Es un servicio de base de datos en la nube que ofrece todas las capacidades de MongoDB sin la complejidad de gestionar la infraestructura.  
- Permite alojar la base de datos en un **clúster global** que abarca un total de **115 países**.

---

### C. Ventajas y características principales

- **Alta disponibilidad** : Garantiza que los datos estén siempre accesibles.  
- **Seguridad** : Ofrece copias de seguridad automatizadas, recuperación ante desastres y cifrado integrado.  
- **Escalabilidad automática** : Permite ajustar recursos (nodos, capacidad) según la demanda.  
- **Mantenimiento simplificado** : No requiere instalación local; todo se gestiona desde el proveedor.

---

### D. Conexión con aplicaciones

- **String de conexión** : El proveedor otorga una cadena de texto que identifica la base de datos y al usuario (autenticación).

- **Estructura del string**  
    ```
    mongodb+srv://<usuario>:<contraseña>@<cluster>.mongodb.net/<nombre_BD>?retryWrites=true&w=majority
    ```

- **Integración**  
    - En aplicaciones (como Spring Boot), basta con indicar este string en el archivo de propiedades para establecer la comunicación.

