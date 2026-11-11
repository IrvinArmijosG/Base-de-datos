# Ejercicio 8

## Camiones

### Enunciado

Se desea informatizar la gestión de una empresa de transporte que reparte paquetes por toda España. Los encargados de llevar los paquetes son los camioneros, de los que se quiere guardar la cédula, nombre, teléfono, dirección, salario y población en que vive. De los paquetes transportados interesa conocer el código del paquete, descripción, destinatario y dirección del destinatario.

Un camionero distribuye muchos paquetes, y un paquete sólo puede ser distribuido por un camionero. De las provincias a las que llegan los paquetes interesa guardar el código de provincia y el nombre. Un paquete sólo puede llegar a una provincia. Sin embargo, a una provincia pueden llegar varios paquetes.

De los camiones que llevan los camioneros, interesa conocer la matrícula, modelo, tipo y potencia. Un camionero puede conducir diferentes camiones en fechas diferentes, y un camión puede ser conducido por varios camioneros.

### Diagrama Entidad-Relación

![Ejercicio 8 - Diagrama Entidad-Relación](../imagenes/ejercicio8_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 8 - Diagrama Relacional](../imagenes/ejercicio8_relacional.png)

### Código SQL

```sql
CREATE TABLE Camionero (
    cedula    VARCHAR(20) PRIMARY KEY,
    nombre    VARCHAR(100) NOT NULL,
    telefono  VARCHAR(20),
    direccion VARCHAR(150),
    salario   DECIMAL(10, 2) CHECK (salario >= 0)
);

CREATE TABLE Camion (
    matricula VARCHAR(20) PRIMARY KEY,
    tipo      VARCHAR(50),
    modelo    VARCHAR(50),
    potencia  INT
);

CREATE TABLE Provincia (
    codigo VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE Destinatario (
    id_destinatario  INT PRIMARY KEY,
    codigo_provincia VARCHAR(20) NOT NULL,
    nombre           VARCHAR(100) NOT NULL,
    direccion        VARCHAR(150),
    FOREIGN KEY (codigo_provincia) REFERENCES Provincia(codigo)
);

CREATE TABLE Paquete (
    id_paquete       INT PRIMARY KEY,
    cedula_camionero VARCHAR(20) NOT NULL,
    id_destinatario  INT NOT NULL,
    codigo_provincia VARCHAR(20) NOT NULL,
    descripcion      VARCHAR(200),
    FOREIGN KEY (cedula_camionero) REFERENCES Camionero(cedula),
    FOREIGN KEY (id_destinatario) REFERENCES Destinatario(id_destinatario),
    FOREIGN KEY (codigo_provincia) REFERENCES Provincia(codigo)
);

CREATE TABLE Maneja (
    cedula_camionero VARCHAR(20),
    matricula_camion VARCHAR(20),
    PRIMARY KEY (cedula_camionero, matricula_camion),
    FOREIGN KEY (cedula_camionero) REFERENCES Camionero(cedula),
    FOREIGN KEY (matricula_camion) REFERENCES Camion(matricula)
);
```

