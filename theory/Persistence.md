# Persistnece (영속성)

영속성이란 프로그램이 종료되어도 데이터가 남아있는 것을 말한다.

영속성을 가지지 않는 데이터는 메모리에 저장되어있기 때문에 프로그램이 종료되면 사라지게 된다.

그렇기 때문에 파일 시스템, 데이터 베이스를 활용해 프로그램이 종료되어도 데이터를 기억해주어야 한다.

#### 다음은 Spring의 영속성을 가지기 위한 방법이다.
### 1. JDBC
### 2. Persistence Framework

* ## JDBC

JDBC는 Java DataBase Connectivity의 약자로 DataBase와 연결, Sql문 처리, 결과 처리, 예외 처리, Transaction 처리를 제공해주는 역할이다.

JDBC Template를 사용하면 이 역할을 자동으로 처리해주고 DataSource만 제공해준다면 알아서 처리해주는 착한 녀석이다.

Repository를 생성하고 비즈니스 로직은 Service에서 처리하기 때문에 Service와 DataBase를 연결해줘야 하는데 그 역할을 DAO(Data Access Object)가 하게된다.

* ## Persistence Framework

Persistence Framework는 JDBC를 복잡한 작업없이 쉬운 작업으로 데이터베이스와 연동할 수 있게 해준다.

Persistence Framework의 종류로는 ORM, SQL Mapper 등이 있다.

#### ORM
ORM은 Object Relation Mapping이라는 뜻으로 객체와 관계형 DB의 데이터를 자동으로 매핑해주는 프레임워크이다.

* 설정된 객체 간의 관계를 자동으로 SQL을 생성해 실행해준다. (SQL 직접 작성 X)

* Spring에서는 보통 JPA를 사용한다.

* 개발자가 더욱 비즈니스 로직에 집중할 수 있도록 한다.

* 재사용 유지 보수 편리성이 좋다.

위와 같은 장점들이 있다.

보통 Hibernate를 사용한다.

#### SQL Mapper
SQL Mapper는 직접 Query를 작성하고 Query의 결과를 객체의 형태로 나타낼 수 있도록 지원하는 프레임워크이다.

Mybatis와 ibatis 등이 포함된다.

Mybatis는 XML파일에 Sql문을 저장하고 @Mapper를 사용해 Sql을 호출할 수 있다.

> ### SQL Mapper보다는 ORM이 훨씬 간편하고 쉬워서 ORM을 더 선호하는 것 같다.

