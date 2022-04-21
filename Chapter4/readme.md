# 자바가 확장한 객체 지향

## 목차
- [01. abstract 키워드 - 추상 메서드와 추상 클래스](#01.-abstract-키워드---추상-메서드와-추상-클래스)
- [02. 생성자](#02.-생성자)
- [03. 클래스 생성 시의 실행 블록, static 블록](#03.-클래스-생성-시의-실행-블록,-static-블록)
- [04. final 키워드](#04.-final-키워드)
- [05. instanceof 연산자](#05.-instanceof-연산자)
- [06. package 키워드](#06.-package-키워드)
- [07. interface 키워드와 implements 키워드](#07.-interface-키워드와-implements-키워드)
- [08. this 키워드](#.-this-키워드)
- [09. super 키워드](#09.-super-키워드)

## 01. abstract 키워드 - 추상 메서드와 추상 클래스

### 추상 메서드, 추상 클래스
- 추상 메서드는 선언부는 있는데 구현부가 없는 메서드를 말한다.
- 추상 메서드를 하나라도 갖고 있는 클래스는 반드시 추상 클래스(Abstract Class)로 선언해야 한다.
  - 물론 추상 메서드 없이도 추상 클래스를 선언할 수는 있다.

### 추상클래스 특징
1. 인스턴스 즉, 객체를 만들수 없다.
- new를 사용할 수 없다.
2. 하위 클래스에게 `추상 메서드`의 구현을 강제한다. `오버라이딩 강제`
3. 추상 메서드를 포함하는 클래스는 반드시 추상 클래스여야 한다.

### 그럼 몸체가 없이 선언만 있는 메서드는 어떤 것이고, 왜 필요할까 ?

``` java

  public class 쥐 extends 동물 {
    void 울어() {
     System.out.println("찍찍찍"); 
    }
  }
  
  public class 고양이 extends 동물 {
    void 울어() {
     System.out.println("야옹 야옹"); 
    }
  }
  
  public class 동물 {
    void 울어() {
     System.out.println("?"); 
    }
  }
  
```

- 동물의 울음소리는 논리적으로 표현하기 어렵다.
- 동물 클래스에 `울어()` 메서드의 몸체를 `{}` 처럼 비워두는 것도 이상하다.
- 하위 클래스에서 `울어()`가 호출되고 있으니 동물에서 `울어()` 메서드는 반드시 존재해야 한다.
- 실수로 동물 객체의 `울어`를 호출하게되면 난감해진다.

### 바로 이런 경우 추상 메서드를 사용하게 된다.

``` java
public abstract class 동물 {
  abstract void 울어();
}
```

- 추상 클래스는 인스턴스 객체를 만들 수 없다. 때문에 실수로라도 동물클래스의 `울어()`를 호출할 수 없다.
- 즉, 기존에 동물 객체의 `울어()` 메서드가 가진 문제점을 `추상 메서드`와 `추상 클래스를` 통해 해결할 수 있다.

## 02. 생성자

### 생성자란?
- 클래스의 인스턴스, 즉 객체를 만들 때마다 new 키워드를 사용한다.
- 생성자는 `반환값이 없고 클래스명과 같은 이름을 가진 객체를 생성하는 메서드`이다.
- 정확하게 표현하면 `객체 생성자 메서드`

### 생성자 특징
- 아무런 생성자도 정의하지 않는다면 자바는 인자가 없는 기본 생성자를 자동으로 만들어 준다.
- 인자가 있는 생성자를 하나라도 만든다면 기본 생성자를 만들어 주지 않는다.
- 생성자는 개발자가 필요한 만큼 오버로딩해서 만들 수 있다.


## 03. 클래스 생성 시의 실행 블록, static 블록
- 클래스가 스태틱 영역에 배치될 때 실행되는 코드 블록이 있다. 바로 static 블록이다.

``` java
public class 동물 {
  static {
    System.out.println("동물 클래스 레디 온");
  }
}
```

- 클래스 정보는 해당 클래스가 맨 처음 사용될 때 T 메모리의 스태틱 영역에 로딩되며, 이 때 단 한번 해당 클래스의 static 블록이 실행된다.
- 여기서 클래스가 제일 처음 사용될 때는 다음 세 가지 경우 중 하나다.
  - 클래스의 정적 속성을 사용할 때
  - 클래스의 정적 메서드를 사용할 때
  - 클래스의 인스턴스를 최초로 만들 때

### 왜 프로그램이 실행될 때 바로 클래스의 정보를 static 영역에 로딩하지 않고 처음 사용되는 시점에 로딩할까 ?

- 메모리는 최대한 늦게 사용을 시작하고 최대한 빨리 반환하는 것이 가장 효율적이기 때문이다.
- static 블록은 한번 올라가면 종료되기 전까지는 반환할 수 없지만, 그럼에도 최대한 늦게 로딩함으로써 메모리 사용을 최대한 늦추기 위해서다.

### static 블록을 사용할 일은 거의 없지만 특성을 기억해 두면 좋다.
- Junit의 @BeforeClass
  - 현재 클래스에서 테스트 메서드를 실행하기 전에 실행된다.
  - 1개의 테스트 클래스에 @Test가 3개인 경우 ?
    - @BeforeClass어노테이션이 붙은 메소드는 최초1번 실행된다.
    - 유사한 이름의 @Before 어노테이션은 3번 실행된다.

### 참고
- static 블록과 유사하게 인스턴스를 위한 블록도 존재한다. 
- 아무런 표시없이 {} 블록을 사용하게 되면 인스턴스가 생성될 때마다 {}블록이 실행된다.
- {}블록은 객체 생성자가 실행되기 전에 먼저 실행된다.

## 04. final 키워드
- final 키워드가 나타날 수 있는 곳은 딱 세 군데다.
  - 클래스
  - 변수
  - 메서드
  - 사실 객체 지향 언어의 구성 요소는 딱 세가지 뿐이다.

### final과 클래스
- 상속을 허락하지 않겠다는 의미다.

### final과 변수
- 반드시 선언과 초기화를 동시에 하거나, 생성자를 통해 초기화 해야한다. (정적 클래스의 경우 static 블록 내부에서 가능)
- 다른 언어에서는 읽기 전용 상수를 `const` 키워드를 사용하기도 한다.

### final과 메서드
- 오버라이딩을 금지한다.

## 05. instanceof 연산자
- 인스턴스는 클래스를 통해 만들어진 객체이다.
- instanceof 연산자는 만들어진 객체가 특정 클래스의 인스턴스인지 물어보는 연산자다.
- instancof는 결과로 true또는 false를 반환한다.


``` java
package com.example.week1.customer;

class Animal {
}

class Bird extends Animal {
}

class Penguin extends Bird {

}

public class Drive {

    public static void main(String[] args) {
        Animal animal = new Animal();
        Animal bird = new Bird();
        Animal penguin = new Penguin();

        System.out.println(animal instanceof Animal);

        System.out.println(bird instanceof Animal);
        System.out.println(bird instanceof Bird);

        System.out.println(penguin instanceof Animal);
        System.out.println(penguin instanceof Bird);
        System.out.println(penguin instanceof Penguin);

        System.out.println(penguin instanceof Object);
    }
}
```

- 위 결과는 모두 true이다.
  - 참조 변수 타입이 아닌 `실제 객체의 타입에 의해 처리`하기 때문이다.
    - bird instanceof Bird 가 true인 이유

### instanceof 연산자는 LSP를 어기는 코드에서 주로 나타난다.
- 따라서 instanceof가 서용된다면 리펙터링의 대상이 아닌지 점검해 봐야 한다.
- [참조](https://flowingmooon.tistory.com/32)

## 06. package 키워드
- package 키워드는 네임스페이스(이름공간)를 만들어주는 역할을 한다.
- 거창하기는 하지만 특별히 하는일은 없다.
- 그냥 스마트폰이라고 하면 누구의 것인지 구분할 수 없지만 철수의 스마트폰, 영희의 스마트폰이라고 하면 구분이 가능하다.
  - 스마트폰이라는 명칭은 갖지만 소유자는 각각이 되는 것이다.
  - 여기서 패키지가 소유자의 역할을 한다.

## 07. interface 키워드와 implements 키워드
- 인터페이스는 추상 메서드와 정적 상수만 가질 수 있기에 따로 메서드에 public, abstract
- 속성에 public, static, final을 붙이지 않아도 자동으로 적용된다.
- 아래 2개의 인터페이스는 동일하다.
  - Speakable2처럼 명확하게 사용하는것이 모범답안. 정답은 없다.

``` java

interface Speakable {
  double PI = 3.14159;
  final double absoluteZeroPoint = -275.15
  
  void sayYes();
}

interface Speakable2 {
  public static final double PI = 3.14159;
  public static final double absoluteZeroPoint = -275.15
  
  public abstract void sayYes();
}
```

### Java8 버전의 변화
- Default Method라고하는 객체 구상 메서드(구현체가 있는 메서드)를 사용할 수 있다.
- 정적 추상 메서드를 지원할 수 있다.

## 08. this 키워드
- this는 객체가 자기 자신을 지칭할 때 쓰는 키워드다.
- 지역 변수와 속성(객체 변수, 정적 변수)의 이름이 같은 경우 지역변수가 우선.
- 객체 변수와 이름이 같은 지역 변수가 있는 경우 객체 변수를 사용하려면 this를 접두사로 사용한다.
- 정적 변수와 이름이 같은 지역 변수가 있는 경우 객체 변수를 사용하려면 클래스명을 접두사로 사용한다.

## 09. super 키워드
- 단일 상속만을 지원하는 자바에서 super는 바로 위 상위 클래스의 인스턴스를 지칭하는 키워드다.
- super 키워드로 바로 위 상위 클래스 인스턴스에 접근할 수 있지만 super.super 형태로 상위의 상위 클래스 인스턴스에 접근은 불가능하다.
