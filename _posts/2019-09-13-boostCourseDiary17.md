---
layout: post
title: "부스트코스 설계자 특강 - spring boot로 바꿔보기"
tags: [부스트코스, 웹]
comments: true
---

##### 강의 내용 정리

spring boot 는 내부적으로 was를 가지고있어서 jar파일로 만들어줌. 

내부적인 was는 jsp를 지원하지 않는다.  



application.properties에 db정보를 입력해주면 스프링 부트는 자동으로 인식해서 datasource 값을 정해준다.

주의할 점은 *이름 설정* 이다. 아래 이름과 같이 꼭 설정해줘야 스프링 부트가 인식할 수 있다.

```properties
spring.datasource.url=
spring.datasource.username=
spring.datasource.password=
spring.datasource.driver-class-name=
```

​    

.sql 파일을 만들어서 프로젝트 안에서 사용하려면 properties 파일에 아래 코드를 써줘야 된다.

```properties
# 이부분 있어야 schema.sql, data.sql 인식
spring.datasource.initialization-mode=always
```

  

@Transactional 의 read only 는 default가 false이다. transactional이 끝날 때마다 commit을 수행하는데 select에서 굳이 수행할 필요가 없으므로 read only를 true로 설정하는 것이 성능에 좋다.

  

thymeleaf 사용 시 pom.xml에 의존성 추가해줘야 함

```xml
<!-- thymeleaf -->
<dependency>
  <groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

  

jsp 사용안하고 이거 사용하는 이유는 실제 jsp 사용하면 jsp 내부엔진이 jsp를 servlet으로 변환 시킴 -> 변환된 servlet이 컴파일 됨 -> 컴파일 된 클래스 파일을 서블릿 엔진에 올림 -> 서블릿 엔진에서 순서대로 동작 이런 복잡한 과정을 피하기 위해서 jsp 사용 안함.  





##### 문제

1. connection pool 테스트 중 에러 발생 

```
java.sql.SQLException: The server time zone value 'KST' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.
```

timezone이 kst로 되어 발생한 에러였는데 spring.data.url 뒷부분에 

```
&serverTimezone=UTC
```

추가하면 제대로 동작한다.



2. .sql 파일 추가 시 에러 발생

![SQL_Dialect_is_Not_Configured_img](/Users/bominjung/bbom/dev/git_blog/bbom16.github.io/images/SQL_Dialect_is_Not_Configured_img.png)

- SQL_Dialect_is_Not_Configured 

  해결 방법은 Preferences > Languages & Frameworks > SQL Dialects 에서 global을 내가 사용하는 DB인 mysql로 설정하면 된다.

- No data sources are configured to run thi SQL and provide advanced code assistance 

  해결 방법은 views > tool windows > database 선택 후 database 윈도우에 ddl 파일 드래그해서 옮기면 해결된다.





##### 추가로 찾아보기

KST 이면 왜 에러인지 찾아보기



##### 출처

No data sources are ~ 에러  :  <https://www.jetbrains.com/help/datagrip/creating-and-editing-sql-files.html>

