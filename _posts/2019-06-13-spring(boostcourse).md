```
layout: post
title: "부스트코스-예약페이지 만들기2(0613)"
tags: [spring, java]
comments: true
```

###java에서 int와 Integer 차이

#### 1. int 

- primitive 자료형
- 산술 연산이 가능하다
- null로 초기화 xxxx



#### 2. Integer

- Wrapper클래스(객체)
- null값 처리가 용이해 SQL과 연동 시 처리가 용이하다.
- 직접적인 산술연산 xxxx



### boxing과 unboxing을 이용해 변환 가능하다.  

####- boxing : primitive -> Wrapper class

####- Unboxing : Wrapper class -> primitive



— 이미지랑 join해서 dao 작성해야됨!!!! 



```
Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column ‘db.table.col’ which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
```

에러 발생 시 

```
mysql > SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```

실행!! 