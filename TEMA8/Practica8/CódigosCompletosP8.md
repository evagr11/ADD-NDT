# Códigos completos practica 8

## Autor.java
```java
package com.example.practica7;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;

@Entity
@Table(name = "autores")
public class Autor {

	@Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;
    private String nacionalidad;

    public Autor() {}

    public Autor(String nombre, String nacionalidad) {
        this.nombre = nombre;
        this.nacionalidad = nacionalidad;
    }

    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    public String getNacionalidad() { return nacionalidad; }
    public void setNacionalidad(String nacionalidad) { this.nacionalidad = nacionalidad; }

}
```

## AutorController.java
```java
package com.example.practica7;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/autores")
public class AutorController {
	@Autowired
	private AutorService autorService;
	
    @PostMapping 
    public ResponseEntity<Long> insertarAutor(@RequestBody Autor autor) { 
        
        Long id = autorService.insertarAutor(autor);
        
        return new ResponseEntity<>(id, HttpStatus.CREATED);
    }
    
    @PutMapping("/{id}") 
    public ResponseEntity<Void> actualizarNacionalidad(
            @PathVariable Long id,
            @RequestParam String nuevaNacionalidad
    ) {
        autorService.actualizarNacionalidad(id, nuevaNacionalidad);
        
        return new ResponseEntity<>(HttpStatus.OK);
    }
    
    @GetMapping("/{id}") 
    public ResponseEntity<Autor> obtenerAutorPorId(@PathVariable Long id) { 

        Autor autor = autorService.obtenerAutorPorId(id);
        
        if (autor != null) {
            return new ResponseEntity<>(autor, HttpStatus.OK);
        } else {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }
    
    @GetMapping("/") 
    public ResponseEntity<List<Autor>> obtenerTodos() {
        
        List<Autor> autores = autorService.obtenerTodos();
        
        if (autores != null && !autores.isEmpty()) {
            return new ResponseEntity<>(autores, HttpStatus.OK);
        } else {
            return new ResponseEntity<>(HttpStatus.NO_CONTENT);
        }
    }
    
    @GetMapping("/nombreIgualA/{dato}") 
    public ResponseEntity<List<Autor>> buscarAutoresPorNombre(@PathVariable String dato) { 
    	
        List<Autor> autores = autorService.obtenerAutoresPorNombre(dato);
        
        if (autores != null && !autores.isEmpty()) {
            return new ResponseEntity<>(autores, HttpStatus.OK);
        } else {
            return new ResponseEntity<>(HttpStatus.NO_CONTENT);
        }
    }

}
```

## AutorService.java
```java
package com.example.practica7;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction; 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import jakarta.persistence.criteria.CriteriaBuilder;
import jakarta.persistence.criteria.CriteriaQuery;
import jakarta.persistence.criteria.Root;

import java.util.List;

@Service
public class AutorService {

	
    @Autowired
    private SessionFactory sessionFactory;
    
    public AutorService() {}
    
    // Insertar (debe devolver el id del objeto insertado).
    public Long insertarAutor(Autor autor) {
    	Session session = sessionFactory.openSession();
    	Transaction transaction = null;
    	
    	try {
    		transaction = session.beginTransaction();
    		session.persist(autor);
    		transaction.commit();
    		return autor.getId();
    	} catch (Exception e) {
    		if(transaction != null) {
    			transaction.rollback();
    		}
    		e.printStackTrace();
    	} finally {
    		session.close();
    	}
		return (long) (-1);
    }
    
    // Borrar dado un objeto (parámetro).
    public void borrarAutor(Autor autor) {
        Session session = sessionFactory.openSession();
        Transaction transaction = null;
        
        try {
            transaction = session.beginTransaction();

			// Borra este autor.
			// Si ya está dentro de la sesión, bórralo directamente.
			// Si no lo está, primero mézclalo (merge) para obtener una versión gestionada y luego bórralo.
            session.remove(session.contains(autor) ? autor : session.merge(autor));
            
            transaction.commit();
        } catch (Exception e) {
            if (transaction != null) {
                transaction.rollback();
            }
            e.printStackTrace();
        } finally {
            session.close();
        }
    }
    
    // Actualizar un objeto dado un id y un atributo (nacionalidad).
    public void actualizarNacionalidad(Long id, String nuevaNacionalidad) {
        Session session = sessionFactory.openSession();
        Transaction transaction = null;
        
        try {
            transaction = session.beginTransaction();
            Autor autor = session.get(Autor.class, id);
            
            autor.setNacionalidad(nuevaNacionalidad);
            session.merge(autor);
            transaction.commit();
            
        } catch (Exception e) {
            if (transaction != null) {
                transaction.rollback();
            }
            e.printStackTrace();
        } finally {
            session.close();
        }
    }
    
    // Obtener un objeto dado un id (parámetro).
    public Autor obtenerAutorPorId(Long id) {
        Session session = sessionFactory.openSession();
        Autor autor = null;
        
        try {
            autor = session.get(Autor.class, id);
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            session.close();
        }
        
        return autor;
    }
    
    public List<Autor> obtenerAutoresPorNombre(String nombreParametro) {
        Session session = sessionFactory.openSession();
        List<Autor> listaAutores = null;
        
        try {
        	/*
            listaAutores = session.createQuery("FROM Autor WHERE nombre = :n", Autor.class)
                                  .setParameter("n", nombreParametro)
                                  .list();*/
        	CriteriaBuilder criteriaBuilder = session.getCriteriaBuilder();
        	
        	CriteriaQuery<Autor> criteriaQuery = criteriaBuilder.createQuery(Autor.class);
        	
        	Root<Autor> root = criteriaQuery.from(Autor.class);
        	
        	criteriaQuery.select(root).where(criteriaBuilder.equal(root.get("nombre"), nombreParametro));
        	
        	listaAutores = session.createQuery(criteriaQuery).getResultList();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            session.close();
        }
        
        return listaAutores;
    }
    
    public List<Autor> obtenerTodos() {
        Session session = sessionFactory.openSession();
        List<Autor> lista = null;
        try {
            lista = session.createQuery("FROM Autor", Autor.class).list();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            session.close();
        }
        return lista;
    }
}
```

