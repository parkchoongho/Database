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