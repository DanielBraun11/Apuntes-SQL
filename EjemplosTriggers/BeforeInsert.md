# Ejemplo TRIGGER BEFORE INSERT
Primero, crearemos la base de datos y tabla para la que se establecerá el trigger:
```sql
/* Crear la base de datos */
CREATE DATABASE IF NOT EXISTS demoTrigger;
```
```sql
/* Usar la base de datos creada */
USE demoTrigger;
```
```sql
/* Crear tabla */
CREATE TABLE IF NOT EXISTS people (age INT, name VARCHAR(150));
```

## Creación de un trigger BEFORE INSERT
A continuación definiremos el trigger. Se ejecutará antes de cada sentencia INSERT para la tabla people y en el caso de que introduzcamos una persona en la tabla people con una edad
negativa, cambiará la edad a 0. **Si intentamos crear el trigger como AFTER INSERT dará un error: Updating of NEW row is not allowed in after trigger**
```sql
delimiter//

CREATE TRIGGER agecheck
BEFORE INSERT
ON people FOR EACH ROW
IF NEW.age < 0
THEN SET NEW.age = 0; END IF;//

delimiter;
```

Insertaremos dos registros para comprobar la funcionalidad del trigger:
```sql
INSERT INTO people VALUES (-20, 'Daniel'), (30, 'Diego');
```

Para terminar, comprobaremos el resultado, como se puede ver, a pesar de que Sid se ha
intentado introducir con edad negativa, la edad es 0:
```sql
SELECT * FROM people;
```

![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/BeforeInsert1.png) 

Adicionalmente podemos verificar el trigger en MySQL Workbench:

![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/BeforeInsert2.png) 

Para mostrar TODOS los triggers de la vbase de datos demoTriggers y tabla people:
```sql
SHOW TRIGGERS
FROM demoTrigger
LIKE 'people';
```

![MensajeEntradaInt](https://github.com/DanielBraun11/ApuntesSQL/blob/main/fotosSQL/BeforeInsert3.png) 

