---
layout: post
title: "부스트코스-pjt5 강의 정리 - PJT5-2"
tags: [부스트코스, 웹, javascript]
comments: true
---

# PJT5 강의 정리 - 2

### **2. JavaScript Regular expression - FE**

- 정규표현식

  문자열의 특정 패턴을 찾을 수 있는 문법

  패턴을 찾아 문자열 조작을 할 수 있음



- 실무에서 사용

  정해진 규칙성을 갖고 있는지 확인하는 경우 많이 사용

  - 이메일, 주소, 전화번호 

  - textarea 불필요한 입력값 추출

  - 트랜스 파일링

    현대화된 개발 문법을 브라우저가 지원하지 않을 때 도구를 개발하는 경우 그런 작업을 할 때 많이 사용(현재 지원하는 코드로 변경할 때)  

  - 개발도구에서의 문자열 치환



- 예제

  1. 숫자 하나 / 두 개 찾기 

  - .match 함수를 사용

    ```javascript
    "1".match(/1/)[0];
    // '/ /' 정규표현식 
    >> 1
    ```

  - 문자열 안에 있는 모든 두자리 숫자를 찾고 싶은 경우

    ```javascript
     .match(/\d\d/);
    ```

  - javascript regex cheatsheet 사용해서 확인하기

  

  2. 우편번호 (구/신)

     ```javascript
     "123-123".match(/\d\d\d-\d\d\d/)[0];
     -> .match(/\d{3}-\d{3}/);
     "12345".match(/\d{5}/)
     ```

     or 연산을 이용하면 123-123 이거나 12345 인 경우 둘다 확인 가능

     ```javascript
     // or연산 -> (a|b)
     .match(/(\d{3}-\d{3}|\d{5})/)[0];
     ```

     이런 or 연산을 하위 표현식이라고 부름

     

     첫 자리가 9가 아닌 숫자만 가능한 경우

     ```javascript
      .match(/(\d{3}-\d{3}|[012345678]\d{4})/)[0]
     //[0-8]까지 가능(연속된 숫자인 경우)
     //[0-46-8] 이 사이에 문자만 가능해짐 
     ```

     [ ] -> 이 중에 하나만 가능 

  

  3. 핸드폰 번호 

     ```javascript
     //017,018,019 만 가능 + 가운데가 3자리, 4자리
     .match(/01[0789]-\d{3,4}-\d{4}/)[0];
     ```

     , 는 수량자라고도 부름

  

  4. 개발도구에서의 함수 찾기

     괄호가 하나 있거나 하나 없거나 

     function 후에 하나 이상의 공백 \s+

     \w a-z까지 문자들

     근데 $를 찾을 수 없어서 -> [a-zA-Z_​\$]

     ```javascript
     \(?function\s+[a-zA-Z_$]+ 
     ```

  

- replace 

  ```javascript
  "011-021-0011".replace(/(\d{2})\d/, "$10");
  >> "010-021-0011"
  ```

  $1이 가리키는 것은 앞에 (\d{2})! 

  괄호는 유지하고 3번째를 0으로 바꿔라!!  

  ```javascript
  "$$$iloveyou###".replace(/.*[a-zA-Z]{8}.*/, "$1");
  >> "iloveyou"
  ```

  \* 은 무서운 친구



- greedy, lazy 수량자

  - greedy : \*, +, {n,}
    - 탐욕스럽게 찾을 수 있는 끝-까지 가서 찾음
  - lazy : \*?, +?, {n,}?
    - 게을러서 찾으면 바로 return 

  

- 일반 함수를 arrow function으로 바꾸기 (실습)

  ```javascript
  var aFuction = "function a(){ return 'hi';}"
  var chagne = aFunction.replace(/(function\s+)/, "");
  >> "a(){return 'hi';}"
  change.replace(/[a-zA-Z]+(\(\))/, "$1 => ");
  >> "() => { return 'hi';}"
  ```

  

  

  

