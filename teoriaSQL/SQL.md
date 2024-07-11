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
**USE** nombre_base;
```
### Crear base de datos
```sql
**CREATE DATABASE IF NOT EXIST ** nombre_base **DEFAULT CHARACTER SET** utf8 **COLLATE** utf8_general_ci;
```
### Ver bases de datos
```sql
SHOW DATABASE;
```
### Crear usuarios
```sql
**CREATE USER** 'usuario'@'localhost' **IDENTIFIED BY** 'password';
```
### Dar privilegios a usuarios
```sql
**GRANT ALL PRIVILEGES ON** nombre_base.* **TO** 'usuario'@'localhost';
```
