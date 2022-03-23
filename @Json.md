## @JsonProperty, @JsonNaming

단어 표현은 카멜, 스네이크 등 많은 종류가 있다.

스네이크는 언더바를 사용하여 단어를 구분하는 방법이다. ex) spring_boot

카멜은 단어의 시작을 대문자로 만들어 구분하는 방법이다. ex) springBoot

JSON은 스네이크 방식을 주로 사용하고 java는 카멜 방법을 주로 사용하는데 데이터를 원활하게 읽어오기 위해서는 카멜 방식으로도 파싱을 해야한다.

### @JsonProperty & @JsonNaming

```java
public class RequestDTO {

    private String email;
    private String password;
    private String phoneNumber;
	
    .....
    
}
```
Request로 보내는 Json이 다음과 같다면 전화번호는 null이 될 것이다.
```javaScript
{
  "account" : "user01",
  "email" : "abc123@gmail.com",
  "address" : "Korea",
  "password" : "adcd",
  "phone_number" : "010-1234-5678"
}
```

그렇기에 카멜 형식만 허용하는게 아니라 스네이크 형식도 허용되도록 하는 것이다.

이럴 때 @JsonProperty와 @JsonNaming을 사용할 수 있다.

ex @JsonPreperty)
```java
public class RequestDTO {

    private String email;
    private String password;
    
    @JsonProper("phone_number")
    private String phoneNumber;
	
    .....
    
}
```

ex @JsonNaming)
```java
@JsonNaming(value = PrepretyNamingStrategy.SnakeCaseStrategy.class)
public class RequestDTO {

    private String email;
    private String password;
    private String phoneNumber;
	
    .....
    
}
```

@JsonNaming은 class 전체의 변수들의 스네이크 형식을 허용한다.
