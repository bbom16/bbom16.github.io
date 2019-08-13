---
layout: post
title: "한이음 - 미세먼지 정보 어플(0731) : 오픈 API 파싱"
tags: [spring, java, 한이음]
comments: true
---

### 2019.07.31

- 오픈 API로 미세먼지 측정값 받기

  - InputStream으로 url 연결해서 통로 open

  - xmlPullParser를 이용해 원하는 데이터 파싱

    - pom.xml에 dependency 추가
  
    - xmlpull은 인터페이스라 꼭 아래 kxml2도 같이 추가해줘야 사용할 수 있음
  
    참고 : <http://www.xmlpull.org/impls.shtml>
  
      ```xml
      <!-- https://mvnrepository.com/artifact/xmlpull/xmlpull -->
      <dependency>
      	<groupId>xmlpull</groupId>
        <artifactId>xmlpull</artifactId>
        <version>1.1.3.1</version>
      </dependency>
      
      <!-- https://mvnrepository.com/artifact/net.sf.kxml/kxml2 -->
      <dependency>
      	<groupId>net.sf.kxml</groupId>
      	<artifactId>kxml2</artifactId>
      	<version>2.3.0</version>
      </dependency>
      ```
  
    - parser를 선언하여 원하는 값으로 파싱
  
      ```java
      XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
      XmlPullParser parser = factory.newPullParser();
      parser.setInput(is,"UTF-8");
      ```
  
    - eventType으로 상황 구분(예: 문서 시작, 태그 시작 등)하고 tag를 찾아  xml 을 파싱한다.
  
      ```java
      int eventType = parser.getEventType();
      switch (eventType) {
        case XmlPullParser.START_TAG: {
         String tag = parser.getName();
          switch (tag) {
           case "items":
                 break;
           case "dataTime":
               System.out.println(parser.nextText()); //태그 옆에 내용 출력
              	break;
           case "pm10Value":
               System.out.println(tag);
              	break;
           }
         }
       }
       	eventType = parser.next(); //다음 event로 이동
      }
      ```
  
      참고 : <https://sosohantalk.tistory.com/14#recentEntries>
  



  

- java에서 switch case문에서 String 사용하기

  - java 7 이상부터 switch문을 String으로 사용할 수 있다고 함.

  - java 9 인데도 실행이 안되는 문제 발생

    ```
    strings in switch are not supported in -source 1.5
      (use -source 7 or higher to enable strings in switch)
    ```

  - maven java compiler가 1.5 인 것 같아서 compiler version을 1.8로 올려줌
  
    - 아래 코드를 pom.xml 추가
  
    ```xml
    <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.1</version>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    ```
  
    참고 : <http://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html>