## Libro.java
```java
package com.example.practica7;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;
import jakarta.persistence.Table;

@Entity
@Table(name = "libros")
public class Libro {

	@Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String titulo;
    private String isbn;

    @ManyToOne
    @JoinColumn(name = "autor_id") // Esta es la clave ajena (FK)
    private Autor autor;

    public Libro() {}

    public Libro(String titulo, String isbn, Autor autor) {
        this.titulo = titulo;
        this.isbn = isbn;
        this.autor = autor;
    }

    public Long getId() { return id; }
    public String getTitulo() { return titulo; }
    public void setTitulo(String titulo) { this.titulo = titulo; }
    public String getIsbn() { return isbn; }
    public void setIsbn(String isbn) { this.isbn = isbn; }
    public Autor getAutor() { return autor; }
    public void setAutor(Autor autor) { this.autor = autor; }
}
```

## LibroController.java
```java
package com.example.practica7;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/libros") 
public class LibroController {

    @Autowired
    private LibroService libroService;

    @Autowired
    private AutorService autorService;

    
    @PostMapping
    public ResponseEntity<Object> insertarLibro(
            @RequestBody Libro libro,  
            @RequestParam Long idAutor
    ) {
        Autor autor = autorService.obtenerAutorPorId(idAutor);

        if (autor == null) {
            return new ResponseEntity<>(
                "Error: No se puede crear el libro porque el autor con ID " + idAutor + " no existe.", 
                HttpStatus.NOT_FOUND
            );
        }

        libro.setAutor(autor);
        Long idGenerado = libroService.insertarLibro(libro);

        return new ResponseEntity<>(idGenerado, HttpStatus.CREATED);
    }
    
    @PutMapping("/{id}/titulo")
    public ResponseEntity<String> actualizarTitulo(
            @PathVariable Long id, 
            @RequestParam String nuevoTitulo) { 
    	
        Libro libro = libroService.obtenerLibroPorId(id);
        
        if (libro == null) {
            return new ResponseEntity<>("Error: No se encontró el libro con ID " + id + " para actualizar su título.", HttpStatus.NOT_FOUND);
        }

        libroService.actualizarTitulo(id, nuevoTitulo);
        return new ResponseEntity<>(HttpStatus.OK);
    }
    
    @PutMapping("/{id}/autor/{idAutor}")
    public ResponseEntity<String> actualizarAutor(@PathVariable Long id, @PathVariable Long idAutor) {
        Libro libro = libroService.obtenerLibroPorId(id);
        if (libro == null) {
            return new ResponseEntity<>("Error: No se encontró el libro con ID " + id, HttpStatus.NOT_FOUND);
        }

        Autor nuevoAutor = autorService.obtenerAutorPorId(idAutor);
        if (nuevoAutor == null) {
            return new ResponseEntity<>("Error: No se encontró el autor con ID " + idAutor, HttpStatus.NOT_FOUND);
        }

        libro.setAutor(nuevoAutor);
        
        libroService.actualizarLibro(libro); 
        
        return new ResponseEntity<>(HttpStatus.OK);
    }
    
	@DeleteMapping("/{id}")
	public ResponseEntity<String> eliminarLibro(@PathVariable Long id) {
	    Libro libro = libroService.obtenerLibroPorId(id);
	     
	    if (libro == null) {
	        return new ResponseEntity<>(
	            "Error: No se puede eliminar. El libro con ID " + id + " no existe en la base de datos.", 
	            HttpStatus.NOT_FOUND
	        );
	    }
	
	    try {
	        libroService.borrarLibro(libro);
	        return new ResponseEntity<>("Libro eliminado correctamente.", HttpStatus.OK);
	    } catch (Exception e) {
	        return new ResponseEntity<>(
	            "Error descriptivo: No se ha podido completar la eliminación.", 
	            HttpStatus.NOT_FOUND
	        );
	    }
	}

	@GetMapping("/{id}")
	public ResponseEntity<Object> obtenerLibroPorId(@PathVariable Long id) {
	    Libro libro = libroService.obtenerLibroPorId(id);

	    if (libro != null) {
	        return new ResponseEntity<>(libro, HttpStatus.OK);
	    } else {
	        return new ResponseEntity<>(
	            "Error: El libro con ID " + id + " no existe en nuestros registros.", 
	            HttpStatus.NOT_FOUND
	        );
	    }
	}
	
	@GetMapping
	public ResponseEntity<List<Libro>> obtenerTodos() {
	    List<Libro> libros = libroService.obtenerTodos();

	    return new ResponseEntity<>(libros, HttpStatus.OK);
	}
}
```

