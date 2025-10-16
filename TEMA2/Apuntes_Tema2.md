# TEMA 2: Introducción al manejo de ficheros
Enlace de apoyo: https://www.w3schools.com/java/java_files.asp

## 1 Operaciones básicas con ficheros
Rellenar

## 2 Clasificacion de los Streams
### Ficheros


### Tuberias
**Tuberias**: Mecanismos que permite la comunicación entre dos hilos (threads) de un mismo programa. Necesario para la comunucacion de un mismo proceso y dos hilos diferentes
- **Bytes**: PipedInputStream - PipedOutputStream
- **Caracteres**: PipedReader - PiedWriter

### Arrays
- **Bytes**: ByteArrayInputStream - ByteArrayOutputStream
- **Caracteres**: CharArrayReader - CharArrayWriter

### Analisis
Permiten analizar ciertas partes de flujos de datos
- **Bytes**: PushBackInputStream - StreamTokenizer
- **Caracteres**: PushBackReader - LineNumberReader

Analisis de datos previo: A veces es necesario leer algo de información para saber que se aproxima e interpreta ña informacion actual

Los bytes leidos luego son empujados de vuelta al flujo para poder seguir haciendo read(). (pushBack)
