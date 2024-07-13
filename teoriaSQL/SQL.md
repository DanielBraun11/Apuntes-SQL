# Apuntes
## IMPORTANTE
- Los campos que son **IDENTIFICADORES** se escriben tal cual (nombre tabla, bases de datos, etc.)
- Los campos que son valores/texto se escriben entre comillas ' '
- Los valores se separan con comas ,

- En estos apuntes, lo que esté en **negrita** seran palabras reservadas.
- Para seleccionar TODO de una base de datos o tabla se usa *
- Para acceder a una parte de algo se utiliza el operador .

## SENTENCIAS
Para usar una base en especifico (crear/modificar tablas, etc.)
```sql
USE nombre_base;
```
### - Crear base de datos
```sql
CREATE DATABASE IF NOT EXIST nombre_base DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```
### - Ver bases de datos
```sql
SHOW DATABASE;
```
### - Crear usuarios
```sql
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'password';
```
### - Dar privilegios a usuarios
```sql
GRANT ALL PRIVILEGES ON nombre_base.* TO 'usuario'@'localhost';
```
### - Crear tablas
```sql
CREATE TABLE ejemplo1(
Campo1 INT(7) PRIMARY KEY, /*Atributo Tipo(Longitud máxima) MODIFICADORES */
Campo2 VARCHAR(10) UNIQUE NOT NULL /*El último no lleva,(coma) al final!! */
...
);

CREATE TABLE ejemplo2(
Campo1 INT(7) PRIMARY KEY,
Campo2 DECIMAL(20), /* (M,D) -> M dígitos, D de los M son decimales */
...                 /* Lo de debajo crea una clave foránea para referenciar la otra tabla */
);
```

## Modificadores
```sql
PRIMARY KEY ; UNIQUE ; NULL ; NOT NULL
```
Delante de estos modificadores podemos agregar:
```sql
UNSIGNED ; ZEROFILL ; AUTO_INCREMENT
```

## Tipos de datos
```sql
BIT ; INT ; INTEGER ; BIGINT ; DOUBLE ; FLOAT ; DATE ; TIME ; DATETIME ; YEAR ; CHAR ; VARCHAR ; BINARY
```

## Modificar tablas
### - Añadir una nueva columna
```sql
ALTER TABLE ejemplo1 ADD edad INT(7) NOT NULL;
```
- **ALTER TABLE ejemplo1**: Modifica la tabla llamada 'ejemplo1'.
- **ADD edad**: Añade una nueva columna llamada 'edad'.
- **INT(7)**: Define el tipo de datos de la columna como entero con una longitud máxima de 7 dígitos.
(Nota: en muchos sistemas de bases de datos, la longitud entre paréntesis para tipos de datos numéricos como 'INT' no afecta la cantidad de dígitos almacenados,
solo es una indicación para propósitos de visualización).
- **NOT NULL**: Establece que esta columna no puede contener valores nulos.

### - Eliminar una columna
```sql
ALTER TABLE ejemplo1 DROP edad;
```
- **ALTER TABLE ejemplo1**: Modifica la tabla llamada 'ejemplo1'.
- **DROP edad**: Elimina la columna 'edad' de la tabla.


### - Modificar una columna
```sql
ALTER TABLE ejemplo2 MODIFY Campo2 VARCHAR(100) UNIQUE;
```
- **ALTER TABLE ejemplo2**: Modifica la tabla llamada 'ejemplo2'.
- **MODIFY Campo2**: Modifica la columna 'Campo2'.
- **VARCHAR(100)**: Cambia el tipo de datos de la columna a 'VARCHAR' con una longitud máxima de 100 caracteres.
- **UNIQUE**: Añade una restricción de unicidad a la columna, asegurando que todos los valores en esta columna sean únicos.

### - Cambiar el nombre de la columna a otro
```sql
ALTER TABLE ejemplo2 CHANGE Campo2 Campo2N INT(40) NOT NULL;
```
- **ALTER TABLE ejemplo2**: Modifica la tabla llamada 'ejemplo2'.
- **CHANGE Campo2 Campo2N**: Cambia el nombre de la columna 'Campo2' a 'Campo2N'.
- **INT(40)**: Cambia el tipo de datos de la columna a 'INT' con una longitud indicada de 40 (la longitud aquí es generalmente ignorada por los sistemas de bases de datos para el tipo 'INT').
- **NOT NULL**: Establece que la nueva columna 'Campo2N' no puede contener valores nulos.

### - Cambiar el nombre de la tabla
```sql
ALTER TABLE ejemplo1 RENAME TO ejemplo1N;
```
- **ALTER TABLE ejemplo1**: Modifica la tabla llamada 'ejemplo1'.
- **RENAME TO ejemplo1N**: Cambia el nombre de la tabla de 'ejemplo1' a 'ejemplo1N'.

