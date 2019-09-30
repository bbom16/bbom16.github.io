---
layout: post
title: "부스트코스 - 기존 reservation spring boot로 바꿔보기"
tags: [부스트코스, 웹]
comments: true
---

### Spring boot 로 바꾸기 

기존 spring mvc로 만들었던 네이버 예약 시스템을 spring boot로 바꾼다. spring  boot 프로젝트를 다시 만들고 그 안의 파일들을 수정하면서 진행하였다. 처음 spring boot는 <https://start.spring.io/> 에서 다운받아서 생성하였다. 

수정할 때 신기했던 것은 전에 만들었던 config파일들이 전혀 필요없다는 점이었다. 전혀 신경쓰지않아도 알아서 만들어주는게 정말 신기하고 편리하였다. 

spring boot는 따로 base url이 없었다. 전에 mvc로 만들었을 땐 프로젝트 명이 base url이었는데 없어서 당황스러웠다. 찾아보니 application.properties에 아래와 같이 추가하면 base url이 설정되었다.

```properties
server.servlet.context-path=/reservation
```

  

jsp를 spring boot의 내장 was가 지원 하지 않기 때문에 기존에 만들었던 jsp 파일들을 다시 html 파일로 변경 후 thymeleaf를 이용하여 수정했다. thymeleaf 문법을 알아볼 수 있다.

#### thymeleaf 문법 간략 정리

보통 th: 로 시작하여 html tag 안에 작성한다. 기본적인 if, each , switch 등이 있고, href 나 onclick도 있다.

##### if 와 unless

```html
<div th:if="${data.cnt} == 0">
  <p> 한번도 방문하지 않았습니다.</p>
</div>
<div th:unless="${data.cnt} == 0">
  <p> 방문한 적 있으시군요:-)</p>
</div>
```

여기서 주의할 점은 unless에도 if와 같은 조건을 써줘야한다. unless는 써져있는 조건이 아닌지를 확인한다.

##### text, value

```html
<div th:text=${data.color}></div>
<input type="hidden" th:value=${data.name}/>
```

각각 text와 value값을 서버에서 받은 값으로 동적으로 설정할 수 있다.

##### each

```html
<div th:each="item : ${itemList}">
  <div th:text="item"></div>
</div>
```

 배열이나 리스트에 들어있는 값을 반복적으로 사용해야 할 때 사용할 수 있다.

##### switch/case

```html
<div th:switch="${type}">
  <div th:case="1"></div>
  <div th:case="2"></div>
  <div th:case="3"></div>
</div>
```

switch case도 사용가능하다.

##### href

```html
<!-- 정적인 값만 있을 때 -->
<a th:href="@{http://bbom16.github.io}"></a>"
<a th:href="@{detail}"</a>

<!-- 파라미터 있을 때 -->
<a th:href="@{detail(id=1)}"></a>
<a th:href="@{detail(id=${id})}"></a>
```

위와 같이 사용할 수 있다. 파라미터 있을 때에 서버에서 받아온 값을 사용하려면 ${id} 처럼 사용하면 가능하다.

##### onclick

```html
<button type="button" class="bk_btn" th:onclick="|javascript:goReserve('${id}')|">
```

이 부분이 계속 안되서 엄청 고생했다. 동적인 값인 id를 넣어서 reserve페이지로 이동하려하는데 제대로 동작이 되지않았다.    해결 방법은 javascript 함수를 만들어서 onclick에서 사용했다. . 위에서 goReserve(id)가 정의한 함수이다.

```html
<script>
    /* reserve 페이지로 이동 */
    function goReserve(id) {
        location.href = "reserve?id=" + id;
    }
</script>
```



##### 궁금한 점

검색을 통해서는 찾지 못했는데 dependency를 보다보니 boot 와 mvc에 추가하는 게 차이가 있는 거 같다. 근데 jackson이나 json parser에서 사용하는 건 boot 전용을 찾지못해 그냥 mvc에서 쓰던 걸 그대로 사용하였다. 동작에는 이상이 없지만 그래도 정확히 어떤걸 사용하는게 맞는건지 찾아보자

##### 앞으로 하고 싶은 것

현업에서는 jdbc 보단 jpa를 많이 사용한다고 한다. 이 프로젝트를 jpa로 바꿔보고 로그인 기능도 진짜 로그인이 될 수 있게 수정해보고 싶다. 



##### 출처

- thymeleaf 문법

  <https://elfinlas.github.io/2018/02/18/thymeleaf-loop-index/>

  <https://firstboos.tistory.com/entry/Thymeleaf-에서-자주-사용하는-예제>

  <https://fors.tistory.com/430>

  <https://zamezzz.tistory.com/308>