---
layout: post
title: "부스트코스-pjt5 강의 정리 - PJT5-4 : 상태유지기술"
tags: [부스트코스, 웹, javascript]
comments: true
---

# PJT5 강의 내용 정리하기 - 4

### **4. 상태유지기술(Cookie & Session) - BE**







#### 전체적인 흐름 정리

Http 는 상태유지가 되지 않는 프로토콜이다.

클라이언트의 응답을 하고 나면 해당 클라이언트와 연결 지속x

상태유지를 위해 쿠키와 세션 기술이 등장

- 쿠키

  사용자 컴퓨터에 저장

  단점 : 다른 사람 또는 시스템이 볼수도 있음

- 세션

  서버에 저장

 ⇒ 어떤 것이 좋고 나쁜 게 아니라 상황에 따라 선택해서 사용하면 됨

간단한 동작 과정

- 쿠키 동작 과정

  클라이언트가 서버 쪽에 요청을 보냄 

  서버는 유지해야되면 쿠키를 생성해서 클라이언트에게 보냄

  쿠키는 이름과 값을 갖고 있음

  클라이언트는 요청을 보낼 때 쿠키를 같이 보냄

  쿠키를 서버가 받아 다시 검사해서 유지 해야하는 지 여부 확인 





 

- 세션 동작 과정

  요청이 들어오면 서버에서 세션키 생성

  저장소를 생성해서 세션키를 저장

  세션키를 담은 쿠키 생성해서 이 쿠키를 클라이언트한테 보냄

  클라이언트를 요청할 때마다 세션키 쿠키를 보내고 서버는 받아서 세션키를 이용해 이전에 생성한 저장소를 활용함 (이 때 httpSession을 사용)





#### 상태 유지 방법

1. Cookie를 이용한 방법 

   - 쿠키 

      클라이언트 단에 저장되는 작은 정보의 단위

     키 + value 로 이루어짐 

     클라이언트에서 생성하고 저장될 수도 있고, 서버단에서 전송한 쿠키가 클라이언트에 저장될 수도 있음

   

   

   

   - 이용 방법 

     서버에서 클라이언트의 브라우저로 전송되어 사용자의 컴퓨터에 저장

     저장된 쿠키는 다시 해당하는 웹페이지에 접속할 때, 브라우저에서 서버로 쿠키를 전송

     쿠키는 이름/값 이외에도 주석, 경로, 유효기간, 버전, 도메인과 같은 정보도 저장함

   

   

   

   - 쿠키는 수와 크기에 제한적

     하나의 쿠키는 4KB

     브라우저는 각각의 웹사이트 당 20개의 쿠키를 허용

     모든 웹 사이트를 합쳐 최대 300개만 허용

     클라이언트 당 쿠키의 최대 용량은 12MB 정도
   
   
   

   

   

   - 실제 사용하는 방법 

     서버에서 쿠키 생성, Response의 addCookie 메소드를 이용해 클라이언트에게 전송 

     ```java
  Cookie cookie = nnew Cookie(이름, 값);
     response.addCookie(cookie);
  ```
   
  웹 클라이언트로 전송 
   
     웹 클라이언트는 서버에게 쿠키를 받으면 쿠키를 서버별로 저장함
   
   

   

   

   - 쿠키의 이름은 알파벳과 숫자로만 구성됨

     한글로 하고 싶으면 인코딩 ,디코딩 과정 필요

     쿠키 값은 공백, 괄호, 등호 콤마, 콜론, 세미콜론은 포함이 안됨, 쿠키값은 항상 **String**!

      웹 클라이언트는 서버에게 다시 요청을 보낼 때 자동으로 쿠키를 같이 보냄 

     ```java
  //HttpServletRequest가 갖고 있는 getCookies를 이용해 쿠키를 가져옴
     Cookie[] cookies = request.getCookies();
  ```
   
  웹 클라이언트가 보낸 쿠키를 받아옴
   
     쿠키가 하나가 아니라 여러 개 일 수 있음
   
     **주의!** 쿠키값이 없으면 null return 하기 때문 null 예외처리 필요
   
  
   
  
   
  
   
   
   
