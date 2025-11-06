# PRACTICA FINAL DOM
Desarrolle una aplicación en Java que permita leer, consultar y modificar información de un
fichero XML utilizando el método de parseo DOM.

Implemente un programa en Java llamado GestorProductosXML que permita al usuario elegir,
mediante un menú, procesando el fichero XML usando DOM.

El menú en cuestión es:

## 1. Mostrar todos los productos (DOM)
- Utiliza la DOM para leer el fichero productos.xml.
- Muestra por pantalla todos los productos con su nombre, categoría, precio y stock.
```java
    // ==========================================================
    // Método 1: Mostrar todos los productos
    // ==========================================================
    public static void mostrarProductos() {
        try {
            Document doc = cargarDocumento();
            NodeList lista = doc.getElementsByTagName("producto"); //PISTA

            System.out.println("\n===== LISTA DE PRODUCTOS =====");

            for (int i = 0; i < lista.getLength(); i++) {
                Element producto = (Element) lista.item(i);
                String id = producto.getAttribute("id");
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
```
## 2. Añadir un nuevo producto (DOM)
- Solicita al usuario los datos del nuevo producto (id, nombre, categoría, precio y stock).
- Añade un nuevo nodo <producto> al documento XML.
- Guardar los cambios en el documento. Usa Transformer para escribir el fichero actualizado. (Código ya implementado)

Investigar por internet sobre el uso de Transformer aunque se facilita el siguiente blog como
información útil y de interés http://jmoral.es/blog/xml-dom.

## 3. Incrementar precios de una categoría (DOM)
- Solicita al usuario una categoría (por ejemplo, “Informática”).
- Incrementa el precio de todos los productos de esa categoría en un 10%.
- Guarda los cambios en un nuevo fichero productos_actualizados.xml. (Código ya implementado).

## Material de apoyo
Se facilitan los siguientes documentos para el desarrollo de la práctica:
- productos.xml: listado de los productos que se deberá consultar.
- GestorProductosXML.java: esqueleto de la clase principal que muestra el menú interactivo.
- DOMManager.java: clase que gestiona las funcionalidades de DOM que se deben implementar.
