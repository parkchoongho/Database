# Triggers and Events

### Triggers

Trigger is a block of SQL code that automatically gets executed before or after an insert, update or delete statement. 우리는 데이터 consistency를 강화하기 위해 Trigger를 사용할 수 있습니다.

예를 들어 payments table에 새로운 데이터가 들어왔다고 하면 invoice table의 payment_total column의 value값이 변화해야합니다. 이럴떄 Trigger를 사용합니다.

```mysql
delimiter $$

create trigger payments_after_insert
	after insert on payments
    for each row
begin
	update invoices
    set payment_total = payment_total + NEW.amount
    where invoice_id = NEW.invoice_id;
end $$

delimiter ;
```

```mysql
insert into payments
values (default, 5, 3, '2019-01-01', 10, 1);
```

