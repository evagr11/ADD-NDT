# REVIEWS DE PELÍCULAS
Se te ha encargado diseñar un sistema de reviews de películas para el cine de tu pueblo. Por desgracia, no ha llegado lo último en tecnología y deberás utilizar ficheros de texto plano para almacenar las reseñas.

Diseñe un programa que cumpla con las siguientes características:

## 1. Menú interactivo: diseñe un menú de inicio que permita al usuario interactuar con el sistema.
- Crear un usuario nuevo.
- Eliminar un usuario.
- Añadir una review de un usuario existente.
- Mostrar las reviews de un usuario existente.
- Salir.
```java
package com.mycompany.cine;

import java.util.Scanner;

public class Cine{

    public static void main(String[] args) {
        //Scanner lee la opción que selecciona el usuario
        Scanner scanner = new Scanner (System.in);
        int opcion;
        
        do {
            // Mostramos el menú en cada iteración
            System.out.println("\nMenú interactivo:");
            System.out.println("1. Crear un usuario nuevo.");
            System.out.println("2. Eliminar un usuario.");
            System.out.println("3. Añadir una review de un usuario existente.");
            System.out.println("4. Mostrar las reviews de un usuario existente.");
            System.out.println("5. Salir");
        
            System.out.print("Elige una opción (1 a 5): ");
            opcion = scanner.nextInt();
        
            switch (opcion){
                case 1:
                    System.out.println("Has accedido a la opción de crear nuevo usuario");
                    // break detiene la ejecución del switch y sale de él
                    break;
                case 2:
                    System.out.println("Has accedido a la opción de eliminar un usuario");
                    break;
                case 3:
                    System.out.println("Has accedido a la opción de añadir una review");
                    break;
                case 4:
                    System.out.println("Has accedido a la opción de mostrar las reviews");
                    break;
                case 5:
                    System.out.println("Saliendo del programa...");
                    break;
                default:
                    System.out.println("Opción no valida. Por favor, elige una opción del 1 al 5.");
            }
        } while (opcion != 5);
        
        scanner.close();
        
    }
}
```
## 2. Estructura de directorios:
- Todo el proyecto se almacenará dentro de una carpeta llamada “cinema2000”.
- Los datos se almacenarán en una carpeta llamada “reviews”, almacenando las reseñas de los usuarios en un fichero .txt con el nombre y código del usuario como nombre del fichero. Por ejemplo, si mi nombre es Guillermo y mi código de usuario es el [1], el fichero donde se almacenen mis reviews se llamará Guillermo-1.txt.
- Los datos de los usuarios del sistema se guardarán en un fichero “users” en el que almacenemos el nombre del usuario, su código y su contraseña.

## 3. Uso de clases: para la gestión de los datos dentro de su aplicación, implemente las clases User y Review.
- La clase User contendrá un nombre, un código y una contraseña.
- La clase Review contendrá un nombre de película y una calificación.

# Código completo
```java
package com.mycompany.cine;

import java.util.Scanner;
import java.io.File;

public class Cine{

    public static void main(String[] args) {
        //Scanner lee la opción que selecciona el usuario
        Scanner scanner = new Scanner (System.in);
        int opcion;
        
        File carpetaBase = new File("cinema2000");
        if (!carpetaBase.exists()){
            carpetaBase.mkdir();
        }
        File carpetaRewiews = new File("cinema2000/reviews");
        if (!carpetaRewiews.exists()){
            carpetaRewiews.mkdir();
        }
        
        do {
            // Mostramos el menú en cada iteración
            System.out.println("\nMenú interactivo:");
            System.out.println("1. Crear un usuario nuevo.");
            System.out.println("2. Eliminar un usuario.");
            System.out.println("3. Añadir una review de un usuario existente.");
            System.out.println("4. Mostrar las reviews de un usuario existente.");
            System.out.println("5. Salir");
        
            System.out.print("Elige una opción (1 a 5): ");
            opcion = scanner.nextInt();
        
            switch (opcion){
                case 1:
                    System.out.println("Has accedido a la opción de crear nuevo usuario");
                    CrearUsuario();
                    // break detiene la ejecución del switch y sale de él
                    break;
                case 2:
                    System.out.println("Has accedido a la opción de eliminar un usuario");
                    EliminarUsuario();
                    break;
                case 3:
                    System.out.println("Has accedido a la opción de añadir una review");
                    AñadirReview();
                    break;
                case 4:
                    System.out.println("Has accedido a la opción de mostrar las reviews");
                    MostrarReviews();
                    break;
                case 5:
                    System.out.println("Saliendo del programa...");
                    break;
                default:
                    System.out.println("Opción no valida. Por favor, elige una opción del 1 al 5.");
            }
        } while (opcion != 5);
        
        scanner.close(); 
    }
    
    public static void CrearUsuario() {
        
    }
    
    public static void EliminarUsuario(){
        
    }
    
    public static void AñadirReview(){
        
    }
    
    public static void MostrarReviews(){
        
    }
}
```
