# Insert, Update, and Delete Data

### Column Attributes

**Datatype**

해당 column에 들어가야되는 data의 종류를 의미합니다.

ex) varchar(50)은 최대 길이 50까지의 문자열을 의미합니다. 

char (50)과 varchar(50)의 차이점: 만일 길이 5의 문자열을 삽입했을시 char(50)은 나머지 45를 채우고 들어갑니다. 하지만 varchar(50)의 경우에는 그 문자열 그대로 입력됩니다. char()는 이렇게 메모리의 낭비를 유발하기에 varchar()를 주로 사용합니다.

**PK(Primary Key)**

**NN(Not Null)**

**AI(Auto Increment)**

### Inserting a Row

데이터를 입력하는 쿼리를 작성해보도록 하겠습니다.

```mysql
insert into customers
values(
	default,
    'Choong Ho',
    'Park',
    '1992-11-26',
    '010-6987-9383',
    'Song pa',
    'Seoul',
    'Korea',
    default
    )
```

위 쿼리와 아래 쿼리는 동일한 결과를 가져옵니다.

```mysql
insert into customers(
	first_name,
    last_name,
    birth_date,
    phone,
    address,
    city,
    state
)
values(
    'Choong Ho',
    'Park',
    '1992-11-26',
    '010-6987-9383',
    'Song pa',
    'Seoul',
    'Korea'
    )
```

column을 넣을 때는 꼭 table에 명시되어 있는 column 순서대로 넣을 필요는 없습니다.

```mysql
insert into customers(
    last_name,
	first_name,
    birth_date,
    phone,
    address,
    city,
    state
)
values(
    'Park',
    'Choong Ho',
    '1992-11-26',
    '010-6987-9383',
    'Song pa',
    'Seoul',
    'Korea'
    )
```

### Inserting Multiple Rows

이번에는 여러개의 데이터를 입력하는 쿼리를 작성해 보겠습니다.

```mysql
insert into shippers (name)
values('Jalhanika'),
	  ('Mothanika'),
	  ('PyeongBumhanika')
```

```mysql
insert into products (name, quantity_in_stock, unit_price)
values('banana milk', 30, 1.2),
	  ('samgakgimbab', 60, 1.1),
	  ('cupramyeon', 40, 1.3)
```

### Inserting Hierarchical Rows

만약 여러개의 table에 데이터를 넣고 싶다면 어떻게 하면 될까요? 

```mysql
insert into orders (customer_id, order_date, status)
values (1, '2019-11-20', 1);

insert into order_items
values 
	(LAST_INSERT_ID(), 1, 1, 2.95),
	(LAST_INSERT_ID(), 2, 3, 3.95);
```

orders table 1개의 row가 orders_items table 여러개의 row를 가질 수 있을 때, table1을 parent라 하고 table2를 child라 합니다. 하나의 orders row에 해당하는 order_items를 2개 rows를 해당 table에 넣었습니다.

**LAST_INSERT_ID()**는 마지막에 넣은 데이터의 ID 값을 가지고 오는 MySQL 내장함수입니다.

### Creating a Copy of a Table

만약 우리가 orders table을 복사한 orders_archive table을 만들고 싶다고 가정해 봅시다.

```mysql
create table orders_archived as
select * from orders;
```

이렇게 쿼리를 작성하면 orders table의 복사본을 생성할 수 있습니다. 그런데 column의 세부사항을 보면 primary key나 auto increment와 같은 사항이 설정되어 있지 않습니다. 이 문제는 어떻게 해결해야 할까요?(이 tutorial에서는 해결하지 않습니다.)

우선 truncate를 활용하여 데이터를 모두 날리고 새로 설정해보겠습니다.

```mysql
select * 
from orders
where order_date < '2019-01-01';
```

위 쿼리로 받은 데이터를 orders_archived table로 넣는 쿼리를 작성하도록 하겠습니다.

```mysql
insert into orders_archived
select * 
from orders
where order_date < '2019-01-01';
```

```mysql
create table invoices_archived as
select i.invoice_id, number, c.name as client, i.invoice_total, i.payment_total, i.invoice_date, i.due_date, i.payment_date
from invoices i
join clients c
	on i.client_id = c.client_id
where i.payment_date is not null;
```

```mysql
create table invoices_archived as
select i.invoice_id, number, c.name as client, i.invoice_total, i.payment_total, i.invoice_date, i.due_date, i.payment_date
from invoices i
join clients c
	using (client_id)
where i.payment_date is not null;
```

### Updating a Single Row

table에 있는 data가 변경되었을 때 이를 어떻게 해야 반영할 수 있을까요?

```mysql
update invoices
set payment_total = 10, payment_date = '2019-11-20'
where invoice_id = 1;
```

```mysql
update invoices
set payment_total = 0, payment_date = null
where invoice_id = 1;
```

```mysql
update invoices
set payment_total = default, payment_date = default
where invoice_id = 1;
```

```mysql
update invoices
set 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
where invoice_id = 3;
```

### Updating Multiple Rows

여러개의 rows를 변경하고 싶다면 이렇게 하면 됩니다.

```mysql
update invoices
set 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
where client_id = 3;
```

where clause의 condition 부분을 여러개가 선택될 수 있게끔 수정하면 됩니다.

```mysql
-- Write a SQL statement to
-- 		give any customers born after 1990
-- 		50 extra points
update customers
set 
	points = points + 50
where birth_date > '1990-12-31';
```

### Updating Subqueries in Updates

```mysql
update invoices
set 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
where client_id = 
		(select client_id
		from clients
		where name = 'Myworks');
```

```mysql
update invoices
set 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
where client_id in 
		(select client_id
		from clients
		where state in ('CA', 'NY');
```

()안에 들어가있는 쿼리를 먼저 실행합니다.

```mysql
update orders
	set comments = 'gold customer'
where customer_id in 
	(select customer_id 
	from customers
	where points > 3000);
```

