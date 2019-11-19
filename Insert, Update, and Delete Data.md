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