## LibroService.java
```java
package com.example.practica7;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction; 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import jakarta.persistence.criteria.CriteriaBuilder;
import jakarta.persistence.criteria.CriteriaQuery;
import jakarta.persistence.criteria.Root;

import java.util.List;

@Service
public class LibroService {

    @Autowired
    private SessionFactory sessionFactory;
    
    public LibroService() {}
    
    // Insertar (debe devolver el id del objeto insertado).
    public Long insertarLibro(Libro libro) {
        Session session = sessionFactory.openSession();
        Transaction transaction = null;
        
        try {
            transaction = session.beginTransaction();
            session.persist(libro); 
            transaction.commit();
            return libro.getId();
        } catch (Exception e) {
            if(transaction != null) {
                transaction.rollback();
            }
            e.printStackTrace();
        } finally {
            session.close();
        }
        return (long) (-1);
    }
    
    // Borrar dado un objeto (parámetro).
    public void borrarLibro(Libro libro) {
        Session session = sessionFactory.openSession();
        Transaction transaction = null;
        
        try {
            transaction = session.beginTransaction();
            session.remove(session.contains(libro) ? libro : session.merge(libro));
            transaction.commit();
        } catch (Exception e) {
            if (transaction != null) {
                transaction.rollback();
            }
            e.printStackTrace();
        } finally {
            session.close();
        }
    }
    
    // Obtener un objeto dado un id (parámetro).
    public Libro obtenerLibroPorId(Long id) {
        Session session = sessionFactory.openSession();
        Libro libro = null;
        
        try {
            libro = session.get(Libro.class, id);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            session.close();
        }
        return libro;
    }
    
    // Actualizar un atributo (título) dado un id.
    public void actualizarTitulo(Long id, String nuevoTitulo) {
        Session session = sessionFactory.openSession();
        Transaction transaction = null;
        
        try {
            transaction = session.beginTransaction();
            Libro libro = session.get(Libro.class, id);
            
            libro.setTitulo(nuevoTitulo);
            session.merge(libro); 

			transaction.commit();
        } catch (Exception e) {
            if (transaction != null) {
                transaction.rollback();
            }
            e.printStackTrace();
        } finally {
            session.close();
        }
    }
    
    // Obtener por otro atributo (ISBN) usando HQL.
    public List<Libro> obtenerLibrosPorIsbn(String isbnParam) {
        Session session = sessionFactory.openSession();
        List<Libro> listaLibros = null;
        
        try {
            
        	/*
            listaLibros = session.createQuery("FROM Libro WHERE isbn = :i", Libro.class)
                                  .setParameter("i", isbnParam)
                                  .list();*/
            

        	CriteriaBuilder criteriaBuilder = session.getCriteriaBuilder();
        	
        	CriteriaQuery<Libro> criteriaQuery = criteriaBuilder.createQuery(Libro.class);
        	
        	Root<Libro> root = criteriaQuery.from(Libro.class);
        	
        	criteriaQuery.select(root).where(criteriaBuilder.equal(root.get("isbn"), isbnParam));
        	
        	listaLibros = session.createQuery(criteriaQuery).getResultList();
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            session.close();
        }
        return listaLibros;
    }
    
    public void actualizarLibro(Libro libro) {
        Session session = sessionFactory.openSession();
        Transaction transaction = null;
        try {
            transaction = session.beginTransaction();
            // Usamos merge en lugar de persist porque el libro ya existe (tiene ID)
            session.merge(libro); 
            transaction.commit();
        } catch (Exception e) {
            if (transaction != null) transaction.rollback();
            e.printStackTrace();
        } finally {
            session.close();
        }
    }
    
    public List<Libro> obtenerTodos() {
        Session session = sessionFactory.openSession();
        List<Libro> lista = null;
        try {
            lista = session.createQuery("FROM Libro", Libro.class).list();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            session.close();
        }
        return lista;
    }
}
```

## Practica7Application.java
```java
package com.example.practica7;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import java.util.List;

@SpringBootApplication
public class Practica7Application implements CommandLineRunner {

    @Autowired
    private AutorService autorService;

    @Autowired
    private LibroService libroService;

    public static void main(String[] args) {
        SpringApplication.run(Practica7Application.class, args);
    }

}
```
