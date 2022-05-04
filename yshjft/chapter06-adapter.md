# Adapter
## Adapter 이란?
인터페이스가 서로 다른 객체들이 같은 형식 아래 작동할 수 있도록 해주는 패턴

## 예시) JDBC
다양한 데이터베이스 시스템을 공통적인 인터페이스인 JDBC를 통하여 조작할 수 있다.

## 구현
* **Client**   
  호환되지 않는 외부 객체를 사용하려는 객체

* **Adaptee**  
  호환되지 않는 외부 객체

* **Target Interface**
  Adapter가 구현하는 인터페이스로서 Client는 Target Interface를 통해 Adaptee를 사용한다.

* **Adapter**
  Client와 Adaptee를 연결해주는 역할을 수행한다.

|객체 어댑터|클래스 어댑터|
|:---:|:---:|
|![](https://images.velog.io/images/yshjft/post/222c371e-6fb0-4249-a953-096aee69d0b3/image.png)|![](https://images.velog.io/images/yshjft/post/62cf41ad-9f11-4933-aab5-1d61af456360/image.png)|

### 객체 어댑터(Object Adapter)
```
public class Client {
    private Target target;

    public Client(Target target) {
        this.target = target;
    }

    public void request() {
        target.operation();
    }
}

public interface Target {
    void operation();
}

public class ObjectAdapter implements Target {
    private Adaptee adaptee;

    public ObjectAdapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void operation() {
        adaptee.specificOperation();
    }
}

public class Adaptee {
    public void specificOperation() {
        System.out.println("hi!");
    }
}

public class Main {
    public static void main(String[] args) {
        Client client = new Client(new ObjectAdapter(new Adaptee()));
        client.request();
    }
}
```
* 합성(구성)을 이용
* 매번 Adaptee를 만들어야 한다.

### 클래스 어댑터(Class Adapter)
```
public class Client {
    private Target target;

    public Client(Target target) {
        this.target = target;
    }

    public void request() {
        target.operation();
    }
}

public interface Target {
    void operation();
}

public class Adaptee {
    public void specificOperation() {
        System.out.println("hi!");
    }
}

public class ClassAdapter extends Adaptee implements Target  {
    @Override
    public void operation() {
        this.specificOperation();
    }
}

public class Main {
    public static void main(String[] args) {
        Client client = new Client(new ClassAdapter());
        client.request();
    }
}
```
* 상속을 이용(다중 상속을 이용)

## 장단점
### 장점 😁
#### SRP 준수
변환 코드와 비즈니스 로직을 분리하여 하나의 책임만 가지도록 한다.

#### OCP 준수
Adapter를 사용함으로서 기존 코드를 변경하지 않아도 된다.

### 단점 🤨
Adapter를 이용한 변환 과정은 코드의 복잡성을 높일 수 있다. 떄로는 Adapter를 사용하지 않고 호환될 수 있도록 코드 수정하는 것이 더 좋은 방법일 수 도 있다.
