# 스프링 삼각형과 설정 정보

## IOC(제어의 역전) / DI(의존성 주입)
### 의존성이란?
**의존성 = new**   
(정확히는 값이 할당되는 모든 순간 의존이 생긴다)

```
interface Tire {
	String getBrand();
}

public class AmericaTire implements Tire {
	public String getBrand() {
		return "미국 타이어";
	}
}

public class KoreaTire implements Tire {
	public String getBrand() {
		return "코리아 타이어";
	}
}
```

```
public class Car {
	Tire tire;

	public Car() {
		tire = new KoreaTire();
		// tire = new AmericaTire();
	}

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}
```
**유연성이 떨어지는 코드!**


### 생성자를 통한 의존성 주입 without Spring
```
public class Car {
	Tire tire;

	public Car(Tire tire) {
		this.tire = tire;
	}

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}
```

```
public class Driver {
	public static void main(String[] args) {
		Tire tire = new KoreaTire();
		//Tire tire = new AmericaTire();
		Car car = new Car(tire);

		System.out.println(car.getTireBrand());
	}
}
```
**확장성이 좋아졌다!**

### 속성을 통한 의존성 주입 without Spring
setter를 이용하여 의존성을 주입한다.

```
public class Car {
	Tire tire;

	public Tire getTire() {
		return tire;
	}

	public void setTire(Tire tire) {
		this.tire = tire;
	}

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}
```
속성보다는 생성자를 이용하여 의존성을 주입하는 것이 선호된다. 실제로 프로그램에서는 한번 주입된 의존성을 사용하는 것이 일반적이다.

### 스프링을 통한 의존성 주입 - XML 파일 사용
```
public class Driver {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("expert002/expert002.xml");

		Car car = context.getBean("car", Car.class);

		Tire tire = context.getBean("tire", Tire.class);

		car.setTire(tire);

		System.out.println(car.getTireBrand());
	}
}
```

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="tire" class="expert002.KoreaTire"></bean>

	<bean id="americaTire" class="expert002.AmericaTire"></bean>

	<bean id="car" class="expert002.Car"></bean>

</beans>
```
* id를 이용한 속성 구분

#### 이점
재컴파일/재배포하지 않아도 XML 파일만 수정하면 프로그램의 실행 결과를 바꿀 수 있다.

### 스프링을 통한 의존성 주입 - XML 파일에서 속성 주입
```
public class Driver {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("expert002/expert002.xml");

		Car car = context.getBean("car", Car.class);

		System.out.println(car.getTireBrand());
	}
}
```

```    
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="koreaTire" class="expert003.KoreaTire"></bean>

	<bean id="americaTire" class="expert003.AmericaTire"></bean>

	<bean id="car" class="expert003.Car">
		<property name="tire" ref="koreaTire"></property>
	</bean>
</beans>
```

### 스프링을 통한 의존성 주입  - @Autowired를 통한 속성 주입
어노테이션을 이용하여 setter 메서드를 이용하지 않고도 설정 파일을 통해 속성을 주입하는 방식

```
IntelliJ IDEAPhpStormWebStorm    
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	❗️xmlns:context="http://www.springframework.org/schema/context"❗️
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		❗️http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd❗️">

	❗️<context:annotation-config />❗️

	<bean id="tire" class="expert004.KoreaTire"></bean>

	<bean id="wheel" class="expert004.AmericaTire"></bean>

	❗️<bean id="car" class="expert004.Car"></bean>❗️
</beans>
```

```
public class Driver {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("expert004/expert004.xml");

		Car car = context.getBean("car", Car.class);

		System.out.println(car.getTireBrand());
	}
}
```

```
import org.springframework.beans.factory.annotation.Autowired;

public class Car {
	@Autowired
	Tire tire;

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}
```

#### @Autowired
스프링 설정 파일을 보고 자동으로 속성의 설정자 메서드에 해당하는 역할을 해주겠다는 의미

### 스프링을 통한 의존성 주입 - 번외 경기
#### @Autowired - type 기준 매칭
> XML 파일에서 Bean의 id를 이용한 속성 구분

좀 더 정확하게 말하면 type을 우선적으로 고려하게 된다. 만약 같은 type을 구현한 클래스가 여러개 있다면 XML파일의 bean 태그의 id로 구분해서 매칭

### 스프링을 통한 의존성 주입 - @Resource를 통한 속성 주입
```
import javax.annotation.Resource;

