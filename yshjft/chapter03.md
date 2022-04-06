# 자바와 객체 지향
**객체 지향은 직관적이고 사람 친화적이다!!**

## 객체 지향의 4대 특징
* Abstraction: 추상화, 모델링
* Generalization(Inheritance): 상속, 일반화, 재사용
* Encapsulation: 캡슐화, 정보 은닉
* Polymorphism: 다형성, 사용 편의

## 클래스 VS 객체
> **클래스 = 개념, 객체 = 실체**

## 추상화: 모델링
* 객체를 특성(속성, 기능)에 따라 분류 → **클래스(분류)**
* 클래스를 이용하여 **객체(클래스의 인스턴스)** 를 만들었다
* **추상화 = 모델링**   
  * 특성(속성, 기능)을 가지고 재조합하는 것이다.
    * 속성: 값
    * 기능: 메서드
  * 클래스의 범위가 있어야 한다.
    * 범위 = 애플리케이션의 경계
    * 목적에 따라 달라진다.
  * **추상화란 구체적인 것을 분해(분류)하여 애플리케이션 경계에 있는 특성만 가지고 재조합하는 것이다.**
    * 실제 사물을 단순하게 묘사하는 것이다.
  > 경계 설정 → 추상화(모델링) → 클래스르 이용하여 객체 생성
  * **Java는 class라는 키워드를 통해 추상화를 지원한다.**
  
### 추상화와 메모리
```
public class Mouse  {
    public class String name;
    public class int age;
    public int cntOfTail;
    
    public void sing() {
        System.out.println(name+" 찍찍!");
    }
}

public class MouseDriver {
    public static void main(String[] args) {
        Mouse mickey = new Mouse();
        
        mickey.name = "미키";
        mickey.age = 25;
        mickey.cntOfTail = 1;
        
        mickey.sing();
        
        mickey = null;
    }
}
```

* main() 메서드 시작 전    
Static 영역: java.lang 패키지, MouseDriver 클래스, Mouse 클래스
  * Mouse 클래스에 아직 변수 저장 공간이 없다.    
  객체가 생성되어야만 힙 영역에 변수 저장 공간이 생긴다.
  

* main() 메서드 시작 후
  * Mouse mickey = new Mouse();
    * **Heap 영역에 Mouse 클래스 인스턴스가 배치된다.**
    * main() 스택 프레임 내의 mickey의 변수 공간에서 Mouse 클래스의 주소 값을 가지고 있다.
  * mickey.sing();    
    * sing(): 객체의 멤버 메서드, **호출 되어도 T 메모리 상에서 변화는 없다.**
    * main(): 클래스의 멤버 메서드
  * mickey = null;
    * mickey 변수는 더 이상 Mouse 객체(인스턴스)를 참조하지 않는다.
    * **가바지 컬렉터가 참조되지 않는 인스턴스를 제거한다.**
      * 가비지 컬렉텨가 언제 실행되는지는 아무도 모른다!


* main() 메서드 종료

## 클래스 멤버 VS 객체 멤버

* **속성 with static 키워드**
  * static(정적) 멤버
  * 클래스 멤버 속성
    * **메모리의 static 영역에 단하나의 저장 공간을 갖게 된다.**
    * 모든 객체가 같은 값을 가질 때 사용하는 것
  * 클래스 멤버 메서드
    * 객체 생성 여부에 관계 없이 사용할 수 있는 메서드
  * UML에서는 밑줄을 사용하여 표시
  
* **속성 without static 키워드**
  * 인스턴스 멤버
  * 객체 멤버 속성
  * 객체 멤버 메서드
  

## 지역 변수 VS 클래스 속성, 객체 속성

|이름 |   다른 이름    |  메모리 영역  |
|:---:|:----------:|:--------:|
| static 변수 | 클래스 속성, 정적 변수, 정적 속성 | static 영역 |
| 인스턴스 변수 | 객체 속성, 객체 변수 | heap 영역  |
| 지역 변수 |   지역 변수    | stack 영역 |


## 상속
### 확장 + 재사용
* 상속은 상위 클래스 특성을 **확장**하는 것이다.
* **하위 클래스는 상위 클래스다**
* 상속은 상위 클래스의 특성을 **재사용**하는 것이이다.
  * 하위 클래스에서의 상위 클래스 메서드 사용
* TIP) 클래스 이름은 **분류**스럽게 객체 참조 변수 이름은 **사물**처럼 작명해야 한다.

### "is a" 관계 보단 "is kind of" 관계 
* 상속은 is kind of 관계이다.
  * **하위 클래스 is kind of 상위 클래스**

### 다중 상속과 자바
* 자바는 다중상속을 지원하지 않는다.
* 어떤 상위 클래스의 메서드를 사용해야 하는가? → 다이아몬드 문제 → 깔끔하게 다중 상속 포기 

### 상속과 인터페이스
* 인터페이스는 is able to 관계이다.
  * 구현 클래스 is able to 인터페이스
  * "무엇을 할 수 있는"
* 인터페이스는 기능을 구현하도록 강제
  * **상위 클래스는 물려줄 특성이 풍성할 수 있도록 좋다.**
  * **인터페이스는 구현을 강제할 메서드의 개수가 적을수록 좋다.**

### 상속과 메모리
```
class Animal {
  
  void showHabitat(){ ... }
}
class Penguin extends Animal {
  void showName(){ ... }
}
```
위와 같은 상황에서 ```Penguin pororo = new Penguin();```을 할경우 Penguin 클래스의 인스턴스와 Animal 클래스의 인스턴스 모두 힙영역에 생긴다. 

![img.png](https://sehun-kim.github.io/sehun/assets/images/extends2.PNG)

* ```Penguin pororo = new Penguin();```
  * pororo는 Penguin 클래스 인스턴스 주소를 저장 
  * showName(), showHabitat() 모두 사용 가능

* ```Animal pingu = new Penguin();```
  * pingu는 Animal 클래스 인스턴스 주소를 저장
  * showName()은 사용 가능 
  * **showHabitat() 사용 불가능**


## 다형성
### 오버라이딩 & 오버로딩
* 다형성의 기본
* 오버라이딩
  * 상위 클래스 같은 메서드 이름, 같은 인자 리스트
  * 구현부 재정의
* 오버로딩
  * 상위 클래스 같은 메서드 이름, 다른 인자 리스트

### 다형성과 메모리
![](https://sehun-kim.github.io/sehun/assets/images/poly1.PNG)
* ```Penguin pororo = new Penguin();```, ```Animal pingu = new Penguin();```
  * pororo
    * ```showName()```: 하위 클래스에서 오버라이딩 한 내용이 실행된다.
    * ```showName(yourName)```: 하위 클래스에서 오버로딩된 내용이 실행된다.
    * ```showHabitat()```: 항위 클래스 메서스 내용 실행
  * pingu
    * ```showName()```: **하위 클래스에서 오버라이딩 한 내용이 실행된다.**
    * ```showName(yourName)```: 사용 못함
    * ```showHabitat()```: 사용 못함

### 다형성은 사용장(개발자)에게 편의성을 준다.

## 캡슐화

#### 접근 제어자
* private: 본인(같은 클래스)만 접근 가능
* default: 같은 패키지 내의 클래스만 접근 가능
* protected: 같은 패키지 + 상속 클래스에서만 접근 가능
* public: 모두가 접근 가능 

#### 정적 멤버 접근
* 정적 멤버는  ```클래스명.정적멤버``` 방법을 사용하라!!

## Call by Value & Call by Reference
### Call by Value
* **값 그 자체를 저장하고 있다.**

### Call by Reference
* **주소값을 저장하고 있다.**
