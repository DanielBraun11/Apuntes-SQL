# Ejemplo TRIGGERS AFTER DELETE
```sql
DROP TABLE IF EXISTS Salaries;

CREATE TABLE Salaries (
  employeeNumber INT PRIMARY KEY,
  salary DECIMAL(10,2) NOT NULL DEFAULT 0
);

INSERT INTO Salaries(employeeNumber,salary)
VALUES
(1002,5000),
(1056,7000),
(1076,8000);
```

Se crea otra tabla llamada SalaryBudgets que almacena el total de salarios de la tabla Salaries:
```sql
DROP TABLE IF EXISTS SalaryBudgets;

CREATE TABLE SalaryBudgets(
total DECIMAL(15,2) NOT NULL
);
```

Se usa la función SUM() para obtener el salario total de la tabla Salaries e insértelo en la tabla SalaryBudgets:
```sql
NSERT INTO SalaryBudgets(total)
SELECT SUM(salary)
FROM Salaries;
```

Verificamos la tabla SalaryBudgets:
```sql
SELECT * FROM SalaryBudgets;
```

foto 2

Se crea un trigger que actualiza el salario total en la tabla SalaryBudgets después de eliminar una fila de la tabla Salaries:
```sql
CREATE TRIGGER after_salaries_delete
AFTER DELETE
ON Salaries FOR EACH ROW
UPDATE SalaryBudgets
SET total = total - old.salary;
```

Probamos y borramos una fila de la tabla Salaries:
```sql
DELETE FROM Salaries
WHERE employeeNumber = 1002;
```

Comprobamos la tabla SalaryBudgets:
```sql
SELECT * FROM SalaryBudgets;
```

foto 3
