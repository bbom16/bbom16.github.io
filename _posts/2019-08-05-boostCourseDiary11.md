---
layout: post
title: "부스트코스-pjt5 강의 정리 - PJT5-6,7 : 인터셉트 & 아규먼트 리졸버 "
tags: [부스트코스, 웹, javascript]
comments: true
---

# PJT5 강의 내용 정리하기 - 6,7

### **6. 인터셉터 - BE**



#### 전체적인 Spring DispatcherServlet의 내부 동작

1. 스프링이 동작할 때 클라이언트로부터 요청이 들어오면 필터가 존재한다면 필터를 수행 

   필터란 응답이 나가기 전 수행되는 것을 말하며,  필터 등록은 web.xml에 필터를 등록하면 됨

2. 디스패쳐서블릿이 수행

   디스패쳐서블릿은 선처리 작업이 존재하면 선처리 작업을 하고 핸들러 매핑을 통해서 어떤 핸들러를 실행해야하는 지 얻어내어 핸들러를 실행 

3. 핸들러 실행

   핸들러 인터셉터가 존재 

4. view의 정보를 디스패처 서블릿에 전달해주고 viewResolover 이용해 view의 정보를 얻어오고 view를 찾아 응답해줌 

 

#### 인터셉터란

디스패처서블릿에서 핸들러로 요청을 보낼 때 

핸들러에서 디스패처서블릿으로 응답을 보낼 때 

동작을 하게되는 것이 인터셉터이다. 



#### 작성 방법

1. HandlerInterceptor를 구현
2. HandlerIntercepotrAdaptor를 상속 
3. javaConfig를 사용하면 WebMvcConfigurerAdapter가 갖고있는 addInterceptors메소드를 오보라이딩해 등록하는 과정
4. xml를 사용하면 < mvc:interceptors >에 인터셉터 등록

  

####실습

- HandlerInterceptorAdaptor를 상속

  preHandle과 postHandle을 오버라이딩함

  preHandle :  Controller 메서드가 실행되기 전에 실행

  postHandle : Controller 메서드가 실행된 후에 실행

  

- 인터셉터 등록

  WebMvcConfiguration에 인터셉트 등록

  ```java
  @Override
  public void addInterceptors(InterceptorRegistry registry) {
  		registry.addInterceptor(new LogInterceptor());
  }
  ```

  



  





###**7. 아규먼트 리졸버 - BE**

  



#### 아규먼트 리졸버란

컨트롤러의 메소드 인자로 사용자가 임의의 값을 전달하는 방법을 제공하는 경우 



#### 작성 순서

1. 스프링이 제공하는 HandleMethodArgumentResolver를 구현

2. supportsParameter메소드를 오버라이딩 후 , 원하는 타입의 인자가 있는 검사한 후 있으면 true return

3. resolveArgument메소드를 오버라이딩 후 메소드의 인자로 전달할 값 return

   

#### 작성 방법

- Java Config에 설정

  WebMvcConfigurerAdapter를 상속받은 Java Config파일에서 addArgumentResolvers메소드를 오버라이딩 한 후 원하는 아규먼트 리졸버 클래스 객체를 등록

- xml 설정

  ```xml
  <mvc:annotation-driven>
  	<mvc:argument-resolvers>
  		<bean class="아규먼트리졸버클래스"></bean>
  	</mvc:argument-resolvers>
  </mvc:annotation-driven>
  ```



#### Spring Mvc의 기본 ArgumentResolver

Controller 메서드에 HttpServletRequest, HttpSession등을 적으면 값이 전달되는데 이게 가능한 이유는 기본 아규먼트 리졸버가 존재해서 가능

Map객체나 Map을 상속받은 객체는 Spring에서 선언한 아규먼트 리졸버가 처리하기 때문에 전달할 수 없다.

Map 객체를 전달하려면 Map을 필드로 가지고 있는 객체를 선언해서 사용해야 가능하다.



#### 실습

Http요청 헤더 정보를 가지고 있는 headerInfo 인자 타입이 메서드에 있을 경우 자동으로 넘겨주는 예제

- Map을 필드로 갖고 있는 HeaderInfo라는 클래스를 선언해서 사용

- supportsParameter

  Controller 메서드의 인자의 개수만큼 supportsParameter 메서드가 매번 호출됨

  supportsParameter가 true를 리턴할 경우에만 resolveArgument 가 호출됨

  이 resolveArgument가 리턴한 값이 Controller 메서드의 인자로 전달

- WebMvcConfiguration에 등록

  ```java
  @Override
  public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
  		argumentResolvers.add(new HeaderMapArgumentResolver());
  }
  ```

  





자세한 내용은 [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16804/) 를 참고 하세요 :-)