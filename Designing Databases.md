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