public class Car {
	@Resource
	Tire tire;

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}
```
* 자바 표준 어노테이션
* id가 타입보다 우선순위가 높다.

#### @Autowired VS @Resource VS property
|       -       | @Autowired                                             | @Resource                 |
|:-------------:|--------------------------------------------------------|---------------------------|
|      출처       | 스프링 프레임워크                                              | 표준 자바                     |
|    소속 패키지     | org.springframework.beans.factory.annotation.Autowired | javax.annotation.Resource |
|    빈 검색 방식    | type → name(id)                                        | name(id) → type           |
|  byName 강제하기  | @Autowired @Qualifier("tire1")                         | @Resource(name="tire1")   |

```
<?xml version="1.0" encoding="UTF-8"?>
<beans ... >
	<context:annotation-config />
	
	<bean id="tire1" class="expert006.KoreaTire"></bean>
	<bean id="tire2" class="expert006.AmericaTire"></bean>

	<bean id="car" class="expert006.Car"></bean>
</beans>
```

```
public class Car {
	@Resource(name = "tire1")
	Tire tire;

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}
```

```
public class Car {
	@Autowired
	@Qualifier("tire2")
	Tire tire;

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}
```
* 책에서는 ```property``` > ```@Resource``` > ```@Autowired```
  * 아직 개발 경험이 많지 않아서 모르겠지만 블로그 또는 강의를 보면서 ```property```, ```@Resource```를 사용하는 것을 본적이 거의 없는거 같다. 좋은 방법이라면 많은 사람들이 사용하고 있어야되는 것이 아닐까?

## AOP
### 핵심 관심사

### 횡단 관심사(cross-cutting-concern)
* 다수의 모듈에서 **공통적으로 나나타는 부분**

```
DB 커넥션 준비
Statement 객체 준비

try {
    DB 커넥션 연결
    Statement 객체 세팅
    
    (쿼리)
    
}catch ... {
    예외 처리
}catch ... {
    예외 처리
}finally{
    DB 자원 반납
}
```

### AOP 적용지점(로직을 주입할 수 있는 지점)
![](https://velog.velcdn.com/images/yshjft/post/e1aae47b-7389-4052-a875-3fe0ca983e26/image.png)

### 코드
```
public interface Person {
	void runSomething();
}

public class Boy implements Person {
	public void runSomething() {
		System.out.println("컴퓨터로 게임을 한다.");
	}
}

public class Girl implements Person {
	public void runSomething() {
		System.out.println("요리를 한다.");
	}
}
```
```
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
public class MyAspect {
	@Before("execution(* runSomething())")
	public void before(JoinPoint joinPoint) {
		System.out.println("얼굴 인식 확인: 문을 개방하라");
	}
}
```
```
public class Start {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("aop002/aop002.xml");

		Person romeo = context.getBean("boy", Person.class);
		Person juliet = context.getBean("girl", Person.class);

		romeo.runSomething();
		juliet.runSomething();
	}
}
```
```
<?xml version="1.0" encoding="UTF-8"?>
<beans 
  xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:aop="http://www.springframework.org/schema/aop"

  xsi:schemaLocation="
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
    http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans.xsd">

	️<aop:aspectj-autoproxy />
	<!--<bean class="org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator" /> -->

	<bean id="myAspect" class="aop002.MyAspect" />
	<bean id="boy" class="aop002.Boy" />
	<bean id="girl" class="aop002.Girl" />
