---
layout: post
title: "한이음 - 미세먼지 정보 어플 : user table 추가 + measurementStation table 수정 "
tags: [spring, java, 한이음]
comments: true
---

### measurementStation 테이블 수정 

x_location_info  -  y_location_info 를 unique key로 설정

미세먼지 측정 정보 입력받을 때 측정소 정보가 없으면 새로 등록, 있으면 측정소 id 받아서 저장함.

-> 이유는 미세먼지 지도에서 측정소 정보를 보여줄 때 현재 유효한(90분 이내에 측정된 정보) 수치가 측정된 측정소만 확인하기 위해 측정소 정보를 저장한 후 사용함.

\+ 측정소 정보를 on duplicate key update를 이용하여 있으면 update하게 사용하려 했는데 어차피 id값을 return 받아야해서 굳이 구현하지않음.  또 이렇게 구현했을 때 simpleInsertAction을 사용할 수 없어 insert됐을 때 id값을 확인하기 어려움. 만약 구현할거면 더 검색이 필요함.



### User 테이블 추가 및 관련 dao, service, controller 작성하기

미세먼지 push 기능을 위해 안드로이드 어플에서 사용자의 정보를 받아와 저장한다. 아이디 + 비밀번호, 사용자의 위치, 설정값들을 받아 저장한다. 

push를 보내기위해선 각 핸드폰에 토큰 값이 필요하다. 그 값을 user table에 같이 저장할 것이고, 이 값을 unique key로 사용한다.(토큰 당 하나씩만 가능)

**에러 발생**

```
nested exception is java.sql.SQLException: Field 'id' doesn't have a default value
```

이유는 id 필드가 not null 인데 빈값이 들어왔기 때문이다. 처음 테이블 생성시 auto_increment로 생성했어야하는데 그부분 누락으로 생긴 에러

auto_increment 속성 추가로 문제 해결했다.

##### \+ 더 생각해볼 것

1. id에도 유니크 속성을 줘야 할 것 같다. 전체적인 서비스과정 조원들과 상의해보기
2. passoword() 함수를 사용해서 보내줄 것인지 내가 받아서 사용할 것인지 

  

### 측정소 위치 + 마지막에 측정된 미세먼지 수치 같이 보내주기

미세먼지 지도를 구현할 때, 측정소 위치 뿐만 아니라 미세먼지 수치까지 같이 필요하다. 그렇기 때문에 미세먼지 수치도 측정소 정보와 함께 api로 내려주려고한다. 문제는 이제 측정소에서 측정된 많은 데이터 중에 최신 데이터들만 모아서 inner join을 해야한다는 것이었다. 그 문제를 해결하기 위해서 서브트리의 서브트리로 inner join을 해서 너무 복잡한 쿼리로 작성되어서 수정이 필요함.

```sql
select m.id, m.name, m.x_location_info, m.y_location_info, max_time, r.dust
from measurementStation m
INNER JOIN (
	select max_time, d.dust, d.measurement_id
	from dustInfo d, 
	(
	select max(measure_time) as max_time, measurement_id from dustInfo 
	where measure_time > (NOW() - interval 70 minute)
    group by measurement_id
	) as t
	where d.measure_time = t.max_time and t.measurement_id = d.measurement_id
)r ON m.id = r.measurement_id;
```

그래서 inner join + inner join 으로 수정했다. 

```sql
select m.id, m.name, m.x_location_info, m.y_location_info, max_time, d.dust
from measurementStation m
INNER JOIN dustInfo d ON d.measurement_id = m.id
INNER JOIN (
	select max(measure_time) as max_time, measurement_id from dustInfo 
	where measure_time > (NOW() - interval 70 minute)
    group by measurement_id
)recent ON recent.max_time = d.measure_time and recent.measurement_id = m.id;
```

\+

그 후 측정소 정보가 전부 나오지 않는 문제가 나타났다. 이유는 시간대 안에 측정 정보가 없는 경우 그 측정소 정보가 자체가 안나오기 때문이었다. 이 문제를 해결하기 위해서 inner join 대신 outer join 을 사용했다.

outer join은 left outer join, right outer join, full outer join, 

```mysql
select m.id, m.name, m.x_location_info, m.y_location_info, max_time, r.dust
from measurementStation m
Left OUTER JOIN (
	select max_time, d.dust, d.measurement_id
	from dustInfo d, 
	(
	select max(measure_time) as max_time, measurement_id from dustInfo 
		WHERE measure_time > (NOW() - INTERVAL 80 Minute)
    group by measurement_id
	) as t
	where d.measure_time = t.max_time and t.measurement_id = d.measurement_id
)r ON m.id = r.measurement_id;
```

이 쿼리는 서브쿼리를 사용하여 작성하였다. 위에 작성한 것처럼 join문으로 연결할 경우 제대로 동작하지 않았다. 성능 개선이 필요하다.

