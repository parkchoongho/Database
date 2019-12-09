# Writing Complex Query

### Subqueries

쿼리안에 쿼리를 작성해야 할때 subquery를 활용해서 작성합니다.

```mysql
-- Find products that are more
-- expensive than Lettuce (id = 3)
select *
from products
where unit_price > (
	select unit_price 
    from products
    where product_id = 3
)
```

```mysql
-- In sql_hr databases:
-- 		Find employees whose earn more than average

select *
from employees
where salary > (
	select avg(salary)
    from employees
)
```

### The IN Operator

이번에는 단 한번도 주문되지 않은 제품에 대한 데이터를 가져오고 싶다고 가정해 봅시다.

```mysql
-- Find the products that have never been ordered

select *
from products
where product_id not in (
	select distinct product_id
	from order_items
)
```

이 subquery는 여러개의 값을 반환합니다.

```mysql
-- Find clients without invoices

select *
from clients 
where client_id not in (
	select distinct client_id
    from invoices
)
```

### Subqueries vs Joins

subquey는 join과 상호보완적인 관계랑 사용될 수 있습니다.

```mysql
-- Find clients without invoices

select *
from clients
where client_id not in (
	select distinct client_id
    from invoices
)

select *
from clients
left join invoices using(client_id)
where invoice_id is null
```

크게 두가지에 근거해서 둘 중 하나를 선택할 수 있습니다. (**Performance, Readability**)

위 두 쿼리는 작동 시간이 비슷하기에 더 읽기 쉬운 쿼리를 사용하는 것이 좋습니다.

여기서는 위 쿼리가 더 직관적으로 가져오려는 데이터를 표현하고 있기에 위 쿼리가 더 좋은 것으로 판단됩니다.

```mysql
-- Find customers who have ordered lettuce (id = 3)
-- 		Select customer_id, first_name, last_name
select customer_id, first_name, last_name
from customers
where customer_id in (
	select o.customer_id
    from order_items oi
    join orders o using (order_id)
    where product_id = 3
)
```

```mysql
-- Find customers who have ordered lettuce (id = 3)
-- 		Select customer_id, first_name, last_name
select distinct customer_id, first_name, last_name
from customers c
join orders o using (customer_id)
join order_items oi using (order_id)
where product_id = 3
```

### The ALL Keyword

```mysql
-- Select invoices lareger than all invoices of
-- client 3

select *
from invoices
where invoice_total > (
	select MAX(invoice_total)
	from invoices
	where client_id = 3
)
```

위 쿼리를 **ALL** 키워드를 사용하여 더 쉽게 표현할 수 있습니다. 

```mysql
-- Select invoices lareger than all invoices of
-- client 3

select *
from invoices
where invoice_total > ALL (
	select *
	from invoices
	where client_id = 3
)
```

이렇게 쿼리를 작성하면 이제 subquery에서 여러개의 값을 받아온 후 **ALL** 키워드로 invoice_total과 비교할 수 있게 됩니다. 모든 값보다 큰 것이므로 그중에 가장 큰 값보다 큰 값들을 도출하므로 위에 있는 쿼리와 동등한 결과가 나타납니다.

### ANY Keyword

```mysql
-- select clients with at least two invoices
select *
from clients
where client_id in (
	select client_id
    from invoices
    group by client_id
    having count(*) >=2
)
```

이 쿼리를 **ANY** Keyword를 사용하여 다르게 표현할 수 있습니다.

```mysql
-- select clients with at least two invoices
select *
from clients
where client_id = ANY (
	select client_id
    from invoices
    group by client_id
    having count(*) >=2
)
```

subquery에서 나온 값들 중 아무 값이라도 client_id와 같으면 그 결과를 출력합니다.

### Correlated Subqueries

```mysql
-- select employees whose salary is
-- above the average in their office
use sql_hr;

select *
from employees e
where salary > (
	select avg(salary)
    from employees
    where office_id = e.office_id
)
```

이처럼 subquery가 outer query를 참조하고 있으면 이를 **Correlated Subqueries**라 합니다.

```mysql
-- Get invoices that are larger than the
-- client's average invoice amount

use sql_invoicing;

select *
from invoices i
where invoice_total > (
	select avg(invoice_total)
    from invoices
    where client_id = i.client_id
) 
```

### The EXISTS Operator

```mysql
-- Select clients that have an invoice

select *
from clients
where client_id in (
	select distinct client_id
    from invoices
)
```

위 쿼리를 **EXISTS** Operator를 활용하여 다르게 작성해 보겠습니다.

```mysql
-- Select clients that have an invoice

select *
from clients c
where exists (
	select client_id
    from invoices i
    where c.client_id = i.client_id 
)
```

첫 번째 쿼리는 subquery를 실행한 후 결과가 나온후에 outer query를 실행합니다. 그런데 만약 subquery의 결과가 100만개만큼 결과 수가 많다면, 쿼리의 퍼포먼스가 떨어지게 될 것입니다. 그런 경우에 **EXISTS** Operator를 활용합니다. 

```mysql
-- Find the products that have never been ordered
use sql_store;

select *
from products p
where not exists (
	select product_id
    from order_items oi
    where oi.product_id = p.product_id
)
```

### Subqueries in the SELECT Clause

```mysql
select
	invoice_id,
    invoice_total,
    (select avg(invoice_total)
		from invoices) as invoice_average,
	invoice_total - (select invoice_average) as difference
from invoices
```

select에서는 alias를 바로 사용할 수 없습니다. 대신에 select문을 활용하여 값을 가져와 위 쿼리와 같이 활용할 수 있습니다.

```mysql
select 
	name,
    client_id,
	(select sum(invoice_total) 
	   from invoices
       where c.client_id = client_id) as total_sales,
	(select avg(invoice_total)
		from invoices) as average,
	(select total_sales - average) as difference
from clients c;
```

### Subqueries in the FROM Clause

쿼리를 실행해서 생성한 테이블을 from clause에서 활용할 수 있습니다. 실제 database에 존재하는 table은 아니지만 from clause로 참조할 수 있습니다.

```mysql
select *
from (
	select 
		name,
		client_id,
		(select sum(invoice_total) 
		   from invoices
		   where c.client_id = client_id) as total_sales,
		(select avg(invoice_total)
			from invoices) as average,
		(select total_sales - average) as difference
	from clients c
) as sales_summary;
```

위처럼 from에서 subquery를 사용할 경우 alias(sales_summary)를 주어야 합니다.

```mysql
select *
from (
	select 
		name,
		client_id,
		(select sum(invoice_total) 
		   from invoices
		   where c.client_id = client_id) as total_sales,
		(select avg(invoice_total)
			from invoices) as average,
		(select total_sales - average) as difference
	from clients c
) as sales_summary
where total_sales is not null
```