</beans>
```

![](./img.png)

* ```@Aspect```: 해당 클래스를 이제 AOP에서 사용하겠다.
* ```<aop:aspectj-autoproxy />```: 스프링 프레임워크에게 AOP 프록시를 사용하라고 알려주는 지시자
* 런타임 시점에 메서드(로직)가 주입된다.

### 스프링 AOP 핵심
* **스프링 AOP는 인터페이스 기반**
* **스프링 AOP는 프록시 기반**
* **스프링 AOP는 런타임 시점에 적용**

#### 참고) AOP 적용 시점
* 컴파일 시점
* 클래스 로딩 시점
* 런타임 시점
  * Bean을 만들 때 프록시 Bean 생성

### 용어
#### Pointcut
* Aspect 적용 위치
* > [접근제한자 패턴] **리턴타입패턴** [패키지&클래스패턴] **메서드이름패턴**(**파라미터패턴**)[throws 예외 패턴]
  * []: 생략 가능
  * **굵은 글씨**: 필수
  * ex) ```* runSomething()```

#### JoinPoint
* Pointcut의 후보가 되는 모든 메서드(Aspect 적용 가능한 지점)
* 호출된 객체의 메서드

#### Advice
* Pointcut에 언제, 무엇을 적용할지 정의한 메서드

#### Aspect
* Advice들 + PointCut들
* 부가 기능 모듈

#### Advisor
* 한개의 Advice + 한개의 Pointcut
* 스프링 AOP에서만 사용되는 개념
* **이제는 쓰지 말라고 권고**

### POJO와 XML 기반의 AOP
```
import org.aspectj.lang.JoinPoint;

public class MyAspect {
	public void before(JoinPoint joinPoint){
		System.out.println("얼굴 인식 확인: 문을 개방하라");
	}
}
```
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<aop:aspectj-autoproxy />	
	
	<bean id="myAspect" class="aop003.MyAspect" />
	<bean id="boy" class="aop003.Boy" />
	<bean id="girl" class="aop003.Girl" />
	
	<aop:config>
		<aop:aspect ref="myAspect">			
			<aop:before method="before" pointcut="execution(* runSomething())" />
		</aop:aspect>
	</aop:config>
</beans>
```

#### 참고)
POJO(Plain Old Java Object)란 **객체 지향적인 원리에 충실**하면서 **환경과 기술에 종속되지 않고** 필요에 따라 재활용될 수 있는 방식으로 설계된 오브젝트를 의미

#### 중복되는 Pointcut 리팩토링
* BEFORE
```
<?xml version="1.0" encoding="UTF-8"?>
<beans 

    ...
    
	<aop:config>
		<aop:aspect ref="myAspect">			
			<aop:before method="before" pointcut="execution(* runSomething())" />
			<aop:after method="lockDoor" pointcut="execution(* runSomething())" />
		</aop:aspect>
	</aop:config>
</beans>
```
```
@Aspect
public class MyAspect {
	@Before("execution(* runSomething())")
	public void before(JoinPoint joinPoint) {
		System.out.println("얼굴 인식 확인: 문을 개방하라");
	}
	
	@After("execution(* runSomething())")
	public void lockDoor(JoinPoint joinPoint) {
		System.out.println("주인님 나갔다: 어이 문 잠가!!!");
	}
}
```


* AFTER
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<aop:aspectj-autoproxy />	
	
	<bean id="myAspect" class="aop004.MyAspect" />
	<bean id="boy" class="aop004.Boy" />
	<bean id="girl" class="aop004.Girl" />
	
	<aop:config>
		<aop:pointcut expression="execution(* runSomething())" id="iampc"/>
		
		<aop:aspect ref="myAspect">			
			<aop:before method="before" pointcut-ref="iampc" />
			<aop:after method="lockDoor" pointcut-ref="iampc" />
		</aop:aspect>
	</aop:config>
</beans>
```

```
@Aspect
public class MyAspect {
	@Pointcut("execution(* runSomething())")
	private void iampc() {
		
	}

	@Before("iampc()")
	public void before(JoinPoint joinPoint) {
		System.out.println("얼굴 인식 확인: 문을 개방하라");
	}

	@After("iampc()")
	public void lockDoor(JoinPoint joinPoint) {
		System.out.println("주인님 나갔다: 어이 문 잠가!!!");
	}
}
```

## PSA(Portable Service Abstraction)
* 일관성 있는 서비스 추상화
  * 어댑터 패턴을 이용하여 다수의 기술을 공통의 인터페이스로 제어할 수 있게하는 것
  * ex) @Transactional   
    PlatformTransactionManager 인터페이스를 사용
    * JpaTransactionManager(JPA), DatasourceTransactionManager(JDBC API) 등의 구현체 존재
    * 구현체를 바꾼다해서 aspect 코드가 변경되지 않습니다.
 
