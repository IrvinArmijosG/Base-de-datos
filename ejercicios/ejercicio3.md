# Ejercicio 3

## Clínica Médica

### Enunciado

Los pacientes solicitan citas; cada cita la atiende un médico a un paciente en fecha, hora y consultorio. De algunas citas se genera una receta que incluye medicamentos con dosis y duración. Cada médico pertenece a una especialidad.

### Diagrama Entidad-Relación

![Ejercicio 3 - Diagrama Entidad-Relación](../imagenes/ejercicio3_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 3 - Diagrama Relacional](../imagenes/ejercicio3_relacional.png)

### Código SQL

```sql
CREATE TABLE Especialidad (
    id_especialidad INT PRIMARY KEY,
    nombre          VARCHAR(80) NOT NULL UNIQUE
);

CREATE TABLE Medico (
    id_medico       INT PRIMARY KEY,
    id_especialidad INT NOT NULL,
    nombre          VARCHAR(50) NOT NULL,
    apellido        VARCHAR(50) NOT NULL,
    FOREIGN KEY (id_especialidad) REFERENCES Especialidad(id_especialidad)
);

CREATE TABLE Paciente (
    id_paciente      INT PRIMARY KEY,
    nombre           VARCHAR(50) NOT NULL,
    apellido         VARCHAR(50) NOT NULL,
    fecha_nacimiento DATE
);

CREATE TABLE Cita (
    id_cita     INT PRIMARY KEY,
    id_paciente INT NOT NULL,
    id_medico   INT NOT NULL,
    fecha       DATE NOT NULL,
    hora        TIME NOT NULL,
    FOREIGN KEY (id_paciente) REFERENCES Paciente(id_paciente),
    FOREIGN KEY (id_medico) REFERENCES Medico(id_medico),
    UNIQUE (id_medico, fecha, hora)
);

CREATE TABLE Receta (
    id_receta     INT PRIMARY KEY,
    id_cita       INT NOT NULL UNIQUE,
    fecha_emision DATE,
    FOREIGN KEY (id_cita) REFERENCES Cita(id_cita)
);

CREATE TABLE Medicamento (
    id_medicamento INT PRIMARY KEY,
    nombre         VARCHAR(100) NOT NULL,
    presentacion   VARCHAR(50)
);

CREATE TABLE Detalle_Receta (
    id_receta      INT,
    id_medicamento INT,
    dosis          VARCHAR(100) NOT NULL,
    duracion       VARCHAR(100) NOT NULL,
    PRIMARY KEY (id_receta, id_medicamento),
    FOREIGN KEY (id_receta) REFERENCES Receta(id_receta),
    FOREIGN KEY (id_medicamento) REFERENCES Medicamento(id_medicamento)
);
```

