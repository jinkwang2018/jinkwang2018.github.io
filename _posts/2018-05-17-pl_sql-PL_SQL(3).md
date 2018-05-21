---
layout: post
title:  "PL/SQL(3)"
date:   2018-05-17 09:05:00
author: 강진광
categories: [pl_sql]
comments: true
tags:
  - pl_sql
---
## 3.function 사용자 정의 함수
<br>

##### to_char() , sum() 오라클에서 제공
##### 사용자가 직접 필요한 함수를 만들어 사용가능
##### 사용방법은 다른 함수사용법과 동일
##### 사용자 정의 함수 paramter  정의 , return 값
##### **프로시저는 단독으로 사용할 때 사용할 수 있고 사용자 정의 함수는 단독으로 사용 못한다.
<br>

~~~sql
create or replace function f_max_sal
(s_deptno emp.deptno%TYPE)
return number   -- return type / public int f_max_sal(int deptno) {  return 10}
is
  max_sal emp.sal%TYPE;
BEGIN
      select max(sal)
        into max_sal
      from emp
      where deptno = s_deptno;
      return max_sal; --실제 return되는 값
END;
~~~
<br>

##### ex) 님이라는 글자를 select할 떄 이름 뒤에 붙이고 싶다라면

<br>

~~~sql
--함수 생성하기
create or replace function f_callname
(vempno emp.empno%TYPE)
return varchar2 -- public String f_callname() { return "홍길동"}
is
  v_name emp.ename%TYPE;
BEGIN
    select ename || '님'
      into v_name
    from emp
    where empno=vempno;
    return v_name;
END;

--사용하기
select * from emp where sal = f_max_sal(10);

select max(sal) , f_max_sal(30) from emp;
~~~

##### 의 사용자 정의 함수를 생성해서 사용할 수 있다.

<br>

## 4.trigger

##### -트리거(trigger)의 사전적인 의미는 방아쇠나 (방아쇠를) 쏘다, 발사하다,
##### ex)[입고] [재고] [출고] 를 연결하는 것
##### -어떠한 이벤트가 발생하면 그에 따라 다른 작업이 자동으로 처리되는 것을 의미한다.
##### -트리거는 테이블의 데이터가 INSERT, UPDATE, DELETE 문에 의해 변경되어질 때 자동으로 수행되므로 이 기능을 이용하며 여러 가지 작업을 할 수 있다.
##### 트리거는 사용자가 직접 실행 할 수 없다.
##### -BEFORE : 테이블에서 DML 실행되기 전에 트리거가 동작
##### -AFTER :  테이블에서 DML 실행후에 트리거 동작
~~~sql 
--trigger_name TRIGGER 의 식별자
--BEFORE | AFTER DML 문장이 실행되기 전에 TRIGGER 를 실행할 것인지 실행된 후에 TRIGGER 를 실행할 것인지를 정의
--triggering_event TRIGGER 를 실행하는 DML(INSERT,UPDATE,DELETE)문을 기술한다.
--OF column TRIGGER 가 실행되는 테이블에서 COLUMN 명을 기술한다.
--table_name TRIGGER 가 실행되는 테이블 이름

