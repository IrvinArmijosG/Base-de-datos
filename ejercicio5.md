# Ejercicio 5

## Empresa Discográfica

### Enunciado

Una empresa discográfica necesita modelar los datos sobre sus diferentes recursos según las siguientes características:

- **Manager:** de cada mánager se almacena un identificador de manager y su nombre. Un manager representa a una serie de artistas.
- **Artista:** un artista es representado por un manager. De los artistas se almacena su nombre completo (usando un único atributo) y su NIF.
- **Evento de promoción:** los artistas participan en eventos de promoción para dar a conocer sus trabajos. En un evento de promoción pueden participar varios artistas. De un evento se almacena un identificador único, la fecha de celebración y el número de asistentes.

### Diagrama Entidad-Relación

![Ejercicio 5 - Diagrama Entidad-Relación](../imageness/ejercicio5_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 5 - Diagrama Relacional](../imageness/ejercicio5_relacional.png)

### Código SQL

```sql
CREATE TABLE Manager (
    id_manager INT PRIMARY KEY,
    nombre     VARCHAR(100) NOT NULL
);

CREATE TABLE Artista (
    nif             VARCHAR(20) PRIMARY KEY,
    id_manager      INT NOT NULL,
    nombre_completo VARCHAR(150) NOT NULL,
    FOREIGN KEY (id_manager) REFERENCES Manager(id_manager)
);

CREATE TABLE Evento (
    id_evento         INT PRIMARY KEY,
    fecha_celebracion DATE NOT NULL,
    numero_asistentes INT CHECK (numero_asistentes >= 0)
);

CREATE TABLE Participa (
    nif       VARCHAR(20),
    id_evento INT,
    PRIMARY KEY (nif, id_evento),
    FOREIGN KEY (nif) REFERENCES Artista(nif),
    FOREIGN KEY (id_evento) REFERENCES Evento(id_evento)
);
```