- 서버는 쿠키를 삭제하는 명령이 없음
   
  삭제는 클라이언트에서! 
   
  서버에서 할 수 있는 방법은 삭제하고 싶은 쿠키와 이름이 같은 쿠키를 생성해 maxAge를 0으로 바꿔 전송한다.
   
     클라이언트 쪽에서 같은 이름의 쿠키를 가질 수 없기 때문에 같은 이름의 쿠키가 들어오면 쿠키를 교체함
   
     유지시간이 0이라서 해당 쿠키는 사라짐
   
     ```java
  Cookie cookie = new Cookie("이름", null);
     cookie.setMaxAge(0);
  response.addCookie(cookie);
     ```

     - setMaxAge()

       클라이언트에게 보내기 전에 서버에서 setMaxAge()로 유효기간 설정 가능 

       인자는 초 단위의 정수형 

       음수로 지정하면 브라우저가 종료될 때 쿠키가 삭제 
  
     
   
   - 쿠키 관련 메소드(javax.servlet.http.Cookie)
   
     | 반환형 | 메소드 이름               | 메소드 기능                 |
  | ------ | ------------------------- | --------------------------- |
     | int    | getMaxAge()               | 쿠키의 유효기간을 알아냄    |
  | String | getName()                 | 쿠키의 이름을 스트링을 반환 |
     | String | getValue()                | 쿠키의 값을 스트링으로 변환 |
  | void   | setValue(String newValue) | 쿠키의 새로운 값을 설정     |
   

   

   

   - spring MVC에서 cookie 사용

     1. @CookieValue 사용
   
        컨트롤러 파라미터에서 annotation을 사용하면 컨트롤러에서 쉽게 읽어드릴 수 있음
   
     2. HttpServletRequst, HttpServletResponse를 이용
   
        컨트롤러 파라미터에서 request를 받아 쿠키를 받고 생성해서 클라이언트에게 보내줌
   
        ```java
        public String list(@RequestParam(name="start", required=false, defaultValue="0") int start, ModelMap model, 
        HttpServletRequest request, HttpServletResponse response){
        }
        ```
   





2. Session을 이용한 방법

   - 세션

     클라이언트 별로 서버에 저장되는 정보

     로그인 정보, 장바구니 정보 등 클라이언트 별로 저장되어야하는 정보가 있는 경우 사용

     ##### 

   - 이용 방법

     클라이언트가 서버에 요청을 하면 클라이언트만 가질 수있는 세션 id를 발급

     이 세션 id를 이용해 key value를 이용한 저장소인 httpSession을 생성

     세션 id로 쿠키를 만듦

     클라이언트에게 세션 id에 해당하는 쿠키를 보냄

     클라이언트는 요청할 때마다 쿠키를 갖고 들어옴

     -> 이 세션id에 해당하는 저장소를 찾아 클라이언트만의 정보를 유지해서 사용할 수 있게 해줌

   

   

   

   - HttpSession

     HttpSession은 new로 생성하지않음 

     서버가 알아서 만들어서 request로 받아오면 됨

     받을 때 세션 id로 되어있는 쿠키가 존재하는 지 여부 확인

     있으면 반환하고 없으면 새로 만들어서 반환(isNew()라는 메소드로 새로운 세션인지 여부 확인)

     ```java
     //인자가 없는 것과 true는 같게 동작
     HttpSession session = request.getSession();
     HttpSession session = request.getSession(true);
     // false라면 세션이 없으면 새로 만들지않고, null 반환
     HttpSession session = request.getSession(false);
     ```

   

   

   

   - setAttribute(String name, Object value)

     name과 value를 쌍으로 저장

     이때 value는 Object

   - getAttribute(String name)

     저장할 때 저장한 name을 이용

     return값은 Object! 알맞은 객체로 맞춰서 형변환 필수

   - removeAttribute(String name)

     name 해당 세션 삭제

   - invalidate()

     모든 세션 정보 삭제

   

   

   

   - 세션 관련 메소드(javax.servlet.http.HttpSession)

     | 반환형 | 메소드 이름                          | 메소드 기능                                                  |
     | ------ | ------------------------------------ | ------------------------------------------------------------ |
     | long   | getCreationTime()                    | HttpSession이 생성된지 얼마나 지났는지 확인(밀리세켠드로 반환) |
     | String | getId()                              | 세션 Id를 String 타입으로 반환                               |
     | int    | getMaxInactiveInterval()             | 세션 유지시간을 구하는 함수 <br />설정하지않으면 기본값은 1800초(30분) |
     | void   | setMaxInactiveInterval(int interval) | 세션 유지시간을 초 단위로 설정                               |

   

   

   - 세션 유지 시간

     세션은 클라이언트가 서버에 접속학는 순간 생성 

     지정하지않으면 기본값 30분

     세션의 유지 시간은 서버에 접속한 후 서버에 요청을 하지않는 최대 시간

     30분 이상 서버에 반응이 없으면 자동으로 세션이 끊어짐

     이 세션 유지 시간은 web.xml에서 설정 가능하다

     ```xml
     <session-config>
     	<session-timeout>30</session-timeout>
     </session-config>
     ```







- 자세한 내용은 [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16798/) 를 참고 하세요 :-)

  

  