---
layout: post
title:  "security"
date:   2018-05-10 18:21:00
author: 강진광
categories: [spring]
comments: true
tags:
  - Spring
  - Security
---
## security 사용하기 ##

Security-context.xml을 만들고
~~~xml
<beans
 xmlns:security="http://www.springframework.org/schema/security"
 xsi:schemaLocation="
  http://www.springframework.org/schema/security
 http://www.springframework.org/schema/security/spring-security.xsd">
~~~
위의 코드를 설정에 추가한다.

~~~xml
<listener>
    <listener-class>
     org.springframework.web.context.ContextLoaderListener
    </listener-class>
</listener>
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>
      /WEB-INF/applicationcontext.xml
      /WEB-INF/security-context.xml
    </param-value>
</context-param>
~~~
web.xml에는 위의 코드를 넣어야 한다.

### Security 페이지 설정 ###

1.provider 를 통해 ROLE_USER, ROLE_ADMIN 두 개의 역할을 가지는 사용자를 추가 합니다.
   각각 아이디가 "user"와 "admin" 입니다.

   - 로그인 폼 URL에는 로그인 하지 않아도 접근할 수 있도록 합니다.

~~~xml
<intercept-url pattern="/login/loginForm.do" access="permitAll" />
~~~

  - 메인화면도 로그인 없이 접근할 수 있도록 합니다.

~~~xml
<intercept-url pattern="/home.do" access="permitAll" />
~~~

  - /admin/** URL 은 ADMIN 권한이 있어야 접근할 수 있도록 합니다.

~~~xml
<intercept-url pattern="/admin/**" access="hasRole('ADMIN')" />
~~~

  - 그 외 나머지는 USER 또는 ADMIN중 하나로도 권한이 있으면 접근할 수 있습니다.

~~~xml
<intercept-url pattern="/**" access="hasAnyRole('USER, ADMIN')" />
~~~


2.form-login 요소를 커스터마이징 합니다.
  + login-page="/login/loginForm.do"
  : 커스텀 로그인 페이지를 지정합니다.

  + default-target-url="/home.do"
  : 로그인 후에 기본으로 보여질 페이지 입니다.
      특정 페이지를 클릭해서 로그인 화면이 나왔다면 로그인 성공후 앞서 클릭한 페이지로 이동합니다.

  + authentication-failure-url="/login/loginForm.do?error"
  : 로그인 실패시 보여질 페이지를 지정합니다.
      여기서는 로그인 페이지를 다시 보여줍니다. 기본값은 /login?error 입니다.

  + username-parameter="id"
  : 로그인 폼에 아이디 입력 필드에 사용될 name 입니다.
      기본값은 "username"입니다.

  + password-parameter="password"
  : 로그인 폼에 비밀번호 입력 필드에 사용될 name 입니다.
      기본값은 "password" 입니다.



3.logout 요소를 커스터마이징 합니다.
  + logout-url="/logout"
  : 로그아웃에 사용될 페이지를 지정합니다. 기본값은 "/logout" 입니다.

  + logout-success-url="/home.do"
  : 로그아웃에 성공하였을 때 이동할 페이지를 지정합니다.

4.로그인 하였으나 권한이 없는 요청을 하였을 경우 보여지는 페이지를 지정합니다.

~~~xml
<access-denied-handler error-page="/login/accessDenied.do" />
~~~
