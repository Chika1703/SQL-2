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

```
SELECT COUNT(*) AS movie_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

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




