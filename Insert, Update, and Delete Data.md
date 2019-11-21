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