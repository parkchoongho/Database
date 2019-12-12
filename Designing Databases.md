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

