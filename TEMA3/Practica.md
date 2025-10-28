## Parte 1. Crear un parseador DOM:
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
