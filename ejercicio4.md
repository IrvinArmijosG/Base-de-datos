# Ejercicio 4

## Sistema de Aerolínea

### Enunciado

Una aerolínea opera vuelos (cada uno con número, fecha, origen y destino), cada vuelo lo realiza un avión. Los pasajeros realizan reservas para un vuelo específico, indicando asiento y tarifa pagada en el momento de la reserva.

### Diagrama Entidad-Relación

![Ejercicio 4 - Diagrama Entidad-Relación](../imageness/ejercicio4_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 4 - Diagrama Relacional](../imageness/ejercicio4_relacional.png)

### Código SQL

```sql
CREATE TABLE Avion (
    id_avion  INT PRIMARY KEY,
    modelo    VARCHAR(50) NOT NULL,
    capacidad INT CHECK (capacidad > 0)
);

CREATE TABLE Pasajero (
    id_pasajero INT PRIMARY KEY,
    nombre      VARCHAR(50) NOT NULL,
    apellido    VARCHAR(50) NOT NULL,
    documento   VARCHAR(20) NOT NULL UNIQUE
);

CREATE TABLE Vuelo (
    id_vuelo INT PRIMARY KEY,
    id_avion INT NOT NULL,
    origen   VARCHAR(100) NOT NULL,
    destino  VARCHAR(100) NOT NULL,
    fecha    DATE NOT NULL,
    FOREIGN KEY (id_avion) REFERENCES Avion(id_avion)
);

CREATE TABLE Reserva (
    id_reserva    INT PRIMARY KEY,
    id_vuelo      INT NOT NULL,
    id_pasajero   INT NOT NULL,
    asiento       VARCHAR(10) NOT NULL,
    tarifa_pagada DECIMAL(10, 2) NOT NULL CHECK (tarifa_pagada >= 0),
    FOREIGN KEY (id_vuelo) REFERENCES Vuelo(id_vuelo),
    FOREIGN KEY (id_pasajero) REFERENCES Pasajero(id_pasajero),
    UNIQUE (id_vuelo, asiento)
);
```

