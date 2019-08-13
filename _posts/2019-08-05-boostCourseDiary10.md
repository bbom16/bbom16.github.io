---
layout: post
title: "부스트코스-pjt5 강의 정리 - PJT5-5 : Spring 에서의 Session 사용법"
tags: [부스트코스, 웹, javascript]
comments: true
---

# PJT5 강의 내용 정리하기 - 5

### **5. Spring 에서의 Session 사용법 - BE**



####@SessionAttributes & @ModelAttribute

 - SessionAttributes의 파라미터 값이 ModelAttribute에 지정된 경우 메소드가 return 한 값은 argument의 이름을 key로 해서 세션이 저장됨 

   이 경우 SessionAttributes는 컨트롤러 위에, ModelAttribute은 메소드 위에 선언

 -  SessionAttriubutes 와 ModelAttribute에 인자로 전달 된 이름이 같을 경우 먼저 세션에서 인자로 전달된 이름으로 저장된 객체를 찾고 해당 객체에 요청으로부터 넘어온 값을 설정해서 전달함

   이 경우 SessionAttributes는 컨트롤러 위에, ModelAttriubute은 함수에 인자로 사용



####@SessionAttribute

인자로 전달된 이름에 해당하는 정보를 세션에서 찾아서 뒤에 선언되어있는 인자에 전달 

```java
//예
public String login(@SessionAttribute(name = "id"), String id ){
  
}
```





#### SessionStatus

SessionAttributes와 ModelAttribute 어노테이션 인자 이름이 같으면 세션에서 정보를 찾아 객체에 전달해준다. 전달 받은 값을 삭제하고자 할 때 sessionStatus가 가지고 있는 setComplate()라는 메서드를 호출하면 삭제할 수 있다.



  

#### Spring MVC - form tag

 tag 라이브러리를 이용하면 세션에 저장된 값을 form에다가 출력이 가능함.





#### 실습

redirectAttirbute를 사용해 addFlashAttribute()를 사용하면 임시로 값을 저장해 redirect를 빨리 전송할 수 있음 

암호 확인, 로그아웃, 로그인되면 게시글 삭제 등 기능을 구현함





자세한 내용은 [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16803/) 를 참고 하세요 :-)