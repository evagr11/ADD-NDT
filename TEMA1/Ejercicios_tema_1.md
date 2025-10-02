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
