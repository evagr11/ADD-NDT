# PREGUNTAS TEMA 2
## 1. PushbackReader
Investigue las clases para el análisis de flujos de datos: PushBackReader. Concretamente investigue el comportamiento de los métodos: read y unread. Para ello, conteste a las siguientes preguntas, incluyendo las explicaciones y capturas de pantalla necesarias.
- ¿El uso de read modifica el archivo?
  No, solo lee el primer caracter
  ![Pregunta1a](Preguntas/Pregunta1a.PNG)
- Utilizando read, ¿puedo leer más de un carácter?
  Si, haciendo un while consigo que lea todos los caracteres, hasta que deja de haberlos
  ![Pregunta1b](Preguntas/Pregunta1b.PNG)
- ¿Qué ocurre si hago unread sin leer previamente un carácter?
  Es necesario pasarle un dato a unread(), le paso un caracter aleatorio. Compruebo que ese dato no vuelve al archivo
  ![Pregunta1c](Preguntas/Pregunta1c.png)
## 2. PipedInputStream y PipedOutputStream
- ¿Qué ocurre si intentas leer antes de que se haya escrito nada?
  El hilo lector se bloquea durante en este caso 3 segundos esperando datos hasta que el escritor los envia
  ![Pregunta2a](Preguntas/Pregunta2a.png)
- ¿Se puede conectar un PipedInputStream con varios PipedOutputStream?
  No, solo se permite una conexión directa por flujo. en el error vemos que nos dice: Already connectedat java.base/java.io.PipedOutputStream.connect
  ![Pregunta2b](Preguntas/Pregunta2b.png)
## 3. StreamTokenizer y LineNumberReader
Crea un programa que lea un archivo de texto y cuente:
- Cuántas palabras tiene.
- Cuántos números aparecen.
- Cuántas líneas se procesaron.
  ![Pregunta3abc](Preguntas/Pregunta3abc.png)
## 4. DataInputStream
- ¿Qué ocurre si lees los datos en un orden distinto al que fueron escritos?
  Se devuelven datos corruptos. El orden de lectura debe coincidir con el de escritura. Ocurre porque los datos se interpretan mal ya que los bytes no coinciden con el tipo esperado
  ![Pregunta4a](Preguntas/Pregunta4a.png)
- ¿Qué pasa si intentas leer más bytes de los disponibles?
  Muestra en consola el primer int y luego se lanza una EOFException al intentar mostrar el segundo int indicando que no hay más datos para leer
  ![Pregunta4b](Preguntas/Pregunta4b.png)
