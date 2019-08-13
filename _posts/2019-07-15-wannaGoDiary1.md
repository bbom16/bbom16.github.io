---
layout: post
title: "한이음 - 미세먼지 정보 어플(0715) : intelliJ spring 시작하기 & db 생성"
tags: [spring, java, 한이음 ]
comments: true
---



### 2019.07.15

한이음에서 미세먼지 정보를 확인할 수 있는 어플을 만드는 프로젝트에 참여했다.

- 어플에 들어가는 데이터를 택배차량이 측정하여 그 결과값을 어플로 확인할 수 있다. 
  - 더 정확하게 미세먼지 수치를 확인할 수 있다.

- 측정하는 기계는 아두이노를 사용할 것이고, 어플은 안드로이드용으로 제작할 예정이다.
- 나는 아두이노와 어플이 통신하는 서버를 구축하고, 아두이노 기계를 제작할 예정이다. 

오늘은 그 첫번째 서버를 구축하려한다. 전에 네이버 예약서비스를 만들 땐 eclipse를 이용하여 만들었는데 이번엔 IntelliJ를 이용하여 서버를 구축할 것이다. 

IntelliJ spring 프로젝트를 생성하였다. 

- .iml 파일에 sourceFolder를 수정하면 working directory를 수정할 수 있다. package에 오류가 생긴다면 이 파일을 확인하자

  ```xml
  <content url="file://$MODULE_DIR$">
        <sourceFolder url="file://$MODULE_DIR$/src/main/java" isTestSource="false" />
        <sourceFolder url="file://$MODULE_DIR$/src/main/resources" type="java-resource" />
        <sourceFolder url="file://$MODULE_DIR$/src/test/java" isTestSource="true" />
        <excludeFolder url="file://$MODULE_DIR$/target" />
  </content>
  ```

- IntelliJ getter, setter 함수 생성 단축키 

  - command + n 

  

mysql을 이용하여 db를 생성

```mysql
create table dustInfo(
id int unsigned auto_increment not null primary key,
location_info point not null,
measure_time timestamp default now()
);
```

point 조회 시 ST_X(), ST_Y() 함수를 이용하면 gps의 경도, 위도 값을 확인할 수 있다.



- 가장 최신 데이터 1개만 select 하는 쿼리문

``` mysql
SELECT id, ST_X(location_info) as x ,ST_Y(location_info) as y
from dustInfo
order by measure_time desc limit 1;
```



- 해결할 문제
  - 내 위치에서 가장 가까운 위치를 구하려면 쿼리문 어떻게 작성 해야할까

참고 

- IntelliJ spring 시작하기 :  <https://cjh5414.github.io/intellij-spring-start/>
- db에 gps값 저장하기/사용하기  : <https://purumae.tistory.com/198>

