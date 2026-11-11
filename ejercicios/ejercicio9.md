# Ejercicio 9

## Veterinaria

### Enunciado

- Del propietario interesa: cédula, apellidos, nombres, dirección, teléfonos.
- De la mascota interesa: identificador, nombre, fecha de nacimiento, tipo.
- Un propietario puede llevar una o varias mascotas.
- Una mascota la lleva uno y solo un propietario.
- Del personal de la clínica veterinaria interesa un código, la cédula, nombre.
- Entre el personal de la clínica se distingue a los veterinarios y los auxiliares, pudiendo haber otros tipos:
  - Del veterinario se guarda además su fecha_alta y especialidad.
  - De los auxiliares interesa su base de cotización.
- Las mascotas pasan consulta con los veterinarios. De manera que:
  - un veterinario puede pasar consulta a ninguna o a varias mascotas, y
  - una misma mascota de la clínica ha pasado consulta una o más veces en fechas diferentes con varios veterinarios.
  - Al pasar consulta se realiza un diagnóstico.
- Cada propietario tiene un familiar de contacto para casos de emergencia.
  - De este familiar interesa la cédula, nombre y teléfono.
  - Si el propietario se da de alta en la clínica veterinaria, el familiar ya no interesa.
  - El familiar es contacto de un solo propietario.

### Diagrama Entidad-Relación

![Ejercicio 9 - Diagrama Entidad-Relación](../imagenes/ejercicio9_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 9 - Diagrama Relacional](../imagenes/ejercicio9_relacional.png)

### Código SQL

```sql
CREATE TABLE Propietario (
    cedula    VARCHAR(20) PRIMARY KEY,
    apellidos VARCHAR(50) NOT NULL,
    nombres   VARCHAR(50) NOT NULL,
    direccion VARCHAR(150)
);

-- Atributo multivaluado "telefonos" del propietario
CREATE TABLE Telefono_Propietario (
    cedula_propietario VARCHAR(20),
    telefono           VARCHAR(20),
    PRIMARY KEY (cedula_propietario, telefono),
    FOREIGN KEY (cedula_propietario) REFERENCES Propietario(cedula)
);

CREATE TABLE Familiar_Contacto (
    cedula             VARCHAR(20) PRIMARY KEY,
    cedula_propietario VARCHAR(20) NOT NULL UNIQUE,
    nombre             VARCHAR(100) NOT NULL,
    telefono           VARCHAR(20),
    FOREIGN KEY (cedula_propietario) REFERENCES Propietario(cedula)
);

CREATE TABLE Mascota (
    identificador      VARCHAR(20) PRIMARY KEY,
    cedula_propietario VARCHAR(20) NOT NULL,
    nombre             VARCHAR(50) NOT NULL,
    fecha_nacimiento   DATE,
    tipo               VARCHAR(50),
    FOREIGN KEY (cedula_propietario) REFERENCES Propietario(cedula)
);

CREATE TABLE Personal (
    codigo          VARCHAR(20) PRIMARY KEY,
    cedula          VARCHAR(20) NOT NULL UNIQUE,
    nombre          VARCHAR(100) NOT NULL,
    tipo_personal   VARCHAR(20) NOT NULL
        CHECK (tipo_personal IN ('Veterinario', 'Auxiliar', 'Otro')),
    fecha_alta      DATE,            -- solo veterinarios
    especialidad    VARCHAR(50),     -- solo veterinarios
    base_cotizacion DECIMAL(10, 2)   -- solo auxiliares
);

-- codigo_veterinario debe corresponder a personal con tipo_personal = 'Veterinario'
-- (esa regla se controla con un trigger o desde la aplicación)
CREATE TABLE Pasa_Consulta (
    identificador_mascota VARCHAR(20),
    codigo_veterinario    VARCHAR(20),
    fecha_consulta        DATE,
    diagnostico           VARCHAR(250),
    PRIMARY KEY (identificador_mascota, codigo_veterinario, fecha_consulta),
    FOREIGN KEY (identificador_mascota) REFERENCES Mascota(identificador),
    FOREIGN KEY (codigo_veterinario) REFERENCES Personal(codigo)
);
```
