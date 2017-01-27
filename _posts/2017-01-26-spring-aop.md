---
layout: post
title: "Aspect Oriented Programming with Spring"
description: "스프링에서의 AOP(Aspect Oriented Programming)"
date: 2017-01-26
tags: [java, spring]
comments: true
exshare: true
---

## Aspect Oriented Programming
스프링에서의 AOP란, 보안, 로깅과 같이 공통으로 반복되는 기능을 특정 위치에서 수행하기 위한 방법입니다.  
이러한 공통적으로 적용될 기능을 Advice라고 부르며 이 Advice를 적용하는 지점을 Jointpoint라고 합니다. 그리고 Jointpoint에 Advice를 적용하는 행위를 Weaving이라고 부릅니다.

##### Weaving이 적용되는 시점
스프링에서의 AOP는 프록시를 사용하여 적용됩니다. 객체에 직접 접근하지 않고 중간에 프록시를 생성하여 접근하는 방식입니다. 스프링이 어떤 객체에 AOP를 적용할지에 대한 것은 설정 파일을 통해 알 수 있습니다.


##### Advice의 종류
스프링에서 구현 가능한 Advice종류는 다음과 같습니다.

| 종류 | 설명 |
|:----|:----|:---|
| `<aop:before>`| Method 실행 전에 실행 |
| `<aop:after-returning>`| Method 정상 실행 후 실행 |
| `<aop:after-throwing>`| Method에서 예외 발생 시 실행 |
| `<aop:after>`| Method 실행 후(exception포함) |
| `<aop:around>`| Method 실행 전,후에 실행(exception포함) |


## XML Based Implementation

### 1. dependency 설정 (pom.xml)

```xml
<org.aspectj-version>1.7.4</org.aspectj-version>

<dependency>
	<groupId>org.aspectj</groupId>
	<artifactId>aspectjweaver</artifactId>
	<version>${org.aspectj-version}</version>
</dependency>
```

### 2. Advice구현 (MyAdvice.java)

```java
public class MyAdvice{
	public Object setAdvice(ProceddingJoinPoint p) throws Throwable 
	{
		System.out.println("started");
		try{
			Object o = p.proceed();
			return o;
		} finally {
			System.out.println("finished");
		}
	}
}
```

### 3. xml에 설정 추가 (setting.xml)

```xml
<bean id="aop" class="com.jdg.ex.MyAdvice"/>

<aop:config>
	<aop:aspect id="myAop" ref="aop">
		<aop:pointcut id="point" expression="within(com.jdg.ex.*)" />
		<aop:around pointcut-ref="point" method="setAdvice"/>
	</aop:aspect>
</aop:config>
```

## @Aspect Annotation Based Implementation

### 1. dependency 설정 (pom.xml)
xml과 동일하게 설정해주면 됩니다.

### 2. @Aspect Annotation을 사용하여 Advice구현

```java
@Aspect
public class MyAdvice{

	@Pointcut("execution(* com.jdg.ex.controller.*Controller.*(..))
		|| execution(* com.jdg.ex.service.*Impl.*(..)
		|| execution(* com.jdg.ex.dao.*DAO.*(..)")
	public void cut(){
	}
	
	@Around("cut()")
	public Object setAdvice(ProceddingJoinPoint p) throws Throwable 
	{
		System.out.println("started");
		try{
			Object o = p.proceed();
			return o;
		} finally {
			System.out.println("finished");
		}
	}
}
```
@Pointcut에 포인트컷 표현식을 작성한 후, 메소드 이름을 Advice어노테이션의 값으로 사용하면 됩니다. 여기서는 `cut()`을  값으로 사용하였으며 포인트컷 표현식 작성 시 포인트컷 지시자로 execution을 사용하였습니다.

### 3. xml에 설정 추가 (setting.xml)

```xml
<aop:aspectj-autoproxy/>
<bean id="aop" class="com.jdg.ex.MyAdvice"/>

```
