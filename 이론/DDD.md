# DDD

DDD(Domain-Driven Design)는 도메인 주도 설계라고 합니다.
도메인 패턴을 중심에 놓고 설계하는 방식을 말합니다.

* ### Domain 이란?

  DDD에서 Domain은 비즈니스를 담당하는 영역입니다.

  각각의 어플리케이션은 비즈니스 Domain에 따라 분리되어 설계될 수 있습니다

---

DDD는 Strategic Design과 Tactical Design으로 나눌 수 있습니다.

* ### Strategic Design

  Strategic Design은 개념 설계를 의미합니다.

  비즈니스 도메인(Business Domain)의 상황(Context)에 맞게 설계하자는 컨셉입니다.
  전략적인 설계를 위해 비즈니스 도메인의 (Context)를 Event storming으로 공유하고 , 비즈니스 목적별로 서비스들을 그룹핑합니다.

  * ##### Bounded Context

    비즈니스 도메인의 사용자, 프로세스, 정책 등을 고유한 비즈니스 목적별로 그룹핑한 것입니다.

  * ##### Event storming

    비즈니스 도메인 내에서 일어나는 것들을 찾아서 Bounded Context를 식별하는 방법론입니다.

  * ##### Ubiguitous Language

    개발자, 디자이너 등 참여자들이 동일한 의미로 이해하는 언어를 말합니다.

    Business Domain에서 정확한 커뮤니케이션을 위해 공통언어를 정의하고 사용해야 합니다.

---

* ### Tactical Design

  Tactical Design은 구체적 설계를 의미합니다.

  * ##### layered Architecture

    Tactical Design시 목적별 계층으로 나누어 설계하는 것을 말합니다.

    Layer의 종류는 Presentation, Service, Domain, Data Layer 등이 있습니다.

    * ##### Presentation Layer: UI Layer

    * ##### Service Layer: Domain Layer과 Data Layer간의 제어와 연결을 담당합니다.

    * ##### Domain Layer: Domain Object별로 Business Logic을 처리합니다.

    * ##### Data Layer: Database 간의 CRUD 처리를 합니다.

  * ##### Aggregate

    Entity들을 대표하는 추상화된 객체를 말합니다.

    Entity간의 직접적인 커뮤니케이션을 피하고 Aggregate간에만 커뮤니케이션 하도록 설계 합니다.
    Event Storming에서 정의한 Aggregate가 Tactical Design 설계시 그대로 이용됩니다.

