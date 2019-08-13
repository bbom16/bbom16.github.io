---
layout: post
title: "부스트코스-pjt5 BE 구현하기 "
tags: [부스트코스, 웹]
comments: true
---

# PJT5 BE 구현하기

### form값 서버로 전송해서 값 db에 저장하기 

form에 예약 정보를 저장해서 서버로 post방식으로 전송한다. 이 때, 사용자가 선택한 좌석 종류와 수량을 같이 보내줘야해서 json 형식으로 저장해서 전송했다. 



**문제**

json 형식으로 보냈더니 매핑이 제대로 되지않는 문제가 발생했다. 스프링에서 매핑을 시키려면 변수 타입과 이름을 동일하게 맞춰야 하는데 거기서 타입을 제대로 맞추지못해 발생한 문제 같았다 

- 해결 

  - 프론트단에서 보낼 때 string값으로 변경해서 전송 후 서버에서 받아 json으로 변경 후 파싱해서 값 사용

  ```javascript
  //js : json형태 데이터를 string으로 변경해 전송 
  amountLabel.value = JSON.stringify(param);
  ```

  ```json
  // json 형태 데이터
  {
    "amountList":
   [
     {
       "cnt":"1",
     	 "typeName":"B"
     },
  		{
        "cnt":"3",
        "typeName":"Y"
      },
     {
       "cnt":"1",
       "typeName":"A"
     }
   ]
  }
  ```

  ```java
  JSONParser parser = new JSONParser();
  try{
    //json 데이터를 string으로 변경 
    JSONObject jsonObject = (JSONObject) parser.parse(reservation.getPriceAmountList());
  	/* json 파싱 */
    JSONArray amountList = (JSONArray)jsonObject.get("amountList");
  	for(int i=0 ; i<amountList.size(); i++){
  	   JSONObject amount = (JSONObject) amountList.get(i);
      	String cnt = (String)amount.get("cnt");
    }
  }
  ```

  json 파싱할 때 데이터 타입이 object 이기 때문에 반드시 필요한 타입으로 변환 후 사용해야함.

  - spring에서 JSONParser 사용시 dependency 추가해줘야함

  ```xml
  <dependency>
     <groupId>com.googlecode.json-simple</groupId>
     <artifactId>json-simple</artifactId>
     <version>1.1</version>
  </dependency>
  ```

  

- 문제 해결을 찾아보면서 알게 된 점 

  - @ModelAttribute

    데이터의 이름을 지정하여 해당 데이터만을 가져온다. 객체 타입일 때 많이 사용함. 

    요청 파라미터를 VO로 받는다.

    get, post방식 모두 사용 가능함. 

     

  - @RequestParam

    get 방식일 땐 body에 데이터가 없기때문에 사용해선 안되고 post방식일 때 사용하자 

    XML, JSON일 때 주로 사용함. 

    



### 예매일 서버에서 내려주기

요구사항 중 예매일을 서버에서 내려주라는 부분이 있었다. 서버에서 현재날짜 + 0-5일을 랜덤으로 생성하여 json으로 보내주었다. 

**문제**

1. localDate 사용하여 날짜 더해주기 

   - localDate를 생성하여 날짜를 더해줬다. 더해줄 때는 .plusDays()라는 함수를 사용함. 

   - 랜덤으로 값을 더해주기 위해 Math.random() 함수 사용함

     Math.random()은 0.0 ~ 1.0까지만 반환하기 때문에 값을 좀 더 랜덤하게 사용하기 위해 *100을 해준 후 사용

     ```java
     int randomNumber = (int)(Math.random()*100)%6;
     LocalDate currentDate = LocalDate.now();
     reservationDate = currentDate.plusDays(randomNumber);
     ```

     

2. 날짜를 localDate를 사용하였는데 date를 사용할 때와 다르게 json으로 보냈을 때 값이 제대로 출력되지않았다.

   - dependency에 jackson-datatype-jsr310을 추가

     ```xml
     <dependency>
     	<groupId>com.fasterxml.jackson.datatype</groupId>
       <artifactId>jackson-datatype-jsr310</artifactId>
       <version>${jackson2.version}</version>
     </dependency>
     ```

     아래와 같이 출력되었다.

     ```json
     reservationDate: [
     2019,
     8,
     12
     ],
     ```

     이 문제를 해결하기 위해 커스터마이드 된 bean을 생성하려고  jsonConfig.java를 추가했지만 **문제해결안됨**

     ```java
     @Configuration
     public class JsonConfig {
     	@Bean
     	public ObjectMapper objectMapper() {
     		return Jackson2ObjectMapperBuilder.json()
           	.featuresToDisable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)
     				.modules(new JavaTimeModule()).build();
     	}
     }
     ```

     그냥 js에서 받을 때 가공해서 사용함.



### 로그아웃 소스

실제 기능 구현에는 필요하지않았지만 개인적으로 다른 이메일로도 로그인 확인할 때를 위해 만듦

생성했던 세션들을 전부 없애줌 

```java
@GetMapping(path="/logout")
public String logout(HttpSession session) {
	session.removeAttribute("isReservation");
	session.removeAttribute("email");
	return "redirect:./";
}
```









- 참고 

  json Parsing :  <https://walkinpcm.blogspot.com/2016/03/java-json-json-parsing.html>

  jackson 추가 및 jsonConfig 추가 :  <https://jsonobject.tistory.com/235>

  @RequestBody와 @ModelAttribute 

   <https://sticky32.tistory.com/entry/SpringMVC-ModelAttribute-RequestBody-에-대해서>

  <https://sjh836.tistory.com/143>

  @ModelAttribute로 객체 List 받기 : <https://m.blog.naver.com/PostView.nhn?blogId=spdlqjdudghl&logNo=220841108598&proxyReferer=https%3A%2F%2Fwww.google.com%2F>

  

  