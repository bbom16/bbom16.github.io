---
layout: post
title: "한이음 - 미세먼지 정보 어플(0719 - 0722) : list방식을 서버에서 받는 방법 & 가장 가까운 위치 찾기(mysql)"
tags: [spring, java, 한이음, mysql]
comments: true
---

### 2019.07.19

- list와 arrayList 차이 
  - 간단하게 얘기하면 list는 interface, arrayList는 list를 상속받은 클래스이다. 
  - 따라서, 다른 list 를 상속받은 클래스(LinkedList,, 등)에서도 사용가능하다.
  - 자세한 설명 참고 : <https://galgum.tistory.com/18>



### 2019.07.22

- 파라미터 값을 list로 어떻게 받을 지 고민함

  - get으로 보면 string으로 & 연결로 쭉 보내서 parsing해서 사용해야함

    - 너무 번거럽고 복잡할 것 같음

  - **post 방식으로 받는 경우 (채택)**

    - post방식으로 받고, list 용 class를 따로 구현해서 그 객체의 list로 값을 받았다. 

    

- point type을 계속 사용하려 했지만 spring에 mapping 시키는 문제

  - 문제를 해결하지 못하고 그냥 db값을 double로 바꿈

  

- json에서 날짜 타입 제대로 출력안되는 경우 발생

  - dto에 @JsonFormat(pattern="yyyy-MM-dd HH:mm:ss",timezone = "Asia/Seoul") 사용
  - timezone 사용 안하면 시간 제대로 안나옴

  

- 측정한 값 중에서 사용자의 위치값이랑 가장 가까운 위치를 어떻게 찾아서 알려줄 것인가

  - 아래 블로그 참고해서 정해진 범위 안에서 가장 가까운 거리를 구함.

    참고 : <https://pnot.tistory.com/8>

    ```sql
    SELECT id, dust, x_location_info, y_location_info, measure_time, 
    (6371*acos(cos(radians(37.4685225))*cos(radians(x_location_info))*cos(radians(y_location_info)
    	-radians(126.8943311))+sin(radians(37.4685225))*sin(radians(x_location_info))))
    	AS distance
    FROM dust_info
    HAVING distance <= 220
    ORDER BY distance limit 1;
    ```

    - 37.4685225, 126.8943311 임의의 값 (sql문에 작성할 때 :x, :y 로 대체 예정)

    - x_location_info, y_location_info는 위도와 경도

    - WHERE절이 아닌 HAVING절 사용 이유는 별칭은 WHERE절에서 모름.

      

  - 문제 해결 중 시간과 거리 중 어느 걸 더 우선순위로 둬야할지 고민

    - 시간이 더 중요하므로 시간에서 먼저 거르고 그 후 거리로 찾음

      - 시간은 NOW()함수를 이용해서 현재 시간부터 1시간 이내 값으로 구함

        (이유는 공공데이터가 1시간에 한번씩 값을 보내주기 때문에 그 전 값으로 구하려 함.) 

        ```sql
        where measure_time > (NOW() - INTERVAL 1 HOUR)
        ```

        *개선사항 :  그 시간대 (4시 35분이면 4시)에 구한 시간만 찾는 방법으로 개선하면 더 좋을 것 같음*

    - 서브쿼리 날리는 중 아래와 같은 에러 발생

      **Every derived table must have its own alias**

      - 서브쿼리에 이름이 없어서 발생한 에러 서브쿼리에 이름을 붙여주면 해결된다.

        ```sql
        FROM (
        	SELECT * 
        	from dustInfo
        	where measure_time > (NOW() - INTERVAL 50 minute)
        )recent
        ```

        

참고 

- post방식으로 List 받는 법 : <https://m.blog.naver.com/PostView.nhn?blogId=21thjojo&logNo=110189156614&proxyReferer=https%3A%2F%2Fwww.google.com%2F>
- mysql 정해진 시간 이내에 데이터 가져오기 : <[https://webisfree.com/2016-05-09/[mysql\]-%EB%82%A0%EC%A7%9C-%EC%9D%B4%EC%A0%84-%EB%98%90%EB%8A%94-%EC%9D%B4%ED%9B%84-%EB%B3%B4%EC%97%AC%EC%A3%BC%EB%8A%94-%EC%BF%BC%EB%A6%AC%EB%AC%B8](https://webisfree.com/2016-05-09/[mysql]-날짜-이전-또는-이후-보여주는-쿼리문)>