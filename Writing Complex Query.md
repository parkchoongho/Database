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

