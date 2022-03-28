# 직렬화란?


## 직렬화 (<i>Serialize</i>)
<h3>
직렬화는 Object나 Data를 외부의 시스템이 사용할 수 있도록 byte 형태로 변환하는 기술이다.
</h3>

## 역직렬화 (<i>DeSerialize</i>)
<h3>
byte로 변환된 Object나 Data를 다시 원래의 Object나 Data로 변환하는 기술을 말한다.
<h3>

--- 

## 직렬화 방법 

```java
public class Member implements Serializable {
    private String name;
    private String email;
    private int age;

    public Member(String name, String email, Long age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    @Override
    public String toString() {
        return String.format("Member{name='%s', email='%s', age='%d'}", name, email, age);
    }
}
```

* 직렬화에는 여러 Format이 존재한다.
* 구조적인 데이터는 JSON 형태 등등

## 역직렬화 방법

직렬화 대상 객체는 동일한 SUID(<i>SerialVersionUID</i>)를 가지고 있어야 한다. 

* ObjectInputStream를 사용하여 역직렬화를 진행한다.

### 어디에 사용되나?

1. 서블릿 세션(<i>Servlet Session</i>)
2. 캐시
3. 원격 시스템 간 통신

### 직렬화의 문제점?

* #### 역직렬화 클래스 구조 변경

```java
public class Member implements Serializable {
    private String name;
    private String email;
    private Long age;
}
```
직렬화한 Data
```
rO0ABXNyABp3b293YWhhbi5ibG9nLmV4YW0xLk1lbWJlcgAAAAAAAAABAgAESQADYWdlSQAEYWdlMkwABWVtYWlsdAASTGphdmEvbGFuZy9TdHJpbmc7TAAEbmFtZXEAfgABeHAAAAAZAAAAAHQAFmRlbGl2ZXJ5a2ltQGJhZW1pbi5jb210AAnquYDrsLDrr7w=
```
클래스 구조 변경
```java
public class Member implements Serializable {
    private String name;
    private String email;
    private Long age;
    // 추가
    private String Authority;
}
```
직렬화한 Data를 역직렬화하면 <span style="color: #ffd33d">java.io.InvalidClassException</span>이 발생한다.

직렬화하는 시스템과 역직렬화를 하는 시스템이 다를 경우 발생하는 문제이다.

#### 해결방법
  
모델의 버전과 호환성 유지를 위해 SUID(<i>SerialVersionUID</i>)를 정의한다.
```java
private static final long SerialVersionUID = 1L;
```

#### Data Size 문제

일반적인 메모리기반의 캐시에서는 Data를 저장할 수 있는 용량의 한계가 있으므로 JSON 같이 경량화된 형태를 직렬화 하는 방법이 있다.
