# Ejercicio 7

## Discos Musicales

### Enunciado

- La tienda vende discos de diferentes géneros musicales y cantantes.
- Cada género musical tiene un identificador.
- Cada disco tiene título, género musical, precio y cantante.
- De un cantante se registra el nombre y su país. Todo disco pertenece a un cantante y un cantante puede tener muchos discos. Además un disco tiene un conjunto de canciones (identificador y título).
- Un disco tiene muchas canciones y una canción puede estar en varios discos. Interesa conocer la posición de una canción en un determinado disco.

### Diagrama Entidad-Relación

![Ejercicio 7 - Diagrama Entidad-Relación](../imageness/ejercicio7_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 7 - Diagrama Relacional](../imageness/ejercicio7_relacional.png)

### Código SQL

```sql
CREATE TABLE Genero (
    id_genero VARCHAR(20) PRIMARY KEY,
    nombre    VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE Cantante (
    id_cantante VARCHAR(20) PRIMARY KEY,
    nombre      VARCHAR(100) NOT NULL,
    pais        VARCHAR(50)
);

CREATE TABLE Disco (
    id_disco    VARCHAR(20) PRIMARY KEY,
    id_genero   VARCHAR(20) NOT NULL,
    id_cantante VARCHAR(20) NOT NULL,
    titulo      VARCHAR(100) NOT NULL,
    precio      DECIMAL(10, 2) NOT NULL CHECK (precio >= 0),
    FOREIGN KEY (id_genero) REFERENCES Genero(id_genero),
    FOREIGN KEY (id_cantante) REFERENCES Cantante(id_cantante)
);

CREATE TABLE Cancion (
    id_cancion VARCHAR(20) PRIMARY KEY,
    titulo     VARCHAR(100) NOT NULL
);

CREATE TABLE Incluye (
    id_disco   VARCHAR(20),
    id_cancion VARCHAR(20),
    posicion   INT NOT NULL CHECK (posicion > 0),
    PRIMARY KEY (id_disco, id_cancion),
    FOREIGN KEY (id_disco) REFERENCES Disco(id_disco),
    FOREIGN KEY (id_cancion) REFERENCES Cancion(id_cancion),
    UNIQUE (id_disco, posicion)
);
```

