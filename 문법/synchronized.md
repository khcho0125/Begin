## synchronized

멀티 스레드를 사용하면 프로그램에서 좋은 성능을 낼 수 있지만

멀티 스레드 환경에서 고려해야할 점인 스레드 간 동기화라는 문제를 해결해야한다.

스레드 간 공유할 수 있는 데이터를 스레드 간 동기화가 되지 않은 상태에서 실행한다면 데이터의 안정성과 신뢰성이 보장되지 않는다.

그렇기 때문에 synchronized 키워드를 사용한다.

synchronized 키워드는 스레드 간의 데이터에 접근을 막는 기능을 가지고 있습니다.

스레드가 synchronized 데이터에 접근하면 다른 스레드가 접근할 수 없도록 block을 합니다.

하지만 오히려 synchronized 키워드를 남발하게 되면 block과 unblock 처리가 많아져 프로그램 성능 저하를 일으킬 수 있습니다.

## 동기화 블록

```java
public void add(int value){

    synchronized(this){
       this.count += value;   
    }
}
```

특정 부분만 동기화하는 편이 효율적인 경우가 있다.

이럴때는 메소드안에 동기화 블럭을 사용하여 따로 작성할 수 있다.

예제 동기화 블럭의 괄호에 this가 사용되었는데 이는 add() 메서드가 호출된 객체를 의미한다.

이렇게 동기화 블럭 안에 전달된 객체를 모니터 객체라고 한다.

```java
public class Block {

    public void test1(String str) {
        synchronized(this) {
            log.info("동기화 블럭 test1 : {}", str);
        }
    }

    public synchronized void test2(String str) {
        log.info("동기화 블럭 test2 : {}", str);
    }
}
```

한 스레드는 한 시점에서 두 동기화된 코드 중 하나만 실행할 수 있다.

## 스태틱 메서드 동기화 블럭

```java
public class Block {

    public static void test1(String str) {
        synchronized(this) {
            log.info("동기화 블럭 test1 : {}", str);
        }
    }

    public static synchronized void test2(String str) {
        log.info("동기화 블럭 test2 : {}", str);
    }
}
```

같은 시점에서 오직 한 스레드만 두 메서드 중 어느 쪽이든 실행 가능하다.

test1메서드에서 synchronized 괄호에 Block class가 아닌 다른 객체를 전달한다면 스레드는 동시에 각 메서드를 실행할 수 있다.
