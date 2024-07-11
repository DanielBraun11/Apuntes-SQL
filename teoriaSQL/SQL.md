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
### Crear base de datos
```sql
CREATE DATABASE IF NOT EXIST nombre_base DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```
### Ver bases de datos
```sql
SHOW DATABASE;
```
### Crear usuarios
```sql
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'password';
```
### Dar privilegios a usuarios
```sql
GRANT ALL PRIVILEGES ON nombre_base.* TO 'usuario'@'localhost';
```
### Crear tablas
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
### Añadir una nueva columna
```sql
ALTER TABLE ejemplo1 ADD edad INT(7) NOT NULL;
```

### Eliminar una columna
```sql
ALTER TABLE ejemplo1 DROP edad;
```

### Modificar una columna
```sql
ALTER TABLE ejemplo2 MODIFY Campo2 VARCHAR(100) UNIQUE;
```

### Cambiar el nombre de la columna a otro
```sql
ALTER TABLE ejemplo2 CHANGE Campo2 Campo2N INT(40) NOT NULL;
```

### Cambiar el nombre de la tabla
```sql
ALTER TABLE ejemplo1 RENAME TO ejemplo1N;
```

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





