# 스프링 삼각형과 설정 정보

- 스프링을 이해하는 데는 POJO(Plain Old Java Object)를 기반으로 스프링 삼각형이라는 애칭을 가진 IoC/DI, AOP, PSA라고 하는 스프링의 3대 프로그래밍 모델에 대한 이해가 필수다.

## IoC/DI - 제어의 역전/의존성 주입

### 의존성 ?
- 의존성이란 new다.
- 결론적으로 `전체가 부분에 의존`한다.
- 객체(전체)와 의존되는 객체(부분) 사이에 집합 관계(Aggregation)와 구성 관계(Composition)로 구분할 수도 있다.
  - 집합 관계: 부분이 전체와 다른 생명 주기를 가질 수 있다.
    - ex) 집 vs 냉장고
  - 구성 관계: 부분은 전체와 같은 생명 주기를 갖는다.
    - ex) 사람 vs 심장


### 주입이란?
- 주입이란 말은 외부에서라는 뜻을 내포한 단어다.

## AOP
- DI가 의존성 주입이라면, AOP는 로직 주입이라고 볼 수 있다.
  - 로깅, 보안, 트랜잭션 등의 공통된 기능(횡단 관심사)를 메서드 안에 주입할 수 있다.
  - Around, Before, After, AfterReturning, AfterThrowing
    - Around: 메서드 전 구역
    - Before: 메서드 시작 전
    - After: 메서드 종료 후
    - AfterReturning: 메서드 정상 종류 후
    - AfterThrowing: 메서드에서 예외가 발생하면서 종료된 후
- Spring AOP는 인터페이스(interface) 기반이다.
- Spring AOP는 프록시(proxy) 기반이다.
  - 재밌는 사실은 호출하는쪽에서나 호출당하는쪽, 그 누구도 프록시의 존재조차 모른다.
  - 오직 스프링 프레임워크만 프록시의 존재를 안다.
- Spring AOP는 런타임(runtime) 기반이다.

### AOP 용어
- Pointcut: 자르는 점
  - 횡단 관심사를 적용할 타깃 메서드를 선택하는 지시자(메서드 선택 필터)
  - `타깃 클래스의 타깃 메서드 지정자`
  - 메소드 선정 알고리즘이라고도 한다.
  - JoinPoint의 부분 집합이다.
  - 어디에(Where)
- JoinPoint: 결합점
  - Pointcut의 후보가 되는 모든 메서드들이 JoinPoint, 즉 Aspect 적용이 가능한 지점이 된다.
  - 따라서 `Aspect 적용이 가능한 모든 지점을 말한다.`
  - 스프링 프레임워크가 관리하는 빈의 모든 메서드에 해당한다.
    - 광의의 JoinPoint란 Acpect 적용이 가능한 모든 지점
    - 협의의 JoinPoint란 호출된 객체의 메서드
- Advice: 조언, 충고
  - pointcut에 적용할 로직, 즉 메서드를 의미하는데 여기에 더해서 언제라는 개념까지 포함하고 있다.
  - 타깃 객체의 타깃 메서드에 적용될 부가 기능
  - 언제(When), 무엇을(What)
- Aspect: 관점, 측면, 양상
  - 여러 개의 Advice와 여러 개의 PointCut의 결합체를 의미하는 용어다.
  - Aspect = Advice들 + PointCut들
  - When + Where + What
- Advisor: 조언자, 고문
  - Advisor = 한 개의 Advice + 한 개의 Pointcut
  - 스프링 AOP에서만 사용하는 용어이며 다른 AOP프레임워크에서는 사용하지 않는다.
  - 스프링 버전이 올라가면서 이제는 쓰지 말라고 권고하는 기능이기도 하다.
    - 스프링이 발전해오면서 다수의 Advice와 하나의 pointcut을 다양하게 조합해서 사용할 수 있는 방법, 즉 Aspect가 나왔기 때문이다.
