# Essential MySQL Functions

### Numeric Functions

```mysql
select ROUND(5.73);
select ROUND(5.73, 1);
select ROUND(5.7345, 2);

select truncate(5.73, 1);

select ceiling(5.73);

select floor(5.73);

select abs(5.73);
select abs(-5.73);

select rand();
```

[MySQL Numberic Functions 참고](https://dev.mysql.com/doc/refman/8.0/en/numeric-functions.html)

### String Functions

```mysql
select length('sky');

select upper('sky');
select lower('SKY');

select ltrim('      sky');
select rtrim('sky        ');
select trim('  sky        ');

select left('kindergarten', 4);
select right('kindergarten', 6);
select substring('kindergarten', 3,6);
select substring('kindergarten', 3);

select locate('n','kindergarten');
select locate('N','kindergarten');
select locate('q','kindergarten');
select locate('garten','kindergarten');

select replace('kindergarten', 'garten', 'garden');

select concat('kinder', 'garten');
```

```mysql
use sql_store;

select concat(first_name, ' ', last_name) as full_name
from customers;
```

[MySQL String Functions 참고](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)

### Date Functions in MySQL

```mysql
select now(), curdate(), curtime();

select year(now());
select month(now());
select day(now());
select hour(now());
select minute(now());
select second(now());
-- 위 함수들은 integer를 return합니다. 결과 값으로 string을 받고 싶은 경우

select dayname(now());
select monthname(now())

select extract(year from now());
select extract(month from now());
select extract(day from now());
```

```mysql
select *
from orders
where year(order_date) = year(now());
```

