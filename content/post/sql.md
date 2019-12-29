---
title: "SQL Command"
author: "Kunyi Lu"
date: 2019-12-29T10:55:02-08:00
tags: ["SQL", "Database"]
draft: false
---

---

# SQL Query Commands

## Fundamentals

1. select all rows and columns from table tbl
   `SELECT * FROM tbl;`
2. select column c1, c2 and all rows from table tbl
   `SELECT c1, c2 FROM tbl;`
3. **DISTINCT**: search for unique values
   `SELECT DISTINCT c1, c2 FROM tbl;`
4. **WHERE**: query for specific rows

```sql
    SELECT c1 FROM tbl
    WHERE conditions;
```

5. **COUNT**: the number of rows taht match a specific condition of a query

```sql
    SELECT COUNT(*) FROM tbl;
```

```sql
    SELECT COUNT(DISTINCT *) FROM tbl;
```

6. **LIMIT**: limit the number of rows you get back after query. We can use this command when we care more about columns than rows

```sql
    SELECT * FROM tbl
    LIMIT 5;
```

7. **ORDER BY**: sort the rows in ascending or descending order

```sql
    SELECT c1, c2 FROM tbl
    ORDER BY c2 ASC;
```

Note: ASC denotes ascending order(default), DESC denotes descending order

8. **BETWEEN**: match a value against a range of values. Can use <, > mathematical functions instead. e.g.

```
    SELECT amount, payment_date FROM payment
    WHERE payment_date BETWEEN '2014-04-06' AND '2014-05-06'
```

9. **IN**: use with **WHERE** clause to check if a value in a list of values. e.g.

```sql
    SELECT c1 FROM tbl
    WHERE c1 IN (value1, value2, ...);
```

Use sub query:

```sql
    SELECT c1 FROM tbl1
    WHERE c1 IN (SELECT c2 FROM tbl2);
```

10. **LIKE**: match pattern. e.g.

```sql
    SELECT first_name FROM customer
    WHERE first_name LIKE 'Jen%';
```

- wildcard
  - %: match any sequence of chars
  - -: match any single char

Note: **LIKE** is case-sensitive, whereas **ILIKE** is case-insensitive

## GROUP BY Statements

1. aggregate functions: **MAX**, **MIN**, **AVG**, **SUM**, **ROUND**, **COUNT**...

```sql
    SELECT ROUND(AVG(amount), 5) FROM payment;
    SELECT MIN(amount) FROM payment;
```

2. **GROUP BY**: divides the rows returned from the **SELECT** statement into groups. For each group, you can apply an aggregate function.

```sql
    SELECT c1, aggregate(expr)
    FROM tbl
    GROUP BY c1
```

**GROUP BY** will group by distinct values. For each distinct value, we do aggregate function.

3. **HAVING**: use with **GROUP BY** clause to filter group rows that do not satisfy a specific condition

```sql
    SELECT c1, aggregate(expr) AS c2
    FROM tbl
    GROUP BY c1
    HAVING c2 > v;
```

Note: **WHERE** and **HAVING** are similar. But **WHERE** applies before **GROUP BY** and **HAVING** applies after **GROUP BY**

4. **AS**: rename columns or table selections with an alias

## JOINS

1. **INNER JOIN**

```sql
    SELECT A.pka, A.c1, B.pkb, B.c2
    FROM A
    INNER JOIN B ON A.pka = B.fka;
```

returns rows in A table that have the corresponding rows in B table

> **JOIN** Types
> There are several types of JOIN: INNER JOIN, FULL OUTER JOIN, LEFT OUTER JOIN, RIGHT OUTER JOIN.
> ![JOIN Type Venn Graph](../image/sql.md/sql_venn_graph.jpg)

2. **UNION**: combines result sets of two or more **SELECT** statements into a single result set

```sql
    SELECT c1, c2
    FROM t1
    UNION
    SELECT c1, c2
    FROM t2;
```

We usually use **UNION** to combine data from similar tables that are not perfectly normalized

## Advanced Commands

1. timestamp
   Each type of database has timestamp datatype. You can check documentation for details
   Examples:

```sql
    SELECT SUM(amount), extract(month from payment_date) AS month
    FROM payment
    GROUP BY month
    ORDER BY SUM(amount) DESC
    LIMIT 1;
```

2. mathmatical functions
   SQL comes with a lot of mathematical operators built-in that are very useful for numeric column types. You can check documentation for details.
   Examples:

```sql
    SELECT customer_id + rental_id AS new_id
    FROM payment;
```

3. string function and operations
   SQL comes with a lot of string operations.
   Examples:

```sql
    SELECT first_name || ' ' || last_name AS full_name
    FROM customer;
```

where || denotes string concatenation.

4. SubQuery
   To construct a subquery, we put the second query in brackets and use it in the **WHERE** clause as an expression

```sql
    SELECT film_id, title, rental_rate
    FROM film
    WHERE rental_rate > (SELECT AVG(rental_rate) FROM film);
```

5. Self-join: combines rows with other rows in same table. Implement by using table alias

```sql
    SELECT a.customer_id, a.first_name, a.last_name, b.customer_id, b.first_name, b.last_name
    FROM customer AS a
    JOIN customer AS b
    ON a.first_name = b.last_name;
```

the same as

```sql
    SELECT a.customer_id, a.first_name, a.last_name, b.customer_id, b.first_name, b.last_name
    FROM customer AS a, customer AS b
    WHERE a.first_name = b.last_name;
```
