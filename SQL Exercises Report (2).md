# ПЕРВАЯ РАБОТА DAY00

## Exercise 00 — First Steps in SQL
**Query:**
```sql
SELECT * FROM person WHERE address = 'Kazan';
```
![Screenshot 1](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/1.png)

---

## Exercise 01 — Selecting Women from Kazan
**Query:**
```sql
SELECT * FROM person WHERE address = 'Kazan' AND gender = 'female' ORDER BY name;
```
![Screenshot 2](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/2.png)

---

## Exercise 02 — List of Pizzerias with Ratings 3.5 to 5
**Query:**
```sql
SELECT name, rating FROM pizzeria WHERE rating BETWEEN 3.5 AND 5 ORDER BY rating;
```
![Screenshot 3](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/3.png)

---

## Exercise 03 — Pizzeria Visits
**Query:**
```sql
SELECT DISTINCT person_id 
FROM person_visits 
WHERE (visit_date BETWEEN '2022-01-06' AND '2022-01-09') OR pizzeria_id = 2
ORDER BY person_id DESC;
```
![Screenshot 4](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/4.png)

---

## Exercise 04 — Generating person_information
**Query:**
```sql
SELECT CONCAT(name, ' (возраст:', age, ',пол:', gender, ',адрес:', address, ')') AS person_information
FROM person
ORDER BY person_information;
```
![Screenshot 5](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/5.png)

---

## Exercise 05 — People Who Ordered Items 13, 14, 18
**Query:**
```sql
SELECT name
FROM person
WHERE id IN (
    SELECT person_id
    FROM person_order
    WHERE order_date = '2022-01-07' AND menu_id IN (13, 14, 18)
);
```
![Screenshot 6](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/6.png)

---

## Exercise 06 — Order Check (check_name)
**Query:**
```sql
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
```
![Screenshot 7](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/7.png)

---

## Exercise 07 — Determining Age Intervals
**Query:**
```sql
SELECT id, name, age,
       CASE
           WHEN age BETWEEN 10 AND 20 THEN 'interval #1'
           WHEN age > 20 AND age < 24 THEN 'interval #2'
           ELSE 'interval #3'
       END AS interval_info
FROM person
ORDER BY interval_info ASC;
```
![Screenshot 8](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/8.png)

---

## Exercise 08 — Selecting Records with Even ID
**Query:**
```sql
SELECT *
FROM person_order
WHERE id % 2 = 0
ORDER BY id;
```
![Screenshot 9](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/9.png)

---

## Exercise 09 — Names of People and Pizzerias
**Query:**
```sql
SELECT 
    (SELECT name FROM person WHERE person.id = pv.person_id) AS person_name,
    (SELECT name FROM pizzeria WHERE pizzeria.id = pv.pizzeria_id) AS pizzeria_name
FROM (
    SELECT person_id, pizzeria_id
    FROM person_visits
    WHERE visit_date BETWEEN '2022-01-07' AND '2022-01-09'
) AS pv
ORDER BY person_name ASC, pizzeria_name DESC;
```
![Screenshot 10](https://github.com/Quenassier/DATABASES-2-semestr/blob/main/10.png)

---
