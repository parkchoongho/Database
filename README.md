# DataBase

## Basic SQL

### What is a Database?

A **Database** is a collection of data stored in a format that can be easily accessed. In order to manage the databases, we use a software application called **database management system(DBMS)**. We connect to a DBMS, give it a instructions for querying or modifying data. The DBMS will execute our instructions and send results back.

There are many **DBMS**s and these are classified into two categories, **relational and non relational**, also called **NoSQL**. In **relational databases**, we store data in tables that are linked to each other using **relationships**. That's why we call these databases relational databases. **SQL** is the language that we use to work with these **relational database management systems**.

(NoSQL DBMS don't understand SQL)

## Retrieving Data From a Single Table

### The SELECT Statement

```sql
USE sql_store;

SELECT * FROM customers
-- WHERE customer_id = 1
ORDER BY first_name;
```

`--` 은 주석처럼 그 줄에 있는 쿼리를 무시할 수 있게 해줍니다.

### The SELECT Clause

```sql
SELECT 
	first_name, 
    last_name, 
    points, 
    (points + 10) * 100 AS "discount factor"
FROM customers;

SELECT DISTINCT state FROM customers;
```

```sql
-- Return all the products
-- 	name
--	unit price
-- 	new price (unit price * 1.1)

SELECT 
	name,
    unit_price,
    unit_price * 1.1 AS "new price"
FROM products;
```

### The WHERE Clause

```sql
SELECT * 
FROM customers 
WHERE state <> 'VA';

SELECT * 
FROM customers 
WHERE state != 'VA';
```

위 쿼리와 아래 쿼리는 같은 결과를 가져옵니다.

```sql
SELECT * 
FROM customers 
WHERE birth_date > '1990-01-01';
```

예시: 1990년 1월 1일 이후에 태어난 고객들 가져오기

```sql
-- Get the orders placed this year
SELECT * 
FROM orders
WHERE order_date >= "2019-01-01";
```

