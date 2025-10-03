# BIBLIOTECA
Una biblioteca desea organizar digitalmente sus libros y sesiones de lectura. Implementa en Java un programa que realice las siguientes acciones, en orden:
## 1. Crear el directorio principal Biblioteca.
  - Si ya existen, no deben volver a crearse.
  - Al crear cualquier directorio debe mostrarse su ruta absoluta.
  ```java
  package com.mycompany.biblioteca;

  import java.io.File;

  public class Biblioteca {
    public static void main(String[] args) {
        File dir = new File("Biblioteca");
        
        if (!dir.exists()){
            dir.mkdir();
            System.out.println("Directorio creado correctamente.");
            System.out.println("Ruta absoluta: " + dir.getAbsolutePath());
        } else {
            System.out.println("El directorio ya existe.");
            System.out.println("Ruta absoluta: " + dir.getAbsolutePath());
        }
    }
  }
  ```
## 2. Crear dentro de Biblioteca las categorías de libros como subdirectorios:
  - Novela, Poesía, Ciencia, Historia, Arte.
  - Si ya existen, no deben volver a crearse.
  ```java
  String[] categorias = {"Novela", "Poesía", "Ciencia", "Historia", "Arte"};
  for (String categoria : categorias) {
      File subdir = new File(dir, categoria);
            
      if (!subdir.exists()){
          subdir.mkdir();
          System.out.println("Subdirectorio " + categoria + " creado.");
          System.out.println("Ruta absoluta: " + subdir.getAbsolutePath());
      } else {
          System.out.println("El subdirectorio '" + categoria + "' ya existe.");
          System.out.println("Ruta absoluta: " + subdir.getAbsolutePath());
      }
  }
  ```
## 3. Crear dentro de cada categoría un archivo catalogo.txt. En este archivo se irá registrando información sobre los libros.
  ```java
  File catalogo = new File(subdir, "catalogo.txt");
        
  if (!catalogo.exists()){
      catalogo.createNewFile();
      if (catalogo.exists()) {
          System.out.println("El archivo 'catalogo.txt' creado en " + categoria + ".");
          System.out.println("Ruta absoluta: " + catalogo.getAbsolutePath());
      }
  } else {
      System.out.println("El archivo 'catalogo.txt' ya existe en " + categoria + ".");
      System.out.println("Ruta absoluta: " + catalogo.getAbsolutePath());
  }
  ```
## 4. Operaciones de escritura y lectura:
  - En Novela/catalogo.txt guardar y leer con bytes: "Don Quijote (1605) - Miguel de Cervantes".
    ```java
    // NOVELA - Escritura y lectura con bytes
    try (FileOutputStream fos = new FileOutputStream("Biblioteca/Novela/catalogo.txt")){
        String novelaTexto = "Don Quijote (1605) - Miguel de Cervantes";
        byte[] arrayBytes = novelaTexto.getBytes();
        fos.write(arrayBytes);
    }
        
    try (FileInputStream fis = new FileInputStream("Biblioteca/Novela/catalogo.txt")) {
        int i;
        while ((i = fis.read()) != -1) {
            System.out.print((char) i);
        }
        System.out.println();
    }
    ```
  - En Poesía/catalogo.txt guardar y leer con caracteres: "Veinte poemas de amor (1924) - Pablo Neruda".
    ```java
    // POESÍA - Escritura y lectura con caracteres
    try (FileWriter fw = new FileWriter("Biblioteca/Poesía/catalogo.txt")){
        String poesiaTexto = "Veinte poemas de amor (1924) - Pablo Neruda";
        fw.write(poesiaTexto);
    }
        
    try (FileReader  fr = new FileReader ("Biblioteca/Poesía/catalogo.txt")) {
        int i;
        while ((i = fr.read()) != -1) {
        System.out.print((char) i);
        }
        System.out.println();
    }
    ```
  - En Ciencia/catalogo.txt guardar y leer con acceso aleatorio: "El origen de las especies (1859) - Charles Darwin". Luego corregir el año a 1858 sin sobrescribir todo el contenido.
    ```java
    // CIENCIA - Acceso aleatorio y modificación parcial
    File cienciaFile = new File("Biblioteca/Ciencia/catalogo.txt");
    
    //Primera escritura
    try (RandomAccessFile raf = new RandomAccessFile(cienciaFile, "rw")) {
        raf.writeBytes("El origen de las especies (1859) - Charles Darwin");
    }

    //Corregir año
    try (RandomAccessFile raf = new RandomAccessFile(cienciaFile, "rw")) {
        raf.seek(27); 
        raf.write("1858".getBytes());
    }

    // Impresión por pantalla
    try (RandomAccessFile raf = new RandomAccessFile(cienciaFile, "r")) {
        int i;
        while ((i = raf.read()) != -1) {
            System.out.print((char) i);
        }
        System.out.println();
    }
    ```

