---
layout: post
title:  "app"
date:   2018-05-23 09:05:00
author: 강진광
categories: [android]
comments: true
tags:
  - Android
  - Cordova
---

## Cordova
> HTML, CSS 및 JS가 포함 된 모바일 앱 그리고 단일 코드 기반의 멀티플 플랫폼을 타겟으로 한 무료이자 오픈소스 프레임워크

### 명령 툴
● Adobe PhoneGap
   : 대표적인 모바일 개발 프레임워크로서 HTML5, CSS, JavaScript를 활용해 모바일 기기를 위한 애플리케이션을 제작할 수 있습니다. 

● Ionic
   : Ionic 프레임워크는 앵귤러(Angular JS)와 조화를 이루며 작동합니다. 또한 다양한 유틸리티를 지원하며 iOS, Android와 같이 각 OS에 따른 개별적 수정도 가능하고 UI 컴포넌트를 제공하며 넓은 사용자 층이 있어 개발자 커뮤니티 또한 굉장히 활발합니다.

● Onsen UI 
   : Ionic의 경쟁자인 OnsenUI는 앵귤러(Angular JS)를 지원하며 UI 개발에서 어려움을 겪은 코르도바(Cordova)/폰갭(PhoneGap) 개발자들을 위해 만들어 졌습니다. 크로스 플랫폼, 코르도바와 폰갭에 친화적인 구성으로 다양한 유틸리티를 제공하고 앵귤러와의 호환성 역시 장점으로 꼽힙니다. 경쟁자인 Ionic과 비교하여 큰 차이가 있다면 Ionic 커뮤니티가 더 활발하기 때문에 더 좋은 프레임워크를 만들기 위한 노력 역시 더 활발하다는 점입니다. 또 프레임워크 자체에서도 Ionic이 좀 더 다양한 요소를 제공해 주고 있습니다.

● Visual studio
   : MS사에서 만든 통합 개발 환경툴로서 Visual C/C++와 Visual Basic를 통합하여 발표한 것을 시작으로 많은 개발자들에게 인기가 있습니다. Android, iOS 및 Windows 용 크로스 플랫폼 앱을 제작하는 데 굉장한 인기가 있고 고급 빌드 및 디버깅 지원으로 완성됩니다.

<br>

## 툴없는 cordova 사용하기
##### 1. NodeJS파일 설치한다.
##### 2. cmd창을 열어서 node -v , npm -v 을 확인한 뒤 npm install cordova -g를 통해 파일을 다운 받는다.
##### 3. cordova -v로 cordova 버전을 확인한다.
##### 4. cordova project를 시작할 폴더 경로로 들어가서 cordova create 폴더명 패키지명 ? -d 로 프로젝트를 생성한다.
##### 5. 위에서 적은 폴더명으로 들어간 뒤에 cordova platforms add android를 실행하면 android 용 cordova가 설치된다. cordova platforms add ios를 실행하면 ios용 cordova가 설치된다.
##### 6. cordova build android를 실행해서 빌드 APK 생성한다.
##### 7. 애뮬레이터 실행(studio에서)
##### 8. load된 애뮬레이터에서 설치하기 
##### 9. cordova run android 
##### 10. 애뮬레이터 설치 완료
##### 11. 코드 수정하면 다시 build와 run을 다시 해야한다.
##### 12. studio에서 file >settings > plugins에서 cordova 검색 한뒤에 plugin을 설치한다.
##### 13.android studio에서 open한 뒤 편집

## PhoneGap Cordova사용하기
##### 1. npm install -g phonegap 으로 설치할 수 있고 설치파일로 설치할 수 있다.

##### 2.프로젝트 생성
##### 3.개인 핸드폰에 phonegap developer 설치한다.
##### 4.서버 IP 핸드폰 IP는 와이파이로 맞춘다.
##### 5.핸드폰에서 developer 실행하고 같은 주소로 들어간다.