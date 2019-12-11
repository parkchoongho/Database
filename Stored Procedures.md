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

### Dropping Stored Procedures

저장되어있는 procedure을 삭제해 보겠습니다.

```mysql
drop procedure get_clients
```

get_clients라는 이름의 procedure가 있다면 삭제가 진행됩니다. 하지만 get_clients가 존재하지 않으면 에러가 발생합니다. 이런 에러를 방지하려면 어떻게 해야 할까요? **IF EXISTS**를 활용합니다.

```mysql
drop procedure if exists get_clients
```

### Parameters

**Parameter**를 **Stored Procedure**에 전달하여 사용할 수도 있습니다.

```mysql
delimiter $$
create procedure get_clients_by_state
(
	state char(2)
)
begin
	select * from clients c
	where c.state = state;
end$$

delimiter ;
```

column state와 parameter state를 구분하기 위해 clients table을 c로 alias하고 `c.state = state` 로 where절을 구성했습니다.

```mysql
call get_clients_by_state('CA')
```

state가 'CA'인 client data를 불러옵니다.

```mysql
call get_clients_by_state()
```

이렇게 parameter를 전달하지 않으면 에러가 발생합니다.

```mysql
-- Write a stored procedure to return invoices
-- for a given client
-- 
-- get_invoices_by_client
delimiter $$
create procedure get_invoices_by_client
(
	client_id int
)
begin
	select * from invoices i
    where i.client_id= client_id;
end$$

delimiter ;
```

```mysql
call get_invoices_by_client(1)
```

### Parameters with Default Value

Parameter에 default 값을 설정하여 아무값도 입력하지 않아도 default value를 통해 procedure가 동작하도록 설정할 수 있습니다.

```mysql
delimiter $$
create procedure get_clients_by_state
(
	state char(2)
)
begin
	if state is null then
		select * from clients;
	else
		select * from clients c
		where c.state = state;
	end if;
end$$

delimiter ;
```

**IF** statement를 사용할 때는 반드시 **END IF** statement로 닫아주어야합니다. 그래야 MySQL이 어디까지가 **IF** statement에 해당하는지 알 수 있기 때문입니다. 

위 쿼리를 아래와 같이 바꿔서 procedure를 생성할 수도 있습니다.

```mysql
delimiter $$
create procedure get_clients_by_state
(
	state char(2)
)
begin	
	select * from clients c
	where c.state = ifnull(state, c.state);
end$$

delimiter ;
```

아래 방법이 더 Professional한 방법입니다. (코드 수가 확 줄어듭니다.)

```mysql
-- Write a stored procedure called get_payments
-- with two parameters
-- 
-- client_id => INT (4)
-- payment_method_id => TINTINT (1) 0 - 255

delimiter $$
create procedure get_payments
(
		client_id INT, 
        payment_method_id TINYINT
)
begin
	select * from payments p
    where 
		p.client_id = ifnull(client_id, p.client_id) and
		p.payment_method = ifnull(payment_method_id, p.payment_method);
end$$
```

```mysql
call get_payments(null, null);
```

### Parameter Validation

이번에는 data를 변경하는 procedure를 생성해 보겠습니다. Data를 변경할 때 입력하는 Data가 올바른 데이터인지 판별하는 절차를 거쳐야합니다. Schema에 맞지 않은 데이터로 변경되지 않도록 검증을 하는 절차를 공부해봅시다. 

```mysql
delimiter $$
create procedure get_payment
(
		invoice_id INT,
    	payment_amount DECIMAL(9, 2),
    	payment_date DATE
)
begin
	UPDATE invoices i
	SET
		i.payment_total = payment_amount,
        i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;
end$$
```

```mysql
call make_payment(2, 100, '2019-01-01');
```

이렇게 쿼리를 진행한 후, invoice table을 확인해보면 invoice_id = 2인 row의 payment_total, payment_date column의 값들이 바뀌어 있는 것을 확인할 수 있습니다.

```mysql
call make_payment(2, -100, '2019-01-01');
```

근데 만약 이렇게 payment_total column 값에 -100이라는 유효하지 않는 데이터를 넣으면 어떻게 될까요? DB상 거르지 못하고 invoice table의 값을 변경시켜 버리고 맙니다. 이런 경우 argument에 대한 유효검사를 procedure에서 수행해야 합니다.

```mysql
delimiter $$
create procedure get_payment
(
		invoice_id INT,
    	payment_amount DECIMAL(9, 2),
    	payment_date DATE
)
begin
	if payment_amount <= 0 then
		signal sqlstate '22003'
			set message_text = 'Invalid payment amount';
	end if;
    
	UPDATE invoices i
	SET
		i.payment_total = payment_amount,
        i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;
end$$
```

sqlstate는 여러가지 종류가 존재합니다. 아래 링크에서 여러 종류의 sqlstate를 확인할 수 있습니다.

https://www.ibm.com/support/knowledgecenter/ko/ssw_ibm_i_73/rzala/rzalaccl.htm

해당 에러가 어떤 에러인지 메세지를 남기는 습관은 DB 설정할 때 권장되는 습관입니다.

