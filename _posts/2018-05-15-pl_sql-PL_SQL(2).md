---
layout: post
title:  "PL/SQL(2)"
date:   2018-05-15 09:05:00
author: 강진광
categories: [pl_sql]
comments: true
tags:
  - pl_sql
---
# PL/SQL(2)
>고급자원 잘못 사용하면 database가 죽는다.

<br>

#### 1.cursor
##### [행단위]로 데이터를 처리하는 방법을 제공
##### 여러건의 데이터를 처리하는 처리하는 방법을 제공 (한 건이상의  row가지고 놀기)

```dark
--사원급여테이블(건설회사)
--정규직 , 일용일 ,시간직 

--사번 , 이름 , 직종명 , 월급 , 시간 , 시간급 , 식대
-- 10   홍길동  정규직   120    null   null     null
-- 11   김유신  시간직   null   10      100     null
-- 12   이순신  일용일   null   null    120     10

최종 출력 / 판단의 기준이 : 직종이다. 
홍길동은 월급이 급여 총액이 되지만
김유신은 시간 * 시간급을 가져와야 급여 총액이된다.
이순신은 시간급 + 식대가 급여 총액이 된다. 

select 해서 메모리에 올린뒤 cursor를 가져가서 직종을 보고 sql을 따로 적용 시킨다.
```

## 예시
```dark
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vsal   emp.sal%TYPE;
  CURSOR c1  IS select empno,ename,sal from emp where deptno=30;
BEGIN
    OPEN c1; --커서가 가지고 있는 문장 실행
    LOOP --data row 건수 만큼 
      --Memory
      /*
        7499 ALLEN 1600
        7521 WARD 1250
        7654 MARTIN 1250
        7698 BLAKE 2850
        7844 TURNER 1500
        7900 JAMES 950
      */
      FETCH c1 INTO vempno , vename, vsal;
      EXIT WHEN c1%NOTFOUND; --더이상 row 가 없으면 탈출
        DBMS_OUTPUT.PUT_LINE(vempno || '-' || vename || '-'|| vsal);
    END LOOP;
    CLOSE c1;
END;
```
#### 위의 코드를 개선할 수 있다.

>for in 구문 , 따로 cursor를 open하지 않아도 된다. open이 in에 포함되어 있다.
```dark
DECLARE
  CURSOR emp_curr IS  select empno ,ename from emp;
BEGIN
   
    FOR emp_record IN emp_curr  --row 단위로 emp_record변수 할당
    LOOP
      EXIT WHEN  emp_curr%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE(emp_record.empno || '-' || emp_record.ename);
    END LOOP;
   
    CLOSE emp_curr;
END;
```
#### java에서 사용하는 개선된 for문 같은 형태로 개선을 할 수 있다.

## QUIZ
##### -문제
##### emp 테이블에서  사원들의  사번 , 이름 , 급여를 가지고
##### 와서 cursor_table insert 를 하는데 totalsum 은 급여 + comm 통해서
##### 부서번호가 10인 사원은 totalsum에 급여 정보만 넣으세요

```dark
insert into CURSOR_TABLE(empno,ename,sal,totalsum)
select empno , ename , sal , sal+nvl(comm,0)
from emp where deptno=20;

select *  from CURSOR_TABLE;

DECLARE
  result number := 0;
  CURSOR emp_curr IS select empno, ename, sal,deptno,comm from emp;
  BEGIN
    FOR vemp IN emp_curr   --row 단위로 emp_record 변수에 할당
    LOOP
        EXIT WHEN emp_curr%NOTFOUND;
        IF(vemp.deptno = 20) THEN 
              result := vemp.sal+nvl(vemp.comm,0);
              insert into cursor_table(empno, ename, sal,deptno,comm,totalsum) 
              values (vemp.empno,vemp.ename, vemp.sal,vemp.deptno,vemp.comm,result);
        ELSIF(vemp.deptno = 10) THEN 
            result := vemp.sal;
              insert into cursor_table(empno, ename, sal,deptno,comm,totalsum) 
              values (vemp.empno,vemp.ename, vemp.sal,vemp.deptno,vemp.comm,result);
        ELSE
            DBMS_OUTPUT.PUT_LINE('ETC');
        END IF;
     END LOOP;   
  END;
```

<br>

#### 2.procedure

<br>

#### 3.function

<br>

#### 4.trigger
