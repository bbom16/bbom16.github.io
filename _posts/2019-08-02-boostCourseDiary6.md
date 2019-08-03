---
layout: post
title: "부스트코스-pjt5 강의 정리 - PJT5-1"
tags: [부스트코스, 웹, javascript]
comments: true
---

# PJT5 강의 정리 - 1

### **1. UI Component module - FE**

- 자바스크립트에서 비슷한 속성을 묶어서 객체를 정의하고 사용하는 것은 자연스러운 일



- 객체 리터럴로 작성하는 경우

  - 여러가지 객체가 필요하다면 어떻게 할까? 
  - 객체마다 중복이 있다면 유지보수 + 메모리 문제가 발생
  - 객체를 동적으로 생성하는 방법 필요!

  

- 객체를 동적으로 생성하는 방법

  - 'new'를 사용하자!

    ```javascript
    function Hi(name1, name2){
      this.name1 = name1;
      this.name2 = name2;
      this.sayHi = function(){
        return this.name1 +"님이 " + this.name2 +  "님에게 인사를 했습니다!";
      }
      //return this;
    }
    var h = new hi("bom", "crong");
    ```

  - this에 받아서 반환하는 것

    - 사실은 숨겨져있지만 return this다!!

  - 위 코드에 Hi가 생성자이다.
    
    - 생성자 : 객체를 생성하는 함수

  

- 객체를 동적으로 사용해도 동일한 함수를 메모리에 중복적으로 놓게 되는 문제는 해결되지 않는다.

  - prototype에 메소드를 두면 공간을 같이 공유한다!!

    

- prototype에 메소드를 공유하는 방법

  - 함수에 기본적으로 존재함

    ```javascript
    Hi.prototype.sayHi = function(){
          return this.name1 +"님이 " + this.name2 +  "님에게 인사를 했습니다!";
    }
    ```

  - 이렇게 생성한 두 객체 함수들은 동일한 메모리에 위치 (메모리 효율성 증가)

    ```javascript
    h2.sayHi === h.sayHi
    >> true
    ```

  - prototype이라는 체인으로 상속도 구현할 수 있다.

    -> 더 찾아보기!! 

  - Object.create나 es6 class 안에 구현하면 prototype 키워드를 직접 사용하지 않아도 됨. 




- prototype을 활용해 Tab UI를 만들어 보기
  - object literal 할 때는 오류가 발생할 수 있기 때문에 arrow function을 사용하지 않는걸 권장한다.
  -  this로 속성값들을 나열해서 상태를 유지해야하는 변수들을 선언해서 사용할 수도 있다.
    - 객체 내의 전역변수 느낌으로 이해할 수 있다.
  - 자세한 실습코드나 내용은 [edwith]("https://www.edwith.org/boostcourse-web/lecture/16795/") 에서 확인!



- 더 생각해보기

  - prototype체인을 이용한  javascript에서 상속하기!

    - javascript 객체는 속성을 정하는 동적인 자신만의 속성을 갖는다.
    - 객체의 어떤 속성에 접근하려면 그 객체 자체 속성 뿐아니라 객체의 prototype, prototype의 prototye 과 같이 prototype 체인 끝까지 속성을 탐색한다.
    - Object.create를 사용하면 __ proto __가 Object를 가리켜 상속처럼 사용할 수 있게 됨.

    

  - 객체를 생성하는 여러 방법이 존재

    - 문법 생성자로 객체 생성
    - 생성자를 이용
    - Object.create를 이용
    - class 키워드르 사용
      - ES6부터 사용할 수 있음.
      - 동작방식은 class기반 언어들과는 다르다.
      - 자바스크립트는 여전히 프로토타입 기반으로 동작한다.

  - 참고 : [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain#%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85_%EC%B2%B4%EC%9D%B8%EC%9D%84_%EC%9D%B4%EC%9A%A9%ED%95%9C_%EC%83%81%EC%86%8D](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain#프로토타입_체인을_이용한_상속)

    

