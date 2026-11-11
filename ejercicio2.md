# Ejercicio 2

## Pedidos de Restaurante

### Enunciado

Los clientes realizan pedidos que contienen varios platos con distinta cantidad. Los platos se agrupan en categorías. Cada pedido lo atiende un único mesero. El precio del plato debe registrarse tal como estaba al momento del pedido, aunque luego cambie.

### Diagrama Entidad-Relación

![Ejercicio 2 - Diagrama Entidad-Relación](../imageness/ejercicio2_entidad_relacion.png)

### Diagrama Relacional

![Ejercicio 2 - Diagrama Relacional](../imageness/ejercicio2_relacional.png)

### Código SQL

```sql
CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY,
    nombre     VARCHAR(100) NOT NULL,
    telefono   VARCHAR(20)
);

CREATE TABLE Mesero (
    id_mesero INT PRIMARY KEY,
    nombre    VARCHAR(100) NOT NULL
);

CREATE TABLE Categoria (
    id_categoria INT PRIMARY KEY,
    nombre       VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE Plato (
    id_plato     INT PRIMARY KEY,
    id_categoria INT NOT NULL,
    nombre       VARCHAR(100) NOT NULL,
    precio       DECIMAL(10, 2) NOT NULL CHECK (precio >= 0),
    FOREIGN KEY (id_categoria) REFERENCES Categoria(id_categoria)
);

CREATE TABLE Pedido (
    id_pedido  INT PRIMARY KEY,
    id_cliente INT NOT NULL,
    id_mesero  INT NOT NULL,
    fecha      DATE NOT NULL,
    estado     VARCHAR(20) NOT NULL DEFAULT 'PENDIENTE',
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente),
    FOREIGN KEY (id_mesero) REFERENCES Mesero(id_mesero)
);

CREATE TABLE Detalle_Pedido (
    id_pedido       INT,
    id_plato        INT,
    cantidad        INT NOT NULL CHECK (cantidad > 0),
    precio_unitario DECIMAL(10, 2) NOT NULL CHECK (precio_unitario >= 0),
    PRIMARY KEY (id_pedido, id_plato),
    FOREIGN KEY (id_pedido) REFERENCES Pedido(id_pedido),
    FOREIGN KEY (id_plato) REFERENCES Plato(id_plato)
);
```

