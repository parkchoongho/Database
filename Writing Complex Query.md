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

