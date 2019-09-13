---
layout: post
title: "한이음 - 미세먼지 정보 어플 : RDS 생성후 서버에 연결하기 + 문제"
tags: [spring, java, 한이음]
comments: true
---



### RDS 생성 후 AWS 서버에 연결하기 

RDS는 AWS에서 제공하는 분산 관계형 데이터 베이스를 말한다. 이 RDS를 사용하여 mysql db를 생성하고 그 db를 전에 만들어놓은 서버에 연결했다. 

1. RDS 생성

   AWS에 접속 후 RDS에서 데이터베이스 생성하기를 클릭한다. 

   mysql을 선택 후 탬플릿에서 **프리티어**를 선택한다. 

   이 후 중요한 것은 마스터유저의 이름과 비밀번호를 꼭 잘 기억해둔다. (생성 후 생성완료 메세지가 위 화면에 뜨고 거기에 보안설정 어쩌구,,하는 버튼이 보인다. 거기 클릭해서 이름,비번, 앤드포인트를 잘 확인하자)

   그 이후엔 데이터베이스 이름, 백업 등을 선택해주면 되는데 혹시 다른 금액이 추가될까봐 따로 선택하지않았다.

   #### 그리고 정말정말 중요한 것은 

   추가 연결 구성에서 **퍼블릭 엑세스**를 예로 설정해준다. 

   (이거 설정안해서 뒤에 나오는 workbench에서 연결이 계속 안됐다ㅜㅜㅜㅜㅜㅜㅜ 꼭 설정해주자)

     

2. 기타 보안설정 + 파라미터 설정해주기

   파라미터 그룹에서 한글에 필요한 파라미터들을 설정해준다 

   파라미터 생성 후 character로 필터링

   character_set_client

   character_set_connection

   character_set_database 							→  **utf8** 로 설정해준다.

   character_set_filesystem

   character_set_results

   character_set_server

      

   collation 필터링

   collation_connection										→ **utf8_general_ci**로 설정

   collation_server

   

   \+ 시간대를 서울 시간대로 맞추기 위해 zone 필터링 후 time_zone을 Asia/Seoul로 설정  

   

   모두 설정했다면 아까 생성한 데이터베이스에서 파라미터 그룹을 만든 그룹으로 변경해준다.   

   

   보안 설정은 보안 그룹 생성 후 db와 관련된 3306에 인바운드 가능한 ip를 설정해주면 된다.   

    

3. workbench에서 연결하기

   위 이미지에 맞게 해당하는 곳에 알맞은 값을 입력후 ok버튼을 누르면 된다. endpoint는 어쩌구어쩌구 .amazone.com으로 끝난다.

4. 연결까지 하면 완료!! ◡̈◡̈



### 그 외 문제

로컬에선 측정시간이 제대로 나오는데 aws서버에서 값이 9시간 후로 나오는 문제가 있었다. mysql에서도 제대로 값이 나와서 확인하니 dust dto에 측정시간에 jsonFormat( , Asia/Seoul)로 설정되어있던 것을 없애니 제대로 값이 나왔다. (로컬에선 9시간 전으로 나옴,,, 대체 왜,,,??????)

왜 이런 문제가 생겼는지 이유는 정확히 모르겠다. 아마 9시간이 더 추가되는 거 같은데 정확한건 더 찾아보고 추가하기

\+

이유는 톰캣이었다. 입력할 때도 시간은 제대로 못받아와서 찾아보니 tomcat timezone이 설정이 안되어있는 문제였다. 수정 후 jsonFormat에 timezone 추가하니 정상적으로 잘 나오고 입력도 제대로 잘 받았다ㅎㅎㅎㅎㅎ

```sh
vi tomcat8/bin/setenv.sh
```

```sh
#!/bin/bash 
export CATALINA_OPTS="$CATALINA_OPTS -Dfile.encoding=UTF8 -Duser.timezone=GMT+9"
```







