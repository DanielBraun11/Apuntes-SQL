# Ejemplo TRIGGERS BEFORE DELETE
```sql
DROP TABLE IF EXISTS Salaries;

CREATE TABLE Salaries (
  employeeNumber INT PRIMARY KEY,
  validFrom DATE NOT NULL,
  amount DEC(12 , 2 ) NOT NULL DEFAULT 0
);

INSERT INTO salaries(employeeNumber,validFrom,amount)
VALUES
(1002,'2000-01-01',50000),
(1056,'2000-01-01',60000),
(1076,'2000-01-01',70000);
```

foto 1

```sql
DROP TABLE IF EXISTS SalaryArchives;

CREATE TABLE SalaryArchives (
  id INT PRIMARY KEY AUTO_INCREMENT,
  employeeNumber INT,
  validFrom DATE NOT NULL,
  amount DEC(12 , 2 ) NOT NULL DEFAULT 0,
  deletedAt TIMESTAMP DEFAULT NOW()
);
```

Creamos un trigger que inserta una nueva fila en la tabla **SalaryArchives** antes de que se elimine una fila de la tabla **Salaries**.
```sql
DELIMITER $$

CREATE TRIGGER before_salaries_delete
BEFORE DELETE
ON salaries FOR EACH ROW
BEGIN
  INSERT INTO SalaryArchives(employeeNumber,validFrom,amount)
  VALUES(OLD.employeeNumber,OLD.validFrom,OLD.amount);
END$$

DELIMITER ;
```
Lo probamos:
Borramos un registro de la tabla Salaries:
```sql
DELETE FROM salaries
WHERE employeeNumber = 1002;
```

Verificamos si se ha guardado en la tabla SalaryArchives:
```sql
SELECT * FROM SalaryArchives;
```

foto 2




