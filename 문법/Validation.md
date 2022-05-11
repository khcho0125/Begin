# Validation 

#### 애플리케이션을 개발할 때 데이터 유효성 검사를 Validation 이라고 한다.

#### 클라이언트의 데이터가 모두 정상적인 방식으로 들어오는 것이 아니기 때문에 데이터 유효성 검사가 필요하다.

## spring boot Validation

### @Valid vs @Validated 

@Valid는 자바에서 지원해주는 어노테이션이고 @Validatied는 Spring에서 지원해주는 어노테이션이다.

@Validation은 @Valid의 기능을 포함하고, 속성 그룹을 만들어 유효성 검사를 가능하게 한다.

##### @Valid 예시
```java
    @RequestMapping(value = "/signup", method = RequestMethod.POST)
    public ResponseEntity<User> signup(@Valid @ModelAttribute SignupDto signupDto) {
        return ResponseEntity.ok(userService.signup(signupDto));
    }
```

##### @Validated 예시
```java
public class ValidationGroups {
    public interface group1 {};
    public interface group2 {};
}

public class Signup {
    @NotEmpty(groups = {ValidationGroups.group1.class})
    private String account;
    @NotEmpty(groups = {ValidationGroups.group2.class})
    private String password;
}
```
```java
    @RequestMapping(value = "/signup", method = RequestMethod.POST)
    public ResponseEntity<User> signup(@Validated(ValidationGroups.groups1.class) @ModelAttribute SignupDto signupDto) {
        return ResponseEntity.ok(userService.signup(signupDto));
    } 
```

#### BindingResult

오류가 발생해도 오류 정보 개체를 BindingResult가 담은 뒤 컨트롤러가 호출된다. ( Rest Api 라면 상관 X )

### 유효성 검사 어노테이션

#### 1. Blank 종류

@Null : null만 허용한다.

@NotNull : null을 허용하지 않는다. ("", " " 허용)

@NotEmpty : null, ""를 허용하지 않는다. (" " 허용)

@NotBlank : null, "", " "를 허용하지 않는다.

#### 2. Pattern 종류

@Email : 이메일 형식을 검사한다

@Pattern : 정규식을 검사할 때 사용한다.

#### 3. Size 종류

@Size : 길이를 제한할 때 사용한다.

@Min : 최소 값을 제한한다.

@Max : 최대 값을 제한한다.

@DecimalMin : 지정 값 이하인지 검사한다.

@DecimalMax : 지정 값 이상인지 검사한다.

#### 3. Value 종류

@Positive : 값을 양수로 제한한다.

@Negative : 값을 음수로 제한한다.

@PositiveOrZero : 값을 양수와 0으로 제한한다.

@NegativeOrZero : 값을 음수와 0으로 제한한다.

#### Date 종류

@Past : 날짜가 과거인지 검사한다.

@Future : 날짜가 미래인지 검사한다.

@PastOrPresent : 날짜가 과거 or 현재인지 검사한다.

@FutureOrPresent : 날짜가 미래 or 현재인지 검사한다.

#### Boolean 종류

@AssertTrue : True인지 검사한다.

@AssertFalse : False인지 검사한다.
