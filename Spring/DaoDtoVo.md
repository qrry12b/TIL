# Vo, DTO, DAO, Entity


```diff
- 잘못된 정보를 발견했다면 이슈를 남겨주세요
```

### VO (Value Object)
Value Object의 약어로 값을 가지는 객체입니다.   
VO는 비즈니스 값을 가져올때 사용하며 값 그자체를 표혀나는 객체입니다.   
보통 생성자를 통해 초기화를 하며 값을 수정 할 수 없습니다. (Read-Only, 불변객체)   
생성자에 전달되는 인자가 많거나 선택적 값을 설정하고 싶을때는 Builder 패턴이 사용되기도 합니다.   

### DTO (Data Transfer Object)
Data Transfer Object의 약어로 값을 가지고 있는 객체 입니다.   
기본적으로 VO와 비슷하지만 불변 객체는 아니며 주로 계층, 시스템간의 데이터 교환을 위한 객체입니다.

### DAO (Data Access Object)
Data Access Object의 약어로 실제 DB에 접속해 가져온 데이터를 Entity로 변환해 가져옵니다.   
대부분의 경우 메소드로 CRUD를 구현합니다.

### Entity
실제 DB의 테이블과 1 : 1 매핑되는 객체이며 id를 통해 각각의 Entity를 구분합니다.

### POJO (Plain Old Java Object)
특정 인터페이스나 클래스를 상속하지 않고 순수하게 Get/Set으로 구성된 자바 객체입니다.
특정 라이브러리나 컨테이너의 기술에 종속적이지 않기에 특정 라이브러리나 프레임워크를 제거해도 영향이 없으므로   
코드의 작성과 코드에 대한 테스트 작업을 더 유연하게 가질 수 있습니다.