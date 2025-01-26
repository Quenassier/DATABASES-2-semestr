# SQL Exercises Report

## Introduction
Отчет по упражнениям SQL, выполненным в pgAdmin.

---

### Exercise 00: First steps into SQL world

**Query:**
```sql
SELECT * FROM person WHERE address = 'Kazan';
![скрин1](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/1.png) 
SELECT * FROM person WHERE address = 'Kazan' AND gender = 'female' ORDER BY name;
![скрин2](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/2.png)
SELECT name, rating FROM pizzeria WHERE rating BETWEEN 3.5 AND 5 ORDER BY rating;
![скрин3](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/3.png)
SELECT DISTINCT person_id FROM person_visits WHERE (visit_date BETWEEN '2022-01-06' AND '2022-01-09')
   OR pizzeria_id = 2
ORDER BY person_id DESC;
![скрин4](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/4.png)
SELECT CONCAT(name, ' (возраст:', age, ',пол:', gender, ',адрес:', address, ')') AS person_information
FROM person
ORDER BY person_information;
![скрин5](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/5.png)
SELECT name
FROM person
WHERE id IN (
    SELECT person_id
    FROM person_order
    WHERE order_date = '2022-01-07' AND menu_id IN (13, 14, 18)
);
![скрин6](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/6.png)
SELECT name,
       CASE
           WHEN EXISTS (
               SELECT 1
               FROM person_order
               WHERE person_order.person_id = person.id
                 AND menu_id IN (13, 14, 18)
                 AND order_date = '2022-01-07'
           ) THEN 'Есть заказ'
           ELSE 'Нет заказа'
       END AS check_name
FROM person;

![скрин7](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/7.png)
SELECT id, name, age,
       CASE
           WHEN age BETWEEN 10 AND 20 THEN 'interval #1'
           WHEN age > 20 AND age < 24 THEN 'interval #2'
           ELSE 'interval #3'
       END AS interval_info
FROM person
ORDER BY interval_info ASC;

![скрин8](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/8.png)
SELECT *
FROM person_order
WHERE id % 2 = 0
ORDER BY id;
![скрин9](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/9.png)
SELECT 
    (SELECT name FROM person WHERE person.id = pv.person_id) AS person_name,
    (SELECT name FROM pizzeria WHERE pizzeria.id = pv.pizzeria_id) AS pizzeria_name
FROM (
    SELECT person_id, pizzeria_id
    FROM person_visits
    WHERE visit_date BETWEEN '2022-01-07' AND '2022-01-09'
) AS pv
ORDER BY person_name ASC, pizzeria_name DESC;
![скрин10](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/10.png)