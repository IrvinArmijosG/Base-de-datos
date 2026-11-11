# Ejercicio 6

## Tienda Informática

### Enunciado

Se desea crear una aplicación para la gestión de una tienda informática. La tienda dispone de una serie de productos que se venden a los clientes. De cada producto informático se desea guardar el código, descripción, precio y número de existencias. De cada cliente se desea guardar el nombre, apellidos, dirección y número telefónico. Un cliente puede comprar varios productos en la tienda y un mismo producto puede ser comprado por varios clientes. Cada vez que se compre un artículo quedará registrada la compra en la base de datos junto con la fecha en la que se compró el artículo. La tienda tiene contactos con varios proveedores que son los que suministran los productos. Un mismo producto puede ser suministrado por varios proveedores. De cada proveedor se desea guardar el código, nombres, apellidos, dirección, provincia y número de teléfono.

### Diagrama Entidad-Relación

![Ejercicio 6 - Diagrama Entidad-Relación](../imageness/ejercicio6_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 6 - Diagrama Relacional](../imageness/ejercicio6_relacional.png)

### Código SQL

```sql
CREATE TABLE Cliente (
    cedula    VARCHAR(20) PRIMARY KEY,
    nombre    VARCHAR(50) NOT NULL,
    apellido  VARCHAR(50) NOT NULL,
    telefono  VARCHAR(20),
    direccion VARCHAR(150)
);

CREATE TABLE Proveedor (
    cedula    VARCHAR(20) PRIMARY KEY,
    nombres   VARCHAR(50) NOT NULL,
    apellidos VARCHAR(50) NOT NULL,
    telefono  VARCHAR(20),
    direccion VARCHAR(150),
    provincia VARCHAR(50)
);

CREATE TABLE Producto (
    codigo           VARCHAR(20) PRIMARY KEY,
    cedula_proveedor VARCHAR(20) NOT NULL,
    nombre           VARCHAR(100) NOT NULL,
    stock            INT NOT NULL DEFAULT 0 CHECK (stock >= 0),
    precio           DECIMAL(10, 2) NOT NULL CHECK (precio >= 0),
    descripcion      VARCHAR(200),
    FOREIGN KEY (cedula_proveedor) REFERENCES Proveedor(cedula)
);

CREATE TABLE Compra (
    cedula_cliente  VARCHAR(20),
    codigo_producto VARCHAR(20),
    fecha_compra    DATE,
    cantidad        INT NOT NULL DEFAULT 1 CHECK (cantidad > 0),
    PRIMARY KEY (cedula_cliente, codigo_producto, fecha_compra),
    FOREIGN KEY (cedula_cliente) REFERENCES Cliente(cedula),
    FOREIGN KEY (codigo_producto) REFERENCES Producto(codigo)
);
```

