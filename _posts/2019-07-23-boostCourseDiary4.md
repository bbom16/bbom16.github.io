---
layout: post
title: "부스트코스-pjt4 강의 내용 정리"
tags: [부스트코스, 웹, javascript]
comments: true
---



# PJT4 강의 내용 정리

### 1. 객체지향 JavaScript구현 - FE



#### 1)  배열의 함수형 메소드

- forEach

  for문 대신 사용, 비교문을 사용하지 않기 때문에 더 간결하고, 실수를 줄일 수 있음.

  ```javascript
  var data = ["바나나", "사과", "딸기"];
  data.forEach((v)=>{
    console.log(v);
  })
  ```

- map

  함수에서 정의한 방법대로 모든 원소를 가공하여 **새로운** 배열을 만든다.

- filter

  함수에서 정의한 조건에 따라 해당하는 원소들로 **새로운** 배열을 만든다.

- immutable

  원본 데이터를 유지하면서 원하는 새로운 데이터를 생성하는 것을 'immutable'하다고 한다.

  immutable한 데이터는 



#### 2) 객체 리터럴과 this

- 자바스크립트에서 객체는 보통 dictionary로 많이 사용, 객체 리터럴로 객체를 생성할 수 있다.
- 비슷한 분류의 것들을 하나의 오브젝트로 묶어놓는 것이 좋다
- 객체.메서드 로 메서드를 실행할 수 있다. 
- 객체 안에서 this는 그 객체 자신을 가리킴
- this는 바뀔 수 있다. this는  현재 context가 참조하고 있는 객체를 말함



#### 3) bind메소드로 this제어하기

- 비동기 함수로 this를 호출할 때는 내가 지정하고 싶은 객체가 아니라 다른 것(window와 같은)이 this가 될 수 있다.
  - allow function (es6)를 사용하면 그 땐 제대로 가리킴
  - polyfill 확인하기 
- bind는 **새로운 함수** 를 반환하는 메소드
- bind의 첫번째 인자로 this를 줘서  그 시점에 this를 기억하게 함
- func.bind()형태로 사용



### 2. 라이브러리 활용과 클린코드



#### 1)  JavaScript 라이브러리

jQuery js라이브러리로 대부분 많이 사용됨

- 다양하게 동작하는 웹브라우저 API간의 차이를 줄여줌

- 복잡하고 어려운 DOM APIs를 추상화해 쉽게 DOM조작이 가능하게 함

- 사고의 흐름에 맞취 프로그래밍 가능

  ```javascript
  //p1의 color를 red로 하고, slideup을 2초, slidedown을 2초 실행함
  $("#p1").css("color", "red").slideUp(2000).slideDown(2000); 
  ```

  위 코드와 같이 메서드를 연속적으로 부르는 방식을 method chaining이라고 함



요즘 많이 사용하지않는 이유

- 브라우저 호환성 이슈가 줄음
- JS Frameworks들이 더 추상화된 방식으로 DOM 접근을 도와줌
- 다른 라이브러리에서도 이런 방식 가능

→ 하지만 아직 legacy 코드는 jQuery를 사용하기 때문에 유지보수를 위해 jQuery를 알고 있어야 됨.



기억해야 할 가이드 

1. 버전을 잘 확인하고, 그에 맞는 메서드를 선택한다
2. 한 페이지에 jQuery 버전은 하나만 사용한다.
3. 가급적 대체 가능하면 표준 JavaScript 메서드를 사용해 jQuery 의존도를 줄인다.



#### 2) handlebar를 활용한 템플릿 작업

handlebar(라이브러리)를 이용하여 templating 작업을 한다.

- 전에 했던 replace로 하나씩 대체하는 거 말고 handlebar를 사용하여 한번에 template 가능

  ```javascript
  var bindTemplate = Handlebars.compile(template);
  ```

- 배열일 경우

  ```handlebars
  {{#each name}}
   <div> {{@index}}번째 : {{this}} </div>
  {{/each}}
  ```

- data 처리가 많으면 forEach나 reduce로 합치기도 가능

- 조건도 가능

  ```handlebars
  {{#if hobby}}
  	<div> 내 취미는 {{hobby}}  </div>
  {{else}} 
  	<div> 아직 취미가 없어 ㅜㅜ </div>
  {{/if}}
  ```

- help function

  html

  ```handlebars
  {{#hobies hobby}}
  	{{hobby}}
  {{/hobbies}}
  ```

  javascript

  ```javascript
  Handlebars.registerHelper("hobies", function (like) {
      if (hobby > 4) {
          return "<span> 취미가 정말 많네요!</span>";
      } else if (hobby < 1) {
          return "취미를 만들어보세요:-)";
      } else {
          return "취미가 " + hobby + "개 있네요!";
      }
  });
  ```

  로 사용할 수 있다.



#### 3) 클린코드

클린 코드는 가독성 좋은 코드들을 말한다. 

아래는 몇 가지 예제이다. 

1. 이름(코딩컨벤션)
2. 의도가 드러난 구현패턴
3. 지역변수가 가능하면 지역변수로!
4. if 중첩문 없애기

더 자세한 내용은 부스트코스 확인하기!



참고 : <https://www.edwith.org/boostcourse-web/joinLectures/12958>

