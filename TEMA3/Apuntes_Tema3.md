# TEMA 3: Trabajo con ficheros XML
Enlaces de apoyo: 

- [DOM](https://www.w3schools.com/js/js_htmldom.asp)
- SAX: [fácil y teórica](https://www.tutorialspoint.com/xerces/xerces_sax_parser.htm) y [un ejemplo real](https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/?utm_source=chatgpt.com)

## Acceso a datos con DOM y SAX
**DOM** (Document Object Model): Modelo basado en nodos que carga todo el fichero XML en memoria. 

**SAX** (Simple API for XML): Modelo basado en eventos que lee el fichero línea a línea sin cargarlo completamente.

### Funcionalidades comunes
- Leer ficheros XML (DOM y SAX)
- Escribir ficheros XML (solo DOM)
- Detectar elementos
- Verificar validez del fichero

## Ventajas y desventajas
### DOM
- **Ventajas**: Crea un árbol de nodos en memoria, útil para tareas complejas.
- **Desventajas**: Almacena todo el fichero en memoria. Más lento que SAX.

### SAX
- **Ventajas**: Procesa el fichero línea a línea, sin cargarlo completamente.
- **Desventajas**: Menos potente, no tiene acceso completo al documento.
  
## Recomendación de uso
**DOM**: Ideal para tareas complejas que requieren conocer todo el fichero.
**SAX**: Adecuado para operaciones simples y lectura secuencial.

## Comparación entre DOM y SAX

| Característica | DOM  | SAX |
|------|------|------|
| Modelo de procesamiento               | Árbol en memoria | Basado en eventos |
| Lectura                               | Nodo a nodo      | Línea a línea     |
| Acceso al documento                   | Acceso completo, permite recorrer hacia delante y detrás | Acceso secuencial, no conoce todo el documento |
| Velocidad de ejecución                | Lento            | Rápido            |
| Uso de memoria                        | Alto (carga todo el fichero) | Bajo (no carga el fichero completo) |
| Escritura de XML                      | Sí               | No                |
| Inserción/eliminación de nodos        | Sí               | No                |
| Recomendación de uso                  | Tareas complejas con acceso total al documento | Operaciones simples y lectura secuencial |


## Configuración del parser DOM

Al trabajar con DOM para procesar ficheros XML, es importante configurar correctamente el parser. Estas son las opciones clave:

### Crear una instancia del parser

Se utiliza la clase `DocumentBuilderFactory` para generar el parser DOM:

```java
DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
```

### Ignorar espacios en blanco en DOM

La configuración `setIgnoringElementContentWhitespace` controla cómo el parser DOM trata los espacios vacíos y saltos de línea en el fichero XML.


####  `factory.setIgnoringElementContentWhitespace(true)`

- El parser **ignora** los espacios en blanco entre etiquetas.
- Solo se crean nodos con contenido significativo.
- El árbol DOM es más limpio y fácil de recorrer.


#### `factory.setIgnoringElementContentWhitespace(false)`
- El parser conserva todos los espacios y saltos de línea como nodos de texto (#text).
- El árbol DOM incluye nodos vacíos que representan espacios.
- Puede dificultar el análisis del contenido real.


### Crear el parser con DocumentBuilder
Se utiliza `DocumentBuilderFactory` para obtener una instancia de `DocumentBuilder`.

Este objeto es el encargado de convertir el fichero XML en un árbol DOM.

```java
DocumentBuilder builder = factory.newDocumentBuilder();
```

### Leer el fichero XML y obtener el documento
Se llama al método `parse()` para analizar el fichero y generar un objeto `Document`.

Este objeto contiene toda la estructura del XML en memoria.

```java
Document doc = builder.parse(file);
```


## Configuración del parser SAX

Al utilizar SAX para procesar ficheros XML, es necesario configurar el parser correctamente. Estas son las opciones clave que se aplican al crear la instancia:

### Crear una instancia del parser

Se utiliza la clase `SAXParserFactory` para generar el parser SAX:

```java
SAXParserFactory factory = SAXParserFactory.newInstance();
```

Esta clase permite configurar y obtener un objeto `SAXParser`.

El parser SAX trabaja de forma secuencial, sin cargar el fichero completo en memoria.

### Validar el documento XML

Activa la validación del fichero XML. Si el documento no es válido, se lanza una excepción:

```java
factory.setValidating(true);
```

### Leer el fichero XML con SAX
El modelo SAX (Simple API for XML) permite procesar ficheros XML de forma secuencial, sin cargar el documento completo en memoria. Es ideal para tareas simples y ficheros grandes.
Este objeto contiene toda la estructura del XML en memoria.

#### Crear una instancia del parser SAX

Se utiliza la clase `SAXParserFactory` para generar el parser SAX:

```java
SAXParser saxParser = factory.newSAXParser();
```

#### Leer el fichero XML

Se representa el fichero como un objeto `File` y se analiza con el parser:

```java
File file = new File("tema3_1.xml");
saxParser.parse(file, new DefaultHandler());
```
