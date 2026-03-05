# Practica 9

> **Nota:**  
> - **Comprobar que hay en el puerto 8080** -> ``` netstat -ano | findstr :8080 ```  
> - **Cortar proceso** -> ``` taskkill /PID <PID> /F ```
---
## Ejercicio 
En esta práctica vamos a aprender a utilizar una base de datos en línea (nube) no relacional y veremos como establecer la conexión con nuestra aplicación desarrollada en Java haciendo uso de SpringBoot. Concretamente utilizaremos una base de datos MongoDB haciendo uso de sus SaaS (Software as a service) MongoDB Atlas.

### Parte 1: Registro en MongoDB Atlas
En primer lugar, como vamos a utilizar una base de datos en línea haciendo uso de MongoDB Atlas, nos dirigimos al siguiente enlace y nos registramos:
https://www.mongodb.com/cloud/atlas/register. Deberemos rellenar algunos datos, el más importante es el lenguaje que vamos a utilizar (Java) y con qué propósito se va a utilizar (microservicios (APIs)).

![Imagen1](IMAGENES/Captura1P9.PNG)

### Parte 2: Creación de un clúster
Para empezar a trabajar con MongoDB Atlas necesitamos crear un clúster.
Un clúster es un conjunto de servidores que trabajan conjuntamente como uno para proporcionar redundancia, escalabilidad y alta disponibilidad en la nube.
Para ello en el proceso de registro, en el segundo paso tenemos una pantalla que dice “Deploy your cluster”. Elegimos “M0” que es un clúster gratuito que ofrece MongoDB Atlas para aprender y explorar bases de datos en la nube de este tipo.
En el nombre del clúster escribimos AccesoADatos y a continuación podemos elegir entre varios proveedores, dejamos AWS y en región podémoste dejar la que encontramos por defecto.

![Imagen2](IMAGENES/Captura2P9.PNG)

Tras esto debemos pulsar en el botón “Create Deployment” que nos creará nuestro clúster para poder alojar una base de datos de tipo MongoDB en la nube.

### Parte 3: Guardar los datos de usuario administrador
A continuación aparecerá una pestaña emergente donde nos indica que se ha creado un usuario y una contraseña con permisos de administrador en las futuras bases de datos que vayamos a crear en nuestro clúster.
Incluya una captura de pantalla del usuario y la contraseña para evitar perder estos datos en un futuro.

![Imagen3](IMAGENES/Captura3P9.PNG)

Tras esto hacemos click en crear usuario y pasamos al siguiente paso haciendo click en “Choose a connection method”.

### Parte 4. Obtención de datos de conexión a MongoDB Atlas.
En este paso, como nos vamos a conectar a una base de datos en la nube, necesitamos obtener una serie de datos para establecer esta conexión desde nuestra aplicación Java.
Elegimos la opción “Drivers” ya que como hasta ahora, nos vamos a conectar a la base de datos haciendo uso de un conector Java.
Tras esto debemos dejar por defecto los datos del driver de Java y debemos copiar y guardarnos el string de conexión. IMPORTANTE NO PERDERLO. Esta cadena de texto nos permitirá establecer la conexión con nuestro clúster y base de datos.

![Imagen4](IMAGENES/Captura4P9.PNG)

### Parte 5. Creación de una BD y colección en MongoDB Atlas.
Nos dirigimos a la opción “Browse collections” de nuestro clúster, en esta sección podemos administrar las bases de datos. Eliminamos la base de datos que incluye por defecto y creamos una con el nombre AccesoADatosDB, para ello deberá hacer click en “Add my own data”. En el nombre de colección ponemos AccesoADatosCollection, una colección es el equivalente a una tabla, por ahora creamos una por defecto para poder continuar pero más adelante crearemos otras más realistas para la realización de la práctica.

![Imagen5](IMAGENES/Captura5P9.PNG)

### Parte 6. Creación de un proyecto en SpringBoot
Como siempre, nos dirigimos a https://start.spring.io/ y cambiamos los metadatos del proyecto a practica12_13_14 menos el package name que lo dejamos con lo que se genere. En la sección de dependencias añadimos “Spring Web” y “Spring Data MongoDB”. Y en tipo de proyecto elegimos “Maven”.

![Imagen6](IMAGENES/Captura6P9.PNG)

