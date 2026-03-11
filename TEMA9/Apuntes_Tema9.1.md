# TEMA 9.1: Introducción a bases de datos NoSQL

## NoSQL concepto

- **Definición** : Las bases de datos **NoSQL** son sistemas de almacenamiento que **no utilizan el enfoque tradicional de las bases de datos relacionales**.

- **Estructura de almacenamiento** : A diferencia de SQL, **no emplean tablas con filas y columnas** para organizar la información.

- **Formato predominante** : El formato más habitual para almacenar datos es **JSON**, aunque no es la única opción disponible.

- **Naturaleza tecnológica** : NoSQL **no es una única tecnología**, sino un **conjunto de productos y conceptos** orientados al almacenamiento de datos no relacional.

- **Características fundamentales**  
    - Utilizan un **modelo de datos no relacional**.  
    - Están diseñadas para funcionar eficientemente en **clústers**.  
    - Suelen ser **de código abierto**.  
    - Poseen un **esquema flexible**, sin estructura fija predefinida.

## 1.1 Clúster concepto

- **Definición** : Un clúster es un grupo de servidores que trabajan de forma conjunta para alojar y gestionar bases de datos en la nube de manera eficiente y escalable.

- **Funcionamiento** : En un clúster, varios nodos (servidores) actúan como si fueran uno solo y pueden estar distribuidos por todo el mundo.

- **Características fundamentales que debe cumplir**
    - **Escalabilidad** : Capacidad de crecer tanto horizontal como verticalmente.  
    - **Alta disponibilidad** : El sistema debe seguir operativo incluso ante fallos de red o hardware.  
    - **Seguridad integrada** : Mediante autenticación, cifrado y redes seguras.  
    - **Rendimiento** : Debe garantizar baja latencia (tiempos de respuesta rápidos).  
    - **Consistencia** : Los datos deben ser idénticos en todos los nodos del clúster.  
    - **Resiliencia** : Capacidad del sistema para recuperarse automáticamente ante fallos.  
    - **Monitoreo y mantenimiento** : Herramientas para rastrear métricas y realizar tareas como copias de seguridad.  
    - **Flexibilidad** : Compatible con múltiples tecnologías y con nodos distribuidos en varios países para reducir la latencia.

- **El Balanceador de Carga (Load Balancer)**  
    - Los clústers suelen contar con estos sistemas para distribuir de manera eficiente el tráfico entrante entre los distintos servidores.  
    - Su objetivo es garantizar mayor disponibilidad, rendimiento y confiabilidad del sistema.

## 1.2 Escalabilidad horizontal vs vertical

- **Escalabilidad** : Se define como la capacidad de crecimiento de una aplicación para atender a un número cada vez mayor de solicitudes y usuarios sin que la calidad del servicio se vea afectada.

- **Existen dos enfoques principales para lograrla:**

---

### A. Escalabilidad Vertical

- **Concepto** : Consiste en aumentar los recursos de un servidor existente, mejorando componentes como la CPU, la memoria RAM o el almacenamiento.

- **Ventajas** : Es fácil de implementar, ya que solo requiere añadir o mejorar los componentes físicos del servidor actual.

- **Limitaciones**  
    - El beneficio y el escalado son limitados; una vez alcanzado el límite tecnológico de un componente, no se puede mejorar más aunque persistan los problemas de carga.  
    - Suele ser más cara debido al alto coste del hardware especializado.

- **Uso común** : Es el modelo típico de las bases de datos relacionales (SQL).

---

### B. Escalabilidad Horizontal

- **Concepto** : Consiste en aumentar la cantidad de servidores (nodos) que atienden una aplicación, configurándolos para trabajar de manera conjunta como un clúster mediante balanceadores de carga.

- **Ventajas**  
    - El escalado es prácticamente ilimitado, ya que se pueden seguir añadiendo servidores bajo demanda.  
    - Es más barata, porque normalmente no requiere hardware de gama ultra alta, sino múltiples servidores estándar.

- **Desventajas** : Es más compleja de implementar, ya que requiere una configuración técnica avanzada para que todos los servidores funcionen coordinadamente como una sola unidad.

- **Uso común** : Es el modelo para el que están diseñadas las bases de datos NoSQL, ideal para arquitecturas distribuidas en la nube.

## 2. ¿Por qué surgieron las bases de datos NoSQL?

- **Motivo general** : Las bases de datos NoSQL surgieron como respuesta a las limitaciones de las bases de datos relacionales (SQL) ante las demandas de las aplicaciones modernas.

