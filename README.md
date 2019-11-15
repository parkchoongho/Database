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

### Joining Across Databases

개발자로 작업하다보면 table뿐만이 아니라 서로 다른 데이터 베이스를 동시에 활용해야하는 일이 생기기도 합니다. 이럴 때 쿼리를 어떻게 작성해야 하는지 알아봅시다.

```mysql
SELECT *
FROM order_items oi
JOIN sql_inventory.products p
ON oi.product_id = p.product_id;
```

여기서 sql_inventory가 다른 database입니다. 이렇게 현재 내가 작업하고 있는 database가 어디냐에 따라서 database가 다른 table끼리 join해서 작업할 수 있습니다.

### Self Joins

같은 table끼리 join하여 새로운 table을 생성하는 것도 가능합니다.

```mysql
SELECT * 
FROM employees e
JOIN employees m
ON e.reports_to = m.employee_id;
```

이렇게 작성하면 직원을 담당하고 있는 관리자의 정보까지 가져올 수 있습니다. (그 관리자도 employees table에 있는 경우.)

```mysql
SELECT e.employee_id, e.first_name, m.first_name AS manager
FROM employees e
JOIN employees m
ON e.reports_to = m.employee_id;
```

### Joining Multiple Tables

2개이상의 table을 join하여 table을 생성할 수도 있습니다.

```mysql
SELECT o.order_id, o.order_date, c.first_name, c.last_name, os.name AS status
FROM orders o
JOIN customers c 
	ON o.customer_id = c.customer_id
JOIN order_statuses os
	ON o.status = os.order_status_id;
```

```mysql
select 
	p.date, 
    p.amount, 
    c.name, 
    pm.name AS payment_method
from payments p
join clients c
	on p.client_id = c.client_id
join payment_methods pm
	on p.payment_method = pm.payment_method_id
```

### Compound Join Conditions

어떤 table에서는 그 어떤 column도 unique한 값을 가지지 않을 수 있습니다. 이런 경우에는 2개의 column을 조합하여 unique한 column을 만들어낼 수 있습니다. 이를 composite primary key라 부르는데 이는 2개 이상의 column을 조합하여 그 table에서의 unique한 primary key가 됨을 의미합니다. 이를 활용하여 join condition을 조합할 수 있습니다.

```mysql
select *
from order_items oi
join order_item_notes oin
	on oi.order_id = oin.order_id
    and oi.product_id = oin.product_id;
```

### Implicit Join Syntax

아래 두 쿼리는 같은 퀴리입니다.

```mysql
select *
from orders o
join customers c
	on o.customer_id = c.customer_id;
```

```mysql
select *
from orders o, customers c
where o.customer_id = c.customer_id;
```

아래와 같은 쿼리 방법을 implicit한 join이라고 합니다. 그런데 아래와 같은 방법을 사용하면 where 절을 빼먹으면 에러가 발생하지 않고 모든 orders와 customers간에 join이 발생하고 table이 생성됩니다. 이는 실수했다는 것을 시스템상에서 알기 힘들기 때문에 explicit하게 join을 작성하여 쿼리를 쓰는 것을 습관화하는 것이 좋습니다.

### Outer Joins

지금까지 확인한 join은 모두 inner join입니다. 이번 파트에서는 outer join에 대해서 알아보는 시간을 갖겠습니다. (join이라고만 쿼리를 작성하면 이는 inner join을 의미합니다. outer join을 사용하고 싶으면 명시적으로 outer join이라고 쿼리를 작성해야 합니다.)

```mysql
select c.customer_id, c.first_name, o.order_id
from customers c
join orders o
	on c.customer_id = o.customer_id
order by c.customer_id;
```

이렇게 쿼리를 작성하면 손님중에 주문한 손님만 볼 수 있습니다. 만일 주문하지 않은 손님까지 데이터를 가지고 오고 싶다면 어떻게 하면 될까요? 아래와 같이 쿼리를 작성하면 됩니다.

```mysql
select 
	c.customer_id, 
    c.first_name, 
    o.order_id
from customers c
left join orders o
	on c.customer_id = o.customer_id
order by c.customer_id;
```

outer join에는 **left join**과 **right join**이 있습니다. 위 경우에는 **left join**을 하면 모든 customer table의값을 가져오고 **right join**을 하면 모든 orders table의 값을 가져오게 됩니다.

```mysql
select 
	c.customer_id, 
    c.first_name, 
    o.order_id
from customers c
right join orders o
	on c.customer_id = o.customer_id
order by c.customer_id;
```

```mysql
select 
	p.product_id,
    p.name,
    oi.quantity
from products p
left join order_items oi
	on p.product_id = oi.product_id
order by p.product_id;
```

### Outer Join Between Multiple Tables

여러개의 table과 outer join을 맺을 수도 있습니다.

```mysql
select 
	c.customer_id,
    c.first_name,
    o.order_id,
    sh.name AS shipper
from customers c
left join orders o
	on c.customer_id = o.customer_id
left join shippers sh
	on o.shipper_id = sh.shipper_id 
order by c.customer_id;
```

이렇게 쿼리를 작성하면 주문하지 않은 모든 사람과 shipper가 없는 order까지 가져올 수 있습니다.(구글검색에서 더 자세히 공부할 것.)

```mysql
select
	o.order_date,
    o.order_id,
    c.first_name AS customer,
    sh.name AS shipper,
    os.name AS status
from orders o
join customers c
	on o.customer_id = c.customer_id
left join shippers sh
	on o.shipper_id = sh.shipper_id
join order_statuses os
	on o.status = os.order_status_id
```

### Self Outer Joins