Tras esto, genere el proyecto e inclúyalo en Eclipse.

### Parte 7. Conectar la aplicación con MongoDB Atlas
En el archivo “application.properties” añadimos una nueva propiedad “spring.data.mongodb.uri=“ y pegamos sin espacios la cadena de conexión que obtuvimos en el paso 4.
Pruebe a iniciar la aplicación. Si falla revise que la cadena de conexión esté actualizada siguiendo este formato:

```mongodb+srv://<usuario>:<contraseña>@<cluster>.mongodb.net/<nombreBaseDatos>?retryWrites=true&w=majority```

Un fallo común es no incluir “nombreBaseDatos” ya que la generación automática de la cadena de conexión no suele incluirlo.

![Imagen7](IMAGENES/Captura7P9.PNG)

### Parte 8. Creación de un modelo de datos
Aunque los datos de las bases de datos no relacionales (llamados documentos) son flexibles y no necesariamente deben adaptarse a un modelo de datos concreto, crearemos un modelo de datos para facilitar el uso de dichos datos de forma
estandarizada en nuestra aplicación.
Para esta práctica vamos a crear el backend de una aplicación que nos permita llevar el control de nuestros gastos e ingresos. Por ello, vamos a crear un documento llamado “Transaction” que representará este concepto de gasto o ingreso.

Debe crear este documento llamado “Transaction” que debe tener como campos.
- id: string clave primaria.
- description: string. (Descripción del ingreso o gasto).
- quantity: double. (Cantidad gastada).
- income: boolean (true si es un ingreso, false si es un gasto).
- date: Date. (Fecha en la que se hizo el gasto u obtuvo el ingreso)

Debe crear adicionalmente un constructor con parámetros para inicializar dichos campos así como todos los getters y setters.

![Imagen8](IMAGENES/Captura8P9.PNG)

### Parte 9. Creación de un repositorio
A continuación debemos crear un repositorio (MongoRepository) que es una interfaz que proporciona un conjunto de operaciones CRUD (crear, leer, actualizar, eliminar) para interactuar con nuestra base de datos de tipo MongoDB. Recuerde que, como hemos visto en teoría, esta interfaz proporciona por defecto una serie de métodos que podemos utilizar para manipular los datos de la base de datos.
No obstante vamos a crear algunos métodos personalizados útiles para nuestra aplicación (Importante utilizar la convención de nombres vista en teoría). Deberá añadir métodos en esta interfaz que permitan:
- Obtener los gastos.
- Obtener los ingresos.
- Obtener todas las transacciones del último mes.
- Obtener las transacciones con un valor superior a un valor.
- Obtener las transacciones con un valor entre dos valores dados.
- Obtener los gastos entre dos fechas.
- Obtener los gastos de una cantidad menor a una dada.
- Obtener los gastos de una cantidad mayor a una dada.

![Imagen9](IMAGENES/Captura9P9.PNG)

### Parte 10. Probando la aplicación.
En la clase main de la aplicación SpringBoot deberá implementar la interfaz “CommandLineRunner” que nos permitirá ejecutar código específico justo después de que la aplicación Spring Boot haya arrancado. Lo utilizaremos para definir el siguiente método:

```java
@Override
public void run(String... args) throws Exception {}
```

Y dentro de este deberá incluir la siguiente funcionalidad:
- Borrado de todos los datos.
- Inserta algunos datos de prueba: Crea al menos 5 transacciones con descripción, cantidad, tipo (ingreso o gasto) y fecha en las que haya datos de todo tipo para hacer las siguientes pruebas y asegurar resultados en las operaciones siguientes.
- Realiza las siguientes consultas:
    - Obtén todas las transacciones de tipo "Ingreso".
    - Obtén todas las transacciones de tipo "Gasto".
    - Obtén las transacciones que ocurrieron entre dos fechas.
    - Busca transacciones que contengan la palabra "Compra" en su descripción.
    - Filtra las transacciones con una cantidad mayor a 100.

Debe mostrar por consola los resultados de las consultas de obtención de datos.

![Imagen10](IMAGENES/Captura10P9.PNG)
![Imagen11](IMAGENES/Captura11P9.PNG)
![Imagen12](IMAGENES/Captura12P9.PNG)