Syntax
CREATE [OR REPLACE] TRIGGER trigger_name
--before 대신 after사용할 수 있다.
BEFORE triggering_event [OF column1, . . .] ON table_name
[FOR EACH ROW [WHEN trigger_condition]
trigger_body;
~~~

##### ex) 신입사원 입사할 때 입사라는 내용을 출력하고 정보 수정할 때 수정을 출력하고 퇴사하면 정보 삭제를 출력한다.

~~~sql
--입사
create or replace trigger tri_01
after insert on tri_emp
BEGIN -- 자동 동작할 내용
    DBMS_OUTPUT.PUT_LINE('신입사원 입사');
END;

insert into tri_emp values(200,'홍길동');

--수정
create or replace trigger tri_02
after update on tri_emp
BEGIN
  DBMS_OUTPUT.PUT_LINE('신입사원 수정');
END;

update tri_emp set ename='변경' where empno= 200;

--삭제
create or replace trigger tri_03
after delete on tri_emp
BEGIN
  DBMS_OUTPUT.PUT_LINE('신입사원 삭제');
END;

delete from tri_emp where empno=200;

--트리거 검색하기
select * from user_triggers where table_name='TRI_EMP';
~~~
<br>

#### DROP TRIGGER 
##### 명령어로 트리거를 삭제할 수 있고 TRIGGER 를 잠시 disable 할 수 있다.
~~~sql
drop trigger tri_01; --delete

ALTER TRIGGER trigger_name DISABLE; --disable
ALTER TRIGGER trigger_name ENABLE; --enable
~~~

<br>

#### FOR EACH ROW 
~~~sql
--for each row 사용안한것(명령어 한 번에 대하여 한 건으로 기록된다.) 
create or replace trigger emp_audit_tr
 after insert or update or delete on emp2
 --for each row
begin
 if inserting then
      insert into emp_audit
      values(emp_audit_tr.nextval, user, 'inserting', sysdate);
 elsif updating then
      insert into emp_audit
      values(emp_audit_tr.nextval, user, 'updating', sysdate);
 elsif deleting then
      insert into emp_audit
      values(emp_audit_tr.nextval, user, 'deleting', sysdate);
 end if;
end;

-- for each row 선언 했을 때(명령어 한 번에 변경된 행만큼 기록된다.)
create or replace trigger emp_audit_tr
 after insert or update or delete on emp2
 for each row
begin
 if inserting then
      insert into emp_audit
      values(emp_audit_tr.nextval, user, 'inserting', sysdate);
 elsif updating then
      insert into emp_audit
      values(emp_audit_tr.nextval, user, 'updating', sysdate);
 elsif deleting then
      insert into emp_audit
      values(emp_audit_tr.nextval, user, 'deleting', sysdate);
 end if;
end;


~~~ 

##### FOR EACH ROW > 이 옵션을 사용하면 행 레벨 트리거가 되어 triggering 문장에 의해 영향받은 행에 대해 각각 한번씩 실행하고 사용하지 않으면 문장 레벨 트리거가 되어 DML 문장 당 한번만 실행된다.
<br>

### OLD,NEW
~~~sql
create or replace trigger tri_order2
before insert on tri_order
for each row
BEGIN
  IF(:NEW.ord_code) not in('desktop') THEN
     RAISE_APPLICATION_ERROR(-20002, '제품코드 오류');
  END IF;
END;

select * from tri_order;

--오류 (desktop) insert되지 않는다.
insert into tri_order values(200,'notebook',sysdate);
--정상실행  insert된다.
insert into tri_order values(200,'desktop',sysdate);
~~~

##### -PL_SQL 두개의 가상데이터(테이블) 제공
##### -:OLD > 트리거가 처리한 레코드의 원래 값을 저장 (ms-sql (deleted)
##### -:NEW > 새값을 포함                           (ms-sql (inserted)
<br>

### INSERT, UPDATE, DELETE
##### 변경되는 내용에 대하여 전/후 데이터를 기록한다.
~~~sql
create table emp_audit (
 id number(6) constraint emp_audit_pk primary key,
 name varchar2(30),
 gubun varchar2(10),
 wdate date,
 etc1 varchar2(20),  -- old
 etc2 varchar2(20)   -- new
);


create or replace trigger emp_audit_tr
 after insert or update or delete on emp2
 for each row
begin
 if inserting then
    insert into emp_audit
    values(emp_audit_tr.nextval, user, 'inserting', sysdate, :old.deptno, :new.deptno);
 elsif updating then
    insert into emp_audit
    values(emp_audit_tr.nextval, user, 'updating', sysdate, :old.deptno, :new.deptno);
 elsif deleting then
    insert into emp_audit
    values(emp_audit_tr.nextval, user, 'deleting', sysdate, :old.deptno, :new.deptno);
 end if;
end;

--insert
insert into emp2(empno,ename,deptno) values (9999,'홍길동',100);

--update 
update emp2 set deptno=200 where empno=9999;

--delete
delete from emp2 where empno=9999;

select * from emp_audit;
~~~

#### 종합
~~~sql
--입고 , 재고

create table t_01 --입고
(
  no number,
  pname varchar2(20)
);

create table t_02 --재고
(
  no number,
  pname varchar2(20)
);

--입고 데이터 들어오면 같은 데이터를 재고 입력
create or replace trigger insert_t_01
after insert on t_01
for each row
BEGIN
  insert into t_02(no, pname)
  values(:NEW.no ,:NEW.pname);
END;

--입고
insert into t_01 values(1,'notebook');

select * from t_01;
select * from t_02;

-- 입고 제품이 변경 (재고 변경)
create or replace trigger update_t_01
after update on t_01
for each row
BEGIN
  update t_02
  set pname = :NEW.pname
  where no = :OLD.no;
END;

update t_01
set pname = 'notebook2'
where no = 1;

select * from t_01;

select * from t_02;

--delete 트리거 만들어 보세요 
--입고 데이터 delete from t_01 where no =1 삭제 되면 재고 삭제
create or replace trigger delete_tri_01
after delete on t_01
for each row
BEGIN
  delete from t_02
  where no=:OLD.no;
END;

delete from t_01 where no=1;

select* from t_01;
select* from t_02;
~~~