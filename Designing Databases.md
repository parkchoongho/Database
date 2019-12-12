# Designing Databases

### Data Modeling

Data Modeling은 총 4가지 프로세스로 구성되어 있습니다.

1. Understand the Requirements

   => 실제 business를 하는데 있어 매우 중요한 과정입니다. 이 부분을 이해하고 들어가야 올바른 데이터베이스를 구축할 수 있습니다.

2. Build a Conceptual Model

   => 이 프로세스에서는 비즈니스에 필요한 entities나 concepts등을 identify하고 서로간의 관계를 규명합니다.

3. Build a Logical Model

   => Logical Model은 Database와는 분리된 추상적인 데이터 모델입니다. 이는 우리에게 필요한 테이블과 column에 어떤 것들이 있는지 보여줍니다.

4. Build a Physical Model

   => Logical Model을 바탕으로 특정 데이터베이스 시스템에 맞는 Physical Model을 bulid하는 과정입니다. Physical Model은 추상화 되어있는 Logical Model을 측정 데이터베이스에 맞게 실행한 것입니다.

### Conceptual Models

#### => Represents the entities and their relationships

강의를 제공하는 사이트를 만든다고 가정해 봅시다. 그렇다면 우리는 학생과 강의라는 **Entity**를 쉽게 도출해 낼 수 있습니다. 이제 둘 사이의 관계를 정립해야 하는데 **Entity Relationship (ER)**과 **Unified Modeling Language (UML)** 크게 2가지 방법이 있습니다. 여기서는 ER을 사용하도록 하겠습니다.

