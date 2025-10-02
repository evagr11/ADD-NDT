# Ejercicios Tema 1

## Ejercicio 1. (0.5 puntos)
  **Haciendo uso de la clase Java.io.File deberá crear un directorio con el nombre "cine_granada" dentro de la carpeta "P1" utilizada para esta práctica.**
  ```java
  import java.io.File;

  public class MyClass {
    public static void main(String args[]) {
      File fichero = new File ("P1/cine_granada");
      fichero.mkdir();
      System.out.println(fichero.getName());
    }
  }
  ```

## Ejercicio 2. (0.5 puntos)
  **Haciendo uso de la clase Java.io.File deberá crear los siguientes directorios dentro de la carpeta "P1" (no confundir con incluir dentro de la carpeta creada en el ejercicio 1): "Lunes, Martes, Miércoles, Jueves, Viernes, Sábado, Domingo".**
  ```java
  import java.io.File;

  public class MyClass {
    public static void main(String args[]) {
      String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
      for (String dia : dias) {
          File dir = new File ("P1/" + dia);
          dir.mkdir();
      }
    }
  }
  ```
## Ejercicio 3. (1 puntos)
  **Haciendo uso de la clase Java.io.File deberá mover los siguientes directorios: "Lunes, Martes,
Miércoles, Jueves, Viernes, Sábado, Domingo" dentro de la carpeta "cine_granada".**
  ```java
  import java.io.File;

  public class MyClass {
    public static void main(String args[]) {
      String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
      for (String dia : dias) {
          File origen = new File ("P1/" + dia);
          File destino = new File ("P1/cine_granada/" + dia);
          origen.renameTo(destino);
      }
    }
  }
  ```
## Ejercicio 4. (1 puntos)
  **Hasta este momento hemos creado los directorios sin realizar ningún tipo de comprobación,
añada al código de los ejercicios 1 y 2 comprobaciones para no crear el directorio si ya existe.**
### Ejercicio 4.1
  ```java
  import java.io.File;

  public class MyClass {
    public static void main(String args[]) {
      File fichero = new File ("P1/cine_granada");
      if (!fichero.exists()) {
          fichero.mkdir();
      }
    }
  }
  ```
### Ejercicio 4.2
  ```java
  import java.io.File;

  public class MyClass {
    public static void main(String args[]) {
      String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
      for (String dia : dias) {
          File dir = new File ("P1/" + dia);
          if (!dir. exists()) {
              dir.mkdir();
          }
      }
    }
  }
  ```
## Ejercicio 5. (0.5 puntos)
  **A partir de la modificación realizada en el ejercicio 4 incluya el código necesario para imprimir por
consola la ruta absoluta de los ficheros o directorios en el momento de crearse. Deberá imprimir
un mensaje como el siguiente "Se ha creado el directorio con ruta absoluta: /…/…/…".**
  ### Ejercicio 5.1
  ```java
  import java.io.File;

  public class MyClass {
    public static void main(String args[]) {
      File fichero = new File ("P1/cine_granada");
      if (!fichero.exists()) {
          fichero.mkdir();
          System.out.println("Se ha creado el directorio con ruta absoluta:" + fichero.getAbsolutePath());
      }
    }
  }
  ```
  ### Ejercicio 5.2
  ```java
  import java.io.File;

  public class MyClass {
    public static void main(String args[]) {
      String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
      for (String dia : dias) {
          File dir = new File ("P1/" + dia);
          if (!dir. exists()) {
              dir.mkdir();
              System.out.println("Se ha creado el directorio con ruta absoluta:" + dir.getAbsolutePath());
          }
      }
    }
  }
  ```
## Ejercicio 6. (0.5 puntos)
  **Haciendo uso de uno de los métodos de la clase Java.io.File, liste y muestre por pantalla todos los archivos del directorio  cine_granada". Deberá mostrar la ruta relativa con el siguiente mensaje: "Archivos creados hasta ahora:" "ruta relativa: /…/…/…", "ruta relativa: /…/…/…", …**
  ```java
  import java.io.File;
  public class MyClass {
    public static void main(String args[]) {
      String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
    
      System.out.println("Archivos creados hasta ahora:");
      for (String dia : dias) {
          File origen = new File ("P1/" + dia);
          File destino = new File ("P1/cine_granada/" + dia);
          origen.renameTo(destino);
          if (!destino. exists()) {
              destino.mkdir();
              System.out.println("· Ruta relativa: " + destino.getPath());
          }
      }
    }
  }
  ```
