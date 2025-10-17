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
package cine;

import java.util.Scanner;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;

public class Cine{

    //Scanner lee la opción que selecciona el usuario
    static Scanner scanner = new Scanner(System.in);
    static File carpetaBase = new File("cinema2000");
    static File carpetaReviews = new File("cinema2000/reviews");
    static File archivoUsuarios = new File("cinema2000/users.txt");
    
    public static void main(String[] args) throws IOException {
        int opcion;
        
        if (!carpetaBase.exists()){
            carpetaBase.mkdir();
        }
        
        if (!carpetaReviews.exists()){
            carpetaReviews.mkdir();
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
                    crearUsuario();
                    // break detiene la ejecución del switch y sale de él
                    break;
                case 2:
                    System.out.println("Has accedido a la opción de eliminar un usuario");
                    eliminarUsuario();
                    break;
                case 3:
                    System.out.println("Has accedido a la opción de añadir una review");
                    anadirReview();
                    break;
                case 4:
                    System.out.println("Has accedido a la opción de mostrar las reviews");
                    mostrarReviews();
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
    
    public static void crearUsuario() throws IOException {
        System.out.print("Introduzca el nombre de usuario: ");
        String nombre = scanner.nextLine();

        //Valido q no este vacio y hago lo mismo para la contraseña
        if (nombre.isEmpty()) {
            System.out.println("Nombre no puede estar vacío.");
            return;
        }

        System.out.print("Introduce contraseña: ");
        String contrasena = scanner.nextLine(); 

        if (contrasena.isEmpty()) {
            System.out.println("Contraseña no puede estar vacía.");
            return;
        }

        //Calculo el siguiente codigo leyendo los usuarios existentes
        int codigo = obtenerSiguienteCodigo();
        //Creo una instancia de User
        User nuevoUser = new User(nombre, codigo, contrasena);
        //Escribo los datos en archivoUsuarios
        try (FileWriter fw = new FileWriter(archivoUsuarios, true)) {
            fw.write(nuevoUser.toString() + System.lineSeparator());
        }

        System.out.println("Usuario creado: " + nombre + " con código " + codigo);
    }



    public static int obtenerSiguienteCodigo() throws IOException {
        //Leo los usuarios desde archivoUsers y los devuelvo en forma d lista
        ArrayList<User> usuarios = leerUsuarios();
        int maxCodigo = 0;
        //Recorro toda la lista
        for (int i = 0; i < usuarios.size(); i++) {
            //Obtiene el obj User(nombre, cdg, contraseña) en la posicioni
            User u = usuarios.get(i);
            //Extraigo el campo del codigo
            int codigo = u.codigo;
            if (codigo > maxCodigo) {
                maxCodigo = codigo;
            }
        }
        return maxCodigo + 1;
    }



    public static ArrayList<User> leerUsuarios() throws IOException{
        //Creo una lista vacia 
        ArrayList<User> lista = new ArrayList<>();
        //leo los usuarios que hay en archivoUsuarios y los almaceno en la lista
        try (Scanner sc = new Scanner(archivoUsuarios)) {
            // Recorro el archivo linea a linea mientras haya lineas por leer
            while (sc.hasNextLine()) {
                //Lee la linea en la que esta
                String linea = sc.nextLine();
                //Divide la linea por partes utilizando la ,
                String[] partes = linea.split(",");
                //El primer campo del array seria el nombre
                String nombre = partes[0];
                //Segundo es el codigo, y lo convierto de String a int
                int codigo = Integer.parseInt(partes[1]);
                //El tercero seria la contraseña
                String contrasena = partes[2];
                //Creo un nuevo obj User con los valores anteriores y los añado a la lista que he creado antes
                lista.add(new User(nombre, codigo, contrasena));
            }
        }
        //Devuelvo la lista que creo
        return lista;
    }
    
    public static void eliminarUsuario() throws IOException{
        System.out.print("Introduzca el código del usuario que quieres eliminar: ");
        int codigoAEliminar = scanner.nextInt();
        scanner.nextLine(); // limpiar buffer

        ArrayList<User> usuarios = leerUsuarios();
        String nombreArchivo = "";

        //Busco el usuario por el codigo, y si existe lo elimino de la lista
        for (int i = 0; i < usuarios.size(); i++) {
            if (usuarios.get(i).codigo == codigoAEliminar) {
                nombreArchivo = usuarios.get(i).nombre + "-" + usuarios.get(i).codigo + ".txt";
                usuarios.remove(i);
                break;
            }
        }
        
        //Reescribo la lista de usuarios sin el eliminado
        FileWriter fw = new FileWriter(archivoUsuarios, false);
        for (int i = 0; i < usuarios.size(); i++) {
            fw.write(usuarios.get(i).toString() + System.lineSeparator());
        }
        fw.close();
        
        //Creo un objeto File que representa la ruta completa del fichero de 
        //reviews combinando la carpeta base carpetaReviews y el nombre 
        //de fichero nombreArchivo
        File reviewFile = new File(carpetaReviews, nombreArchivo);
        if (reviewFile.exists()) {
            reviewFile.delete();
            System.out.println("Archivo de reviews eliminado.");
        }
        
    }
    
    
    public static void anadirReview() throws IOException {
        System.out.print("Codigo de usuario: ");
        int codigo = scanner.nextInt();
        scanner.nextLine();

        //Abro el Scanner local para leer el fichero de usuarios
        Scanner sc = new Scanner(archivoUsuarios);
        //Variable que almacena el nombre del usuario, si existe
        String nombreUsuario = null;
        //Recorro el fichero users buscando el codigo
        while (sc.hasNextLine()) {
            String[] partes = sc.nextLine().split(",");
            if (partes.length == 3 && Integer.parseInt(partes[1]) == codigo) {
                //Guardo el nombre para luego crear el archivo
                nombreUsuario = partes[0];
                break;
            }
        }
        sc.close();

        System.out.print("Pelicula: ");
        String pelicula = scanner.nextLine();
        System.out.print("Calificacion (1-10): ");
        int nota = scanner.nextInt();
        scanner.nextLine();

        //Creo el obj Review con los datos leidos
        Review nueva = new Review(pelicula, nota);
        //Construyo el fichero
        String nombreArchivo = nombreUsuario + "-" + codigo + ".txt";
        //Creo un obj File que representa el fichero dentro de la carpeta reviews
        File archivoReview = new File(carpetaReviews, nombreArchivo);
        //Escribo la review en el fichero
        FileWriter fw = new FileWriter(archivoReview, true);
        fw.write(nueva.toString() + "\n");
        fw.close();

        System.out.println("Review guardada.");
    }

    public static void mostrarReviews() throws IOException {
        System.out.print("Codigo de usuario: ");
        int codigo = scanner.nextInt();
        scanner.nextLine();

        //Abro el Scanner local para leer el fichero de usuarios
        Scanner sc = new Scanner(archivoUsuarios);
        String nombreUsuario = null;
        while (sc.hasNextLine()) {
            String[] partes = sc.nextLine().split(",");
            if (partes.length == 3 && Integer.parseInt(partes[1]) == codigo) {
                nombreUsuario = partes[0];
                break;
            }
        }
        sc.close();
        
        // Si tras leer todo el fichero no se encontró el usuario, informa y sale
        if (nombreUsuario == null) {
            System.out.println("Usuario no encontrado.");
            return;
        }

        //Construyo el nombre del fichero de reviews asociado al usuario
        String nombreArchivo = nombreUsuario + "-" + codigo + ".txt";
        //Creo un objeto File que representa el fichero dentro de carpetaReviews
        File archivoReview = new File(carpetaReviews, nombreArchivo);

        // Si el fichero no existe, informa que no hay reviews y sale
        if (!archivoReview.exists()) {
            System.out.println("No hay reviews.");
            return;
        }

        // Muestra encabezado indicando de quién son las reviews
        System.out.println("Reviews de " + nombreUsuario + ":");
        // Recorre el fichero de reviews imprimiendo cada línea
        Scanner lector = new Scanner(archivoReview);
        while (lector.hasNextLine()) {
            System.out.println("- " + lector.nextLine());
        }
        lector.close();
    }
}
```
``` java
package cine;

public class User {
    String nombre;
    int codigo;
    String contrasena;

    public User(String nombre, int codigo, String contrasena) {
        this.nombre = nombre;
        this.codigo = codigo;
        this.contrasena = contrasena;
    }

    @Override
    public String toString() {
        return nombre + "," + codigo + "," + contrasena;
    }
}
```
``` java
package cine;

public class Review {
    String nombrePelicula;
    int calificacion;

    public Review(String nombrePelicula, int calificacion) {
        this.nombrePelicula = nombrePelicula;
        this.calificacion = calificacion;
    }

    @Override
    public String toString() {
        return nombrePelicula + " - " + calificacion + "/10";
    }
}

```
```java
public static int obtenerSiguienteCodigo() throws IOException {
        //Leo los usuarios desde archivoUsers y los devuelvo en forma d lista
        ArrayList<User> usuarios = leerUsuarios();
        
        //TODO: ordenar usuario por ID
        
        int maxCodigo = 0;
        //Recorro toda la lista
        for (int i = 1; i < usuarios.size()+1; i++) {
            //Obtiene el obj User(nombre, cdg, contraseña) en la posicioni
            User u = usuarios.get(i-1);
            //Extraigo el campo del codigo
            int codigo = u.codigo;
            if (codigo == i) {
                maxCodigo = codigo +1;
            }else {
                maxCodigo = i;
                break;
            }
        }
        return maxCodigo;
    }
```
