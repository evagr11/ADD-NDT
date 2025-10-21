# TEMA 2: Introducción al manejo de ficheros
Enlaces de apoyo: 
- [FileInputStream](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/FileInputStream.html)
- [PipedInputStream](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/PipedInputStream.html) y [PipedOutputStream](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/PipedOutputStream.html)

- [CharArrayReader](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/CharArrayReader.html) y [CharArrayWriter](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/CharArrayWriter.html)

- [PushbackReader](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/PushbackReader.html)

- [StreamTokenizer](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/StreamTokenizer.html)

- [LineNumberReader](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/LineNumberReader.html) 

- [DataInputStream](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/DataInputStream.html) y [DataOutputStream](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/io/DataOutputStream.html)

## Definición de flujo Stream
**Concepto**: Secuencia ordenada de información; recurso de entrada o salida; unidireccional.

**Propósito**: Transportar datos de un origen a un destino de forma secuencial sin asumir estructura fija.
  
## Streams basados en ficheros
**Uso principal**: Acceso a datos persistentes en disco.

**Tipos por unidad**:
- **Bytes**: FileInputStream, FileOutputStream — leer/escribir secuencias de bytes.
- **Caracteres**: FileReader, FileWriter — lectura/escritura con codificación de caracteres.

**Nota**: RandomAccessFile no se clasifica típicamente como Stream secuencial tradicional.

## Tuberías Pipe entre hilos
**Propósito**: Comunicación entre dos hilos dentro del mismo proceso; permiten pasar datos en tiempo real de un hilo a otro.

**Tipos por unidad**:
- **Bytes**: PipedInputStream, PipedOutputStream.
- **Caracteres**: PipedReader, PipedWriter.

**Patrón básico**: conectar una salida piped con una entrada piped; un hilo escribe en la salida y otro hilo lee de la entrada.

```java
final PipedOutputStream salida = new PipedOutputStream();
final PipedInputStream entrada = new PipedInputStream(salida);

Thread escritor = new Thread(() -> {
  try { salida.write("Hola por aquí!".getBytes()); } catch (IOException ignored) {}
});
Thread lector = new Thread(() -> {
  try {
    int b;
    while ((b = entrada.read()) != -1) System.out.print((char)b);
  } catch (IOException ignored) {}
});
escritor.start(); lector.start();
```

## Streams basados en arrays
**Propósito**: Leer y escribir datos directamente en arrays en memoria; útiles para pruebas, buffers y transformaciones intermedias.

**Tipos por unidad**:
- **Bytes**: ByteArrayInputStream, ByteArrayOutputStream.
- **Caracteres**: CharArrayReader, CharArrayWriter.

**Patrón básico**: crear reader/writer sobre un array, leer/escribir secuencialmente y convertir de/para String o arrays.

```java
// Lectura desde array de chars
char[] texto = "texto de ejemplo".toCharArray();
CharArrayReader r = new CharArrayReader(texto);
int c;
while ((c = r.read()) != -1) System.out.print((char)c);
r.close();

// Escritura en CharArrayWriter
CharArrayWriter w = new CharArrayWriter();
w.write("texto de ejemplo 2");
char[] resultado = w.toCharArray();
System.out.println(new String(resultado));
w.close();
```

## Análisis de flujos Stream parsing
**Propósito**: Leer partes del flujo para interpretar estructura o tokens antes de procesar completamente los datos.

### Clases de inspección y utilidades
- **Pushback**: PushbackInputStream, PushbackReader — permiten "deshacer" la lectura empujando bytes/caracteres de vuelta al flujo para relectura.
  ```java
  // PushbackReader: leer y devolver carácter
  PushbackReader pr = new PushbackReader(new FileReader("tema2.txt"));
  int d = pr.read();
  System.out.println((char)d);
  pr.unread(d);
  d = pr.read();
  System.out.println((char)d);
  pr.close();
  ```
- **Tokenización**: StreamTokenizer — identifica palabras, números, caracteres especiales, fin de línea y fin de fichero. Campos comunes: TT_EOF, TT_EOL, TT_NUMBER, TT_WORD.
  ```java
  // StreamTokenizer desde String
  StreamTokenizer st = new StreamTokenizer(new StringReader("Este es el texto de ejemplo numero 12"));
  while (st.nextToken() != StreamTokenizer.TT_EOF) {
    if (st.ttype == StreamTokenizer.TT_WORD) System.out.println("Letra: " + st.sval);
    else if (st.ttype == StreamTokenizer.TT_NUMBER) System.out.println("Numero: " + st.nval);
  }
  ```
- **Conteo de líneas**: LineNumberReader — leer líneas y obtener/setear el número de línea con getLineNumber y setLineNumber.
  ```java
  // LineNumberReader: leer con número de línea
  LineNumberReader lnr = new LineNumberReader(new FileReader("tema2.txt"));
  String linea;
  while ((linea = lnr.readLine()) != null) {
    System.out.println("Contenido de la linea numero: " + lnr.getLineNumber());
    System.out.println(linea);
  }
  lnr.close();
  ```
## Tratamiento de información Datos primitivos
**Propósito**: Leer y escribir tipos primitivos Java de forma binaria y portable.

**Clases**: DataOutputStream, DataInputStream.

**Operaciones habituales**: 
- writeInt — readInt
- writeFloat — readFloat
- writeDouble — readDouble
- writeLong — readLong

 ```java
  // Escritura de primitivos
  DataOutputStream dos = new DataOutputStream(new FileOutputStream("tema2_1.txt"));
  dos.write(65); dos.writeInt(12345); dos.writeFloat(3.14f); dos.writeDouble(2.718);
  dos.close();
  
  // Lectura de primitivos
  DataInputStream dis = new DataInputStream(new FileInputStream("tema2_1.txt"));
  System.out.println(dis.read());        // byte leído como int
  System.out.println(dis.readInt());     // int
  System.out.println(dis.readFloat());   // float
  System.out.println(dis.readDouble());  // double
  dis.close();

 ```
  

