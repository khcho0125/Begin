# Sealed Class

#### Java 17에서 Sealed Class가 정식으로 확정되었다.

#### Java 15부터 프리뷰로 추가되었다는데 처음 알게된 사실이다..

- ### Sealed Class

Sealed Class, Interface는 상속하거나 구현할 클래스를 지정해 두고 해당 클래스만 상속을 허용하는 키워드이다.

```java
public sealed interface Bread permits Hamburger, Sandwich {}

public final class Hamburger implements Bread {}
public non-sealed class Sandwich implements Bread {}
```

Sealed Class는 몇가지의 약속이 있는데

- 상속/구현하는 클래스는 final, non-sealed, sealed 중 하나로 선언되어야한다.
- Permitted SubClass들은 동일한 module 내에 속해야 하며 지정되지 않은 module 선언 시에는 같은 Package 내에 속해야 한다.
- Permitted SubClass는 Sealed Class를 직접 확장해야 한다.

이러한 제약의 목표는 코드를 작성하면서 어떤 Super Class의 Sub Class인지를 명확하게 인지할 수 있어야 한다를 목표로 한다.

- ### Final 

  final은 extends, implement가 완전히 금지된 상태이다.

  final은 sealed + permits와 같다고 볼 수 있다.

- ## 사용

클래스를 봉인할때 모든 하위클래스에 대해 명확하게 추론할 수 있다.

하위 클래스에 대해 추론하는 방법은 instanceof 검사를 	하는 것이다.

```java
if(bread instanceof Hamburger hamburger) {
  	return hamburger.getKcal();
} else if(bread instanceof Sandwich sandwich) {
  	return sandwich.getKcal();
} else {
  	throw new RuntimeException("instance Failed");
}
```

if - else 를 사용하면 비효율적이기 때문에

switch를 사용하여 나타내면 더욱 좋다.

```java
return switch (bread) {
    case Hamburger hamburger -> hamburger.getKcal();
    case Sandwich sandwich -> sandwich.getKcal();
    default -> throw new RuntimeException("instance Failed");
};
```

- ## Record

- record Class는 final 클래스라 상속할 수 없다.

- 각 필드는 private final 필드로 정의된다.

- 모든 필드를 초기화하는 RequireAllArgument 생성자가 생성된다.

- 각 필드의 getter는 get ~ () 형태가 아닌 필드명을 딴 getter가 생성된다. name(), age() 등등

