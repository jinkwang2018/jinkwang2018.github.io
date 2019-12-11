---
title:  "윈도우에 apache, php 설치하고 연동하기"
tags: apache, php
key: post-apache-php
---
# 설치 순서
1. 압축파일을 다운로드하여 압축 해제한다.
2. apache를 설치한다.
3. PHP를 설치한다.

# 압축파일 다운로드

https://www.apachelounge.com/download/ -> 아파치 다운로드 링크
https://windows.php.net/download/      -> PHP 다운로드 링크(Thread Safe 사용을 권장합니다.)
zip파일 다운로드 한 뒤 폴더를 정리하여 압축을 풀어줍니다.
저는 apache 는 c:/apm/Apache24/ php는 c:/apm/php7 의 경로에 압축을 풀어주었습니다.
![folder](https://drive.google.com/uc?id=1y-8CRZBs1WykZmvECSApvZ_6eN999qsl)
경로는 상관이 없지만 인지하고 있어야 이 후 설정에서 사용할 수 있습니다.

# apache 설치하기
/conf/httpd.conf을 열어서
ServerRoot, Listen, DocumentRoot 의 값을 현재폴더 경로에 맞게 변경합니다.
![folder](https://drive.google.com/uc?id=1EupD_UpqF_MObawSjplvCaUYKv3dyMbG)

```bash
...
#
# ServerRoot: The top of the directory tree under which the server's
# configuration, error, and log files are kept.
#
# Do not add a slash at the end of the directory path.  If you point
# ServerRoot at a non-local disk, be sure to specify a local disk on the
# Mutex directive, if file-based mutexes are used.  If you wish to share the
# same ServerRoot for multiple httpd daemons, you will need to change at
# least PidFile.
#
Define SRVROOT "c:/apm/Apache24"

ServerRoot "${SRVROOT}"

...

#
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, instead of the default. See also the <VirtualHost>
# directive.
#
# Change this to Listen on specific IP addresses as shown below to 
# prevent Apache from glomming onto all bound IP addresses.
#
#Listen 12.34.56.78:80
Listen 80

...

#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "${SRVROOT}/htdocs"
<Directory "${SRVROOT}/htdocs">
...
```

마지막 부분에 아래의 내용을 추가로 넣어줍니다.
```bash
PHPIniDir "C:/apm/php7"
LoadModule php7_module "c:/apm/php7/php7apache2_4.dll"
AddType application/x-httpd-php .html .php
AddHandler application/x-httpd-php .php
```

이후 명령프롬프트를 관리자모드로 실행한 뒤에 apache폴더 안의 bin폴더까지 이동해줍니다. ex) cd c:/apm/Apache24/bin
설치 명령어를 입력해 줍니다.
```bash
httpd.exe -k install -> 설치
httpd.exe -k ununstall -> 삭제
```
설치 완료 후 환경변수 Path에 bin폴더까지의 경로를 추가한 후 실행한다.
![folder](https://drive.google.com/uc?id=1D-uRDTdHad4gEVDTm5fgOx1PPaCOMf5M)
![folder](https://drive.google.com/uc?id=1O9bmuQAbvOmA9Z3ig1FxE76GheqT6GLB)
![folder](https://drive.google.com/uc?id=1mX29h9eAp79IHEaKyADSuDr-6Z4-dupQ)

```bash
httpd -k start -> 실행
httpd -k stop -> 중지
```

# apache 설치 중 ERROR 
1. (OS 10013)액세스 권한에 의해 숨겨진 소켓에 액세스를 시도했습니다.  : AH00072: make_sock: could not bind to address [::]:8080
   -> 서비스 들어가서, world wide web publishing service 를 중단 후 삭제한 뒤 다시 설치합니다.
![folder](https://drive.google.com/uc?id=1QJF8JG2GMe_7Q5EMtp3PfNbrC2pnx7TA)
2. AH00558 : Could not reliable determine the server’s fully qualified domain name   
   -> httpd.conf의 ServerName의 주석 해제 후 hostname을 입력 로컬에서만 사용한다면 localhost로 변경합니다.
```bash
#
# ServerName gives the name and port that the server uses to identify itself.
# This can often be determined automatically, but we recommend you specify
# it explicitly to prevent problems during startup.
#
# If your host doesn't have a registered DNS name, enter its IP address here.
#
ServerName localhost:80
```

# 윈도우에 PHP 설치하기


php.ini-production파일을 아래와 같이 변경합니다.
```bash
; Directory in which the loadable extensions (modules) reside.
; http://php.net/extension-dir
;extension_dir = "c:\apm\php7\ext"
```
변경 후 php.ini-production 파일의 이름을 php.ini파일로 변경해줍니다.

# 확인하기
apache폴더의 htdocs폴더에 아래의 내용으로 phpinfo.php파일을 생성한 뒤에
브라우저에서 localhost/phpinfo.php를 입력하였을 때 php페이지가 나오면 설치가 완료된 것입니다.
```bash
<?php 
phpinfo(); 
?>
```