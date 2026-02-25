#  Ejercicios SQL - Tabla `users`

Colección de consultas SQL organizadas por nivel de dificultad para practicar filtros, condiciones, agregaciones y funciones de fecha.

---

##  Nivel 1

###  Ejercicio 1 – Mostrar todos los usuarios

```sql
SELECT * 
FROM users u; 
```
###  Ejercicio 2 – Mostrar solo first_name, last_name, email.

```sql
SELECT first_name, last_name,email from users u 
```
###  Ejercicio 3 – Filtrar usuarios cuyo role sea 'admin'.

```sql
select * from users u 
where role = "admin"
```

###  Ejercicio 4 – Filtrar usuarios con document_type = 'CC'..

```sql
SELECT * FROM users 
	where document_type  = 'cc'
```

###  Ejercicio 5 –Mostrar usuarios mayores de 18 años (calcular edad desde birth_date).

```sql
SELECT * from users u 
WHERE birth_date <= DATE_SUB(CURDATE(), INTERVAL 18 YEAR);
```

### Ejercicio 6 - Mostrar usuarios cuyo ingreso sea mayor a 5,000,000
```sql
SELECT * from users
	where monthly_income >= 5000000
```

###  Ejercicio 7 – Mostrar usuarios cuyo nombre empiece por "A"..

```sql
SELECT * from users  
where first_name like "A%"
```
###  Ejercicio 8 – Mostrar usuarios que no tengan company.


```sql
SELECT * from users 
where company is NULL
```
##  Nivel 2

###  Ejercicio 9 - Usuarios mayores de 25 años que sean 'employee'.
```sql
SELECT *FROM users 
where birth_date  < "2001-01-01" AND role ='employee';
```
###  Ejercicio 10 - Usuarios con 'CC' que estén activos.
```sql
SELECT *FROM users 
where document_type = 'CC' and is_active ='1';
```
###  Ejercicio 11 - Usuarios mayores de edad sin empleo.
```sql
SELECT *FROM users 
	WHERE birth_date <= DATE_SUB(CURDATE(), INTERVAL 18 YEAR) AND profession is NULL;
```
###  Ejercicio 12 - Usuarios con empleo y con ingresos mayores a 3,000,000.
```sql
SELECT * FROM users 
	where profession is not NULL AND monthly_income >= 3000000
```
###  Ejercicio 13 - Usuarios casados con al menos 1 hijo.
```sql
SELECT * from users 
	where marital_status = 'Casado' and children_count >= 1
```
###  Ejercicio 14 - Usuarios entre 30 y 40 años.
```sql
SELECT *
FROM users
WHERE birth_date <= DATE_SUB(CURDATE(), INTERVAL 30 YEAR)
  AND birth_date >= DATE_SUB(CURDATE(), INTERVAL 40 YEAR);
```
###  Ejercicio 15 - Usuarios 'admin' verificados mayores de 25 años.
```sql
SELECT * FROM users 
	WHERE birth_date <= DATE_SUB(CURDATE(), INTERVAL 25 YEAR) AND role = 'admin'
```

##  Nivel 3

###  Ejercicio 16 - Contar usuarios por role.
```sql
SELECT role, COUNT(*) AS total_usuarios
FROM users
GROUP BY role;
```
###  Ejercicio 17 - Contar usuarios por document_type.
```sql
SELECT document_type, COUNT(*) AS total_usuarios
FROM users
GROUP BY document_type;
```
###  Ejercicio 18 - Contar cuántos usuarios están desempleados.
```sql
SELECT COUNT(*) AS total_desempleado
FROM users
WHERE company IS NULL;
```
###  Ejercicio 19 - Calcular el promedio general de ingresos.
```sql
SELECT AVG(monthly_income) AS promedio_ingresos
FROM users;
```
###  Ejercicio 20 - Calcular el promedio de ingresos por role.
```sql
SELECT role, AVG(monthly_income ) AS promedio_ingresos
FROM users
GROUP BY role;
```


## Nivel 4

###  Ejercicio 21 - Mostrar profesiones con más de 10 personas.
```sql
SELECT profession, COUNT(*) AS total_personas
FROM users
GROUP BY profession
HAVING COUNT(*) > 10;
```

###  Ejercicio 22 - Mostrar la ciudad con más usuarios.
```sql
SELECT city, COUNT(*) AS total_usuarios
FROM users
GROUP BY city
ORDER BY total_usuarios DESC
LIMIT 1;
```

###  Ejercicio 23 - Comparar cantidad de menores vs mayores de edad.
```sql
SELECT 
  SUM(CASE 
        WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) < 18 
        THEN 1 ELSE 0 
      END) AS menores,
      
  SUM(CASE 
        WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) >= 18 
        THEN 1 ELSE 0 
      END) AS mayores
FROM users;
```
###  Ejercicio 24 - Promedio de ingresos por ciudad ordenado de mayor a menor.
```sql
SELECT 
    city, 
    ROUND(AVG(monthly_income ), 2) AS promedio_ingresos
FROM users
GROUP BY city
ORDER BY promedio_ingresos DESC;
```
###  Ejercicio 25 - Mostrar las 5 personas con mayor ingreso.
```sql
SELECT first_name , monthly_income 
FROM users
ORDER BY monthly_income  DESC
LIMIT 5;
```

## Nivel 5

### Ejercicio 26 - Clasificar usuarios como: `Menor` `Adulto` `Adulto mayor`

```sql
SELECT 
    id,
    first_name ,
    TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) AS edad,
    CASE 
        WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) < 18 
            THEN 'Menor'
        WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) BETWEEN 18 AND 59 
            THEN 'Adulto'
        ELSE 'Adulto mayor'
    END AS clasificacion
FROM users;
```

### Ejercicio 27 - Mostrar cuántos usuarios hay en cada clasificación anterior.
```sql
SELECT 
    CASE 
        WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) < 18 
            THEN 'Menor'
        WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) BETWEEN 18 AND 59 
            THEN 'Adulto'
        ELSE 'Adulto mayor'
    END AS clasificacion,
    COUNT(*) AS cantidad
FROM users
GROUP BY clasificacion;
```
### Ejercicio 28 - Ranking de ingresos por ciudad.
```sql
SELECT 
    city,
    SUM(monthly_income ) AS ingreso_total
FROM users
GROUP BY city 
ORDER BY ingreso_total DESC;
```
### Ejercicio 29 - Profesión con mayor ingreso promedio.
```sql
SELECT 
    company,
    AVG(monthly_income) AS Best_sueldo
FROM users 
GROUP BY company 
ORDER BY Best_sueldo  DESC
LIMIT 1;
```

### Ejercicio 30 - Mostrar usuarios cuyo ingreso esté por encima del promedio general.
```sql
SELECT *
FROM users
WHERE monthly_income >= (
    SELECT AVG(monthly_income)
    FROM users
);
```