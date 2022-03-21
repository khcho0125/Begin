# Logging

로깅이란 시스템의 작동 정보, 로그를 기록하는 행위를 말한다.

## Logging Framework

### 1. Log4J

### 2. Java Logging API

### 3. tinylog

### 4. Logback

### 5. Apache Commons Logging (JCL)

### 6. SLF4J

1 ~ 4 까지는 로깅 프레임워크 타입으로 구분되지만, JCL와 SLF4J는 Wrapper로 구분된다.

Logging Wrapper는 그 자체로 사용이 불가능하고 다른 로깅 프레임워크가 필요하다.

JCL과 SLF4J는 퍼사드 패턴이다.

## SLF4J(Simple Logging Facade For Java)

여러 로깅 구현체들을 추상화한 인터페이스이다.

단독으로 사용할 수 없고 다른 로깅 프레임워크가 필요하다.

ex) slf4j + logback
ex) slf4j + log4j

## Log4j2

Log4j2는 가장 최근에 출시된 로깅 프레임워크이다. 출시순서 ( Log4j -> Logback -> Log4j2 )

람다 표현식과 사용자 정의 로그 레벨도 지원해준다.
