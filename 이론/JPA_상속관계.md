# JPA 상속관계 매핑

> ### @Inheritance( strategy = `InheritanceType` )
+ #### DB의 논리 모델을 물리 모델로 구현하는 방법이다.
#### default 값은 SINGLE_TABLE 이다.
 ```java
 public enum InheritanceType {
    SINGLE_TABLE, // 단일 테이블 전략
    
    TABLE_PER_CLASS, // 테이블 퍼 클래스 전략
    
    JOINED; // 조인 전략

    private InheritanceType() {
    }
}
 ```

> ### @DiscriminatorColumn( name = "DTYPE" )
+ #### 부모클래스에 선언한다.
+ #### 자식클래스를 구분하는 컬럼
#### default 값은 DTYPE 이다.

> ### @DiscriminatorValue( "Classname" )
+ #### 자식클래스에 선언한다.
+ #### 부모클래스의 DTYPE에 저장할 값을 지정한다.
#### 선언하지 않을경우 클래스 이름으로 지정된다.

---
+ > ## JOINED 관계
#### JOINED 전략에서는 @DiscriminatorColumn을 선언하지 않으면 DTYPE Column이 생성되지 않는다.
#### 되도록이면 넣어주는게 좋다.
  + #### Item
    ```java
    @Entity()
    @Inheritance(strategy = InheritanceType.JOINED)
    @DiscriminatorColumn
    public class Item {

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        public Long id;

        private String name;
        private int price;
    
    }
    ```
  + #### Movie
    ```java
    @Entity
    public class Movie extends Item {

        private String director;
        private String actor;
    }
    ```
  + #### Album
    ```java
    @Entity
    public class Album extends Item {

        private String artist;
    }
    ```
    **ex)**
    
    **TABLE Item**
      | dtype | id | name | price |
      |:-----:|:--:|:----:|:-----:|
      | movie |  1 | 기생충 | 39900 |
      | album |  2 | 게르니카 | 33000 |
      
      **TABLE movie**
      | id | actor | director |
      |:--:|:-----:|:--------:|
      | 1 | 송강호 | 봉준호 |
      
      **TABLE album**
      | id | artist |
      |:--:|:------:|
      | 2 | 피카소 | 
___
+ > ## SINGLE_TABLE 관계
#### 한 테이블에 전부 저장하는 단일 테이블 
#### INSERT, SELECT 쿼리가 한 번이다. 성능이 좋다.
#### 단일 테이블 전략은 @DiscriminatorColumn을 선언하지 않아도 DTYPE 칼럼이 생성된다.
#### *한 테이블에 모든 칼럼을 저장해 DTYPE 없이 판단할 수 없다.*
 + #### Item
   ```java
   @Entity
   @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
   @DiscriminatorColumn
   public class Item {

      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      public Long id;

      private String name;
      private int price;
   }
   ```
   + #### Movie, Album Table은 JOINED 전략과 같습니다.
   **ex)**
   
   **TABLE Item**
   
   |dtype|id|name|price|artist|actor|director|
   |:---:|:-:|:--:|:---:|:----:|:---:|:------:|
   |movie|1|기생충|39900| |송강호|봉준호|
   |album|2|게르니카|33000|피카소| | |
___
+ > ## TABLE_PER_CLASS 관계
#### 부모 클래스의 칼럼을 자식 클래스의 칼럼으로 내린다.
#### name, price 칼럼이 중복됨을 허용한다.
#### Item Entity는 사용하지 않으므로 abstract 클래스로 변경한다. @DiscriminatorColumn도 필요 X
#### @GeneratedValue(strategy = GenerationType.IDENTITY)를 사용하면 에러가 생긴다.
 + #### Item
   ```java
   @Entity
   @Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
   public abstract class Item { // abstract로 테이블 생성 X

      @Id
      @GeneratedValue(strategy = GenerationType.AUTO) // or GenerationType.TABLE
      public Long id;

      private String name;
      private int price;
   }
   ```
   + #### Movie, Album Table은 JOINED 전략과 같습니다.
   
   **ex)**
   
   **TABLE movie**
   
   |id|name|price|actor|director|
   |:-:|:-:|:-:|:-:|:-:|
   |1|기생충|39900|송강호|봉준호|
   
   
   **TABLE album**
   
   |id|name|price|artist|
   |:-:|:-:|:-:|:-:|
   |2|게르니카|33000|피카소|
   |3|기생충|15000|박소담|
   
___
> + ### JOINED 관계 전략
>  + #### 장점
>    + #### 외래키 참조 무결성 제약조건 활용이 가능하다.
>    + #### 저장공간이 효율적이다.
>  + #### 단점
>    + #### 데이터 조회시 조인을 많이해 성능이 떨어진다.
>    + #### 데이터 저장시 INSERT가 부모, 자식 2번 발생한다.

> + ### SINGER_TABLE 관계 전략
>  + #### 장점
>    + #### 조인을 하지 않으므로 일반적인 조회 성능이 뛰어나다.
>    + #### 조회 쿼리가 단순하다.
>  + #### 단점
>    + #### 자식 Entity가 매핑한 컬럼을 NULL이 허용되게 해야한다.
>    + #### 테이블이 커질 수 있다.
>    + #### JOINED 전략보다 성능이 오히려 느려질 수 있다.

> + ### TABLE_PER_CLASS 관계 전략
>   + #### 장점
>     + #### NOTNULL 제약조건을 부여할 수 있다.
>     + #### 자식클래스를 명확하게 알아볼 수 있다.
>   + #### 단점
>     + #### 여러 자식 테이블을 함께 조회해 성능이 느리다.
>     + #### 변경할 때 굉장히 비효율적이다.

 ## 결론 : JOINED 전략을 주로 쓰고 단일 테이블 전략은 상황에 따라 선택하자
 ### TABLE_PER_CLASS 관계 전략은 되도록이면 사용하지 말자...
