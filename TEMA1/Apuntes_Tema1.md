# TEMA 1: Introducción al manejo de ficheros
Enlace de apoyo: https://www.w3schools.com/java/java_files.asp

## 1 Operaciones básicas con ficheros
Crear, modificar y borrado

## 2 Definición y tipos de ficheros
**Definición**: Sucesión de bits almacenada enn un fichero

**Tipos**
- **ASCII**: Lineas de texto en formato ASCII
- **Binarios**: Información en codigo binario

### 2.1 Clase File (java.io.File)
Representa de forma abstracta un fichero o directorio
- Constructor: **File** (**String** *pathname*)
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/file.txt");
  ```
#### Crear
- Metodo: [boolean] createNewFile()
- Uso: Crea un fichero nuevo y vacío con la ruta indicada en el constructor
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/file.txt");
  fichero.createNewFile();
  ```
#### Borrar
- Metodo: [boolean] delete()
- Uso: Elimina el fichero o directorio definido por la ruta indicada en el constructor
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/file.txt");
  fichero.delete();
  ```
#### Crear directorio
- Metodo: [boolean] mkdir()
- Uso: Crear el directorio deseado
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.mkdir();
  ```
- Metodo: [boolean] mkdirs()
- Uso: Crea el directorio deseado **incluyendo cualquier directorio padre no existente**
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.mkdirs();
  ```
#### Nombre del fichero
- Metodo: [String] getName()
- Uso: Devuelve el pathname
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.getName();
  ```
#### Renombrar
- Metodo: [boolean] renameTo(File dest)
- Uso: Renombra el fichero. Puede servir para mover archivos
- Ejemplo:
  ```java
  File fichero1 = new File ("/Desktop/fichero1.txt");
  File fichero2 = new File ("/Desktop/fichero2.txt");
  fichero1.renameTo(fichero2);
  ```
#### Comprobar existencia
- Metodo: [boolean] exists()
- Uso: Comprueba si existe el fichero
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/fichero1.txt");
  fichero.exists();
  ```

#### Comprobar ruta
  ##### Ruta absoluta
- Metodo: [boolean] getAbsolutePath()
- Uso: Devuelve la ruta completa del recurso desde el directorio raíz
- Ejemplo:
  ```java
  File fichero = new File(“/Desktop/folder);
  fichero.getAbsolutePath();
  ```
##### Ruta relativa
- Metodo: [String] getPath()
- Uso: Devuelve una parte de la ruta, teniendo en cuenta el directorio actual
- Ejemplo:
  ```java
  File fichero = new File(“/Desktop/folder);
  fichero.getPath();
  ```
  
#### Nombre del directorio superior
- Metodo: [String] getParent()
- Uso: Devuelve el pathname del directorio superior (padre).
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.getParent();
  ```
  
#### Nombre del directorio superior
- Metodo: [ File[ ] ] listFiles()
- Uso: Devuelve un array de Files que representan los ficheros del directorio indicado.
- Ejemplo:
  ```java
  File fichero = new File ("/Desktop/folder");
  fichero.listFiles();
  ```
  
#### Comprobación para escribir y leer
- Metodo: [boolean] canWrite() y [boolean] canRead()
- Uso: Comprueba si se puede escribir (write) o leer (read) el archivo.
- Ejemplo:
  ```java
  File fichero = new File(“/Desktop/fichero.txt);
  fichero.canWrite();
  fichero.canRead();
  ```

## 3 Formas de accceder a un fihero
  - **Secuencial**: Accedemos al fichero carácter a carácter (o byte a byte) de forma ordenada desde el inicio hasta el final
  - **Aleatorio**: Nos permite acceder a posiciones concretas de nuestro fichero
    
### 3.1 Acceso secuencial mediante Bytes
**Clase FileInputStream**: Leer datos en bruto como bytes desde un archivo. Es ideal para leer archivos binarios.
- Metodo: [int] read()
- Uso: Lee un byte del fichero (en orden). Devuelve -1 si no quedan datos que leer.
- Ejemplo:
  ```java
  FileInputStream entrada = new
  FileInputStream(“desktop/fichero.bin”);
  entrada.read();
  entrada.close();
  ```
**Clase FileOutputStream**: Escribir datos en bruto como bytes a un archivo. Es ideal para escribir archivos binarios.
- Metodo: write(byte[ ] b)
- Uso: Escribe un conjunto de bytes en un fichero.
- Ejemplo:
  ```java
  FileOutputStream output = new FileOutputStream(“desktop/fichero.txt”);
  String cadena = “prueba de escritura”;
  byte [ ] arrayBytes = cadena.getBytes();
  output.write(arrayBytes);
  output.close();
  ```
  
### 3.2 Acceso secuencial mediante caracteres
**Clase FileReader**: Leer datos de caracteres de un archivo. Es adecuado para leer archivos de texto como .txt, .xml, .json, etc.
- Metodo: [int] read()
- Uso: Lee un carácter del fichero (en orden). Devuelve -1 si no quedan datos que leer.
- Ejemplo:
  ```java
  FileReader entrada = new FileReader(“desktop/fichero.txt”) char datos = (char)entrada.read();
  entrada.close();
  ```
**Clase FileWriter**: Escribir datos de caracteres en un archivo. Es adecuado para escribir archivos de texto como .txt, .xml, .json, etc.
- Metodo: write()
- Uso: Escribe un conjunto de caracteres en un fichero.
- Ejemplo:
  ```java
  FileWriter escritor = new FileWriter(“desktop/fichero.txt”);
  escritor.write(“texto de ejemplo a escribir”);
  escritor.close();
  ```
## 4 Acceso aleatorio basado en bytes
Permite leer/escribir en cualquier punto del fichero
##### **Constructor**: 
RandomAccessFile(String path, string mode) o RandomAccessFile(File file, string mode)
##### **Modos:**
- r : solo lectura
- rw : lectura y escritura
- rwd : lectura y escritura, sin garantizar actualización de metadatos
- rws : lectura y escritura, garantizando actualización de metadatos

**Metodos:**
- seek(long position) : Permite posicionarnos en el punto que indiquemos en el fichero
  Ejemplo:
  ```java
  file.seek(12);
  ```
- read() : Permite leer un Byte donde está colocado el puntero
  Ejemplo:
  ```java
  file.read();
  ```
- readLine() : Permite leer la siguiente línea de Bytes a partir de donde está colocado el puntero
  Ejemplo:
  ```java
  file.readLine();
  ```
- write(byte[] b) : Permite escribir un Byte donde está colocado el puntero. El puntero avanza tras escribir
  Ejemplo:
  ```java
  file.write(“Example”.getBytes());
  ```
- writeBytes(String s) : Permite escribir un String como una secuencia de Bytes
  Ejemplo:
  ```java
  file.writeBytes(“Example”);
  ```
  
