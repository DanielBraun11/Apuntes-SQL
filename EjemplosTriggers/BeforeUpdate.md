# Ejemplo TRIGGER BEFORE UPDATE
```sql
DROP TABLE IF EXISTS sales;

CREATE TABLE sales (
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

INSERT INTO sales(product, quantity, fiscalYear, fiscalMonth)
VALUES
('2003 Harley-Davidson Eagle Drag Bike',120, 2020,1),
('1969 Corvair Monza', 150,2020,1),
('1970 Plymouth Hemi Cuda', 200,2020,1);
```

Verificamos la inserción de datos:
```sql
SELECT * FROM sales;
```

![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/BeforeUpdate1.png) 

Se crea un trigger que se activa automáticamente antes de que se produzca un evento de UPDATE para cada fila de la tabla de sales. Si actualiza el valor en la columna
de quantity a un nuevo valor que es 3 veces mayor que el valor actual, el activador genera un error y detiene la actualización.
```sql
DELIMITER $$

CREATE TRIGGER before_sales_update
BEFORE UPDATE
ON sales FOR EACH ROW
BEGIN
    DECLARE errorMessage VARCHAR(255);
    SET errorMessage = CONCAT('The new quantity ',
                    NEW.quantity,
                    ' cannot be 3 times greater than the current quantity ',
                    OLD.quantity);

IF NEW.quantity > OLD.quantity * 3 THEN
  SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = errorMessage;
END IF;
END $$

DELIMITER ;
```

Para probarlo, actualizamos quantity de la fila 1 a 150
```sql
UPDATE sales
SET quantity = 150
WHERE id = 1;
```

Verificamos la actualización:
```sql
SELECT *
FROM sales;
```

![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/BeforeUpdate2.png) 

Ahora intentamos actualizar la cantidad de la fila con id 1 a 500 (más de 3 veces la cantidad actual):
```sql
UPDATE sales
SET quantity = 500
WHERE id = 1;
```
Salta el siguiente error:
**Error Code: 1644. The new quantity 500 cannot be 3 times greater than the current quantity 150**


![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/BeforeUpdate3.png) 






