---
layout: post
title: "Using Mybatis in Spring"
description: "Mybatis 사용법, 스프링 JDBC, SQL"
date: 2017-01-27
tags: [mybatis, jdbc, spring]
comments: true
share: true
---

## MyBatis

### 1. 라이브러리 추가(pom.xml)

mybatis관련 dependency를 추가해줍니다.

```xml
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis</artifactId>
	<version>3.2.8</version>
</dependency>
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis-spring</artifactId>
	<version>1.2.2</version>
</dependency>
```

##### oracle사용 시
oracle을 사용하기 위해서는 추가적으로 아래와 같이 관련 dependency를 추가해줍니다.

```xml
<dependency>
	<groupId>com.oracle</groupId>
	<artifactId>ojdbc6</artifactId>
	<version>11.1.0.7.0</version>
</dependency>
```

상단 properties위쪽에 repository도 추가해줍니다.

```xml
<repositories>
<repository>
	<id>oracle</id>
	<name>ORACLE JDBC Repository</name>
	<url>http://maven.jahia.org/maven2</url>
</repository>
</repositories>
```

### 2. 스프링 설정파일 수정
mybatis를 사용하기 위해 스프링 설정파일에 sqlSessionFactory와 sqlSession과 관련된 설정도 추가해줍니다.

```xml
<beans:bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<beans:property name="dataSource" ref="dataSource"/>
	<beans:property name="mapperLocations" value="classpath:mapper/*.xml"/>
</beans:bean>

<beans:bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
	<beans:constructor-arg index="0" ref="sqlSessionFactory"/>	
</beans:bean>
```

### 3. DAO 생성

mybatis매핑 파일을 작성하기에 앞서 매핑에 필요한 dao를 생성해 줍니다.

```java
public interface IDao{
	public ArrayList<BoardVo> list();
	public void write(String writer, String content);
	public BoardVo view(String id);
	public void delete(String id);
}
```


### 4. xml파일 생성

##### classpath:mapper/*.xml

```xml
<mapper namespace="~.IDao">
	<select id="list" resultType="~.BoardVo">
	SELECT * FROM BOARD
	</select>
	
	<insert id="write">
	INSERT INTO BOARD (ID, WRITER, CONTENT) VALUES (BOARD_SEQ.NEXTVAL, #{param1}, #{param2})
	</insert>
	
	<delete id="delete">
	DELETE FROM BOARD WHERE ID = #{param1}
	</delete>
</mapper>
```
각 id의 값은 DAO interface의 메소드 이름을 나타냅니다.


### 5. sqlSession사용

```java
@Autowired
private SqlSession sqlSession;
	
@RequestMapping("/list")
public String list(Model model){
	IDao dao = sqlSession.getMapper(IDao.class);
	model.attribute("list", dao.list());
	return "/list";
}
	
@RequestMapping("/write")
public String write(HttpServletRequest request, Model model){
	IDao dao = sqlSession.getMapper(IDao.class);
	dao.write(request.getParameter("writer"),
request.getParameter("content"));
	return "redirect:list";
}
```
위와 같이 SqlSession을 @Autowired시킨 후 용도에 맞게 dao를 getMapper를 통해 호출하여 사용하시면 됩니다.