[해당 ER Diagram](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Untitled Diagram.drawio#R7VfJbtswEP0aHxto8ZZjvCQ9pGhgo2h6KhiRkohQpEpRsZWv71AitTGLE6BFUNQHQ%2FM4wyHnvaGoSbjOjlcS5ekXgQmbBB4%2BTsLNJAgWywD%2BNVA1wDRcNEAiKW4gvwP29JEY0DNoSTEpBo5KCKZoPgQjwTmJ1ABDUoqDcTPTxYINs%2BYoIQ6wjxBz0e8Uq7RBl8Giwz8TmqQ2sz8%2Fb0YyZJ1N6iJFWBx6ULidhGsphGqesuOaMF07W5cm7vKZ0XZhknB1SsCiDIrzn6ubXx7%2Fuvt2uLrNL2afzCwPiJVmw3tVYj1js2ZV2UIUB5oxxMFaRSll%2BBpVotSJC4Wie2utUiHpo%2BAKMRjyAYBhqQyvoTfw2OtIgDUaU8bWgglZZws9bxnFscbBs4df1j%2FApSg5Jtgk0V4mhz%2B1tlm7Vy9CivuWwEDHkwL8b2z1vBa6RoWyS7eM6VGMirTOpw3EaMLhOYJoAktbuWTYyhKpyLEHGXKuiMiIkhW4mFGrk2poHjrV%2BXODpX3FWUdklJ60M3digAejhzdoI3C0wVFGJsEcePB1FUiGKOvZGCmyIwktoCZQqrGCbNEYiVXLieWWi1pafRkYqMhRRHlyXUdtpiO2NZe6xhRa9sIkUCK3dKK7VgNSKKR6NpBliXbZe7FZXqfUcBieyOH5n6Jw5lC4FqUsyP%2Fu%2FtvdHX649p472lBUsX5%2F55JGfZtyqHQZKaCrAxVKin%2Bv1Wcn8%2FtRWn3h0OmwQjBcbYwJ26eq2hGGFBV82404JU9VZrudcHyh71RgbncZ4pU9ARwUppfVrSl%2FbfzQxtnMmptjf3BTWetI1a3NBs%2B9KLC6IG3YmGe7s4CzLiKvv%2BFgBwl5SQ2mVXT9XtRCn3zvqWa2oKzr%2FjC8Yz4lCZPjRlB9JWuvCv7oMFmMVNXs3ET174SjicLRRP54oqY0zkS1QtuNv1%2B0S0e0Wy4FY%2B6JAm2ohmJ0Tunx2ZBRjGtFj4%2BHXG%2Bn3uBsNZlttITCFUN3hK3glZXUL5%2FeWymuf6efHbYTx2dH%2B5FiljL4DnjqneGdeUG4HBBk7yDvFZB1EXFckDdSCmb3tdK4d5984fY3)

이 단계에서는 각 attribute가 어떻게 구성될 것인지에 대해 생각할 필요가 없습니다.

### Logical Models

위에서 만든 Conceptual Model을 통해 Logical Model을 만들어 보겠습니다. Logical Model은 좀 더 디테일한 abstract data model이라고 생각하시면 됩니다. Logical Model도 entity와 entity 사이의 관계들에 대해서 보여줍니다.

각 Attribute의 데이터 타입 정하기. (그런데 varchar와 같이 세부적으로 정하지는 않는다.)

특정 column을 더 쪼개서 저장할 수 없는지 살펴보기. (예를 들어, name의 경우에는 last name, first name 이렇게 나눌 수 있으므로 단순히 name으로만 저장하고 query를 날릴 경우 쿼리가 복잡해질 가능성 Up)

#### Relationship

- One-to-One
- One-to-Many
- Many-to-Many

관계는 기본적으로 이 세가지를 기준으로 파생됩니다.

만일 학생이 언제 강의를 등록했는지 알고 싶다면 해당 Attribute는 어디에 있어야 할까요? Student?, Coures? 둘다 아닌것 같습니다. Enroll이라는 새로운 Entity를 생성하고 거기에 Attribute를 만들겠습니다.

[해당 ER Diagram](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1#R7VhLU9swEP41mSmHdvxK4hwhCeUAHYZQQo%2FClm0NspWRZWLz67uyJT%2BTNExLaQdyyHi%2F3dVj99v1JiN7HudfOdpEV8zHdGQZfj6yFyPLmroWfEugqADHnlZAyIlfQWYDrMgzVqCh0Iz4OO0YCsaoIJsu6LEkwZ7oYIhztu2aBYx2d92gEA%2BAlYfoEF0TX0QV6lrTBr%2FAJIz0zuZkVmlipI3VTdII%2BWzbguzlyJ5zxkT1FOdzTGXsdFwqv%2FM92vpgHCfiGIc8frh7fL5bWg%2Fr72P3Qsyv7PVnlZ0nRDN14ZXIfLlidWZR6ECkWxJTlIB05kWE%2BpeoYJncOBXIe9TSWcQ4eWaJQBRUJgCg5kLl1TY6FivpCbBEA0LpnFHGy91sw3C9IJA4WLbw8%2FIDOGdZ4mNfbSKt1B6mo2V1dqM8BGePdQIt6Y9TsL%2FW0TNq6BKlQh9dZ0xqfZRG5X5SQJSECTx74I3haGcqjJgLnO%2FNj1lnHaoFsxgLXoCJctA8KbritmGdOVFY1GacNkSK6WG9ckMGeFB8eAE37AE3AsJT8Q3FGOBPEFOShCcjawJ5MWVUKDqkxTEidLfKRwLf4JCkEEyIMdhI5JbE%2BGTAQx16igNRZ1YzJGElQdtkUlC6QR7sell6LZweZyQjZPIIFP6p2kCwjSYFeqiZxJlAoiVDfjVdjuLA%2FiocEkMxwT6SCbPXIoI7IMIy4YzS%2BKNPvEWfcLqNwnTfvFPMBgSR9dur47raN5x4pTKgDIn3UeHuv13hetpqZXCQFuzDYKREuCwRxQ2mSBCWLBvNIOaRiHWF48Q%2FlRMZiMsbyNktu0JJUSrgDvcq3KXwQwpfrLGWF3lbuyi0lBNxr1eH58ptOlZi4yUF7VRdTN7mcAHC5VnGPfzrNyR0rhAfyv5sd%2Fbb6TZ2FawGeRnop%2B6Bd5FA7XHNiOzMumPY3YbhGD0aVfdUTu0JsreO2VvI7i9UBWKwUEnJ%2Bt6%2FwVLzLVn6B3kz%2Fj94M%2B69afrt51jeTMw9b6y%2FxBtnQJs5nDzFrzO7TD9ml72zizPpd5Dhi2%2B2i9HOa735xgNuQM%2Bge37CDCaXWkMSsM48ATnc6SlQmHY072DmcQ7PPNMjU2%2FYL049iM3%2FKlUXaf6cspc%2FAQ%3D%3D)

### Physical Models

Physical Model은 특정 database 기술로 Logical Model을 실제로 실행한 것입니다. table 이름을 정할 때는 단수로 할지 복수로할지 한가지를 정해서 그 규칙을 계속 지켜나가는 것이 좋습니다. 여기서는 MySQL을 사용하겠습니다.

여기서는 실제 어떠한 데이터 타입이 될것인지, NULL을 허용할 것인지, 등등을 설정합니다. 매우 많은 부분을 여기서 다루게 되니 다음 부분부터 모두 이 Physical Model에 해당하신다고 보면 되겠습니다.

### Primary Key

Primary Key는 해당 Table에서 각 record를 유일하게 구별하는 column입니다. Primary Key는 값이 변하지 않아야 합니다. 따라서 유일한 값인 email을 primary key로 설정하는 것은 좋지 않습니다. email은 나중에 바꿀 수도 있기 때문입니다. 2개의 column을 묶어서 composite primary key를 만들수도 있습니다.

### Foreign Key

두 table의 relationship을 정의할 때는 한 쪽은 parent, primary key table이 되고 다른 한 쪽은 child, foreign key table이 됩니다. foreign key는 다른 table의 primary key를 참조하고 있는 column입니다.

### Foreign Key Constraints

Table에 Foreign Key를 설정할 때마다 해당 Foreign Key에 Constraints를 설정해주어야 합니다. 이는 기본적으로 데이터가 망가지는 것을 방지해줄 것입니다.

예를 들어 students table의 id가 2인 data의 id가 1로 바뀌었다고 가정해봅시다. 그럼 이 primary key를 참조하고 있는 모든 foreign key도 값이 바뀌어야 할 것입니다. update할 때 이를 cascade한다고 합니다. 이를 설정해 놓으면 자동으로 primary key가 바뀌면 foreign key값을 변경시켜줍니다. delete의 경우에는 (이 예시한에서) restrict합니다. 왜냐하면 delete을 cascade하면 회원이 나갈 경우 해당 정보가 없어져 이 강의로 얼마큼의 매출을 기록했나 등을 알 수 없어지기 때문입니다.

### Normalization

우리는 우리가 설계한 Table이 redundant하고 중복되는 데이터를 허용하지 않기를 바랍니다. Redundancy는 데이터베이스의 크기를 증가시키고 insert, update, delete등의 기능들이 실행되는 시간들도 증가시킵니다.

따라서 이런 것을 방지하고자 **Normalization**을 실행합니다. (DB 정규화) 정규화에는 7가지 Rule이 있고 각각의 rule은 앞에 rule이 적용되어 있다고 가정합니다. 그리고 대부분 99%의 appilcation들은 앞에 3가지 rule만 적용시켜도 됩니다.

(1st Normal Form, 2nd Normal Form, 3rd Normal Form)

