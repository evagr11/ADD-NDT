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
