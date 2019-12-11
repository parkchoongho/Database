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

[해당 ER Diagram](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Untitled%20Diagram.drawio#R7VfJbtswEP0aHxto8ZZjvCQ9pGhgo2h6KhiRkohQpEpRsZWv71AitTGLE6BFUNQHQ%2FM4wyHnvaGoSbjOjlcS5ekXgQmbBB4%2BTsLNJAgWywD%2BNVA1wDRcNEAiKW4gvwP29JEY0DNoSTEpBo5KCKZoPgQjwTmJ1ABDUoqDcTPTxYINs%2BYoIQ6wjxBz0e8Uq7RBl8Giwz8TmqQ2sz8%2Fb0YyZJ1N6iJFWBx6ULidhGsphGqesuOaMF07W5cm7vKZ0XZhknB1SsCiDIrzn6ubXx7%2Fuvt2uLrNL2afzCwPiJVmw3tVYj1js2ZV2UIUB5oxxMFaRSll%2BBpVotSJC4Wie2utUiHpo%2BAKMRjyAYBhqQyvoTfw2OtIgDUaU8bWgglZZws9bxnFscbBs4df1j%2FApSg5Jtgk0V4mhz%2B1tlm7Vy9CivuWwEDHkwL8b2z1vBa6RoWyS7eM6VGMirTOpw3EaMLhOYJoAktbuWTYyhKpyLEHGXKuiMiIkhW4mFGrk2poHjrV%2BXODpX3FWUdklJ60M3digAejhzdoI3C0wVFGJsEcePB1FUiGKOvZGCmyIwktoCZQqrGCbNEYiVXLieWWi1pafRkYqMhRRHlyXUdtpiO2NZe6xhRa9sIkUCK3dKK7VgNSKKR6NpBliXbZe7FZXqfUcBieyOH5n6Jw5lC4FqUsyP%2Fu%2FtvdHX649p472lBUsX5%2F55JGfZtyqHQZKaCrAxVKin%2Bv1Wcn8%2FtRWn3h0OmwQjBcbYwJ26eq2hGGFBV82404JU9VZrudcHyh71RgbncZ4pU9ARwUppfVrSl%2FbfzQxtnMmptjf3BTWetI1a3NBs%2B9KLC6IG3YmGe7s4CzLiKvv%2BFgBwl5SQ2mVXT9XtRCn3zvqWa2oKzr%2FjC8Yz4lCZPjRlB9JWuvCv7oMFmMVNXs3ET174SjicLRRP54oqY0zkS1QtuNv1%2B0S0e0Wy4FY%2B6JAm2ohmJ0Tunx2ZBRjGtFj4%2BHXG%2Bn3uBsNZlttITCFUN3hK3glZXUL5%2FeWymuf6efHbYTx2dH%2B5FiljL4DnjqneGdeUG4HBBk7yDvFZB1EXFckDdSCmb3tdK4d5984fY3)

이 단계에서는 각 attribute가 어떻게 구성될 것인지에 대해 생각할 필요가 없습니다.



