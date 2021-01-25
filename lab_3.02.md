-- Lab | SQL Joins on multiple tables
-- Write a query to display for each store its store ID, city, and country.
SELECT s.store_id, ci.city, co.country
FROM sakila.store s
JOIN sakila.address a
ON s.address_id = a.address_id
JOIN sakila.city ci
ON a.city_id = ci.city_id
JOIN sakila.country co
ON co.country_id = ci.country_id;

-- Write a query to display how much business, in dollars, each store brought in.
SELECT st.store_id, sum(p.amount)
FROM sakila.staff s
JOIN sakila.payment p
ON s.staff_id = p.staff_id
JOIN sakila.store st
ON st.store_id = s.store_id
GROUP BY st.store_id;

-- What is the average running time of films by category?
SELECT round(AVG(fi.length),2) AS avgLength, ca.name
FROM sakila.film fi
JOIN sakila.film_category fica
ON fi.film_id = fica.film_id
JOIN sakila.category ca
ON ca.category_id = fica.category_id
GROUP BY ca.name;

-- Which film categories are longest?
SELECT round(AVG(fi.length),2) AS avgLength, ca.name
FROM sakila.film fi
JOIN sakila.film_category fica
ON fi.film_id = fica.film_id
JOIN sakila.category ca
ON ca.category_id = fica.category_id
GROUP BY ca.name
ORDER BY avgLength DESC;

-- Display the most frequently rented movies in descending order.
SELECT f.title, count(*) AS rentalFreq
FROM sakila.rental r
JOIN sakila.inventory i
ON r.inventory_id = i.inventory_id
JOIN sakila.film f
ON f.film_id = i.film_id
GROUP BY f.title
ORDER BY rentalFreq DESC;

-- List the top five genres in gross revenue in descending order.
SELECT c.name, sum(p.amount) AS revenue
FROM sakila.payment p
JOIN sakila.rental r
ON p.rental_id = r.rental_id
JOIN sakila.inventory i
ON r.inventory_id = i.inventory_id
JOIN sakila.film_category f
ON i.film_id = f.film_id
JOIN sakila.category c
ON c.category_id = f.category_id
GROUP BY c.name
ORDER BY revenue DESC
LIMIT 5;

-- Is "Academy Dinosaur" available for rent from Store 1?
SELECT f.title, s.store_id, r.rental_date, r.return_date
FROM sakila.rental r
JOIN sakila.inventory i
ON r.inventory_id = i.inventory_id
JOIN sakila.store s
ON i.store_id = s.store_id
JOIN sakila.film f
ON i.film_id = f.film_id
WHERE f.title = 'Academy Dinosaur' AND s.store_id = 1 AND r.return_date IS NOT NULL
;
