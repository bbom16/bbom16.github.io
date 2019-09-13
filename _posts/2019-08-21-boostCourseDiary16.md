---
layout: post
title: "부스트코스-pjt6 강의 정리 - PJT6-3 
파일 업로드"
tags: [부스트코스, 웹]
comments: true
---

# PJT6 강의정리 6-3



### 3. 파일 업로드 & 다운로드

##### Multipart

웹 클라이언트가 요청을 보내면 http 바디 부분에 데이터를 여러부분으로 나눠서 보내는 것을 의미

웹 클라이언트가 전송하는 multipart 데이터를 읽어들이는 메소드를 httprequestServlet은 제공x 

input stream만을 제공하기 때문에 사용자가 multipart 부분을 잘 나눠서 사용해야됨. 실제 구현하는 것이 아니라 아파치에서 제공하는 commons-fileupload를 사용함

  

##### Spring MVC에서 파일 업로드

몇가지 라이브러리와 설정을 추가해야됨

commons-fileupload, commons-io 라이브러리 추가

MultipartResolver Bean 추가

DispatcherServlet은 준비과정에서 'multipart/form-data'가 요청이 올 경우 MultipartResolver를 사용



##### 파일 업로드 폼

```html
<form method="post" action="/file" enctype="multipart/form-data">
  <input type="file" name="file">
</form>
```

type 이름이 file이고, name이 같은 input이 여러개이면 배열형태로 전송됨



##### Controller에서 처리

업로드 하나면 @RequestParam("file") MultipartFile file

업로드 여러개 @RequestParam("file") MultipartFile[] files

MultipartFile의 메소드르 이용하면 파일이름, 파일크기 등을 구하고 InputStream을 얻어 파일을 서버에 저장

  

##### 다운로드

서버의 특정 디렉토리는 외부에서 접근할 수가 없는데 이런 파일을 외부에서 사용할 수 있도록 다운로드 기능 제공

파일을 읽어 HttpServletResponse의 OutputStream으로 출력

