## IOC

> IOC(Inversion of Countrol), 제어의 역전이란 객체의 생성과 관리, 모든 제어권이 전부 바뀌었다는 뜻이다.
> 제어권이 개발자에서 스프링으로 넘어갔다는 것이다.
> 
> 객체간의 결합도를 줄이고 유연한 코드를 작성하게해 코드의 가독성을 높이고 (유연성 상승) 중복, 유지보수를 낮추는 역할을 한다.

### DL

Bean에 접근하기 위해 컨테이너의 API를 이용하여 주입하는 것을 말한다.

## DI

의존성 주입으로 의존관계를 자동으로 연결해 주는 것을 말한다.

## IOC 컨테이너

IOC에서 객체의 제어를 맡은 것이 IOC 컨테이너다.

IOC 컨테이너 == BeanFactory

BeanFactory를 상속받은 ApplicationContext에 모든 Bean이 들어있다.

## Bean

IOC 컨테이너 안의 객체를 Bean이라고 한다.

Bean을 등록하는 방법은 @Component, @Bean 어노테이션으로 등록할 수 있다.
