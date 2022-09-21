# extends와 implements

## extends (상속)

extends는 상속을 해주는 키워드이다.
부모 클래스의 메서드와 변수를 자식 클래스에서도 사용할 수 있도록 해준다.

class는 class만 interface는 interface만 extends가 가능하다.

## implements (상속, 구현)

implements는 부모객체에 메서드나 변수의 선언만 해두고
자식 클래스에서 부모객체의 메서드와 변수를 구현, 재정의가 가능하게 해주는 키워드이다.

extends와 다른 점은 class에 interface의 상속이 가능하고 
implements는 부모객체의 메서드와 변수를 전부 Override로 구현해줘야 한다는 것이 다르다.
extends는 다중 상속이 불가능하지만 implements는 다중 상속이 가능하다.
