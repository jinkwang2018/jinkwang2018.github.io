---
title:  "JAVA-Calendar"
tags: Java
---
# Calendar

* Calendar 를 상속받아 완전히 구현한 클래스는 
* GregorianCalendar

   buddhisCalendar 있는데 getInstance()는 [시스템의 국가와 지역설정]을 [확인]해서 태국인 경우 buddhisCalendar 의 인스턴스를 반환하고 그 외에는 GregorianCalendar 의 인스턴스를 반환한다
   
   GregorianCalendar 는 Calendar를 상속받아 오늘날 전세계 공통으로 사용하고 있는 그레고리력에 맞게 구현한 것으로 태국을 제외한 나머지 국가에서는 
   
   GregorianCalendar 사용
   그래서 인스턴스를 직접 생성해서 사용하지 않고 메서드를 통해서 인스턴스를 반환받게하는 이유는 최소의 변경으로 프로그램 동작을 하도록 하기 위함
   ~~~java
   class MyApp{
     static void main(){
      Calendar cal = new GregorianCalendar();
      //다른 종류의 역법의 사용하는 국가에서 실행하면 변경......    }  }
   class MyApp{
     static void main(){
      Calendar cal = new getInstance();
      //다른 종류의 역법의 사용하는 국가에서 실행하면 변경......   }  }
	~~~

# data type

* 기본 타입(값타입): 8가지 >> JAVA API 제공
* 8가지 기본 타입에 대해서 객체 형태로 사용가능 하도록 만든 것
* wrapper class (참조 타입) : 기본형타입 변수도 때로는 객체형태로 다루어져야 하는 경우
##### 1.매개변수로 객체가 요구 될 때
##### 2.기본형 값이 아닌 객체로 저장되어야 할 때
##### 3.객체간의 비교가 필요할 때
##### 이 때 wrapper 사용하면... wrapper 클래스에는 각 타입의 정보(최소크기, 최대크기 상수값 형태로 저장)
#### ex)
~~~java
	Integer.parseInt(s)
	ArrayList<Integer> li = new ArrayList<>(); //>> parameter형태
	
	System.out.println(Integer.MIN_VALUE);
	System.out.println(Integer.MAX_VALUE);
~~~

<br>

# inner class
#### 클래스안에 클래스가 있다
~~~java
class OuterClass {
	public int pdata = 10;
	private int data = 30;
	
	//inner class (자원에 대한 접근을 편하게)
	//member filed가 선언되는 곳에
	class InnerClass {
		void msg() {
			System.out.println("outerclass data: " + data);
			System.out.println("outerclass data: " + pdata);
		}
	}
}
//abstract class Person2 어차피 강제 구현을 목적으로
//추상클래스는 객체 생성 불가능
//Person2 상속하는 클래스 없이도 1회성으로 사용가능한 클래스 (이름이 없는 클래스): 익명클래스
~~~