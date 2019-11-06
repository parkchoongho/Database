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

### The AND, OR and NOT Operators

```sql
SELECT * 
FROM customers
WHERE birth_date > "1990-01-01" AND points > 1000;
```

```sql
SELECT * 
FROM customers
WHERE birth_date > "1990-01-01" OR points > 1000;
```

```sql
SELECT * 
FROM customers
WHERE birth_date > "1990-01-01" OR points > 1000 AND
	state = 'VA';
```

AND operator는 OR operator에 선행해서 동작합니다.

더 클린하게 논리구조를 표현하고 싶으면 아래와 같이 괄호를 사용할 수 있습니다.

```sql
SELECT * 
FROM customers
WHERE birth_date > "1990-01-01" OR (points > 1000 AND
	state = 'VA');
```

```sql
SELECT * 
FROM customers
WHERE NOT (birth_date > "1990-01-01" OR points > 1000);
```

위는 아래 쿼리와 같은 결과를 가져옵니다.

```sql
SELECT * 
FROM customers
WHERE birth_date <= "1990-01-01" AND points <= 1000);
```

```sql
-- From the order_items table, get the items
-- for order #6
-- where the total price is greater than 30

SELECT *
FROM order_items
WHERE order_id = 6 AND unit_price * quantity > 30;
```

### The IN Operator

```sql
SELECT *
FROM customers
WHERE state = 'VA' OR state = 'GA' OR state = 'FL';
```

위와 같이 쿼리를 작성하면 state가 'VA' 또는 'GA' 또는 'FL'인 데이터들이 뽑아져 나옵니다. 그런데 이렇게 쿼리를 작성하는 것은 다소 복잡해 보입니다. 아래와 같이 쿼리를 **IN** 을 활용해서 작성하면 더 간편하게 작성할 수 있습니다.

```sql
SELECT *
FROM customers
WHERE state IN ('VA', 'GA', 'FL');
```

 **NOT**과 결합해서 **IN**에 해당하지 않는 state를 가진 데이터를 가져올 수도 있습니다.

```sql
SELECT *
FROM customers
WHERE state NOT IN ('VA', 'GA', 'FL');
```

```sql
-- Return products with
-- 		quantity in stock equl to 49, 38, 72
SELECT *
FROM products
WHERE quantity_in_stock IN (49, 38, 72);
```

### The BETWEEN Operator

```sql
SELECT *
FROM customers
WHERE points >= 1000 AND points <= 3000;
```

위와 같은 쿼리는 아래처럼 더 간편하게 작성할 수 있습니다.

```sql
SELECT *
FROM customers
WHERE points BETWEEN 1000 AND 3000;
```

**BETWEEN**은 이상 이하이므로 입력되어 있는 범위를 포함합니다.

```sql
-- Return customers born
-- 		between 1/1/1990 and 1/1/2000
SELECT *
FROM customers
WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```

### The LIKE Operator

만약 유저중에서 성이 'b'로 시작하는(대소문자 구별없이) 사람을 찾고 싶다고 하면 **LIKE**를 활용해서 쿼리를 작성할 수 있습니다.

```sql
SELECT *
FROM customers
WHERE last_name LIKE 'b%';
```

만일 성에 'b'가 존재하는 사람에 관한 데이터를 뽑고 싶다면 이처럼 쿼리를 작성하면 됩니다.

```sql
SELECT *
FROM customers
WHERE last_name LIKE '%b%';
```

성이 'y'로 끝나는 사람에 대한 데이터를 가져오고 싶으면 이렇게 쿼리를 작성합니다.

```sql
SELECT *
FROM customers
WHERE last_name LIKE '%y';
```

```sql
SELECT *
FROM customers
WHERE last_name LIKE 'b____y';
-- % any number of characters
-- _ single character
```

위와 같이 코드를 작성하면 b로 시작하고 그 사이에 4개의 문자열이 오고 y로 끝나는 성을 가진 사람에 대한 데이터를 가져옵니다.

```sql
-- Get the customers whose
-- 		addresses contain TRAIL or AVENUE

SELECT * 
FROM customers
WHERE address LIKE '%trail%' OR 
	  address LIKE '%avenue%';

-- 		phone numbers end with 9

SELECT *
FROM customers
WHERE phone LIKE '%9';
```

**LIKE**를 활용하는 방법은 다소 올드한 방법이고 모든 문자열 패턴에 대해 검색이 가능한 방법이 존재합니다.