```mysql
use sql_hr;

select
	e.employee_id,
	e.first_name,
    m.first_name AS manager
from employees e
left join employees m
	on e.reports_to = m.employee_id;
```

### The USING Clause

```mysql
select 
	o.order_id,
    c.first_name
from orders o
join customers c
	on o.customer_id = c.customer_id;
```

쿼리가 복잡해질 수록 on condition이 쿼리를 이해하는 것을 방해할 수 있습니다. 그런데 위 처럼 같은 column 값인 customer_id를 가리키고 있으면 **USING**을 사용하여 더 간단하게 표현할 수 있습니다.

```mysql
select 
	o.order_id,
    c.first_name
from orders o
join customers c
	using (customer_id)
```

이 쿼리는 그 위의 쿼리와 같은 결과 값을 되돌려줍니다.

```mysql
select 
	o.order_id,
    c.first_name,
    sh.name AS shipper
from orders o
join customers c
    using (customer_id)
left join shippers sh
	using (shipper_id);
```

```mysql
select *
from order_items oi
join order_item_notes oin
	on oi.order_Id = oin.order_Id AND
		oi.product_id = oin.product_id;
```

위 쿼리를 아래처럼 **USING**을 이용하여 간단하게 만들 수 있습니다.

```mysql
select *
from order_items oi
join order_item_notes oin
	using (order_id, product_id);
```

```mysql
select 
	p.date,
    c.name AS client,
    p.amount,
    pm.name AS payment_method
from payments p
join clients c
	using (client_id)
join payment_methods pm
	on p.payment_method = pm.payment_method_id;
```

### Natural Joins

MySQL에는 더 간단하게 두 테이블을 join하는 방법이 있습니다. 이는 **Natural Join**이라하고 쉽게 작성할 수 있지만 권장하는 방식은 아닙니다. 왜냐하면 종종 원치 않는 결과를 야기하기 때문입니다. 

```mysql
select
	o.order_id,
	c.first_name
from orders o
natural join customers c;
```

이렇게 쿼리를 작성하면 MySQL에서 자동으로 같은 이름을 가진 column을 join해서 결과로 보여줍니다.

그런데 만약 column 값이 바뀌었거나 테이블에 변화가 생겼을 경우 예상치 않은 결과가 나타날 수 있으니 join condition을 명시하는 방법이 더 권장됩니다.

### Cross Joins

**Cross Join**을 활용하면 첫 번째 테이블의 모든 record를 두 번째 테이블의 모든 record와 join할 수 있습니다.

```mysql
select *
from customers c
cross join products p;
```

모든 record를 join하는 것이기 때문에 condition이 필요하지 않습니다.

```mysql
select 
	c.first_name AS customer,
    p.name AS product
from customers c
cross join products p;
```

위 쿼리와 아래 쿼리는 같은 값을 가져옵니다. 위 쿼리처럼 작성하는 것을 권장합니다.

```mysql
select 
	c.first_name AS customer,
    p.name AS product
from customers c, products p
```

```mysql
select 
	sh.name AS shipper,
    p.name AS product
from shippers sh, products p
```

```mysql
select 
	sh.name AS shipper,
    p.name AS product
from shippers sh
cross join products p;
```

### Unions

join을 활용하면 column을 결합해 여러가지 table 결과를 만들어낼 수 있습니다. 그런데 column뿐만이 아닌 row도 combine을 할 수 있습니다.

```mysql
select *
from orders
```

이렇게 쿼리를 작성하면 order_date를 확인할 수 있습니다. 그런데 만약에 올해 주문에는 active라는 label을 붙히고 그 전 주문에 대해서는 archive라는 label을 붙히고 싶다면 어떻게 해야 할까요?

```mysql
select *
from orders
where order_date >= '2019-01-01'
```

하나는 이렇게 작성하는 것이 있습니다. 하지만 이는 좋은 방법이 아닙니다. 왜냐하면 2019년을 하드코딩해서 넣었기 때문에 만약에 년도가 바뀌면 다시 '2020-01-01'이라고 입력해야 되기 때문입니다.

```mysql
select 
	order_id,
    order_date,
    'Active' As status
from orders
where order_date >= '2019-01-01';

select 
	order_id,
    order_date,
    'Archive' As status
from orders
where order_date < '2019-01-01';
```

이렇게 쿼리를 작성하면 쿼리문 마다 table이 다르게 생성됩니다. (table이 2개가 생성됨을 의미합니다.)

```mysql
select 
	order_id,
    order_date,
    'Active' As status
from orders
where order_date >= '2019-01-01'
UNION
select 
	order_id,
    order_date,
    'Archive' As status
from orders
where order_date < '2019-01-01';
```

이 쿼리는 서로 다른 2개의 table을 묶어서 나타내줍니다. **Union**을 활용할 때는 2개의 table의 column 수가 같아야합니다. 가령,

```mysql
select first_name, last_name
from customers
union
select name
from shippers;
```

이 쿼리는 에러를 발생시킵니다. 첫 번째 table 결과는 column이 2개 두 번째 table 결과는 column이 1개이기 때문입니다. 그리고 **Union**을 활용한 table 결과 column값은 첫 번째 쿼리의 column 값과 동일하게 나타납니다.

```mysql
select 
	customer_id, 
    first_name, 
    points,
    'Gold' AS status
from customers 
where points >= 3000
union
select 
	customer_id, 
    first_name, 
    points,
    'Silver' AS status
from customers 
where points between 2000 and 3000
union
select 
	customer_id, 
    first_name, 
    points,
    'Bronze' AS status
from customers 
where points <= 2000
ORDER BY first_name;
```

