---
layout: post
title:  "JAVA-UbuntuMaven"
date:   2018-07-07 12:00:00
author: 강진광
categories: [Aws]
comments: true
tags:
  - AWS
  - EC2
  - Putty
  - AWS RDS
  - Puttygen
  - Ubuntu
  - Putty
  - Maven
---
# [우분투 환경에서 Maven으로 프로젝트 배포하기]

### tomcat8-admin 설치
> sudo apt-get install tomcat8-admin

### 설치가 끝나면 톰캣의 설정 파일을 수정해야 한다
> cd /etc/tomcat8 폴더로 들어가기

### tomcat-user.xml 파일 열기
> sudo nano tomcat-user.xml
### 안에 
~~~xml
<!--</tomcat-users> 위에 설정 내용을 입력한다.-->
 <role rolename="manager-script"/>
 <role rolename="manager-gui"/>
 <role rolename="manager-jmx"/>
 <role rolename="manager-status"/>
 <user username="관리자 계정명" password="비밀번호" roles="manager-gui,manager-script,manager-status,manager-jmx"/>

~~~
### 입력한다. 다음에
>sudo vi /etc/tomcat8/server.xml
### 파일을 연뒤에 port를 8090으로 변경시킨다.
### tomcat8 시작 
> sudo service tomcat8 start<br>
> sudo service tomcat8 restart<br>
> sudo service apache2 restart
### 를 한뒤에 http://서버 IP주소:8090/manager 로 접속 후 관리자 계정명/비밀번호를 작성한다. manager page가 나와야 한다.

