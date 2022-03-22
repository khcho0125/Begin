# Exception

Exception 처리 방법을 알아보겠다.

## 필요한 어노테이션

#### @ExceptionHandler

Exception이 발생하면 이벤트가 실행된다.

#### @ RestControllerAdvice

RestController에서 발생하는 모든 Exception을 처리한다.

## ErrorCode

ErrorCode를 작성해준다

```java
@Getter
@AllArgsConstructor
public enum ErrorCode {

    INVALID_TOKEN(401, "Invalid Token"),

    EMAIL_ALREADY_EXISTS(409, "Email Already Exists"),
    USERNAME_ALREADY_EXISTS(409, "UserName Already Exists"),
                ⁝
    private int status;
    private String message;
}
```
ErrorResponse를 작성해준다.

```java
@Getter
@Setter
public class ErrorResponse {
    private int status;
    private String message;
    private String code;

    public ErrorResponse(ErrorCode errorCode){
        this.status = errorCode.getStatus();
        this.message = errorCode.getMessage();
        this.code = errorCode.getErrorCode();
    }
}
```

RuntimeException을 상속받는 CustomException을 생성

```java
@Getter
public class CustomException extends RuntimeException {

    private ErrorCode errorCode;

    public CustomException(ErrorCode errorCode) {
        super(errorCode.getMessage());
        this.errorCode = errorCode;
    }
}
```

아까 말했던 @RestControllerAdvice와 @ExceptionHandler로 ExceptionHandler를 작성해준다.

```java
@Slf4j
@RestControllerAdvice
public class CustomErrorHandler {

    @ExceptionHandler(CustomException.class)
    public ResponseEntity<ErrorResponse> handleFCMException(CustomException e) {
        ErrorCode errorCode = e.getErrorCode();
        log.error("Error : {}\nlesson : {}", errorCode.getStatus(), errorCode.getMessage());
        ErrorResponse response = new ErrorResponse(errorCode);
        return new ResponseEntity<>(response, HttpStatus.valueOf(errorCode.getStatus()));
    }
}
```
위는 예시이다.

@ExceptionHandler(지정Exception.class) 로 특정 Exception 처리도 가능하다.

