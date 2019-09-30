---
layout: post
title: "일일커밋 플랫폼 만들기"
tags: [웹, spring]
comments: true
---

# 일일커밋 플랫폼 만들기

### 1. 소개

일일커밋 플랫폼은 일일커밋 스터디를 위해 그룹원들의 커밋 여부를 확인한다. 커밋 여부와 더불어 커밋 내역을 페이지에서 확인할 수 있다. 

### 2. 스펙

서버 : spring boot 2.18

db : mysql 8.05

### 3. 프로젝트 진행 

- #### 기능 설명

  사용자는 github의 아이디, github key(private info 이용시 필요)를 등록한다.

  - 등록 시 이미 등록된 id 여부, github에 존재하는 id인지 여부 체크 필요

  등록된 github 아이디로 커밋 내역을 확인한다. (커밋 내역 확인은 오후 5시, 오후 11시에 진행한다.)

  - 커밋 내역은 github api 를 이용한다. 
  - https://api.github.com/users/{github_id}/events

  최신 커밋 날짜가 오늘인지 여부를 확인한다. 

  오늘 들어온 커밋이 있다면 커밋내용들을 저장하고, 사용자의 일일 커밋 여부를 true로 설정한다.

  - 필요한 table

    userInfo, commitInfo
    
    

- ####  github id와 key 값 받기 

  회원가입을 할 때 github id 와 github key값을 받는다. 이 때, github id 와 key는 모두 유일성을 만족시켜야 하기 때문에 unique key로 설정한다. 이렇게 unique key로 설정 후 동일값이 들어올 경우 예외처리를 어떻게 해야 할까?

  - 예외 처리

    DataIntegrityViolationException을 이용하여 예외처리를 해줬다. 이 때 id 값이 null 이므로 뒤에 controller에서 이 값을 이용하여 등록 여부를 확인했다. 등록 실패하면 원래 form 등록페이지로 이동해 '이미 등록된 id입니다.'라는 문구를 보게 했다.

  

  github public key는 왜 필요한걸까?

  - github 공식 문서를 보면 key를 사용하는 걸 권장한다고 한다. 하지만 없어도 값을 보내주지만 private repo 정보는 보내주지않는다. 그래서 key값을 헤더에 추가해서 보내봤지만 제대로 값이 나오지 않았다,, 왜인지 모르겠다ㅜㅜ 왜 필요한지 더 찾아봐야겠다.  

      

- #### JSON Parsing 하기 

  ```xml
  <dependency>
    <groupId>com.googlecode.json-simple</groupId>
    <artifactId>json-simple</artifactId>
    <version>1.1</version>
  </dependency>
  ```

  json 의존성을 추가한다.

  의존성 추가 후 받아온 jsonData를 jsonParser를 이용해 parsing 해준다.

  ```java
  try{
    JSONParser jsonParser = new JSONParser();
    JSONArray jsonArray = (JSONArray) jsonParser.parse(jsonData);
  }catch(ParseException e){
    e.printStackTrace();
  }
  ```

  ​    

  

- #### commit date 받기

  날짜를 String으로 받기 때문에 LocalDateTime으로 타입 변경이 필요하다. 

  String -> LocalDateTime

  ```java
  LocalDateTime.parse(String)
  ```

   위 함수를 이용하면 문제가 해결된다. 하지만 제대로 실행되지 않고 **java.time.format.DateTimeParseException** 이 발생했다.

  - LocalDateTime은 기본형이 yyyy-MM-ddThh:mm:ss 이다. 근데 github api 에서 제공하는 형식은 yyyy-MM-ddThh:mm:ssZ(zoneddatetime)이다. 뒤에 있는 Z때문에 제대로 된 parsing이 이뤄지지 않았다. 

  ```java
  LocalDateTime.parse(commitDate, DateTimeFormatter.ISO_ZONED_DATE_TIME)
  ```

  ​	위처럼 DateTimeFormatter.ISO_ZONED_DATE_TIME을 이용하면 parsing이 제대로 이뤄진다.

    

  데이터를 db에 저장하는데 시간이 제대로 되지않는 현상이 발생했다. 9시간 일찍 시간으로 저장된다.

  - 9시간 일찍이니까 UTC timezone이 적용되는 거 같다라고 생각해서 database timezone을 kst로 변경했다. 하지만 그래도 문제가 해결되지않았고, application.properties에서 server timezone을 Asia/Seoul로 설정해서 문제를 해결하였다.

  

- #### Scheduler 사용하기

  spring boot에서 Scheduler를 사용하려면 프로젝트가 실행되는 main이 있는 java 파일에 어노테이션을 붙여서 아래와 같이 사용해야한다.

  ```java
  @SpringBootApplication
  @EnableScheduling
  public class DailycommitApplication {
      public static void main(String[] args) {
          SpringApplication.run(DailycommitApplication.class, args);
      }
  }
  ```

  그 후 사용할 controller 위에 @Scheduled를 써주면 된다.  

  





##### 참고

- string to LocalDateTime

  <https://jekalmin.tistory.com/entry/자바-18-날짜-정리>
  
- Spring boot timezone 변경

  <https://blog.wky.kr/13>

- JSON parser

  <https://ktko.tistory.com/entry/JAVA%EC%97%90%EC%84%9C-JSON-%ED%8C%8C%EC%8B%B1%ED%95%98%EA%B8%B0>