# Summarizing Data

### Aggregate Functions

이 섹션에서는 데이터를 요약하는 쿼리를 어떻게 작성하는지를 배워보겠습니다. 먼저 aggregate function을 사용하는 방법부터 알아봅시다. MySQL에는 여러가지 함수들이 내장되어 있습니다. 이 중 몇몇 함수를 aggregate 함수라고 칭하는데, 그 이유는 이 함수가 여러가지 값을 받은 후에 이를 aggregate하여 하나의 값을 반환하기 때문입니다.

```mysql
MAX()
MIN()
AVG()
SUM()
COUNT()
....
```

위 함수들은 non null value에 대해서만 작동합니다. 만약 해당 column에 null 값이 들어가 있다면 위 함수들은 그 값은 제외하고 실행될 것 입니다.

```mysql
select 
	MAX(invoice_total) AS highest,
	MIN(invoice_total) AS lowest,
    AVG(invoice_total) AS average,
    SUM(invoice_total) AS total,
    count(invoice_total) AS number_of_invoices
    count(payment_date) AS count_of_payments
    count(*) AS total_records
from invoices;
```

```mysql
select 
	MAX(invoice_total) AS highest,
	MIN(invoice_total) AS lowest,
    AVG(invoice_total) AS average,
    SUM(invoice_total) AS total,
    count(*) AS total_records,
    count(distinct client_id) AS total_records
from invoices
where invoice_date > '2019-07-01'
```

duplicate되는 value를 제거하고 싶으면 distinct 키워드를 사용하면 됩니다.

```mysql
select 
	'First half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total - payment_total) AS what_we_expect
from invoices
where invoice_date < '2019-07-01'
union
select 
	'Second half of 2019',
    SUM(invoice_total),
    SUM(payment_total),
    SUM(invoice_total - payment_total)
from invoices
where invoice_date > '2019-06-30'
union
select 
	'Total',
    SUM(invoice_total),
    SUM(payment_total),
    SUM(invoice_total - payment_total)
from invoices;
```

### The GROUP BY Clause

```mysql
select
	sum(invoice_total) as total_sales
from invoices
```

이와 같이 쿼리를 작성하면 총합이 나오게 됩니다. 근데 만약 client마다의 total_sales를 확인하고 싶다면 어떻게 해야 할까요?

```mysql
select
	client_id,
	sum(invoice_total) as total_sales
from invoices
group by client_id;
```

```mysql
select
	client_id,
	sum(invoice_total) as total_sales
from invoices
where invoice_date > '2019-06-30'
group by client_id
order by total_sales desc;
```

clause가 입력되는 순서를 기억하시기 바랍니다.

이번에는 여러개의 column을 동시에 group by하는 쿼리를 작성하겠습니다.

```mysql
select
	c.state,
	c.city,
	sum(i.invoice_total) as total_sales
from invoices i
join clients c using(client_id)
group by state, city;
```

```mysql
select
	p.date as payment_date,
    pm.name as pay_method,
    sum(p.amount) as total_payments
from payments p
join payment_methods pm
	on p.payment_method = pm.payment_method_id
group by date, pay_method
order by date;
```

