# 객체 지향 설계 5원칙 - SOLID
응집도는 높이고 결합도는 낮추자!!

## SRP(Single Responsibility Principle)
* 단일 책임 원칙(책임을 분리하는 것)
* 속성, 메서드, 패키지, 모듈, 컴포넌트, 프레임워크 등에 적용될 수 있다.

### 속성에서 단일 책임 원칙
```
class 사람 {
    String 군번;
}

사람 로미오 = new 사람();
사람 줄리엣 = new 사람();

줄리엣.군번 = "1573042009"
```
사람 클래스를 남자 클래스와 여자 클래스로 나눈다. 

#### 공통점이 없는 경우 
* 사람 클래스를 제거한다.

#### 공통점이 존재할 경우
* 사람 클래스에 공통점을 두고 남자 클래스와 여자 클래스가 사람 클래스를 상속한다.

#### 참고) 하나의 속성이 여러 의미를 갖는 경우도 단일 책임 원칙을 위반하는 경우

### 메서드에 대한 단일 책임 원칙
```
class 강아지 {
    final static Boolean 수컷 = true;
    final static Boolean 암컷 = false;
    Booolean 성별;
    
    void 소변보다() {
        if(this.성별 == 수컷) {
            ...
        }else{
            ...
        }
    }
}
```
* 메서드가 **분기 처리**를 사용하여 2가지 기능을 수행하고 있다.
  * 분기 처리를 위한 if는 단일 책임 원칙을 위배하는 냄새다 

#### 개선
클래스를 나눈다.
```
class 강아지 {
    abstract void 소변보다();
}

class 수컷강아지 extends 강아지 {
    void 소변보다() {
        ...
    }
} 

class 암컷강아지 extends 강아지 {
    void 소변보다() {
        ...
    }
} 
```

### 추상화
설계를 할 때 반드시 SRP를 고려하자!!



## OCP(Open Closed Principle)
* **자신의 확장에는 열려 있고 주변의 변화에 대해서는 닫혀 있어야 한다.**
* 상위 클래스 또는 인터페이스를 이용하여 구현
  ```
  class Car{
    void drive() { ... }
  }
  
  class Acar extends Car {
    @Override
    void drive() { ... }
  }
  
  class Bcar extends Car {
    @Override
    void drive() { ... }
  }
  ```
* jdbc는 대표적인 OCP 준수의 예시
  * DB가 MySql이던, 오라클이던 자바 어플리케이션에 영향을 주지 않는다.
* **유연성, 재사용성, 유지보수성 등을 얻을 수 있다.** 

## LSP(Liskov Substitution Principle)
* **서브 타입은 언제나 자신의 기반 타입으로 교체할 수 있어야 한다.**
* 분류도 관계
  * ```동물 뽀로로 = new 펭귄()``` 
  * 하위에 존재하는 것은 상위에 있는 것의 역할을 할 수 있어야 한다.