# Домашнее задание к занятию «SQL. Часть 2» Kolb Dmitry 

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

**Решение**
```
SELECT 
    CONCAT(s.last_name, ' ', s.first_name) AS staff_name,
    ci.city AS store_city,
    COUNT(c.customer_id) AS customer_count
FROM store st
JOIN staff s ON st.store_id = s.store_id
JOIN address a ON st.address_id = a.address_id
JOIN city ci ON a.city_id = ci.city_id
JOIN customer c ON st.store_id = c.store_id
GROUP BY st.store_id, staff_name, ci.city
HAVING COUNT(c.customer_id) > 300;
```
![image 1](png/1.png)

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
**Решение**
```
SELECT COUNT(*) AS movie_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
![image 2](png/2.png)
### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

**Решение**
```
SELECT 
    DATE_FORMAT(p.payment_date, '%Y-%m') AS payment_month,
    SUM(p.amount) AS total_payments,
    COUNT(r.rental_id) AS total_rentals
FROM payment p
JOIN rental r ON p.rental_id = r.rental_id
GROUP BY payment_month
ORDER BY total_payments DESC
LIMIT 1;
```
![image 3](png/3.png)
## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.
### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».
**Решение**
```
SELECT 
    s.staff_id,
    CONCAT(s.first_name, ' ', s.last_name) AS staff_name,
    COUNT(p.payment_id) AS count_sales,
    CASE 
        WHEN COUNT(p.payment_id) > 8000 THEN 'YES' 
        ELSE 'NO' 
    END AS award
FROM staff s
JOIN payment p ON s.staff_id = p.staff_id
GROUP BY s.staff_id, s.first_name, s.last_name
ORDER BY count_sales DESC;  
```
![image 4](png/4.png)
### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.




