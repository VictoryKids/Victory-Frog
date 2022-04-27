# 스프리이 사랑한 디자인 패턴

## 어댑터 패턴(Adapter Pattern)
* 서로 다른 두 인터페이스 사이애 통신이 가능하게 한다.
* OCP를 활용한 패턴
```
class ServiceA {
    void runServiceA(){...}
}

class ServiceB {
    void runServiceB(){...}
}

class AdapterServiceA {
    ServiceA sa1 = new ServiceA();
    
    void runService() {
        sa1.runServiceA();
    }
}

class AdapterServiceB {
    ServiceB sb1 = new ServiceB();
    
    void runService() {
        sb1.runServiceA();
    }
}

class ClientWithAdapter {
    public static void main(String[] args) {
        AdapterServiceA asa1 = new AdapterServiceA();
        AdapterServiceB asb1 = new AdapterServiceB();
        
        asa1.runService();
        asb1.runService();
    }
}
```
* 위의 코드를 통해서 확인할 수 있지만 ```runServiceA```, ```runServiceB```가 ```runService```라는 메서드로 통일되었다.
  * ServiceA와 ServiceB의 코드 변경 어댑터 클래스에 연향을 주지 사용자 측에 주지 않는다.


## 프록시 패턴(Proxy Pattern)
```
public interface IService {
    String runSomething();
}

public class Service implements IService {
    public String runSomething() {
        return "GOOD!";
    }
}

public clss ProxyService implements IService {
    IService service;
    
    public String runSomething() {
        System.out.println("START"); // 별도의 로직
        
        service = new Service();
        return service.runSomething();
    }
}

class ClientWithProxy {
    public static void main(String[] args) {
        IService proxyService = new ProxyService();
        String ret = proxyService.runSomething(); 
    }
}
```
* 대리자는 실제 서비스와 같은 이름의 메서드를 구현한다.(인터페이스 사용)
* 대리자는 실제 서비스에 대한 참조 변수를 갖는다.
* 대리자는 실제 서비스와 같은 이름을 가진 메서드 호출하고 그 값을 클라이언트에 반환한다.
* 대리자는 호출 전후에 별도의 로직을 수행할 수 있다.
* **프록시 패턴은 반환 값의 가감을 목적으로 하지 않는다.**
  * 제어의 흐름 변경
  * 다른 로직 수행
* **개방 폐쇄 원칙**과 **의존 역전 원칙**이 적용
  * 인터페이스를 수정 전파를 막는다 -> OCP
  * 의존 역전 원칙 -> 사용자는 추상적인 인터페이스를 사용(의존)