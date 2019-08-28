---
layout: post
title: "한이음 - 미세먼지 정보 어플(0805) : Spring Mvc AWS 배포하기!"
tags: [spring, java, 한이음]
comments: true
---

### 2019.08.05



#### AWS를 이용해 Spring MVC 배포하기(EC2)

공공데이터 api까지 기능이 완성되어 안드로이드에서 테스트를 위해 aws로 배포를 하기로 했다. 처음 한 일은 aws 홈페이지에 가입해 프리티어를 받았다. aws 회원 가입할 때 신용카드 크레딧 정보를 꼭 입력해야 한다.

아래 블로그를 따라 순서대로 선택하여 aws에서 인스턴스를 설정했다.

헷갈렸던 부분은 보안설정을 어떻게 할지 고민했었는데 80, 8080은 어디서나 접근할 수 있게 했고 나머지는 내 ip에서만 접근 가능하게 했다 

mac은 putty 설치할 필요없이 키 파일 있는 폴더로 들어가서 ssh 접속해주면 가능하다.

(ssh 접속은 본인 인스턴스 들어가서 연결 누르면 나오는 ssh -i 어쩌구로 하면됨)

aws에 tomcat과 java를 설치해줘야하는데 이 때 java  설치 시, oracle에 회원 가입 해야한다. 

각 홈페이지에서 다운 받은 후 filezila를 통해 서버로 옮겨주었다. 

내 java와 tomcat 경로는 /usr/local/ 아래에 각각 위치해 있다. 이 때, 접근을 위해 접근 권한 수정이 필요하다. 



마지막으로 war파일을 tomcat webapps 파일에 넣어주면 완료다! 

war파일을 만드는 방법은 intellij에서

 projet structure -> artifacts 에서 + 버튼을 눌러 Web application:archieve -> war 생성을 누르면 됨

그 후 빌드에서 Build Artifacts를 눌러 빌드하면 완료!



다음 시간엔 RDS를 통한 db 연결을 할 예정이다!

- 참고 

전체적인 배포 흐름 : <https://windosakacastle.tistory.com/12?category=691705>

tomcat / java 설치 : <https://jinhongstar.tistory.com/30>