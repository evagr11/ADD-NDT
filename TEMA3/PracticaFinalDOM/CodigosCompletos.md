# Archivo con todos los códigos comentados y listos para copiar y pegar

## `DOMManager.java`
```java
package practicafinaldom;

import java.io.File;
import java.util.Scanner;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.NodeList;


public class DOMManager {

    private static final String RUTA_XML = "productos.xml";

    // ==========================================================
    // Método 1: Mostrar todos los productos
    // ==========================================================
    public static void mostrarProductos() {
        try {
            
            Document doc = cargarDocumento();
            NodeList lista = doc.getElementsByTagName("producto"); //PISTA

            System.out.println("\n===== LISTA DE PRODUCTOS =====");

            for (int i = 0; i < lista.getLength(); i++) {
                // convierto el nodo actual en un elemto para acceder a sus datos
                Element producto = (Element) lista.item(i);
                // obtengo el ATRIBUTO 'id' del producto
                String id = producto.getAttribute("id");
                // obtengo el texto de cada uno de los subelementos
                String nombre = producto.getElementsByTagName("nombre").item(0).getTextContent();
                String categoria = producto.getElementsByTagName("categoria").item(0).getTextContent();
                String precio = producto.getElementsByTagName("precio").item(0).getTextContent();
                String stock = producto.getElementsByTagName("stock").item(0).getTextContent();

                System.out.println(
                        "ID: " + id + 
                        " | Nombre: " + nombre + 
                        " | Categoria: " + categoria + 
                        " | Precio: " + precio + 
                        " | Stock: " + stock);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // ==========================================================
    // Método 2: Añadir nuevo producto
    // ==========================================================
    public static void agregarProducto() {
        Scanner sc = new Scanner(System.in);
        try {
            Document doc = cargarDocumento();
            // obtiene el elemento raíz del documento (<productos>)
            Element raiz = doc.getDocumentElement(); //PISTA
            
            
            System.out.println("\n===== ANADA UN NUEVO PRODUCTO =====");

            NodeList lista = doc.getElementsByTagName("producto");
            
            // busco el ID más alto para generar uno nuevo
            int maxId = 0;
            for (int i = 0; i < lista.getLength(); i++) {
                Element producto = (Element) lista.item(i);
                String idExistente = producto.getAttribute("id");
                int num = Integer.parseInt(idExistente.substring(1)); //extrae el numero del ID
                if (num > maxId) maxId = num; //actualizo el máximo si es necesario
            }
            //genero el nuevo ID
            String id = "P" + (maxId + 1);
        
            //TODO: Solicitar al usuarios los datos del producto
            System.out.print("Nombre: ");
            String nombre = sc.nextLine();
            System.out.print("Categoria: ");
            String categoria = sc.nextLine();
            System.out.print("Precio: ");
            String precio = sc.nextLine();
            System.out.print("Stock: ");
            String stock = sc.nextLine();
            
            //TODO: Crear el nuevo producto (nodo) 
            
            //creo un nuevo elemento producto en el XML
            Element nuevoProducto = doc.createElement("producto");
            //asigno el ATRIBUTO ID al nuevo producto
            nuevoProducto.setAttribute("id", id);

            Element nombreElem = doc.createElement("nombre");
            //establezco el texto del elemento con el valor del usuario
            nombreElem.setTextContent(nombre);
            //añado el elemento como hijo del nodo <producto>
            nuevoProducto.appendChild(nombreElem);

            Element categoriaElem = doc.createElement("categoria");
            categoriaElem.setTextContent(categoria);
            nuevoProducto.appendChild(categoriaElem);

            Element precioElem = doc.createElement("precio");
            precioElem.setTextContent(precio);
            nuevoProducto.appendChild(precioElem);

            Element stockElem = doc.createElement("stock");
            stockElem.setTextContent(stock);
            nuevoProducto.appendChild(stockElem);

            //añado el nuevo producto al XML
            raiz.appendChild(nuevoProducto);
        
            //Guarda los cambios realizados con Transformer
            guardarDocumento(doc, RUTA_XML);
            System.out.println("Producto agregado correctamente.");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // ==========================================================
    // Método 3: Incrementar precios por categoría
    // ==========================================================
    public static void incrementarPreciosPorCategoria() {
        Scanner sc = new Scanner(System.in);
        try {
            Document doc = cargarDocumento();
            
            NodeList lista = doc.getElementsByTagName("producto");

            //TODO: Solicitar la categoría al usuario
            
            System.out.println("\n===== ACTUALIZACION DE PRECIOS POR CATEGORIA =====");
            System.out.println("Esto hara que el precio de todos los productos de la categoria que escojas se incremente en un 10%");
            System.out.print("Ingrese la categoria a actualizar: ");
            String categoriaInput = sc.nextLine().toLowerCase();
            
            boolean existe = false;

            //TODO: Recorrer el listado de productos y modificar su valor
            
            // recorro la lista de productos para buscar las categorias iguales y modificar los precios
            for (int i = 0; i < lista.getLength(); i++) {
                Element producto = (Element) lista.item(i);
                String cat = producto.getElementsByTagName("categoria").item(0).getTextContent().toLowerCase();
                if (cat.equals(categoriaInput)) {
                    existe = true; // marco q si existe si se encuentra
                    Element precioElem = (Element) producto.getElementsByTagName("precio").item(0);
                    // convierto el texto del precio a tipo double
                    double precio = Double.parseDouble(precioElem.getTextContent());
                    double nuevoPrecio = precio * 1.10;
                    // actualizo el contenido del precio con el nuevo valor, formateado a dos decimales
                    precioElem.setTextContent(String.format("%.2f", nuevoPrecio));
                }
            }
            
            
            // si la categoría no existe, informo al usuario y vuelve al menu
            if (!existe) {
                System.out.println("Categoria no valida. Volviendo al menu...");
                return;
            }
            
            guardarDocumento(doc, "productos_actualizados.xml");
            System.out.println("Precios actualizados para la categoria: " + categoriaInput);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // ==========================================================
    // Métodos auxiliares
    // ==========================================================
    private static Document cargarDocumento() throws Exception {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder builder = factory.newDocumentBuilder();
        Document doc = builder.parse(new File(RUTA_XML));
        doc.getDocumentElement().normalize();
        return doc;
    }

    private static void guardarDocumento(Document doc, String ruta) throws Exception {
        TransformerFactory tf = TransformerFactory.newInstance();
        Transformer transformer = tf.newTransformer();
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");

        DOMSource source = new DOMSource(doc);
        StreamResult result = new StreamResult(new File(ruta));

        transformer.transform(source, result);
    }
}

```

