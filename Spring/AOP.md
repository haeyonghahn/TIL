# AOP (Aspect Oriented Programming)
AOP의 개념은 관점 지향 프로그래밍으로 핵심 관심만 집중할 수 있도록, 필요하지만 중복해서 작성해야 하는 핵심 이외의 코드들은 외부로 빼놓는 것이다.   
![AOP](https://github.com/haeyonghahn/TIL/blob/master/Spring/images/AOP.PNG)   
위의 그림과 같이 여러 객체에서 공통적으로 작성해야 하는 부분들을 횡단 관심이라고 하는데, 이 횡단 관심을 제거하여 핵심 관심( 비즈니스 로직 )만 집중할 수 있도록 하고자 하는 프로그래밍 기법이 AOP이다.
AOP에서는 핵심 관심 모듈의 중간 중간에 필요한 횡단 관심 모듈을 직접 호출하지 않고 위빙(Weaving)이라 불리는 작업을 해서 횡단 관심 코드가 삽입되도록 한다.
따라서 핵심 관심 모듈에서는 횡단 관심 모듈이 무엇인지조차 인식할 필요가 없다.   

## 용어
### 1) Pointcut
어느 부분(where)에 횡단 관심 모듈을 삽입할 것인지 정의한다.   
[ 제어자, 반환타입, 패키지, 클래스, 이름, 파라미터, 예외 ]를 작성   
### 2) Joinpoint
언제(when) 횡단 관심 모듈을 삽입할 것인지 정의한다.   
1) 메서드 실행 전 ( before )
2) 메서드 실행 후 ( after )
3) 반환된 후 ( AfterReturning )
4) 예외가 던져지는 시점 ( AfterThrowing )
5) 메서드 실행 전, 후 ( around )   
### 3) Advice
핵심 관심 모듈에 삽입될 횡단 모듈 자체(What)를 의미한다.   
## 환경 설정 : pom.xml   
```xml
<!-- spring aspect -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aspects</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```   
dependency를 추가한다.   
## applicationContext.xml
```xml
<!-- auto proxy -->
	<aop:aspectj-autoproxy>
	
	</aop:aspectj-autoproxy>
  
  <context:component-scan base-package="com.douzone.mysite.service, com.douzone.mysite.repository, com.douzone.mysite.aspect">
      <context:include-filter type="annotation"
         expression="org.springframework.stereotype.Repository" />
      <context:include-filter type="annotation"
         expression="org.springframework.stereotype.Service" />
      <context:include-filter type="annotation"
         expression="org.springframework.stereotype.Component" />
   </context:component-scan>
```   
<aop:aspectj-autoproxy /> 방식을 둔다. proxy란 어떤 핵심 기능을 수행 하기 전과 후에 공통 기능을 수행한다고 할 때, aspect가 곧바로 핵심 기능에서 실행되는 것이 아니라, proxy(대행자)에서 공통 기능이 수행하도록 하는 것이다.
즉 proxy는 핵심 기능을 하는 메서드에서 핵심 기능을 수행하고 다시 돌아와서 공통 기능을 수행하는 로직을 거치게 된다.
또한, base-package에 com.douzone.mysite.aspect 패키지를 추가하도록 한다.   
## spring-servlet.xml
```xml
<!-- auto proxy -->
	<aop:aspectj-autoproxy>
	
	</aop:aspectj-autoproxy>
```   
개발을 하면서 어디에 횡단 관심 모듈을 삽입할 지 모르기 때문에 spring-servlet.xml 파일에도 추가하는 것이 좋다.   
## MyAspect.java
```java
package com.douzone.aoptest.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class MyAspect {
  // execution() : joinpoint에 함수를 실행한다.
	// 제어자 : public
	// 반환타입 : ProductVO
	// 패키지 : com.victolee.aoptest
	// 클래스 : ProductServcie
	// 메서드 : find
	// 예외 던지기 생략 가능
	// Before PointCut이 실행되기 전에 beforeAdvice를 실행하라는 의미이다.
	@Before("execution(public com.douzone.aoptest.vo.ProductVo com.douzone.aoptest.service.ProductService.find(String))")
	public void beforeAdvice() {
		System.out.println("---- before Advice");
	}
	
  // 모든 패키지 내 ProductServcie 클래스의 모든 메서드
	@After("execution(* *..*.service.ProductService.*(..))")
	public void afterAdvice() {
		System.out.println("---- after Advice ----");
	}
	
	@AfterReturning("execution(* *..*.service.ProductService.*(..))")
	public void afterReturningAdvice() {
		System.out.println("---- afterReturning Advice ----");
	}
	
  // 모든 패키지의 모든 클래스의 모든 메서드
	// ProductService에서 예외를 던질 경우 그 예오를 받는다.
	// return하기 전에 exception이 발생하면 이 부분으로 들어간다.
	@AfterThrowing(value="execution(* *..*.*.*(..))", throwing="ex")
	public void afterThrowingAdvice(Throwable ex) {
		System.out.println("---- afterThrowing Advice " + ex + "----");
	}
	
  // pjp를 통해 "핵심 모듈의 메서드"의 실행이 가능
	// aoptest 패키지의 모든 클래스 모든 메서드 ( execution은 일반적으로 이렇게 쓰인다. )
	// Around before랑 after랑 합친거다.
  // 실제로 가장 많이 쓰이는 joinpoint는 @Around입니다.
  // @Around는 @Before와 @After가 합쳐진 것인데, 매개변수로 ProeedingJoinPoint 객체를 받습니다.
  // ProeedingJoinPoint 객체는 핵심 관심 모듈에 대한 정보를 갖고 있으며,
  // @Around 내부에서는 before와 after를 나누는 기준이 됩니다.
	@Around("execution(* *..*.service.ProductService.*(..))")
	public Object aroundAdvice(ProceedingJoinPoint pjp) throws Throwable {
		// before advice
		System.out.println("@Around(before) Advice");
	
		// PointCut Method 실행
		// 파라미터 가로채기(바꿔버러기)
		//Object[] params = {"Camera"};
		Object result = pjp.proceed();
		
		// after advice
		System.out.println("@Around(after) Advice");
		
		return result;
	}
}

```
## 실행시간 측정 예제
### com.douzone.mysite.aoptest/ MeasureExecutionTimeAspect.java
```java
package com.douzone.mysite.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;
import org.springframework.util.StopWatch;

@Aspect
@Component
public class MeasureExecutionTimeAspect {

	@Around("execution(* *..*.repository.*.*(..)) || execution(* *..*.controller.*.*(..))")
	public Object aroundAdvice(ProceedingJoinPoint pjp) throws Throwable {
		
		StopWatch sw = new StopWatch();		
		sw.start();
		
		// before
		Object result = pjp.proceed();
		
		// after
		sw.stop();
		Long totalTime = sw.getTotalTimeMillis();
	
		String className = pjp.getTarget().getClass().getName();
		String methodName= pjp.getSignature().getName();
		String task = className + "." + methodName;
		System.out.println("[Execution Time][" + task + "] " + totalTime + "millis");
		
		return result;
	}
}

```
환경설정을 통해 @Aspaect 어노테이션을 스캔할 수 있도록 해주고, 위와 같이 정의하여 repository 패키지에 있는 모든 클래스의 메소드와 Controller 패키지에 있는 모든 클래스의 메소드에 대해서 횡단 관심 모듈이 실행될 것이다.
