---
layout: post
title: "부스트코스-pjt4 구현"
tags: [부스트코스, 웹, javascript]
comments: true
---

### 2019.07.24 - 2019.07.30

#### pjt4 구현하기

- **문제 상황** 

  1. **상단 배너 이미지를 버튼 눌러 무한 슬라이드 하는 것이 제대로 구현이 안됨**

     그 위치에 오게 하는 건 가능하지만 원하는 방향으로 이동하는 것처럼 보이지 않음.

  - **해결책**

    - 이미지가 2개라면 이미지를 추가로 만들어서 한 이미지 양 옆에 다른 이미지를 놓는다. 

    필요한 이미지보다 더 필요하고, 효율적인 방법이 아니라 개선이 필요하다고 생각함

    일단 구현하는 것이 중요하다고 생각해 비효율적이더라도 구현함.

    - 구현 방법 

    1. 이미지를 1-1, 2-1, 1-2, 2-2 이렇게 4개로 만들어서 나열한다. 

    2. prev 버튼을 누르면 1-1 → 2-1 방향으로 움직인다. 갈 때마다 내 왼쪽 이미지를 내 방향 맨 끝에 위치 시킨다.

       예) 2-1 → 2-2 방향으로 움직이면 1-1 이미지를  2-2 다음에 위치 시킨다.

    3. next 버튼을 누르면 1-1 → 2-2 방향으로 움직인다. 내가 이동할 방향으로 이미지들을 움직여서 위치 시킨다.

       예 ) 1-1 → 2-2 방향으로 움직이면 2-1 이미지를 2-2 다음 이미지 위치로 옮겨 놓는다 .

  

  2. **get방식으로 받은 parameter값을 어떻게 하면 javascript에서 쓸 수 있을까**

  - **해결책**
    - 구글 검색을 통해 코드를 검색함
    
    - js에 slice와 split을 이용해서 함수를 이용해서 구현
    
    - slice(start, cnt) : start 인덱스에서 cnt개 만큼 부분 배열을 만들어 반환해준다.
    
    - split(char) : char를 기준으로 문자열을 나눠 배열을 반환해준다.
    
    - 코드 출처 : <https://kdarkdev.tistory.com/116>
    
      ⁺ 추가
    
      - decodeURIComponent , decodeURI, unescape (encodeURIComponent, encoudeURI, escape) 차이
        - 모두 URI를 인코딩/디코딩 할 때 사용하는 javascript function.
        - 인코딩을 사용하는 이유는 get방식 통신, ajax 통신 등에서 사용할 수 없는 특수문자들이나 한글 깨짐 문제들을 해결하기 위함.
      - unescape(escape)은 deprecated (점점 사용되어지지 않을) 함수이기 때문에 지양하라고 나옴.
      - decodeURI(encodeURI)는 보통 URI(path) 를 인코딩/디코딩할 때 사용하라고 함.
      - decodeURIComponent(encodeURIComponent)는 URI 파라미터를 인코딩/디코딩할 때 사용하라고 나와있음
      - 참고 
        - 인코딩 이유 :  <https://www.hooni.net/xe/study/2195>
        - URI 인코딩 함수들의 차이 : <http://blog.kazikai.net/?p=194>



3. **소수점이 한 자리까지 나오게 하는 방법?**

   - **해결책**

     - toFixed(소수점 자릿수) 으로 사용

     - 나머지 자릿수는 반올림

       ```javascript
       avgScore.toFixed(1);
       ```

     - 참고 : <https://squll1.tistory.com/entry/javascript-소수점-자리수-지정자르기>

       

4. **a 태그를 눌렀을 때, 화면이 상단으로 움직이는 것을 방지하려면?**

   - **해결책**

     - href = javascirpt:를 사용한다.

       ```javascript
       <a href="javascript:" class="bk_more _open">
       ```

     - 참고 : <https://blog.hanumoka.net/2018/08/23/html-20180823-html-a-tag-no-action/>

