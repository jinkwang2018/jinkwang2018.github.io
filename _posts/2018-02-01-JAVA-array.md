---
layout: post
title:  "JAVA-Array"
date:   2018-02-01 12:00:00
author: 강진광
categories: [JAVA]
comments: true
tags:
  - java
  - Array
---
# Array
> 배열(Array)는 객체다

##### Array 배열은 각각의 방을 가지고 있다 (각각의 방은 타입(int) 크기 결정)
##### 각각의 방의 접근은: 첨자,index로 접근: 배열의 시작 index: 0
##### ex) score[0], score[1], score[2]
##### 배열의 개수(length)는 마지막 첨자(index)보다 1이 항상 크다
##### 배열은 타입의 초기값을 가진다.	
> Array와 궁합이 제일 좋은 제어문은?  (for)

~~~java
point //(암기)
//배열을 만드는 3가지 방법
int[] arr = new int[5]; 
//기본 (방의 개수, 기본값)
int[] arr2 = new int[]{100, 200, 300}; //3개의 값
int[] arr3 = {11, 12, 13, 14, 15}; //컴파일러가 내부적으로 new 사용
JavaScript: var cars = ["Saab", "Volvo", "BMW"];
~~~
<br>

## example quiz
<br>

~~~java
/*
 *1. 1~45까지의 난수를 발생시켜 6개의 배열에 담으세요
 *2. 배열에 담긴 6개의 배열값은 중복값이 나오면 안되요 
 *3. 배열에 있는 6개의 값은 낮은 순으로 정렬 시키세요 
 *4. 위 결과를 담고 있는 배열을 출력하세요 
*/
public class Ex02_Lotto_Main_Teacher {

	public static void main(String[] args) {
		System.out.println(Math.random());
		System.out.println(Math.random() * 45);
		int[] lotto = new int[6];

	    //번호 추출 (중복값 제거)
		 for (int i = 0; i < 6; i++) {
			 	lotto[i] = (int) (Math.random() * 45 + 1); //난수 추출
	            for (int j = 0; j < i; j++)
	                if (lotto[i] == lotto[j]) {
	                    i--; //point
	                    break;
	                }
	        }
		 //정렬하기
		 for (int i = 0; i < lotto.length; i++) {
	            for (int j = i + 1; j < lotto.length; j++) {
	                if (lotto[i] > lotto[j]) {
	                    int temp = lotto[i];
	                    lotto[i] = lotto[j];
	                    lotto[j] = temp;
	                }
	            }
	        }
		 //출력하기
		 for(int i = 0 ; i < lotto.length ; i++) {
			 System.out.println(lotto[i]);
		 }
	}
}
~~~

#### Today Point(Array (정적배열) >> Collection((동적배열)java에서 가장 중요
##### 개선된 for문
##### C#: for(Type변수명 in 배열 or 컬렉션) { 출력구문 } => JavaScript와 동일
##### JAVA: for(Type변수명 : 배열 or 컬렉션) { 출력구문 }