## Eliminar tabla
```sql
DROP TABLE IF EXIST ejemplo2;
```

## Insertar datos
Uso un ejemplo diferente para que se entienda mejor.
```sql
INSERT INTO departamentos(idDepar, nomDepar, idDeparResp, idSede, presup, idGerente)
VALUES ('DIR', 'Dirección', NULL, '1', '245000.87', '1');
```

## Seleccioanr información
### - Selecciona todas las columnas de la tabla (*)
```sql
SELECT * FROM ejemplo1;
```
### - Selecciona las columnas que le digamos
```sql
SELECT Campo1, Campo2, ... FROM ejemplo1;
```
### - Selecciona valores unicos sin repetición (DISTINCT) elimina duplicados
```sql
SELECT DISTINCT Campo1, Campo2, ... FROM ejemplo1;
```

## Operaciones con atributos
```sql
COUNT /* Cuenta nº filas criterio */
AVG /* Promedio */
SUM /* Sumar */
MAX /* Máximo */
MIN /* Mínimo */
```

## Partículas de construcción
### - Comparar
```sql
= /* Igual */
<> /* Distinto */
!= /* Distinto */
> /* Mayor */
>= /* Mayor o igual */
< /* Menor */
<= /* Menor o igual */
```

### - Lógicos
```sql
AND /* ... Y ... */
OR /* ... O ... -> Se cumple uno y aparece */
NOT /* NO ... */
```

### - Valores
```sql
BETWEEN ... AND ... /* Entre ... y ...  */
Columna IN (valor1, valor2, ...) /* Los que tengan esos valores en esa columna */
```

```sql
IS NULL ; IS NOT NULL ;
LIKE 'A%' /* Busca un patron que empiece por 'A' */
NOT LIKE '%A' /* Busca un patron que NO acabe por 'A' */
%A% /* Contiene el caracter 'A' */

EXISTS ; ANY ; SET ...
```

## WHERE
El uso de WHERE permite aplicar una condición para **filtrar** los resultados.
```sql
SELECT Campo1, Campo2, ...
FROM ejemplo1
WHERE Campo1 > 10;
```

## ORDER BY
El uso de ORDER BY  permite aplicar una condición para **ordenar** los resultados.
```sql
SELECT Campo1, Campo2, ...
FROM ejemplo1
ORDER BY Campo2 DESC, ...; /* También existe ASC */
```

## INNER JOIN
JOIN se utiliza para combinar filas de 2 o mas tablas, basada en una columna relacionada entre ellas.
```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.columna_name = columna_name;
```
EJEMPLO:
```sql
SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID = Customers.CustomerID;
/* Esto seleccionará la "OrderID" de la tabla "Orders" y "CustomerName" de la tabla "Customers",
 donde "CustomerID" es común entre ambas tablas */
```

## GROUP BY
GROUP BY se utiliza con funciones de agregado (COUNT, AVG, SUM, etc.) para agrupar el resultado por una o más columnas.
```sql
SELECT column1, COUNT(column2)
FROM table_name
GROUP BY column1;
```
EJEMPLO:
```sql
SELECT Country, COUNT(CustomerID)
FROM Customers
GROUP BY Country;
/* Esto seleccionará la columna "Country" y el número de "CustomersID" para cada "Country" de la tabla "Customers" */
```

## HAVING
HAVING se utiliza para filtrar los resultados de una consulta que incluye una función de agregado.
```sql
SELECT column1, COUNT(column2)
 FROM table_name
 GROUP BY column1
 HAVING COUNT(column2) > value;
```
EJEMPLO:
```sql
SELECT Country, COUNT(CustomerID)
 FROM Customers
 GROUP BY Country
 HAVING COUNT(CustomerID) > 5;
/*Esto seleccionará la columna "Country" y el número de "CustomerID" para cada "Country" de la tabla "Customers",
 pero solo los países que tienen más de 5 clientes*/
```
## Consultas anidadas
Utilizar el reusltado de una consulta como parte del predicado o criterio de selección de otra.
```sql
SELECT apellido1, apellido2, nombre
FROM empleados
WHERE idDepar IN (SELECT idDepar FROM departamentos
                  WHERE idSede IN (SELECT idSede
                                   FROM sedes
                                   WHERE ciudadSede='Sevilla'));
/*IN indica que el valor del atributo que la precede se encuentre entre los devueltos (una lista)
 por la consulta que la sigue (Cada color ≠ es un anidado) */
/*Además de IN/NOT IN, se pueden utilizar otros operadores como ANY o ALL*/
```


