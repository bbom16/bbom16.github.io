---
layout: post
title: "부스트코스-예약페이지 만들기(0709)"
tags: [spring, java]
comments: true
---

### 2019.07.09

- #####문제 상황

  1. pjt4 진행중 pjt3 수정이 필요. git에 pjt4까지 올려놓은 상태

     pjt3 마지막 수정 commit으로 checkout 후 수정하고 commit 했지만 master로 checkout하면 사라짐

  - 해결책

    pjt3 마지막 commit으로 checkout  한 후 branch 생성해서 수정 후 merge 할 예정

    

  2. 프로모션 배너 작동이 제대로 되지않을 때가 있음

  - setTimeout -> requsetAnimaionFrame으로 수정
  
  - setTimeout, setInterval 은 시간만 생각하고, 프레임은 고려하지않는다.
  
  - **requestAniamationFrame**
    - 실제 화면이 갱신되어 표시되는 주기에 따라 함수를 호출해주기 때문에 자바스크립트가 프레임 시작 시 실행되도록 보장
    
    - 현재 창에 표시되는 경우만 애니메이션 실행 
    
    - requestAnimationFrame(callback함수)로 사용.
    
    - 재귀적으로 사용하여 무한슬라이딩 구현
    
      
  
  3. .gitignore file 수정
  
  - 아래 명령어 입력 후 push
  
    ```c
    git rm -r --cached .
    git add .
    git commit -m "Apply .gitignore"
    ```



- 참고 

  - .ginignore:  <https://nesoy.github.io/articles/2017-01/Git-Ignore>
  - requestAnimation 과 setTimeout 차이 : <https://fullest-sway.me/blog/2019/01/28/requestAnimationFrame/>

  

​		