---
layout: post
title:  "JAVA-Exception"
date:   2018-02-12 12:00:00
author: 강진광
categories: [JAVA]
comments: true
tags:
  - Java
  - Exception
  - Error
  - String
  - Wrapper
  - Innerclass
---
# Exception 처리하기
##### 1. 에러(Error): 네트워크 장애, 메모리, 하드웨어
##### 2. 예외(Exception): 개발자 코드 처리 (로직 제어 ...> 예측가능)
##### 예외처리 목적: 프로그램을 정상적으로 수정하는 것이 아니고 문제가 발생시 비정상적인 종료 못하게 하는 것

### ex1)
~~~java
//문제가 발생될 수 있는 코드
try{
	//문제가 될 수 있는 코드
}catch(Exception e)
{
	/*
	처리 (문제에 대한 인지를 하고...)
	ex)관리자에게 메일을 보낼까?
	log파일에 기록할까?
	*/
}finally{
	//예외가 발생하던 발생하지않던 강제적으로 수행되는 구문
}
~~~
### ex2)
~~~java
public static void main(String[] args) {
		try {
			copyFiles();
			startInstall(); //설치 중단 되던, 설치 완료 되던 DISK 설치 파일 삭제
			
			//개발자(사용자) 강제로 예외를 처리
			//사용자 예외 던지기 (예외 객체를 개발자가 직접 생성 new 해라)
			//IOException io = new IOException("Install 처리 중 문제 발생");
			//throw io; //catch가 처리
			throw new IOException("Install 처리 중 문제 발생");
		}catch(Exception e) {
			System.out.println("예외 메시지 출력하기: " + e.getMessage());
		}finally { //예외가 발생하던 발생하지 않던 강제적 실행 블럭
			fileDelete();
		}
		System.out.println("실행");
		//주의 사항
		//**함수 종료 (return;)있다 하더라도 finally 블럭이 있으면 {실행하고}...종료
	}



		//클래스 설계시 내가 가지고 있는 자원을 사용하는 개발자에게 강제로 예외처리를 하도록 하는 방법
		//생성자, 함수 뒤에 [throws 예외클래스명, 예외클래스명, 예외클래스명, ...]

		//JAVA API 제공 클래스들은 throws ... (IO)
		//public FileInputStream(String name)
		//throws FileNotFoundException
		/*
		try {
			FileInputStream fi = new FileInputStream("C:\\temp\\a.txt");
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
~~~