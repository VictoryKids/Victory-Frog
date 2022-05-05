  # 스프링이 사랑한 디자인 패턴
  
| 요리 	| 객체 지향 프로그래밍(OOP) 	|
|---	|---	|
| 요리도구 	| 4대 원칙(캡! 상추다) 	| 
| 요리도구 사용법 	| 설계 원칙(SOLID) 	|
| 레시피 	| 디자인 패턴 	|

## 어댑터 패턴
- 합성, 즉 `객체를 속성으로 만들어서 참조`하는 디자인 패턴
- 호출당하는 쪽의 메서드를
- 호출하는 쪽의 코드에 대응하도록
- 중간에 변환기를 통해 호출하는 패턴


``` java
public class AdapterServiceA {
  ServiceA serviceA = new ServiceA();
  
  void runService() {
    serviceA.runA();
  }
}

public class AdapterServiceB {
  ServiceB serviceB = new ServiceB();
  
  void runService() {
    serviceB.runB();
  }
}

// other class..
public static void main() {
  AdapterServiceA a = new AdapterServiceA();
  AdapterServiceB b = new AdapterServiceB();
  
  a.runService();
  b.runService();
}

```

- 각 서비스에서 같은 역할을 하는 이름이 다른 runA, runB 메서드를
- 어뎁터를 이용해 같은 이름으로 호출할 수 있다.

## 프록시 패턴

`프록시 패턴은 제어 흐름을 조정하기 위한 목적으로 중간에 대리자를 두는 패턴`

- 프록시는 대리자, 대변인이라는 뜻을 가진다.
- 대리자는 실제 서비스와 같은 이름의 메서드를 구현한다.
  - 이때 인터페이스를 사용한다.
- 대리자는 실제 서비스에 대한 참조 변수를 갖는다.(합성)
  - `Iservice service`
- 대리자는 실제 서비스의 같은 이름을 가진 메서드를 호출하고 그 값을 클라이언트에게 돌려준다.
- 대리자는 실제 서비스의 메서드 호출 전후에 별도의 로직을 수행할 수도 있다.

``` java

public Interface IService {
  String runSomething();
}

public class Service Implements IService {
  public String runSomething() {
    return "this is Real Service";
  }
}

public class Proxy implements IService {
  IService service;
  
  public String runSomething() {
    System.out.println("");
    
    service = new Service();
    return service.runSomething();
  }
}
```
## 데코레이션 패턴
- 클라이언트가 받는 반환값에 장식을 더한다.
- `메서드 호출의 반환값에 변화를 주기 위해` 중간에 장식자를 두는 패턴

## 싱글턴 패턴
- 하나의 인스턴스만 만들어 사용하는 패턴

## 템플릿 메서드 패턴
- 상위 클래스의 견본 메서드에서 하위 클래스가 오버로딩한 메서드를 호출하는 패턴.

``` java

public abstract class Animal {

  // 템플릿 메서드
  public void playWithOwner() {
    System.out.println("산책하자");
    play(); // 하위 클래스가 오버로딩한 메서드를 호출하게 된다.
    runSomething();
  }
  
  abstract void play();
  
  // Hook(갈고리) 메서드, 선택적으로 오버라이딩할 수 있는 메서드
  void runSomething() {
    System.out.println("꼬리를 흔든다.);
  }

}

```

## 팩터리 메서드 패턴

`오버라이드된 메서드가 객체를 반환하는 패턴`

- 팩터리는 공장.
- 공장은 물건을 생산, 객체 지향에서 공장은 객체를 생성.
- 결국 객체를 생성 반환하는 메서드를 의미한다.

``` java

public abstracit class Shop {
  // 추상 팩터리 메서드 !
  abstract Product getProduct();
}

// 팩터리 메서드가 반환하는 객체의 상위 클래스
public abstract class Product {
  abstract void identify();
}

// 반환객체 정의
public class Robot extends Product {
  public void identify() {
    System.out.println("Robot");
  }
}

public class ToyShop extends Shop {
  // 추상 팩터리 메서드 재정의
  @Override
  Product getProduct() {
    return new Robot();
  }
}

```

## 전략 패턴
- 디자인 패턴의 꽃
- 반드시 기억해야 
- 디자인 패턴의 꽃할 세가지 요소
  - 전략 메서드를 가진 전략 객체
  - 전략 객체르 사용하는 컨텍스트(전략 객체의 사용자/소비자)
  - 전략 객체를 생성해 컨텍스트에 주입하는 클라이언트(제3자, 전략 객체의 공급자)

## 템플릿 콜백 패턴
- 전략 패턴의 변형으로, 스프링 3대 프로그래밍 모델 중 하나인 DI(의존성 주입)에서 사용하는 특별한 형태의 전략 패턴.
- 전략 패턴과 모든 것이 동일하지만, 전략을 `익명 내부 클래스`로 정의해서 사용한다.
