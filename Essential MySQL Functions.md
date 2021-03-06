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

### Formatting Dates and Times

```mysql
select date_format(now(), '%y');
select date_format(now(), '%Y');
select date_format(now(), '%m %Y');
select date_format(now(), '%M %Y');
select date_format(now(), '%M %d %Y');

select time_format(now(), '%h:%i %p');
```

[MySQL Date Formatter](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html)

### Calculating Dates and Times

```mysql
select date_add(now(), interval 1 day);
select date_add(now(), interval 1 year);

select date_add(now(), interval -1 year);
select date_sub(now(), interval 1 year);

select datediff(now(), '2019-01-02');
select datediff(now(), '2019-01-02 09:00');

select time_to_sec('09:00');
select time_to_sec('09:02') - time_to_sec('09:00')
```

### The IFNULL and COALESCE Functions

```mysql
select
	order_id,
    ifnull(shipper_id, 'Not Assigned') as shipper
from orders
order by order_id;
```

```mysql
select
	order_id,
    coalesce(shipper_id, comments, 'Not Assigned') as shipper
from orders
order by order_id;
```

**IFNULL**은 해당 column 값이 null일 경우 2번째 parameter 값을 출력해서 보여줍니다. **COALESCE**는 shipper_id가 null인 경우 comments를 보여주고 comments까지 null인경우에 'Not Assigned'를 출력합니다.

```mysql
select 
	concat(first_name, ' ', last_name) as customer,
    ifnull(phone, 'Unknown') as phone
from customers;
```

```mysql
select 
	concat(first_name, ' ', last_name) as customer,
    coalesce(phone, 'Unknown') as phone
from customers;
```

### The IF Function

```mysql
select
	order_id,
    order_date,
    if(
		year(order_date) = year(now()), 
        'Active', 
        'Archive') as category
from orders;
```

**IF** function에서 첫번째는 expression이고 두번째는 true일 때 결과 값, 세번째는 false일 때 결과 값입니다.

```mysql
select
	product_id,
    name,
    count(*) as orders,
    if(count(*) > 1, 'Many Times', 'Once') as frequency
from products p
join order_items oi using (product_id)
group by p.product_id;
```

```mysql
select
	product_id,
    name,
    (select count(*)
		from order_items oi
        where p.product_id = product_id) as orders,
	if((select orders) > 1, 'Many Times', if((select orders) = 1, 'Once', 'Not Yet')) as frequency
from products p;
```

### THe CASE Operator

만일 처리해야할 expression이 여러개라면 어떻게 쿼리를 작성해야 할까요?

```mysql
select
	order_id,
    order_date,
    case
		when year(order_date) = year(now()) then 'Active'
        when year(order_date) = year(now()) - 1 then 'Last Year'
        when year(order_date) < year(now()) - 1 then 'Archived'
        else 'future'
	end as category
from orders;
```

이렇게 작성하면 여러개의 **CASE**에 대비하여 쿼리를 작성할 수 있습니다.

```mysql
select
	concat(first_name, ' ', last_name) as customer,
    points,
    case
		when points >= 3000 then 'Gold'
        when points >= 2000 then 'Silver'
        else 'Bronze'
	end as category
from customers
order by points desc;
```

