# 자바와 절차적/구조적 프로그래밍

## 자바 프로그램의 개발과 구동 

### JDK, JRE, JVM
| 이름  | 설명                                            |
|-----|-----------------------------------------------|
| JDK | 자바 개발 도구, JVM용 개발 소프트웨어, javac.exe(자바 소스 컴파일러) |
| JRE | 자바 실행 환경, JVM용 OS, java.exe(자바 프로그램 실행기)      |
| JVM | 자바 가상 기계, 가상의 컴퓨터                             |

Java 프로그램은 운영체제 비종속적이다 → **JVM이 각 운영체제에서 문제 없이 작동할 수 있도록 만들어준다.**

### 객체지향 프로그램의 메모리 사용 방식  
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile23.uf.tistory.com%2Fimage%2F9988E33359A771AB09B623)   
[이미지 출처](https://lktprogrammer.tistory.com/1)

## 자바에 존재하는 절차적/구조적 프로그램밍의 유산
* **절차적 프로그래밍**   
  * goto 사용 금지

* **구조적 프로그래밍**   
  * 함수 사용
    * 중복 코드 제거
    * 논리 분할
  * 지역 변수 사용 


## main()
```
public class Start  {
    public static void main(String[] args) {
        System.out.println("Hello OOP!");
    }
}
```
>1. JRE는 프로그램안에 main() 메서드가 있는지 확인
>2. JVM의 전처리
>   * java.lang 패키지를 static 영역에 놓는다.
>   * 모든 클래스와 임포토 패키지를 static 영역에 놓는다.
>3. Stack 영역에 main()의 stack frame이 할당된다.
>4. main()의 인자 args를 저장할 변수 공간을 stack frame에 맨밑에 확보
>5. main()이 끝난 후 Stack 영역에 main()의 stack frame이 소멸된다.

** main() 메서드는 프로그램의 시작이자 끝이다!**

## 변수와 메모리
```
public class Start  {
    public static void main(String[] args) {
        int i;
        i = 10;
    }
}
```
>_1 ~ 4 는 이전 main()에서의 설명과 동일_  
>5. i 변수 공간이 args 변수 공간 위에 확보(**Stack**)
>   * 변수 공간은 초기화 되지 않았기 때문에 쓰레기 값이 들어가 있다.
>6. i 변수 공간에 10을 할당 
>7. main()이 끝난 후 Stack 영역에 main()의 stack frame이 소멸된다.

```int i = 10```은 선언과 초기화를 한번에 한 것일 뿐 1)변수 공간 확보, 2) 변수 공간에 값 할당의 단계는 동일하다.


