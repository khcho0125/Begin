# Lombok 정리

* ## @Getter

객체 변수의 get필드명() 함수를 생성

* ## @Setter 

객체 변수의 set필드명() 함수를 생성

* ## @AllArgsCounstrctor

필드 값을 파라미터로 받는 생성자를 생성

* ## @NoArgsCounstrctor

디폴트 생성자를 생성

* ## @RequireArgsCounstrctor

final이나 @NonNull이 붙은 변수를 파라미터로 받는 생성자를 생성

* ## ToString

ToString() 함수를 생성, exclude = "필드 변수 명" 를 사용하면 필드 변수 명을 가진 변수를 제외하고 ToString() 함수 생성

* ## EqualsAndHashCode

equals() 함수와 HashCode() 함수를 생성해준다.

#### callSuper = true로 설정하면 부모 클래스 필드까지 비교한다.

* ## Data

Data는 @RequireArgsCounstrctor, @EqualsAndHashCode, @Getter, @Setter, @ToString 을 전부 붙여준다.
