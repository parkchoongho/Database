# Views

### Creating Views

쿼리 작성시, 여러개의 join이나 subquery를 작성하면 쿼리문이 굉장히 복잡해지는 문제가 발생합니다. 이런 경우 View를 사용하면 편리합니다. 쿼리문이나 subqueries들을 view에 저장하여 활용할 수 있습니다. 이렇게 View를 통해 쿼리문을 더 간편하게 작성할 수 있고 반복되는 쿼리문에 사용할 수 있습니다.

```mysql
select
	c.name,
    c.client_id,
    sum(invoice_total) as total_sales
from clients c
join invoices i using (client_id)
group by c.client_id, name;
```

 이 쿼리는 나중에 여러 다른 쿼리에서 활용될 수 있습니다. 그런 상황을 대비하여 View로 저장합니다.

```mysql
create view sales_by_client as
select
	c.name,
    c.client_id,
    sum(invoice_total) as total_sales
from clients c
join invoices i using (client_id)
group by c.client_id, name;
```

이렇게 쿼리문을 날리면 sales_by_client라는 이름으로 view를 하나 생성합니다. 이렇게 생성한 View는 하나의 Table처럼 작동합니다. 하지만 View가 실제 데이터를 저장하고 있지는 않다는 점에 주의해야합니다.

```mysql
-- Create a view to see the balance
-- for each client.
-- 
-- clients_balance

-- client_id
-- name
-- balance

create view clients_balance as
select
	c.client_id,
    c.name,
    sum(i.invoice_total - i.payment_total) as balance
from clients c
join invoices i using(client_id)
group by c.client_id;
```

### Altering or Dropping Views

이번에는 만들어진 View를 바꾸거나 삭제하는 작업을 해보겠습니다.

```mysql
drop view sales_by_client
```

이렇게 하면 만들어진 view를 삭제할 수 있습니다.

```mysql
create or replace view sales_by_client as
select
	c.name,
    c.client_id,
    sum(invoice_total) as total_sales
from clients c
join invoices i using (client_id)
group by c.client_id, name
order by total_sales desc;
```

replace를 추가하여 view를 바꿀 수 있습니다.

