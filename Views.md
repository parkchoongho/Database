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

### Updatable Views

작성한 view를 가지고 **특정 상황**에서는 insert, update, delete등의 작업을 할 수 있습니다.

특정 상황이란 아래의 명령어가 쿼리에 없는 경우입니다.

- **DISTINCT**
- **Aggregate Functions(MIN, MAX, SUM, .....)**
- **GROUP BY / HAVING**
- **UNION**

이 명령어가 없는 경우의 View를 Updatable View라고 합니다.

```mysql
create or replace view invoice_with_balance as 
select 
	*, (invoice_total - payment_total) as balance
from invoices
where (invoice_total- payment_total) > 0
```

해당 쿼리는 위의 조건을 충족하므로 Updatable View입니다. 위 View를 변경해보겠습니다.

```mysql
delete from invoice_with_balance
where invoice_id = 1
```

이 쿼리를 날리고 invoices table을 확인해보면 invoice_id가 1인 row가 사라진 것을 확인할 수 있습니다. 

```mysql
update invoice_with_balance
set due_date = date_add(due_date, interval 2 day)
where invoice_id = 2;
```

 이 쿼리를 날리고 invoices table을 확인해보면 invoice_id가 2인 row의 due_date column 값이 2일 추가된 것을 확인할 수 있습니다. 

insert의 경우에는 해당 view가 기초하고 있는 table의 모든 column값을 가지고 있을 때만 가능합니다.