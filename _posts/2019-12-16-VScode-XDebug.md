---
title:  "XDebug in VScode"
tags: VS code
key: post-vscode
---
# vscode 
vscode에서 php파일을 불러오자 아래와 같은 error가 나타났습니다.

- error "PHP 실행파일이 설정되지 않았기때문에 유효성 검사를 할 수 없습니다." 

해당 에러는 settings.json의 설정이 현재 설치된 php의 위치와 달라서 생기는 에러입니다.

해결방법은 다음과 같습니다.
- ctrl + , 로 설정 연다
- php.validate.executablePath 검색 후 settings.json에
"php.validate.executablePath": "c:/apm/php7/php.exe",     -> php설치경로
"php.validate.run" : "onType" 
추가한다.

이후 vscode를 재실행하면 에러가 사라집니다.

# XDebug in VScode

vscode에서 Xdebug를 사용하기 위한 방법입니다.
해당 확장프로그램은 php의 debug를 확인하기 위해서 사용하였습니다.

- vscode에서 php debug과 PHP IntelliSense v2.3.5 extension pack를 설치한다.
- https://xdebug.org/wizard.php 에 localhost/php.info의 내용을 붙여넣어 다운로드할 파일을 자동으로 확인 후 페이지 아래의 내용을 따라 설정을 마친다.
    php.ini에
    ```bash
    [XDebug] 
    xdebug.remote_host=127.0.0.1 
    xdebug.remote_port=9000 
    xdebug.remote_handler=dbgp 
    xdebug.remote_enable = 1 
    xdebug.remote_autostart = 1
    zend_extension = C:\apm\php7\ext\php_xdebug-2.8.1-7.4-vc15-x86_64.dll
    ```
    를 추가한다.
    <br> debug의 launch.json에 아래의 내용을 넣는다.
    ```bash
    {
        // IntelliSense를 사용하여 가능한 특성에 대해 알아보세요.
        // 기존 특성에 대한 설명을 보려면 가리킵니다.
        // 자세한 내용을 보려면 https://go.microsoft.com/fwlink/?linkid=830387을(를) 방문하세요.
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Listen for XDebug",
                "type": "php",
                "request": "launch",
                "port": 9000
            },
            {
                "name": "Launch currently open script",
                "type": "php",
                "request": "launch",
                "program": "${file}",
                "cwd": "${fileDirname}",
                "port": 9000
            }
        ]
    }
    ```
서버 restart하고 debug하고 싶은 파일 폴더를 apache의 htdoc에 넣는다. 중단점을 잡아주고 해당 url로 접속하면 중단점에서의 변수값, 함수 결과 들을 알 수 있다.