## 5. Listar todo el contenido de Biblioteca (carpetas y archivos), mostrando sus rutas relativas desde Biblioteca.
   ```java
  File lista = new File ("Biblioteca");
  File[] fileList = lista.listFiles();
  int i;
  System.out.println("Aquí tienes todo el contenido existente en la biblioteca");
  for(i = 0; i < fileList.length; i++){
     System.out.println(fileList[i]); 
  }
  ```
# Código completo
```java
package com.mycompany.biblioteca;

import java.io.File;
import java.io.IOException;
import java.io.FileOutputStream;
import java.io.FileInputStream;
import java.io.FileWriter;
import java.io.FileReader;
import java.io.RandomAccessFile;

public class Biblioteca {

    public static void main(String[] args) throws IOException {
        File dir = new File("Biblioteca");
        
        if (!dir.exists()){
            dir.mkdir();
            System.out.println("Directorio creado correctamente.");
            System.out.println("Ruta absoluta: " + dir.getAbsolutePath());
        } else {
            System.out.println("El directorio ya existe.");
            System.out.println("Ruta absoluta: " + dir.getAbsolutePath());
        }
        
        String[] categorias = {"Novela", "Poesía", "Ciencia", "Historia", "Arte"};
        for (String categoria : categorias) {
            File subdir = new File(dir, categoria);
            
            if (!subdir.exists()){
                subdir.mkdir();
                System.out.println("Subdirectorio " + categoria + " creado.");
                System.out.println("Ruta absoluta: " + subdir.getAbsolutePath());
            } else {
                System.out.println("El subdirectorio " + categoria + " ya existe.");
                System.out.println("Ruta absoluta: " + subdir.getAbsolutePath());
            }
            
            File catalogo = new File(subdir, "catalogo.txt");
        
            if (!catalogo.exists()){
                catalogo.createNewFile();
                if (catalogo.exists()) {
                    System.out.println("El archivo 'catalogo.txt' creado en " + categoria + ".");
                    System.out.println("Ruta absoluta: " + catalogo.getAbsolutePath());
                }
            } else {
                System.out.println("El archivo 'catalogo.txt' ya existe en " + categoria + ".");
                System.out.println("Ruta absoluta: " + catalogo.getAbsolutePath());
            }
        }
        
        // NOVELA - Escritura y lectura con bytes
        try (FileOutputStream fos = new FileOutputStream("Biblioteca/Novela/catalogo.txt")){
            String novelaTexto = "Don Quijote (1605) - Miguel de Cervantes";
            byte[] arrayBytes = novelaTexto.getBytes();
            fos.write(arrayBytes);
        }
        
        try (FileInputStream fis = new FileInputStream("Biblioteca/Novela/catalogo.txt")) {
            int i;
            while ((i = fis.read()) != -1) {
                System.out.print((char) i);
            }
            System.out.println();
        }
        
        // POESÍA - Escritura y lectura con caracteres
        try (FileWriter fw = new FileWriter("Biblioteca/Poesía/catalogo.txt")){
            String poesiaTexto = "Veinte poemas de amor (1924) - Pablo Neruda";
            fw.write(poesiaTexto);
        }
        
        try (FileReader  fr = new FileReader ("Biblioteca/Poesía/catalogo.txt")) {
            int i;
            while ((i = fr.read()) != -1) {
                System.out.print((char) i);
            }
            System.out.println();
        }
        
        // CIENCIA - Acceso aleatorio y modificación parcial
        File cienciaFile = new File("Biblioteca/Ciencia/catalogo.txt");
        
        //Primera escritura
        try (RandomAccessFile raf = new RandomAccessFile(cienciaFile, "rw")) {
            raf.writeBytes("El origen de las especies (1859) - Charles Darwin");
        }

        //Corregir año
        try (RandomAccessFile raf = new RandomAccessFile(cienciaFile, "rw")) {
            raf.seek(27); 
            raf.write("1858".getBytes());
        }

        // Impresión por pantalla
        try (RandomAccessFile raf = new RandomAccessFile(cienciaFile, "r")) {
            int i;
            while ((i = raf.read()) != -1) {
                System.out.print((char) i);
            }
            System.out.println();
        }

        File lista = new File ("Biblioteca");
        File[] fileList = lista.listFiles();
        int i;
        System.out.println("Aquí tienes todo el contenido existente en la biblioteca");
        for(i = 0; i < fileList.length; i++){
           System.out.println(fileList[i]); 
        }
    }
}
```
