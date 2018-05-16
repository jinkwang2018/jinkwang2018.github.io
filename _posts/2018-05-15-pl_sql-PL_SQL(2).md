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

>고급자원 잘못 사용하면 database가 죽는다.

<br>

#### 1.cursor
##### [행단위]로 데이터를 처리하는 방법을 제공
##### 여러건의 데이터를 처리하는 처리하는 방법을 제공 (한 건이상의  row가지고 놀기)

~~~sql
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
~~~

## 예시
~~~sql
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
~~~
#### 위의 코드를 개선할 수 있다.

>for in 구문 , 따로 cursor를 open하지 않아도 된다. open이 in에 포함되어 있다.
~~~sql
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
~~~
#### java에서 사용하는 개선된 for문 같은 형태로 개선을 할 수 있다.

## QUIZ
##### -문제
##### emp 테이블에서  사원들의  사번 , 이름 , 급여를 가지고
##### 와서 cursor_table insert 를 하는데 totalsum 은 급여 + comm 통해서
##### 부서번호가 10인 사원은 totalsum에 급여 정보만 넣으세요

~~~sql
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
~~~

<br>

#### 2.procedure

##### -PL-SQL 트랜잭션 및 예외 처리하기
~~~sql 
 DECLARE
    v_ename emp.ename%TYPE := '&p_ename';
    v_err_code NUMBER;
    v_err_msg VARCHAR2(255);
    BEGIN
          DELETE emp WHERE ename = v_ename;
          IF SQL%NOTFOUND THEN
              RAISE_APPLICATION_ERROR(-20002,'my no data found'); --사용자가 예외 만들기 (강제 예외 던지기)
          END IF;
       EXCEPTION 
        WHEN OTHERS THEN
            ROLLBACK;
              v_err_code := SQLCODE;
              v_err_msg := SQLERRM;
              DBMS_OUTPUT.PUT_LINE('에러 번호 : ' || TO_CHAR(v_err_code));
              DBMS_OUTPUT.PUT_LINE('에러 내용 : ' || v_err_msg);
      END;
        
select * from emp where ename ='KING';

delete from emp where ename='aaa';
~~~

##### -지금까지 만들었던 작업은 영속적으로 저장 되지 않았다
##### -crerate table , create view 
##### -내가 위에서 만든 커서를 영속적으로 저장 (객체)
##### -객체 형태로 저장 해놓으면 그 다음번에 코딩하지 않고 불러 사용

##### -Oracle : subprogram(procedure)
##### -Ms-sql : procedure

##### -자주 사용되는 쿼리를 모듈화 시켜서 객체로 저장하고 필요한 시점에 불러(호출) 해서 사용하겠다

~~~sql
create or replace procedure usp_emplist --create or replace(생성가능, 수정가능)
is
  BEGIN
    update emp
    set job = 'TTT'
    where deptno=30;
  END;

--실행방법
execute usp_emplist;
~~~

### procedure  장점
##### -1. 네트워크 트래픽 감소(시간 단축)
##### -2. 보안 (네트워크 상에서 ...보안 요소)해결
##### -3. parameter사용이 가능하다.(input,output) default는 input이다.

<br>

#### input parameter

~~~sql
create or replace procedure usp_update_emp
(vempno emp.empno%TYPE)
is
  BEGIN
    update emp
    set sal = 0
    where empno = vempno;
  END;

--실행방법(parameter보내기)
exec usp_update_emp(7788);
~~~

#### output parameter
~~~sql
create or replace procedure usp_getemplist
(vempno emp.empno%TYPE)
is
  --내부에서 사용하는 변수
  vname emp.ename%TYPE;
  vsal  emp.sal%TYPE;
  BEGIN
      select ename, sal
        into vname , vsal
      from emp
      where empno=vempno;
      
      DBMS_OUTPUT.put_line('이름은 : ' || vname);
      DBMS_OUTPUT.put_line('급여는 : ' || vsal);
  END;

