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

```mysql
SELECT *
FROM customers
WHERE last_name LIKE 'b____y';
-- % any number of characters
-- _ single character
```

위와 같이 코드를 작성하면 b로 시작하고 그 사이에 4개의 문자열이 오고 y로 끝나는 성을 가진 사람에 대한 데이터를 가져옵니다.

```mysql
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

### The REGEXP Operator

```mysql
SELECT *
FROM customers
WHERE last_name LIKE '%field%';
```

위 쿼리는 **REGXEXP**를 활용해 다르게 작성할 수 있습니다.

```mysql
SELECT *
FROM customers
WHERE last_name REGEXP 'field';
```

```mysql
SELECT *
FROM customers
WHERE last_name REGEXP '^field';
-- field로 성이 시작하는 사람의 데이터를 가져옵니다.

SELECT *
FROM customers
WHERE last_name REGEXP 'field$';
-- field로 성이 끝나는 사람의 데이터를 가져옵니다.

SELECT *
FROM customers
WHERE last_name REGEXP 'field|mac|rose';
-- 성이 field, mac, rose 중 하나라도 포함하는 사람의 데이터를 가져옵니다.

SELECT *
FROM customers
WHERE last_name REGEXP 'field$|mac|rose';
-- 성이 field로 끝나거나 mac, rose 중 하나라도 포함하는 사람의 데이터를 가져옵니다.

SELECT *
FROM customers
WHERE last_name REGEXP '[gim]e';
-- 성이 ge, ie, me 중 하나라도 포함하는 사람의 데이터를 가져옵니다.

SELECT *
FROM customers
WHERE last_name REGEXP '[a-h]e';
-- ae ~ he까지 중 하나라도 포함하는 사람의 데이터를 가져옵니다.

-- ^ beginning
-- $ end
-- | logical or
-- [abcd]
-- [a-d]
```

```mysql
-- GET the customers whose

-- 		first names are ELKA or AMBUR
SELECT *
FROM customers
WHERE first_name REGEXP 'ELKA|AMBUR';

--  	last names end with EY or ON
SELECT *
FROM customers
WHERE last_name REGEXP 'EY$|ON$';

-- 		last names start with MY or contains SE
SELECT *
FROM customers
WHERE last_name REGEXP '^MY|SE';

-- 		last names contain B followed by R or U
SELECT *
FROM customers
WHERE last_name REGEXP 'B[RU]';
```

### The IS NULL Operator

NULL은 해당 데이터가 존재하지 않음을 의미합니다. 그렇다면, 특정 열에 해당하는 데이터가 없는 유저들에게 이메일을 보내고 싶을 때 어떻게 하면 될까요? **IS NULL**을 사용하면 편리하게 쿼리를 작성할 수 있습니다.

```mysql
SELECT *
FROM customers
WHERE phone IS NULL;
```

**NOT**을 활용하면 폰 번호가 있는 유저들 데이터를 가져올 수 있습니다.

```mysql
SELECT *
FROM customers
WHERE phone IS NOT NULL;
```

```mysql
-- Get the orders that are not shipped
SELECT *
FROM orders
WHERE shipped_date IS NULL;
```

### The ORDER BY Clause

```mysql
SELECT *
FROM customers
ORDER BY first_name;
```

이렇게 쿼리를 작성하면 first_name에 대해 오름차순으로 데이터를 정렬해서 보여줍니다.

내림차순으로 정렬하고 싶으면 아래와 같이 쿼리를 작성하면 됩니다.

```mysql
SELECT *
FROM customers
ORDER BY first_name DESC;
```

여러 column에 대해서도 정렬을 할 수 있습니다.

```mysql
SELECT *
FROM customers
ORDER BY state, first_name;
```

이렇게 되면 state에 대해 정렬을 먼저 한 후, 그 다음 first_name으로 정렬합니다. 

각 column 다음에 **DESC**를 붙여 오름차순, 내림차순을 결정할 수 있습니다.

```mysql
SELECT *
FROM customers
ORDER BY state DESC, first_name;
```

추가적으로 MySQL에 있는 기능인데 MySQL에서는 해당 컬럼이 선택된 clause가 아닐지라도 그 column에 대해서 정렬할 수 있습니다.

```mysql
SELECT first_name, last_name
FROM customers
ORDER BY birth_date;
```

```mysql
SELECT first_name, last_name
FROM customers
ORDER BY 1, 2;
```

여기서 1은 first_name을 의미하고 2는 last_name을 의미합니다.이러한 sort 방식은 지양해야 합니다. 왜냐하면 query를 어떻게 날리느냐에 따라 1, 2의 의미가 변화하기 때문입니다. 되도록이면 column 명을 입력하도록 합시다.

```mysql
SELECT *, quantity * unit_price AS total_price
FROM order_items
WHERE order_id = 2 
ORDER BY total_price DESC;
```

### The LIMIT Clause

만약 가져오는 데이터중 3개만 보고 싶으면 어떻게 하면 될까요? **LIMIT**을 사용하면 가능합니다.

```mysql
SELECT *
FROM customers
LIMIT 3;
```

아래와 같이 쿼리를 작성하면 7~9번째 데이터를 가져올 수 있습니다.

```mysql
SELECT *
FROM customers
LIMIT 6, 3;
```

참고적으로 **LIMT**은 쿼리 마지막에 와야 합니다.

**WHERE** => **ORDER BY** => **LIMIT**

```mysql
-- Get the top three loyal customers
SELECT * 
FROM customers
ORDER BY points DESC
LIMIT 3;
```

## Retrieving Data From Multiple Tables

### Inner Joins

```mysql
SELECT *
FROM orders
JOIN customers
ON orders.customer_id = customers.customer_id;
```

위와 같이 쿼리를 작성하면 orders의 customer_id column 값과 customers의 customer_id column이 같은 부분을 연결해서 table를 만들어 줍니다.

```mysql
SELECT order_id, customer_id, first_name, last_name
FROM orders
JOIN customers
ON orders.customer_id = customers.customer_id;
```

위와 같이 쿼리를 작성하면 에러가 발생합니다. 그 이유는 customer_id 때문입니다. 여기서 customer_id는 orders 테이블과 customers 테이블 모두에 존재하는 column입니다. 이런 경우 mysql은 두 table중 어느 테이블에서 customer_id 값을 가져올지 알 수 없습니다. 따라서 쿼리에 명시적으로 어느 테이블에서 가져올 것인지 작성해주어야 합니다.

```mysql
SELECT order_id, orders.customer_id, first_name, last_name
FROM orders
JOIN customers
ON orders.customer_id = customers.customer_id;
```

그리고 여기서 작성한 쿼리를 보면 orders와 customers같은 table이 계속 반복되는 것을 확인하실 수 있습니다. 이런경우 아래와 같이 간단하게 줄여서 쿼리를 작성할 수 있습니다.

```mysql
SELECT order_id, o.customer_id, first_name, last_name
FROM orders o
JOIN customers c
ON o.customer_id = c.customer_id;
```

```mysql
SELECT order_id, oi.product_id, quantity, oi.unit_price
FROM order_items oi
JOIN products p
ON oi.product_id = p.product_id;
```