- **Causas principales**

    - **Necesidad de manejar Big Data**  
        - Con el auge de la Web 2.0 (redes sociales, correo electrónico, IoT, etc.), el volumen de datos creció de forma exponencial.  
        - Las bases de datos SQL tienen dificultades para escalar horizontalmente y gestionar grandes cantidades de datos distribuidos de manera eficiente.

    - **Escalabilidad Horizontal**  
        - Las bases de datos SQL suelen escalar verticalmente, lo cual tiene un límite físico y tecnológico.  
        - Las NoSQL están diseñadas nativamente para la escalabilidad horizontal, lo que las hace más económicas e ideales para arquitecturas distribuidas en la nube.

    - **Flexibilidad de los esquemas de datos**  
        - SQL utiliza un esquema fijo (tablas con columnas predefinidas).  
        - NoSQL permite el uso de datos flexibles, lo cual es esencial para aplicaciones modernas que trabajan con datos no estructurados.

    - **Manejo de diferentes tipos de datos**  
        - SQL se limita a estructuras tabulares.  
        - NoSQL permite almacenar información en formatos como **JSON, XML, Grafos o pares Clave-valor**.

    - **Alta Disponibilidad**  
        - Al estar alojadas en clústers, las bases de datos NoSQL garantizan que si un nodo falla, otro pueda atender la solicitud.  
        - En SQL, aunque se pueden usar varios servidores, mantener la consistencia es más complejo y suele presentar problemas técnicos.

## 3. Tipos de bases de datos NoSQL

Existen cuatro categorías principales de bases de datos NoSQL, clasificadas según su modelo de almacenamiento:

---

### A. Bases de datos clave-valor

- **Funcionamiento** : Almacenan la información como pares de clave y valor.

- **Ventaja principal** : Son ideales para aplicaciones que necesitan realizar búsquedas extremadamente rápidas basándose en una clave.

- **Casos de uso** : Gestión de sesiones de usuario, sistemas de caché o configuraciones de aplicaciones.

- **Ejemplo** : Una clave como `"user123"` puede tener asociada información del perfil como nombre, email y rol.

---

### B. Bases de datos basadas en documentos

- **Funcionamiento** : Guardan los datos en formatos de documentos como **JSON, BSON o XML**.

- **Esquema flexible** : Cada documento es independiente y puede tener una estructura distinta.

- **Casos de uso** : Ideales para aplicaciones con datos no estructurados o sin una forma fija predefinida.

- **Ejemplo** : Un documento que representa una *Laptop* puede incluir campos anidados con sus especificaciones (marca, procesador, memoria).

---

### C. Bases de datos basadas en columnas

- **Funcionamiento** : Organizan la información en **columnas** en lugar de filas.

- **Eficacia** : Cada bloque de datos contiene información de una sola columna, optimizando consultas sobre grandes volúmenes de datos.

- **Casos de uso** : Muy utilizadas en **Big Data Analytics** para análisis masivo de información.

---

### D. Bases de datos basadas en grafos

- **Funcionamiento** : Están diseñadas para priorizar las **relaciones** entre los datos.

- **Estructura** : Utilizan **nodos** para representar entidades y **aristas** para representar relaciones.

- **Casos de uso** : Fundamentales en redes sociales, motores de recomendación y sistemas de gestión de rutas.

## 4. Cuándo se suelen utilizar bases de datos NoSQL

El uso de bases de datos NoSQL es especialmente recomendable en los siguientes escenarios técnicos y de negocio:

- **BIG DATA**  
    - Cuando es necesario gestionar y procesar grandes volúmenes de datos que superan la capacidad de las bases de datos relacionales tradicionales.

- **Necesidad de escalabilidad horizontal**  
    - En proyectos donde se requiere añadir más servidores al clúster para manejar el crecimiento de la demanda.

- **Datos no estructurados o semi-estructurados**  
    - Cuando la información no tiene un esquema fijo o predefinido, lo que requiere la flexibilidad de formatos como **JSON** o **XML**.

- **Aplicaciones en tiempo real**  
    - Para sistemas que exigen baja latencia y un número muy elevado de operaciones por segundo.  
    - Ejemplos:  
        - Gestión de logs y mensajes de chat.  
        - Plataformas de comercio electrónico.  
        - Aplicaciones para automóviles o dispositivos conectados (IoT).

- **Relaciones complejas entre datos**  
    - Cuando el valor principal reside en cómo se conectan los datos entre sí, siendo ideal el uso de modelos basados en **grafos**.

- **Alta disponibilidad y tolerancia a fallos**  
    - En aplicaciones críticas donde el sistema debe permanecer operativo incluso si ocurren fallos en la red o en el hardware de algún nodo.

- **Datos que cambian frecuentemente**  
    - Cuando la estructura de la información evoluciona con rapidez, dificultando el mantenimiento de un esquema rígido en SQL.