## `GestorProductosXML.java`
```java
package practicafinaldom;

import java.util.Scanner;

public class GestorProductosXML {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int opcion;

        do {
            mostrarMenu();
            opcion = Integer.parseInt(sc.nextLine());

            switch (opcion) {
                case 1 -> DOMManager.mostrarProductos();
                case 2 -> DOMManager.agregarProducto();
                case 3 -> DOMManager.incrementarPreciosPorCategoria();
                case 0 -> System.out.println("Saliendo del programa...");
                default -> System.out.println("Opcion incorrecta. Intente de nuevo.");
            }

            System.out.println();
        } while (opcion != 0);

        sc.close();
    }

    private static void mostrarMenu() {
        System.out.println("===== GESTOR DE PRODUCTOS XML =====");
        System.out.println("1. Mostrar todos los productos (DOM)");
        System.out.println("2. Anadir un nuevo producto (DOM)");
        System.out.println("3. Incrementar precios de una categoria (DOM)");
        System.out.println("0. Salir");
        System.out.print("Seleccione una opcion: ");
    }
}
```

## `productos.xml`
```xml
<productos>
    <producto id="P1">
        <nombre>Portátil Lenovo</nombre>
        <categoria>INF</categoria>
        <precio>799.99</precio>
        <stock>15</stock>
    </producto>
    <producto id="P2">
        <nombre>Ratón Logitech</nombre>
        <categoria>PER</categoria>
        <precio>25.50</precio>
        <stock>50</stock>
    </producto>
    <producto id="P3">
        <nombre>Monitor Samsung</nombre>
        <categoria>INF</categoria>
        <precio>199.99</precio>
        <stock>30</stock>
    </producto>
    <producto id="P4">
        <nombre>Teclado Razer</nombre>
        <categoria>GAM</categoria>
        <precio>99.99</precio>
        <stock>10</stock>
    </producto>
</productos>
```
