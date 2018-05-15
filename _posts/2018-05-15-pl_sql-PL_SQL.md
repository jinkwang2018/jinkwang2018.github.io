---
layout: post
title:  "PL/SQL"
date:   2018-05-15 09:05:00
author: 강진광
categories: [pl_sql]
comments: true
tags:
  - pl_sql
---
# PL/SQL

>프로그램이 가미된 sql이다.

#### -PL/SQL 은 Oracle's Procedural Language extension to SQL. 의 약자 입니다. 
#### -SQL문장에서 변수정의, 조건처리(IF), 반복처리(LOOP, WHILE, FOR)등을 지원하며, 
#### -오라클 자체에 내장되어 있는Procedure Language입니다
#### -DECLARE문을 이용하여 정의되며, 선언문의 사용은 선택 사항입니다. 
#### -PL/SQL 문은 블록 구조로 되어 있고PL/SQL 자신이 컴파일 엔진을 가지고 있습니다.

## 예시
#### 1.pl-sql 블럭 단위 실행
~~~db
BEGIN
  DBMS_OUTPUT.PUT_LINE('HELLO WORLD');
END;
~~~

#### 2.pl-sql
##### -선언부(변수) 
##### -실행부(변수 값을 할당 , 제어구문)
##### -예외부(Exception)

~~~db
DECLARE --선언
  vno number(4);
  vname varchar2(20);
BEGIN
  vno := 100; -- 할당 >  String s; s = "홍길동"
  vname := 'kglim';
  DBMS_OUTPUT.PUT_LINE(vno); --화면 출력
  DBMS_OUTPUT.PUT_LINE(vname || '입니다');
END;
~~~
> begin과 end사이의 영역에서 declare한 변수들을 사용할 수 있다.

#### 3.변수 선언 방법 (타입)
##### -DECLARE
##### -v_job varchar2(10);
##### -v_count number ;= 10; --초기값 설정
##### -v_date date := sysdate + 7; --초기값 설정
##### -v_valid boolean not null := true

~~~db
DECLARE
  vno number(4);
  vname varchar2(20);
BEGIN
   select empno ,ename
      into vno , vname 
   from emp
   where empno=&empno; 
   
   DBMS_OUTPUT.PUT_LINE('변수값 : ' || vno || '/' || vname);
END;
~~~
>into라는 구문으로 변수에 담아준다. 

>&는 java에서의 scanner같은 기능을 한다. <br>
>커서를 사용하기 전까지는 row갯수가 1개인 것만 적용이 가능하다.

#### 4.변수 제어하기(타입)
##### -1.1 타입 : v_empno number(10)
##### -1.2 타입 : v_empno emp.empno%TYPE  (emp 테이블에 있는 empno 컬럼의 타입 사용)
##### -1.3 타입 : v_row emp%ROWTYPE (v_row 변수는 emp 테이블 모든 컬럼 타입 정보)

## QUIZ
#### 두개의 정수를 입력받아서 그 합을 출력
~~~db
DECLARE
  v_no1 number := '&no1';
  v_no2 number := '&no2';
  result number;
BEGIN
    result := v_no1 + v_no2;
    DBMS_OUTPUT.PUT_LINE('result : ' || result);
END;
~~~
#### 5.sequence
~~~db
create sequence empno_seq
increment by 1
start with 8000
maxvalue 9999
nocycle
nocache;
~~~
>8000부터 9999까지 1씩 증가하는 sequence를 만들겠다. <br>
>cycle : maxvalue까지 찍고 다시 start값으로 돌아간다.<br>
>nocycle : maxvalue까지 찍고 끝난다. <br>
>cache : 성능을 위해서 처음에 만들어 놓겠다.




