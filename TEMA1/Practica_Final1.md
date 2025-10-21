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
        ArrayList<User> usuarios = leerUsuarios();
        usuarios.add(nuevoUser);
        reescribirLista(usuarios);


        System.out.println("Usuario creado: " + nombre + " con código " + codigo);
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
        reescribirLista(usuarios);

        
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
        String nombreUsuario = obtenerNombrePorCodigo(codigo);
        
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


    public static int obtenerSiguienteCodigo() throws IOException {
        ArrayList<User> usuarios = leerUsuarios();

        // Crear una lista de códigos existentes
        ArrayList<Integer> codigos = new ArrayList<>();
        for (int i = 0; i < usuarios.size(); i++) {
            codigos.add(usuarios.get(i).codigo);
        }

        // Buscar el primer número que no esté en la lista de códigos
        for (int i = 1; i <= usuarios.size() + 1; i++) {
            boolean encontrado = false;
            for (int j = 0; j < codigos.size(); j++) {
                if (codigos.get(j) == i) {
                    encontrado = true;
                    break;
                }
            }
            if (!encontrado) {
                return i;
            }
        }

        // Por seguridad, aunque no debería llegar aquí
        return usuarios.size() + 1;
    }


    public static void reescribirLista(ArrayList<User> usuarios) throws IOException {
        // Ordenar la lista por código (forma básica)
        for (int i = 0; i < usuarios.size() - 1; i++) {
            for (int j = i + 1; j < usuarios.size(); j++) {
                if (usuarios.get(i).codigo > usuarios.get(j).codigo) {
                    // Intercambiar posiciones
                    User temp = usuarios.get(i);
                    usuarios.set(i, usuarios.get(j));
                    usuarios.set(j, temp);
                }
            }
        }

        // Reescribir el archivo con los usuarios ordenados
        FileWriter fw = new FileWriter(archivoUsuarios, false); // false para sobrescribir
        for (int i = 0; i < usuarios.size(); i++) {
            fw.write(usuarios.get(i).toString() + System.lineSeparator());
        }
        fw.close();
    }

    
    public static ArrayList<User> leerUsuarios() throws IOException {
        ArrayList<User> lista = new ArrayList<>();

        try (Scanner sc = new Scanner(archivoUsuarios)) {
            while (sc.hasNextLine()) {
                String linea = sc.nextLine().trim();

                // Ignorar líneas vacías
                if (linea.isEmpty()) continue;

                String[] partes = linea.split(",");

                // Validar que la línea tenga exactamente 3 partes
                if (partes.length != 3) {
                    System.out.println("Línea mal formateada en users.txt: " + linea);
                    continue;
                }

                String nombre = partes[0].trim();
                int codigo;
                try {
                    codigo = Integer.parseInt(partes[1].trim());
                } catch (NumberFormatException e) {
                    System.out.println("Código inválido en línea: " + linea);
                    continue;
                }

                String contrasena = partes[2].trim();
                lista.add(new User(nombre, codigo, contrasena));
            }
        }

        return lista;
    }
    
    public static String obtenerNombrePorCodigo(int codigo) throws IOException {
    ArrayList<User> usuarios = leerUsuarios();
    for (User u : usuarios) {
        if (u.codigo == codigo) {
            return u.nombre;
        }
    }
    return null;
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
