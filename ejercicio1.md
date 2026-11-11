# Ejercicio 1

## Sistema de Biblioteca

### Enunciado

Una biblioteca registra libros y autores. Un libro puede tener varios autores y un autor varios libros. Los socios toman libros prestados; de cada préstamo interesan la fecha de retiro y la de devolución.

### Diagrama Entidad-Relación

![Ejercicio 1 - Diagrama Entidad-Relación](../imageness/ejercicio1_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 1 - Diagrama Relacional](../imageness/ejercicio1_relacional.png)

### Código SQL

```sql
CREATE TABLE Libro (
    id_libro         INT PRIMARY KEY,
    titulo           VARCHAR(150) NOT NULL,
    isbn             VARCHAR(20) UNIQUE,
    anio_publicacion INT
);

CREATE TABLE Autor (
    id_autor     INT PRIMARY KEY,
    nombre       VARCHAR(100) NOT NULL,
    nacionalidad VARCHAR(50)
);

CREATE TABLE Socio (
    id_socio INT PRIMARY KEY,
    nombre   VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    telefono VARCHAR(20),
    email    VARCHAR(100) UNIQUE
);

CREATE TABLE Es_Escrito (
    id_libro INT,
    id_autor INT,
    PRIMARY KEY (id_libro, id_autor),
    FOREIGN KEY (id_libro) REFERENCES Libro(id_libro),
    FOREIGN KEY (id_autor) REFERENCES Autor(id_autor)
);

CREATE TABLE Prestamo (
    id_socio         INT,
    id_libro         INT,
    fecha_retiro     DATE,
    fecha_devolucion DATE,
    PRIMARY KEY (id_socio, id_libro, fecha_retiro),
    FOREIGN KEY (id_socio) REFERENCES Socio(id_socio),
    FOREIGN KEY (id_libro) REFERENCES Libro(id_libro),
    CHECK (fecha_devolucion IS NULL OR fecha_devolucion >= fecha_retiro)
);
```

