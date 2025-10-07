# REVIEWS DE PELÍCULAS
Se te ha encargado diseñar un sistema de reviews de películas para el cine de tu pueblo. Por desgracia, no ha llegado lo último en tecnología y deberás utilizar ficheros de texto plano para almacenar las reseñas.

Diseñe un programa que cumpla con las siguientes características:

## 1. Menú interactivo: diseñe un menú de inicio que permita al usuario interactuar con el sistema.
- Crear un usuario nuevo.
- Eliminar un usuario.
- Añadir una review de un usuario existente.
- Mostrar las reviews de un usuario existente.
- Salir.

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
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;

public class Cine{

    //Scanner lee la opción que selecciona el usuario
    static Scanner scanner = new Scanner(System.in);
    static File carpetaBase = new File("cinema2000");
    static File carpetaRewiews = new File("cinema2000/reviews");
    static File archivoUsuarios = new File("cinema2000/users.txt");
    
    public static void main(String[] args) throws IOException {
        int opcion;
        
        if (!carpetaBase.exists()){
            carpetaBase.mkdir();
        }
        
        if (!carpetaRewiews.exists()){
            carpetaRewiews.mkdir();
        }
        
        if (!archivoUsuarios.exists()) {
            archivoUsuarios.createNewFile();
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
            scanner.nextLine();
        
            switch (opcion){
                case 1:
                    System.out.println("Has accedido a la opción de crear nuevo usuario");
                    CrearUsuario();
                    // break detiene la ejecución del switch y sale de él
                    break;
                case 2:
                    System.out.println("Has accedido a la opción de eliminar un usuario");
                    eliminarUsuario();
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
    
    public static void CrearUsuario() throws IOException {
        System.out.println("Introduzca el nombre de usuario: ");
        String nombre = scanner.nextLine();
        
        int codigo = contarUsuarios() + 1;

        System.out.print("Introduce contraseña: ");
        String contraseña = scanner.nextLine();

        User nuevoUser = new User(nombre, codigo, contraseña);

        FileWriter fw = new FileWriter(archivoUsuarios, true);
        fw.write(nuevoUser.toString() + "\n");
        fw.close();

        System.out.println("Usuario creado: " + nombre + " con código " + codigo);
        
    }
    
    public static int contarUsuarios() throws IOException {
        int lineas = 0;
        Scanner sc = new Scanner(archivoUsuarios);
            while (sc.hasNextLine()) {
                sc.nextLine();
                lineas++;
            }
        sc.close();
        return lineas;
    }

    public static ArrayList<User> leerUsuarios() throws IOException{
        ArrayList<User> lista = new ArrayList<>();
        
        try (Scanner sc = new Scanner(archivoUsuarios)) {
            while (sc.hasNextLine()) {
                String linea = sc.nextLine();
                String[] partes = linea.split(",");
                if (partes.length == 3) {
                    String nombre = partes[0];
                    int codigo = Integer.parseInt(partes[1]);
                    String contraseña = partes[2];
                    lista.add(new User(nombre, codigo, contraseña));
                }
            }
        }
        return lista;
    }
    
    public static void eliminarUsuario() throws IOException{
        System.out.print("Introduzca el código del usuario que quieres eliminar: ");
        int codigoAEliminar = scanner.nextInt();
        scanner.nextLine(); // limpiar buffer

        ArrayList<User> usuarios = leerUsuarios();
        String nombreArchivo = "";

        // Buscar y eliminar el usuario con el código
        for (int i = 0; i < usuarios.size(); i++) {
            if (usuarios.get(i).codigo == codigoAEliminar) {
                nombreArchivo = usuarios.get(i).nombre + "-" + usuarios.get(i).codigo + ".txt";
                usuarios.remove(i);
                break;
            }
        }
        
        //Reescribe la lista de usuarios sin el eliminado
        FileWriter fw = new FileWriter(archivoUsuarios, false);
        for (User u : usuarios) {
            fw.write(u.toString() + "\n");
        }
        fw.close();
        
        File reviewFile = new File(carpetaRewiews, nombreArchivo);
        if (reviewFile.exists()) {
            reviewFile.delete();
            System.out.println("Archivo de reviews eliminado.");
        }
        
    }
    
    
    public static void AñadirReview(){
        
    }
    
    public static void MostrarReviews(){
        
    }
}
```
``` java
package com.mycompany.cine;

public class Review {
    String nombrePelicula;
        int calificacion; 

        Review(String nombrePelicula, int calificacion) {
            this.nombrePelicula = nombrePelicula;
            this.calificacion = calificacion;
        }

        @Override
        public String toString() {
            return nombrePelicula + " - " + calificacion + "/10";
        }
}
```
``` java

package com.mycompany.cine;

public class User {
    
    String nombre;
        int codigo;
        String contraseña;

        User(String nombre, int codigo, String contraseña) {
            this.nombre = nombre;
            this.codigo = codigo;
            this.contraseña = contraseña;
        }

        @Override
        public String toString() {
            return nombre + "," + codigo + "," + contraseña;
        }
    
}
```
