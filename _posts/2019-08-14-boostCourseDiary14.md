---
layout: post
title: "부스트코스-pjt6 강의 정리 - PJT6-1 파일 업로드"
tags: [부스트코스, 웹]
comments: true
---

# PJT6 강의 정리 - 1

### 1. 파일 업로드

#### 1) file upload의 이해



```html
<input type="file" name="reviewImg" id="reviewImg" accept="image/*">
```

type을 file로 설정하면 됨

accept는 브라우저 지원 범위가 좋진 않은데 허용가능한 범위를 설정할 때 사용함. 이 예제에선 모든 이미지만 가능, image/jpg, image/png 이런식으로 구체적으로도 설정 가능함

일반적으로 form data를 전송하면 http header에는 'application/x-www-form-urlencoded'라는 정보로 노출됨. 실제로 파일 데이터를 넣어서 그냥 전송하면 파일자체가 서버로 전송되는 것이 아니라 파일 이름만 전송이 됨. 

파일까지 제대로 전송하려면 아래와 같이 enctype을 multipart로 설정해줘야됨

```html
<form action="/join" method="post" id="myForm" enctype="multipart/form-data">
```

multipart로 날아갈 땐 boundaryr값이 추가됨. 기본과는 다른 payload로 전송됨. 

추가로 서버에선 또 다르게 처리해줘야됨. 대부분 라이브러리로 지원하니까 어렵지않게 처리 가능

file upload를 ajax통신을 갖고도 가능. FormData라는 속성을 가지고 내용을 넣어서 ajax로도 전송가능하다.





   

#### 2) file upload의 확장자검사 및 썸네일 노출

**확장자 검사 **

이미지 파일의 확장자가 여러개 있지만 제한을 둬서 서버나 UX적인 이유로 제한을 둘 수 있다. 그 외에에도 확장자를 제한 둘 수 있다. 

쉬운 방법은 accept로 ,로 이어서 허용하는 창만 나오게 할 수있다. 

accept가 브라우저마다 지원되지않을 수 있어서 모든 브라우저에서 사용할 수 있는 방법을 알아보자

change 이벤트를 통해서 이것을 감지할 수 있다. 파일의 정보를 받아서 확장자를 확인해 원하는 확장자만 처리할 수 있도록 해준다.

  ```javascript
파일 엘리먼트.addEventListener("change", (evt)=>{
  
})
  ```



**썸네일 노출**

보통 ajax로 미리 서버에 올려서 제대로 전송되면 success를 받고 그 다음에 그 이미지가 저장된 웹브라우저에서 접근할 수 있는 url을 알려줌. 그러면 그 이미지를 받아서 img src에 값을 넣어주면 됨.(src가 바뀌면 바로 이미지가 보이게 되어있음)

편의상 서버에 직접 전송이 아니라 실제 올리기 전에 createObjectURL을 이용해 썸네일 노출 방법으로 알아봄

```javascript
//위에 파일 전송 성공 후에 아래코드로 url 생성후 이미지에 넣어줌
const elImg = document.querySelector(".viewImg"); //이미지 태그
elImg.src = window.URL.createObjectURL(image);
```















 



