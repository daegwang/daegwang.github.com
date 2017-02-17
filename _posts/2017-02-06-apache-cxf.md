---
layout: post
title: "Apache CXF Configuration"
description: "Apache CXF 설정"
date: 2017-02-06
tags: [spring, cxf]
comments: true
share: true
---

웹 서비스 프레임워크인 Apache CXF의 설정 방법에 대해 알아보도록 하겠습니다.

## Maven Setting
##### pom.xml

```xml
<dependency>
	<groupId>org.apache.cxf</groupId>
	<artifactId>cxf-rt-frontend-jaxrs</artifactId>
	<version>3.1.0</version>
</dependency>
```

CXF를 사용하기 위해 dependency를 추가하여 줍니다.

## Spring Context 수정

##### root-context.xml

```xml
<beans xmlns:jaxrs="http://cxf.apache.org/jaxrs" 
xsi:schemaLocation="http://cxf.apache.org/jaxrs http://cxf.apache.org/schemas/jaxrs.xsd">

	<context:component-scan base-package="com.jdg.service" />

	<jaxrs:server id="restContainer" address="/">
		<jaxrs:serviceBeans>
			<ref bean="boardService"/>
		</jaxrs:serviceBeans>
	</jaxrs:server>
</beans>
```

스프링 설정에 jaxrs와 관련된 부분을 추가하여 줍니다.(위 코드 참고)

## Service 구현
##### BoardService.java

```java
@Service
@Path("/test")
public class BoardService {
	
	@GET
	@Path("/{number}")
	public boolean testNumber(@PathParam("number") int number){
		return true;
	}
}
```

@Service 어노테이션을 이용해 BoardService를 서비스로 설정해 줍니다.
@Path를 통해 특정 주소에 접근 시 해당 메소드를 수행 할 수 있도록 할 수 있습니다.
함수 내의 @PathParam을 통해  {number}와 같이 특정 주소에 있는 데이터를 parameter로 활용할 수 있습니다.

## Annotation
##### @Path("/test/{number}")
리소스를 주소와 결합하기 위하여 사용합니다. 상대 경로이며, 정규 표현식 사용이 가능합니다.

##### @GET, @POST, @PUT, @DELETE
HTTP 메소드를 나타내는 Annotation입니다. 해당하는 메소드가 호출 될 시 실행됩니다.

##### @Produces("application/xml"), @Produces("application/json")
Response로 나타낼 객체에 대한 타입을 설정하는 Annotation입니다. 

```java
@GET
@Path("/setAdmin")
@Produces("application/json")
public boolean testNumber(){
	BoardVo vo = new BoardVo();
	vo.setName("admin");
	vo.setType(0);
	return vo;
}
```

##### @PathParam("number") int number
URI의 {number}에 해당하는 값이 number에 매핑됩니다.

##### @QueryParam

URI에서 Parameter들을 가져와서 사용하고 싶을 때 사용합니다.

```
/users/search?name=bill&location=seoul
```

```java
@GET
@Path("/search")
public boolean searchInfo(@QueryParam("name") String name, @QueryParam("location") String location){
	...
}
```

동일한 Parameter로 리스트 형태의 데이터로 받는 것도 가능합니다.


```
/users/find?type=student&type=teacher
```


```java
@GET
@Path("/find")
public boolean searchInfo(@QueryParam("type") List<String> type){
	...
}
```

##### @FormParam
form 데이터들을 받아 올 때 사용됩니다.

```java
@POST
@Path("/add")
public boolean addUser(@FormParam("name") String name, @FormParam("location") String location){
	...
}
```


## References
- [CXF User's Guide][link1]

[link1]: http://cxf.apache.org/docs/index.html