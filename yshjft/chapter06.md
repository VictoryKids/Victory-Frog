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
  * 의존 역전 원칙 -> 사용자는 추상적인 인터페이스를 사용 -> DIP

## 데코레이터 패턴(Decorator Pattern)
* 프록시 패턴과 거의 동일하다
  * **단 데코레이터 패턴은 반환값에 가감을 한다.** 
```
public interface IService {
    String runSomething();
}

public class Service implements IService {
    public String runSomething() {
        return "GOOD!";
    }
}

public clss Decorator implements IService {
    IService service;
    
    public String runSomething() {
        service = new Service();
        return "very "+service.runSomething(); // 반환 값에 가감
    }
}

class ClientWithProxy {
    public static void main(String[] args) {
        IService decorator = new Decorator();
        String ret = decorator.runSomething(); 
    }
}
```
* OCP와 DIP가 적용된 패턴이다.

## 싱글턴 패턴(Singleton Pattern)
```
public class Singleton {
	static Singleton singletonObject;
	
	private Singleton(){}
	
	public static Singleton getInstance() {
		if(singletonObject == null) {
		  singletonObject = new Singleton(); 
		}
		
		return singletonObject;
	}
}
```
* 인스턴스를 다수 만들 경우 불필요한 자원을 사용하게 되는 경우 또는 다수의 인스턴스를 만들었을 때 예상치 못한 결과를 나올 가능성이 있을 때 사용
* 싱글톤 패턴의 특징
  * **private 생성자 갖는다.**
  * **단일 객체 참조 변수를 정적 속성으로 갖는다.**
  * **딘일 객체를 반환하는 정적 메서드를 갖는다.**
  * **쓰기 가능한 속성을 갖지 않는 것이 정석이다.**

## 텡플릿 메서드 패턴(Template Method Pattern)
* 상속을 통해 동일한 부분은 상위 클래스
* 달라지는 부부만 하위 클래스
```
public abstract class Animal {
  // 템플릿 메서드(동일한 부분)
  public void playWithOwner() {
    System.out.println("귀염둥이 이리온...");
    play();
    runSometing();
    System.out.println("잘했어");
  }
  
  // 추상 메서드
  abstract void play();
  
  // Hook 메서드
  void runSomething() {
    System.out.println("꼬리 살랑 살랑");
  }
}

public class Dog extends Animal {
  // 추상 메서드 오버라이딩
  @Override
  void play() {
    System.out.println("멍! 멍!");
  }
  
  // Hook 메서드 오버라이딩
  @Override
  void runSomething() {
    System.out.println("멍! 멍! 꼬리 살랑 살랑")
  }
}

public class Cat extends Animal {
  // 추상 메서드 오버라이딩
  @Override
  void play() {
    System.out.println("야옹! 야옹!");
  }
  
  // Hook 메서드 오버라이딩
  @Override
  void runSomething() {
    System.out.println("야옹! 야옹! 꼬리 살랑 살랑")
  }
}
```
* 공통적인 로직을 수행하는 템플릿 메서드
* 오버라이딩을 강제하는 추상 메서드
* 선택적으로 오버라이딩을 할수 있는 훅 메서드 
* 의존 역전의 법칙(DIP)을 활용 


