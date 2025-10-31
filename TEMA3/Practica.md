# TEMA 3. Práctica 1

## Parte 1. Crear un parseador DOM:
Tal y como hemos visto en la explicación teórica del tema 3, para crear un parseador de
tipo DOM de la librería javax.xml.parsers necesitábamos:

1.1 Crear un objeto de tipo DocumentBuilderFactory que nos permitirá crear un objeto de
tipo DocumentBuilder (el propio parseador XML de tipo DOM).

1.2 Podemos definir propiedades para los parseadores DOM que se vayan a crear a partir
de la clase DocumentBuilderFactory como activar la validación del XML o ignorar los
elementos vacíos que no son necesarios para un análisis XML.

1.3 Ahora tal y como hemos mencionado en el punto 1.1, creamos un parseador DOM
haciendo uso de la clase DocumentBuilder (Recuerde que debe realizar un control de
excepciones)

1.4 El siguiente punto será leer el propio documento XML haciendo uso del parser de tipo
DOM, para ello creamos una variable de tipo File apuntando al fichero y almacenaremos en
memoria el archivo XML haciendo uso de un dato de tipo Document (Los parseadores de
tipo DOM almacenan en memoria todo el XML)
```java
package practica3;

import java.io.File;
import java.io.IOException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import org.w3c.dom.Document;
import org.xml.sax.SAXException;

public class Practica3 {

    public static void main(String[] args) throws ParserConfigurationException {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        //factory.setValidating(true);
        factory.setIgnoringElementContentWhitespace (true);
        
        try{
            DocumentBuilder builder = factory.newDocumentBuilder();
            File file = new File("ejemploXML.xml");
            Document doc = (Document) builder.parse(file);
            doc.getDocumentElement().normalize();
        } catch (ParserConfigurationException | SAXException | IOException e){
            e.printStackTrace();
        }
    }
    
}
```

## Parte 2. Procesamiento de un fichero XML haciendo uso de un parseador DOM:
Para procesar (en este caso leer) un archivo XML vamos a hacer uso de la clase XPath, la
cual es una recomendación del W3C. Esta clase permite navegar y seleccionar nodos
dentro de un documento XML.

2.1 Para crear una instancia de la clase XPath lo hacemos de la siguiente forma:

2.2 Lo siguiente que debemos hacer es crear lo que se conoce en XPath como “expresión”.
Esto es un String que se utiliza para navegar por un documento XML. Por ejemplo,
crearemos la siguiente expresión que nos permitirá buscar todos los nodos <student> que
son hijos del nodo <class>:

2.3 A continuación vamos a crear un objeto de tipo NodeList (lista de nodos, almacenará los
nodos analizados por nuestro parseador y almacenado en la variable “doc”):

Vamos a explicar la línea anterior con más detalle:

2.3.1 XPath.compile(expresion): compila (Significa preparar o transformar la expresión
(escrita como un string) en un formato que el motor de XPath pueda interpretar y ejecutar
eficientemente.) y almacena la expresión definida anteriormente para su uso.

2.3.2 .evaluate(doc, XPathConstants.NODESET): Ejecuta la expresión definida sobre el
documento especificado en el primer parámetro y obtiene todos los nodos que cumplen
esa expresión. El segundo argumento especifica que queremos que devuelva un conjunto
de nodos.

2.4 En este punto ya tenemos la lista de nodos de nuestro archivo XML que son de tipo
student e hijos directos de class. Ahora se propone imprimir de nuestro archivo XML todos
los datos de estos estudiantes

Para ello tendremos que recorrer nuestra lista de nodos haciendo uso de un bucle de la
siguiente forma, donde en cada iteración obtenemos un determinado nodo. En este caso,
cada nodo es un elemento de tipo “student” ya que así lo hemos especificado en la
expresión utilizada por XPath):

(Nota: todo el código mostrado a continuación se sitúa dentro del bucle anteriormente
mencionado)

2.5 Si por ejemplo queremos mostrar “Current Element: student” tal y como muestra la
Figura 1 podemos utilizar el siguiente método (getNodeName() obtiene el nombre del nodo):

2.6 Para mostrar por ejemplo “Student Roll no: …”, es decir un atributo de un nodo,
podemos utilizar el siguiente método getAttribute(String attribute)

Se hace un casting a la clase “Element” para poder utilizar más métodos interesantes para
este ejercicio guiado.

2.7 Para obtener la siguiente línea de la Figura 1 “First Name: …” podemos utilizar el
método getElementsbyTagName(“String tagName”):

```java
package practica3;

import java.io.File;
import java.io.IOException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

public class Practica3 {

    public static void main(String[] args) {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        //factory.setValidating(true);
        factory.setIgnoringElementContentWhitespace (true);
        
        try{
            DocumentBuilder builder = factory.newDocumentBuilder();
            File file = new File("ejemploXML.xml");
            Document doc = (Document) builder.parse(file);
            doc.getDocumentElement().normalize();
            
             XPath xPath = XPathFactory.newInstance().newXPath();
            String expression = "/class/student";
            NodeList nodeList = (NodeList) xPath.compile(expression).evaluate(doc, XPathConstants.NODESET);
            for (int i = 0; i < nodeList.getLength(); i++){
                Node nNode = nodeList.item(i);
                System.out.println("Current Element: " + nNode.getNodeName());
                Element eElement = (Element) nNode;
                System.out.println("Student roll no: " + eElement.getAttribute("rollno"));
                System.out.println("First Name: " + eElement.getElementsByTagName("firstname").item(0). getTextContent());
                System.out.println("Last Name: " + eElement.getElementsByTagName("lastname").item(0). getTextContent());
                System.out.println("Nick Name: " + eElement.getElementsByTagName("nickname").item(0). getTextContent());
                System.out.println("Marks: " + eElement.getElementsByTagName("marks").item(0). getTextContent());
            }
        } catch (XPathExpressionException | ParserConfigurationException | SAXException | IOException e){
            e.printStackTrace();
        }
    }
}

```