--실행방법(parameter보내기)
exec usp_getemplist(7902);
~~~

### JDBC로 사용하기

#### 1. SELECT 하기
DB에서 실행하는 sql문이다.

~~~sql
CREATE OR REPLACE PROCEDURE usp_EmpList
(
 p_sal IN number,
  p_cursor OUT SYS_REFCURSOR --APP 사용하기 위한 타입
)
IS
 BEGIN
     OPEN p_cursor
        FOR
           SELECT empno, ename, sal FROM EMP WHERE sal > p_sal;
  END;
~~~

eclipse에서 실행하는 코드이다.
~~~java
public class Ex08_Oracle_Procedure_Select {

	public static void main(String[] args) {
		 Connection conn = null;
		  //명령객체
		  CallableStatement cstmt= null; //변경 (procedure 처리하는 객체)
		  ResultSet rs = null;
		  try{
		   Class.forName("oracle.jdbc.OracleDriver");
		   conn = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:XE", "bituser", "1004"); 
		      
		   String sql = "{call usp_EmpList(?,?)}";
		      cstmt = conn.prepareCall(sql);
		      
		      //usp_EmpList(?,?) input , output
		      cstmt.setInt(1, 3000); 
		      cstmt.registerOutParameter(2, OracleTypes.CURSOR); 
		      cstmt.execute(); //가장 많이 다르다 실행만 한다.
		      
		   rs = (ResultSet)cstmt.getObject(2); // 두번째 물음표
		   
		   while(rs.next()){
		    System.out.println(rs.getInt(1) +"/" + rs.getString(2) +"/" + rs.getInt(3));
		   }
		   
		  }catch(Exception e){
		   
		  }finally{
		   if(rs != null){try{rs.close();}catch(Exception e){}};
		   if(cstmt != null){try{cstmt.close();}catch(Exception e){}};
		   if(conn != null){try{conn.close();}catch(Exception e){}};
		  }

	}
~~~


#### 2. DML sql
DB에서 실행하는 sql문이다.
~~~sql
 CREATE OR REPLACE PROCEDURE usp_insert_emp
(
 vempno IN emp.empno%TYPE,
 vename IN emp.ename%TYPE,
 vjob   IN emp.job%TYPE,
  p_outmsg OUT VARCHAR2
 )
 IS
   BEGIN
      INSERT INTO EMP(empno , ename, job) VALUES(vempno ,vename , vjob);
      COMMIT;
        p_outmsg := 'success';
        EXCEPTION WHEN OTHERS THEN
        p_outmsg := SQLERRM;
        ROLLBACK;
   END;
~~~

eclipse에서 실행하는 코드이다.
~~~java
public class Ex09_Oracle_Procedure_DML {

	public static void main(String[] args) {
		
			  Connection conn = null;
			  //명령객체
			  CallableStatement cstmt= null; //변경
			  try{
			   Class.forName("oracle.jdbc.OracleDriver");
			   conn = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:XE", "bituser", "1004"); 
			      
			   String sql = "{call usp_insert_emp(?,?,?,?)}";
			      cstmt = conn.prepareCall(sql);
			      
			      //usp_insert_emp(?,?) input , output
			      cstmt.setInt(1, 9000);
			      cstmt.setString(2, "hong");
			      cstmt.setString(3, "IT");
			      cstmt.registerOutParameter(4, Types.VARCHAR);
			      
			   cstmt.execute();
			      
			   String Oracle_msg = (String)cstmt.getObject(4);
			   
			   System.out.println("Oracle_Msg : " + Oracle_msg);
			   
			   
			  }catch(Exception e){
			   
			  }finally{
			   if(cstmt != null){try{cstmt.close();}catch(Exception e){}};
			   if(conn != null){try{conn.close();}catch(Exception e){}};
			  }
			  

	}

}
~~~


<br>

#### 3.function

<br>

#### 4.trigger
