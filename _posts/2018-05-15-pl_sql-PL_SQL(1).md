---
layout: post
title:  "PL/SQL(1)"
date:   2018-05-15 09:05:00
author: 강진광
categories: [pl_sql]
comments: true
tags:
  - pl_sql
---

# PL/SQL(1)

>프로그램이 가미된 sql이다.

#### -PL/SQL 은 Oracle's Procedural Language extension to SQL. 의 약자 입니다. 
#### -SQL문장에서 변수정의, 조건처리(IF), 반복처리(LOOP, WHILE, FOR)등을 지원하며, 
#### -오라클 자체에 내장되어 있는Procedure Language입니다
#### -DECLARE문을 이용하여 정의되며, 선언문의 사용은 선택 사항입니다. 
#### -PL/SQL 문은 블록 구조로 되어 있고PL/SQL 자신이 컴파일 엔진을 가지고 있습니다.
<br>

#### 1.pl-sql 블럭 단위 실행
~~~sql
BEGIN
  DBMS_OUTPUT.PUT_LINE('HELLO WORLD');
END;
~~~

<br>

#### 2.pl-sql
##### -선언부(변수) 
##### -실행부(변수 값을 할당 , 제어구문)
##### -예외부(Exception)

~~~sql
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

<br>

#### 3.변수 선언 방법 (타입)
##### -DECLARE
##### -v_job varchar2(10);
##### -v_count number ;= 10; --초기값 설정
##### -v_date date := sysdate + 7; --초기값 설정
##### -v_valid boolean not null := true

~~~sql
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

<br>

#### 4.변수 제어하기(타입)
##### -1.1 타입 : v_empno number(10)
##### -1.2 타입 : v_empno emp.empno%TYPE  (emp 테이블에 있는 empno 컬럼의 타입 사용)
##### -1.3 타입 : v_row emp%ROWTYPE (v_row 변수는 emp 테이블 모든 컬럼 타입 정보)

## QUIZ
#### 두개의 정수를 입력받아서 그 합을 출력
~~~sql
DECLARE
  v_no1 number := '&no1';
  v_no2 number := '&no2';
  result number;
BEGIN
    result := v_no1 + v_no2;
    DBMS_OUTPUT.PUT_LINE('result : ' || result);
END;
~~~

<br>

#### 5.sequence
~~~sql
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

<br>

#### 6.if문
~~~sql
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vdeptno emp.deptno%TYPE;
  vname varchar2(20) := null;
BEGIN
  select empno , ename , deptno
    into vempno , vename , vdeptno
  from emp
  where empno=7788;
  --제어문 if(조건문){실행문}
  IF(vdeptno = 10) THEN vname := 'ACC'; -- if(vdeptno==10) { vname = "ACC"}
    ELSIF(vdeptno=20) THEN vname := 'IT';
    ELSIF(vdeptno=30) THEN vname := 'SALES';
  END IF;
  DBMS_OUTPUT.PUT_LINE('당신의 직종은 : ' || vname);
END;
~~~

### QUIZ
##### -사번이 7788번인 사원의 사번 , 이름 , 급여를 변수에 담고
##### -변수에 담긴 급여가 2000 이상이면 '당신의 급여는 BIG' 출력하고
##### -그렇지 않으면(ELSE) '당신의 급여는 SMALL' 이라고 출력하세요

~~~sql
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vsal   emp.sal%TYPE;
BEGIN
  select empno , ename , sal
    into vempno , vename , vsal
  from emp
  where empno=7788;
    IF(vsal >  2000) THEN 
         DBMS_OUTPUT.PUT_LINE('당신의 급여는 BIG ' || vsal);
    ELSE
         DBMS_OUTPUT.PUT_LINE('당신의 급여는 SMALL ' || vsal);
    END IF;
 END;
 ~~~

<br>

#### 7.case문
~~~sql
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vdeptno emp.deptno%TYPE;
  v_name varchar2(20);
BEGIN
     select empno, ename , deptno
        into vempno, vename , vdeptno
     from emp
     where empno=7788;
     
--    v_name := CASE vdeptno
--                WHEN 10  THEN 'AA'
--                WHEN 20  THEN 'BB'
--                WHEN 30  THEN 'CC'
--                WHEN 40  THEN 'DD'
--              END;
-- 비교 값이 딱딱 떨어질 때 사용한다.

    v_name := CASE 
                WHEN vdeptno=10  THEN 'AA'
                WHEN vdeptno in(20,30)  THEN 'BB'
                WHEN vdeptno=40  THEN 'CC'
                ELSE 'NOT'
              END;
    DBMS_OUTPUT.PUT_LINE('당신의 부서명:' || v_name);            
END;
~~~

<br>

#### 8.반복문 (roop,while,for)
##### roop
~~~sql
DECLARE
  n number :=0;
BEGIN
  LOOP
    DBMS_OUTPUT.PUT_LINE('n value : ' || n);
    n := n + 1;
    EXIT WHEN n > 5; --조건문
  END LOOP;
END;
~~~

>조건문을 주지 않으면 무한 roop를 돌게 된다.

##### while
~~~sql
DECLARE
  num number := 0;
BEGIN
  WHILE(num < 6)
  LOOP
    DBMS_OUTPUT.PUT_LINE('num 값 : ' || num);
    num := num +1;
  END LOOP;
END;
~~~

##### for
~~~sql
BEGIN
  FOR i IN 0..10 LOOP
    DBMS_OUTPUT.PUT_LINE(i);
  END LOOP;
END;
~~~
>..은 ~와 뜻이 같다. 

## QUIZ
##### -위 FOR 문을 사용해서 (1~100까지 합) 구하세요
~~~sql
DECLARE
total number :=0;
BEGIN
  FOR i IN 1..100 LOOP
    total := total + i;
  END LOOP;
  DBMS_OUTPUT.PUT_LINE('1~100 총합 : ' || total);
END;
~~~

<br>

#### 9.continue
##### -11g 이전 (continue (x))
##### -11g (continue 추가)
~~~sql
DECLARE
  total number := 0;
BEGIN
  FOR i IN 1..100 LOOP
    DBMS_OUTPUT.PUT_LINE('변수 : ' || i);
    CONTINUE WHEN i > 5; --skip
    total := total + i; -- 1 , 2 , 3 , 4, 5
  END LOOP;
    DBMS_OUTPUT.PUT_LINE('합계 : ' || total);
END;
~~~