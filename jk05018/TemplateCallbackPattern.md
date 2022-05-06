# Template CallBack Pattern

> 템플릿 콜백 패턴(Template Callback Pattern)
> 
> 
>  전략패턴에서 변형된 패턴**DI (Dependency injection) 의존성 주입에서 사용하는 특별한 전략 패턴**
> 

**탬플릿 콜백 패턴은 전략 패턴에 익명 내부 클래스를 가미해서 사용합니다.**

## 전략 패턴 간단하게 remind하기

<img width="647" alt="Untitled" src="https://user-images.githubusercontent.com/68465557/167098577-6e62dd67-808c-4bdf-b97b-cf4f44dc07ac.png">


[Strategy](https://refactoring.guru/design-patterns/strategy)

행위를 클래스로 캡슐화해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴으로 전략을 쉽게 바꿀 수 있다는 장점이 있습니다.

Navigator에서 필요에 따라 동적으로 구체적인 전략을 바꿀 수 있도록 setter 메서드를 제공하기 때문에 저희는 원하는 전략을 사용할 수 있습니다.

템플릿 콜백 패턴은 여기에서 익명 내부 클래스를 넘겨줌으로써 동작합니다.

## 템플릿 콜백 패턴 예시

```java
public interface Strategy {
	void runStrategy();
}
```

```java
public class Solider {
	void runContext(Strategy strategy) {
		System.out.println("배틀 그라운드 시작");
		strategy.runStrategy();
		System.out.println("배틀 종료");
	}

}
```

```java
public class ClinetWithCallback {

	public static void main(String[] args) {
		Solider rambo = new Solider();
	
		rambo.runContext(new Strategy() {
			
			@Override
			public void runStrategy() {
				System.out.println("템플릿 콜백버전: 탕! 타탕!! 탕탕탕!");
				
			}
		});
		
		System.out.println("\n");

		rambo.runContext(new Strategy() {
			
			@Override
			public void runStrategy() {
				System.out.println("템플릿 콜백버전: 수류탄 투척~! 쾅!!!");
				
			}
		});
		
	}

}
```

[[Design_Pattern] 탬플릿 콜백 패턴(Template Callback Pattern)](https://limkydev.tistory.com/85)

## JDBC 동작 
<img width="709" alt="Untitled-2" src="https://user-images.githubusercontent.com/68465557/167098766-1d4f3fc6-2ebd-4e95-a7f8-af1cb77bbf29.png">


JDBC가 동작하는 방식은 다음과 같이 이루어져 있습니다.

1. Connection , PrepareStatement 획득
2. statement  작성 및 실행
3. Connection , PrepareStatement 반환

이 코드는 2번 빼고 반복된다는 문제가 있습니다.

이제 저희는 실행해야 할 동작(strategy)를 넘겨줌으로써 반복되던 코드를 줄일 수 있습니다.

## 직접 JdbcTemplate 내부 보기!
