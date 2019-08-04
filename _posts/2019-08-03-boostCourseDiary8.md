---
layout: post
title: "부스트코스-pjt5 강의 정리 - PJT5-3 : form 데이터"
tags: [부스트코스, 웹, javascript]
comments: true
---

# PJT5 강의 정리 - 3

### **3. form 데이터 보내기 - FE**



- html form을 구성에 하는 여러 속성들이 있다

  - input type에는 select, password, textarea 등이 있다.
  - name은 서버와 맞춰서 설정
  - 자세한 건 필요할 때 더 찾아보기

- 서버에서 받은 데이터에 대해서 유효성 체크를 한 후 응답(response)

  - submit 버튼을 누르면 action URL로 데이터를 보냄
  - /join일 때 처리하는 코드가 있음
  - 클라이언트가 보낸 데이터 획득(request 객체에서)
  - 브라우저는 서버에서 보낸 응답을 받아 화면에 보여줌

  

- 더 알아보기 

  - form을 ajax를 통해서 보낼 수도 있음 
  - 화면 refresh 없이 더 빠르게 상태를 알려줄 수 있는 장점이 있음
  - UX면에서 좋음

  

- 좋은 UX를 위해 사용자가 빨리 고칠 수 있게 해주는 것이 좋음

  - 서버에 전송하기 전에 javascript로 체크 후 전송할 수 있음
  - default 행위를 막아주기 위해선 .preventDefault(); 
  - 이메일 유효성 체크

  ```javascript
  //true, false 체크
  var bValid = (/^[\w+_]\w+@\w+\.\w+$/).test(emailValue);
  ```



자세한 내용은 [부스트코스](https://www.edwith.org/boostcourse-web/lecture/22959/) 를 참고 하세요 :-)

