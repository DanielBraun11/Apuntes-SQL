# Ejemplo TRIGGER AFTER UPDATE
```sql
DROP TABLE IF EXISTS Sales;

CREATE TABLE Sales (
  id INT AUTO_INCREMENT,
  product VARCHAR(100) NOT NULL,
  quantity INT NOT NULL DEFAULT 0,
  fiscalYear SMALLINT NOT NULL,
  fiscalMonth TINYINT NOT NULL,

CHECK(fiscalMonth >= 1 AND fiscalMonth <= 12),
CHECK(fiscalYear BETWEEN 2000 and 2050),
CHECK (quantity >=0),
UNIQUE(product, fiscalYear, fiscalMonth),
PRIMARY KEY(id)
);

INSERT INTO Sales(product, quantity, fiscalYear, fiscalMonth)
VALUES
('2001 Ferrari Enzo',140, 2021,1),
('1998 Chrysler Plymouth Prowler', 110,2021,1),
('1913 Ford Model T Speedster', 120,2021,1);
```

Mostrar tabla con los datos insertados:
```sql
SELECT * FROM Sales;
```

![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/AfterUpdate1.png) 

Crear una tabla que almacene los cambios en la columna **quantity** de la tabla **Sales**
```sql
DROP TABLE IF EXISTS SalesChanges;

CREATE TABLE SalesChanges (
  id INT AUTO_INCREMENT PRIMARY KEY,
  salesId INT,
  beforeQuantity INT,
  afterQuantity INT,
  changedAt TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

Crear el trigger, si actualiza el valor en la columna de cantidad a un nuevo valor, el disparador inserta una nueva fila para registrar
los cambios en la tabla SalesChanges.
```sql
DELIMITER $$

CREATE TRIGGER after_sales_update
AFTER UPDATE
ON sales FOR EACH ROW
BEGIN
  IF OLD.quantity <> NEW.quantity THEN
    INSERT INTO SalesChanges(salesId,beforeQuantity, afterQuantity)
    VALUES(old.id, OLD.quantity, NEW.quantity);
  END IF;
END$$

DELIMITER ;
```

Lo probamos:
```sql
UPDATE Sales
SET quantity = 350
WHERE id = 1;
```

![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/AfterUpdate2.png) 




