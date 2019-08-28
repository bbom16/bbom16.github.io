---
layout: post
title: "부스트코스-pjt6 강의 정리 - PJT6-2 로깅"
tags: [부스트코스, 웹]
comments: true
---

# PJT6 강의 정리 2

### 2. 로깅

1) 로깅이란?

​	정보를 제공하는 일련의 기록인 로그(log)를 생성하도록 시스템을 작성하는 활동

​	프린트 줄 넣기(printlining)는 간단한, 보통은 일시적인, 로그를 생성하기만 한다.

​	시스템 설계자들은 시스템의 복잡성 때문에 로그를 이해하고 사용해야 한다.

​	로그가 제공하는 정보의 양은, 이상적으로는 프로그램이 실행되는 중에도, 설정 가능해야 한다.

- 일반적으로 로그 기록의 이점
   로그는 재현하기 힘든 버그에 대한 유용한 정보를 제공

   로그는 성능에 관한 통계와 정보를 제공

  로그는 예기치 못한 특정 문제들을 디버그하기 위해 그 문제들을 처리하도록 코드를 수정하여 다시 적용하지않아도 일반적인 정보를 처리할 수 있도록 해줌

- 로그를 출력하는 방법

  System.out.println() 이용

  ​	출력되는 로그양, 수준을 조절 암됨, 파일등에 저장하기 어렵고 성능면에서도 좋지 않음

  **로깅 라이브러리 이용** 

  - 종류

    - java.util.logging

      기능이 많이 부족해 다른 로그 라이브러리 많이 사용 

    - Apache Commons logging

    - Log4j 

      아파치 재단 제공

      로그 라이브러리에서 많이 사용!!

    - Logback

      Log4j의 단점을 보완하기 위해 만들어진 라이브러리

      실습할 때 사용

  

2) slf4j 설정하기

​	다양한 로깅 관련 라이브러리들을 하나의 통일된 방식으로 사용할 수 있는 방법을 제공

​	로깅 facade, 로깅에 대한 추상 레이어를 제공, interface의 모음

 - 장점 

   나중에 더 좋은 로그 라이브러리가 개발 돼 변경된다 하더라도 application 코드를 변경할 일이 없어짐

- spring 사용방법

  slf4j, logback 의존성 추가 

  ​	logback classic 설치하면 slf4j 따로 추가 안해도 됨(이미 의존성 갖고 있기 때문)

  ​	spring은 기본적으로 commons-logging 사용해서 제거하고 사용해야 됨

  ​	commons-logging을 삭제하면 commons-logging을 찾으려고해서 notfount error발생 

  ​	-> jcl-over-slf4j 추가 필요

  logback.xml

  ​	appender 어디에다가 어떤 포멧으로 로그를 남길 것인가

   - ConsoleAppender 설정 

     콘솔에 어떤 포멧으로 출력한것인가

     %logger : logger 이름을 축약 

     { length} :  최대 자릿수

     %-5level : 로그레벨을 5의 고정폭 값으로 출력

     %msg : 사용자가 남긴 메세지

     ```xml
     <app ender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
      <encoder>
       	<Pattern>%d{HH:mm} %-5level%logger{36}-%msg%n </Pattern>
       </encoder>
     </appender>
     ```

  - RollingFileAppender 설정

    디렉토리 경로가 없으면 이클립스 설치 경로에 생성됨

    ```xml
     <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <file>access.log</file>
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <fileNamePattern>access-%d{yyyy-MM-dd}.log</fileNamePattern>
                <maxHistory>30</maxHistory>
            </rollingPolicy>
            <encoder>
                <Pattern>%d{HH:mm} %-5level %logger{36} - %msg%n</Pattern>
            </encoder>
        </appender>
    
    
    ```

    

  Log Level

  1. trace
  2. debug
  3. info
  4. warn
  5. error

  아래로 내려갈수록 심각한 상황, 어떤 오류를 어떤 방식으로 기록할 것인지 설정해야됨

  로그백에서 logger라는 엘리먼트는 어떤 패키지 이하의 클래스에서 어떤 레벨 이상의 로그를 출력할지를 결정

  

  Logger 객체 선언 후 사용

    

  로그 출력 메소드

  - 문자열 결합을 위해 '+' 연산자 사용x (속도 느려짐)

    (출력한 로그 문자열 , 가변인자 )

  ```java
  logger.trace({}{}, arr1, arr2);
  logger.debug();
  ...
  ```

  



3) slf4j를 이용한 로그남기기

guestbook intercept에 실습 적용해봄























