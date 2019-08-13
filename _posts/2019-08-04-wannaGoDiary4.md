---
layout: post
title: "한이음 - 미세먼지 정보 어플(0804) : spring에서 스케줄러 사용하기"
tags: [spring, java, 한이음]
comments: true
---

### 2019.08.04



#### 스프링에서 스케줄러 사용하기

정해진 시간(매시간 10분)마다 미세먼지 수치를 공공데이터에서 받아와야하는 기능이 필요했다. 시간마다 컨트롤러를 어떻게 동작해야할지가 문제였다. 그것과 관련해 찾아보던 중 스케줄러를 이용하면 해결할 수 있다는 것을 알게 되었다. 

- **quartz**

  spring에서 스케줄을 지원해주는 라이브러리라고 함

  spring에 장점인 @Autowired로 객체주입이 불가능하다고 해서 더 이상 찾아보지않음



- **@scheduled**

  spring에서 자체적으로 제공하는 scheduler를 사용함

  - 사용방법

    applicationContext.xml에 아래 사항들을 추가했다.

    ```xml
    <context:annotation-config/>
    <context:component-scan base-package="com.wannago" />
    <task:executor id="executor" pool-size="15" queue-capacity="255" />
    <task:scheduler id="scheduler" pool-size="15" />
    <task:annotation-driven executor="executor" scheduler="scheduler" />
    ```

    꼭 applicationContext xml일 필요는 없지만 componet-scan과 annotation-config와 같은 xml에 넣어줘야 제대로 동작한다고 한다.

      

    내가 고생했던 부분은 아래 코드가 제대로 추가 되지않고 자꾸 에러를 일으켜서 한참을 찾아봤다. 

    ```xml
    <task:executor id="executor" pool-size="15" queue-capacity="255" />
    <task:scheduler id="scheduler" pool-size="15" />
    <task:annotation-driven executor="executor" scheduler="scheduler" />
    ```

    해결 방법은 beans부분에 아래코드를 추가해주니까 제대로 동작하게 됨

    ```xml
    http://www.springframework.org/schema/task
    http://www.springframework.org/schema/task/spring-task-3.2.xsd"
    ```

      

     

      컨트롤러 안에 시간마다 처리해야할 함수를 정의하고 @Scheduled추가한다.

    ```java
    @Scheduled(cron = "0 10 * * * ?")
    public void checkForBatch(){
       dustService.getDustApi();
    }
    ```

    cron은 유닉스에 cron과 동일한 순서인데 순차적으로 초, 분, 시, 일, 월, 요일, (년도)라고 한다

    내가 적은 의미는 매일 매시간 10분마다 서비스를 동작하게 했다.

    cron의 사용법은 아래 블로그를 참고했다.

    



- 참고
  - scheduled에 beans 추가 : <http://blog.miyu.pe.kr/264>
  - cron 사용법 : <https://huskdoll.tistory.com/819>
  - 전체적인 scheduled 사용법 : <https://ojava.tistory.com/128>