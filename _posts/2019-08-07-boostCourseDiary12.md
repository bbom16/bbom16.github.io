---
layout: post
title: "부스트코스-pjt5 FE 구현하기 "
tags: [부스트코스, 웹]
comments: true
---

# PJT5 FE 구현하기

### HTML을 JSP 파일로 바꾸기

이번 프로젝트부터 session을 이용해줘야해서 html 파일을 전부 jsp 파일로 바꿔주었다. 

**문제**

1. jsp파일에서 script부분에 작성해놓은 \${}을 제대로 인식하지 못했다

   -  \${}을 {{ }}로 바꿔서 문제를 해결

2. jsp파일을 WEB-INF에 views폴더에 저장했다. a태그에 detail.jsp로 넘어가게 구현하려했는데 jsp파일을 찾지 못했다.

   WEB-INF폴더는 중요한 설정파일들을 넣는 곳이라 외부에서 접근을 못하게 되어있다. 그렇기 때문에 파일을 찾을 수가 없다. 

   - /detail로 보내주고 Controller에서 detail.jsp를 찾아가게 함

3. displayInfoId를 jsp파일로 보낸 id값으로 js에서 받기 

   전에 getParameter로 받았던 것을 jsp를 이용하여 변경하려함

   - 아래와 같게 변경하라고 했지만 js파일이 분리되어있어서 제대로 실행이 안됨

   - ```javascript
     var a = "${id}";
     var a = "<%=id%>";
     ```

   - input hidden으로 설정하여 값을 넣어놓고 document.querySelector를 이용하여 값 받아옴

     ```html
     <input type="hidden" id="displayInfoId" value="${id }">
     ```

     ```javascript
     var id = document.querySelector("#displayInfoId").value;
     ```





### UI별로 객체 구성하기

ui별로 객체 구성하는 것에 대해서 어떻게 나눌지 고민을 많이 했었다. ajax를 요청해서 데이터를 처리하는 부분이 중복되어 값을 받아와야하는 ui와 예약 form을 구성하는 ui로 나누어 객체를 생성하였다.

-> 이렇게 구성하였지만 ajax 요청을 따로 나누고 거기서 각각 객체들을 호출하여 값을 보여주는 방식으로 변경해도 괜찮을 것 같다



### ajax로 받은 배열을 한번에 보여주기

전시할 때 하나의 전시에 여러 가격이 존재했다. 이 가격들을 handlebar를 이용해 매핑 시키는데 가격을 한 번에 보여주고 싶고, 그 값이 3개로 정해져있었다.

**문제**

1. 가격을 어떻게 for문으로 여러 태그를 만드는 것이 아니라 하나의 태그에 한번에 다 보여주고, 3개를 만족시켜서 보여줄 수 있을까

   - handlebar에 helper를 이용

     기본적인 If나 each 외에 사용자가 정의해서 helper를 사용할 수 있다. 앞에 함수를 사용하고, 그 뒤에 파라미터값을 주는 방식으로 값을 나올 수 있게 했다. 

     helper는 javascrip 코드에 아래와 같이 정의해서 사용할 수 있다.

     ```handlebars
     //html
     {{price id}}
     ```

     ```javascript
     //js
     Handlebars.registerHelper('setPrice', (index)=>{
     	let len = displayInfoObject.prices.length;
     	if(len <= index) {
     		return displayInfoObject.prices[len-1].price;
     	}
     	return displayInfoObject.prices[index].price;
     });
     ```

     위 코드는 price에 개수가 3개보다 적을 때도 값이 다 나올 수 있게 구현하였다. 설계서에 구체적이지 않은 부분들이 있어 임의로 예시 화면에 맞춰서 개발함.

   

### 유효성 체크

email과 전화번호에 대해 유효성 체크를 했는데 유효성 체크 시점을 다른 곳을 클릭할 때, 즉 포커스가 떠날 때로 선택하여 유효성체크를 진행하였다.

```javascript
.addEventListener("focusout", ()=>{
  
})
```



### 예약하기 버튼 활성화

언제 예약하기 버튼을 활성화해야할지 고민했었는데 이용약관 체크만 되면 예약버튼을 활성화 시켰다.

대신 폼 제출 전에 유효성체크 + 티켓 수량 선택을 다시 확인하여 만족하지 않으면 제출하지못하도록 구현하였다.





### replace를 정규표현식을 사용하여 replaceAll로 사용하기

```javascript
var str = "2019.12.31";
str.replace(/\./gi, "-");
```

g : 전역검색

i : 대소문자 상관없이



### 특정 노드의 자손을 class 이름으로 찾기

```javascript
var ul = document.querySelector("ul");
ul.getElementsByClassName("img");
```

ul의 자손 중 img라는 이름의 클래스를 가진 요소들을 다 찾을 수 있다. 주의할 점은 html Collection으로 배열형태이기 때문에 하나밖에 없더라도 꼭 index로 접근하자

비슷한 함수로 tagName으로 검색할 수 있는 getElementsByTagName()도 있다.



### jstl if-else 사용하기

jstl은 if만 있고, else if나 else는 따로 없다. 그래서 if-else로 사용하기 위해 비슷한 choose를 사용했다. 

```jsp
 <c:choose>
		<c:when test="${name == '탐탐'}"> 
      <p> 나는 탐탐이야 </p>
   </c:when>
	<c:otherwise> 
    <p> 난 탐탐 아니야 </p>
  </c:otherwise>
</c:choose>

<!-- ---------------- eq -> equals와 동일 ----------------------- -->
<c:when test="${name eq '탐탐'}"></c:when>

```





### JavaScript에서 날짜 비교

숫자 비교와 마찬가지로 > , < , ==, === 연산자를 이용하여 비교해주면 된다.

너 나중 시점이 큰 것이라고 생각하면 된다.

```javascript
var today = new Date(); //2019-08-13
var newYear = new Date(2019-01-01);

today > newYear // true 
```













- 참고

  jsp 파일 controller로 연결 <https://all-record.tistory.com/165>
  
  handlebars 사용자 정의 helper <https://sailboat-d.tistory.com/30>
  
  replace 정규표현식 이용해서 사용 <http://www.codejs.co.kr/자바스크립트에서-replace를-replaceall-처럼-사용하기>
  
  getElementsByClassName <https://begindeveloper.tistory.com/entry/자바스크립트DOM-노드-다루기>
  
  jstl choose <https://fruitdev.tistory.com/131>
  
  js 날짜 비교 <https://gafani.tistory.com/entry/자바스크립트-날짜-비교-이전-이후-날짜-계산하기>
  
  