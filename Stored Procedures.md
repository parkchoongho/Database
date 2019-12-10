# Stored Procedures

### What are Stored Procedures?

Database를 갖춘 Application을 만든다고 가정해봅시다. 우리는 Application 코드와 SQL 코드가 같이 쓰이는 것을 원하지 않을겁니다. 왜냐하면 그렇게 코드를 작성하면 코드가 굉장히 지저분해지기 때문입니다. 따라서 SQL 코드만을 따로 담아두는 장소가 필요합니다. 이것이 바로 **Stored Procedures**입니다.

**Stored Procedure**은 SQL 코드르 포함하고 있는 데이터베이스 객체입니다. **Stored Procedures**는 다음 3가지 장점을 가지고 있습니다.

- Store and organize SQL
- Faster execution
- Data security

### Creating a Stored Procedure

이번에는 Stored Procedure를 생성해보겠습니다.

```mysql
delimiter $$
create procedure get_clients()
begin
	select * from clients;
end$$

delimiter ;
```

begin과 end 사이에는 해당 procedure에 body가 오게 됩니다. body에는 여러개의 쿼리문이 올 수 있기 때문에 statement가 한개일지라도 뒤에 `;` 를 붙혀줘야 합니다.

delimiter의 경우에는 전체 procedure를 하나의 unit 형태로 전달하는 역할을 합니다. 그리고 `delimiter ;` 을 통해 해당 procedure를 실행할 수 있습니다. 

이렇게 코드를 작성하고 실행하면 Stored Procedures 폴더에 파일이 생성됩니다.

```mysql
call get_clients()
```

위 코드를 실행하면 저장되어 있는 procedure가 호출되어 실행됩니다.

```mysql
-- Create a stored procedure called
-- 		get_invoices_with_balance
-- 		to return all the invoices with a balance > 0
delimiter $$
create procedure get_invoices_with_balance()
begin
	select *, (invoice_total - payment_total) as balance
    from invoices
    where invoice_total - payment_total > 0;
end$$
delimiter ;
```

