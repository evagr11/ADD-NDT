# TEMA 1: Introducción al manejo de ficheros
Enlace de apoyo: https://www.w3schools.com/java/java_files.asp

## Introducción al manejo de ficheros
Crear, modificar y borrado

## Definición y tipos de ficheros
Definición: Sucesión de bits almacenada enn un fichero

**Tipos**
- **ASCII**: Lineas de texto en formato ASCII
- **Binarios**: Información en codigo

### 2.1 Clase File
- File: representación abstracta de un fichero en java
- Constructor: **File** (**String** *pathname*)
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/file.txt");
  ```
#### Crear
- Metodo: [boolean] createNewFile()
- Crear fichero: Crea un fichero nuevo y vacio con la ruta indicada en el constructor
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/file.txt");
  fichero.createNewFile();
  ```
#### Borrar
- Metodo: [boolean] delete()
- Crear fichero: Elimina el fichero o directorio definido por la ruta indicada en el constructor
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/file.txt");
  fichero.delete();
  ```
#### Crear directorio
- Metodo: [boolean] mkdir()
- Crear fichero: Crear el directorio deseado
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.mkdir();
  ```
- Metodo: [boolean] mkdirs()
- Crear fichero: Crea el directorio deseado **incluyendo cualquier directorio padre no existente**
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.mkdirs();
  ```
- Metodo: [String] getName()
- Crear fichero: Devuelve el pathname
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.getName();
  ```
