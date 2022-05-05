# Proxy 패턴

- 프록시 패턴은 `제어 흐름을 제어`하기 위한 목적으로 중간에 대리자(Proxy)를 두는 패턴이다.
  - 중요한점은 흐름제어만 할 뿐 `결과를 조작하거나 변경시키면 안된다.`
    - 반환 값을 변경할 수 있는 패턴은 `데코레이터 패턴`

# 특징
- 대리자는 실제 서비스와 같은 이름의 메서드를 구현한다.
  - 이때 인터페이스를 사용한다.
- 대리자는 실제 서비스에 대한 참조 변수를 갖는다(합성)
- 대리자는 `실제 서비스의 같은 이름을 가진 메서드를 호출`하고 그 값을 클라이언트에게 돌려준다.
- 대리자는 실제 서비스의 메서드 호출 전후에 별도의 로직을 수행할 수 있다.

![image](https://user-images.githubusercontent.com/26343023/166925735-4a9f032b-96d0-4002-b089-d50a5cb1e357.png)


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
    System.out.println("Before Somthing ...");
    
    service = new Service();
    return service.runSomething();
    
    System.out.println("After Somthing ...");
  }
}

```


## 프록시 패턴이 적용될 수 있는 경우

- 객체의 초기화를 지연시킬 수 있다. `가상 프록시`
  - 시스템 리소스 낭비를 예방할 수 있다.
  - Customer 객체에 List<Product> 정보를 가지고 있다고 한다면, 프록시 패턴을 통해 특정 고객이 구매한 상품리스트를 사용하는 시점까지 로딩을 지연시킬 수 있다.
  
- 액세스 제어 `보호 프록시`
  - 자격이 증면된 경우에만 서비스가 호출되도록 할 수 있다.
  
- 로깅 요청 `로깅 프록시`
  - 서비스에 전달하기 전에 로그를 남길 수 있다.
  
- 캐시를 관리할 수 있다. `캐싱 프록시`
  - 항상 동일한 결과를 생성하는 반복 요청에 대해 캐싱을 구현할 수 있다. 프록시는 요청의 매개변수를 캐시 키로 사용할 수 있다.
  
## 참조
  
- [guru](https://refactoring.guru/design-patterns/proxy)
- [개구리책](http://www.yes24.com/Product/Goods/17350624)
