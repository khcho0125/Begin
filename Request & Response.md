# Request & Response

## HTTP

Http는 클라이언트와 서버 간의 요청과 응답을 처리하기 위한 규약이다.

Http Requset은 세 가지로 구분되는데

1. Start Line
2. Headers
3. Body

## Request

### Start Line

Http의 시작 줄이다. 여기도 세 부분으로 나눠진다.

1. Http Method

Http의 Action을 보여준다. Method의 종류로 GET, POST, PUT, PATCH, DELETE 등이 있다.

2. Http version

말 그대로 Http의 version을 담는다.
 
3. Request Target

해당 Request가 전송되는 Url을 말한다.

### Headers

해당 Request의 추가 정보를 가지고 있다.

Key - Value 형태로 정보를 전송한다.

자주 사용되는 header 정보는 HOST, Accept, Content-Type 등이 있다.

### Body

해당 Request의 실제 메시지를 담는 곳이다.

## Response 

Response도 마찬가지로 Start Line, Headers, Body로 이루어져있다.

### Start Line

1. Http version

Request와 같다.

2. Status Code

응답의 상태를 나타내는 코드를 말한다.

OK는 200, Server Error는 500

3. Status Text

응답의 상태를 요약해서 설명해준다.

Not Found, Unauthorize 등

### Headers

Request 같지만 Response 에서만 사용되는 header 값이 있다.

### Body

Request와 동일하다.

## ResponseEntity

Spring에서 Http 응답을 빠르게 만들기 위해 사용하는 객체이다.

```java
public class ResponseEntity extends HttpEntity {

  private final Object status;
}

public class HttpEntity<T> {
    public static final HttpEntity<?> EMPTY = new HttpEntity<>();
  
  
    private final HttpHeaders headers;
  
    @Nullable
    private final T body;
}
```

ResponseEntity는 HttpEntity를 상속하는 구조로 구현이 되어있습니다.

HttpEntity에서는 body가 제네릭 타입으로 되어있어 타입을 지정해줄 수 있다.

ResponseEntity에 status, header, body를 지정해서 응답을 만들 수 있다.
