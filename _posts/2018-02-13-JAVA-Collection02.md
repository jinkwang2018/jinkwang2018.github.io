---
title:  "JAVA-Collection-Stack"
tags: Java
key: post-java-10
---
# [stack]
~~~java
public static void main(String[] args) {
	
//LIFO(Last in First Out)
//Collection Framework 제공하는 Stack
/////////////////////////////////////
//JAVA API 제공하는 Stack 클래스
Stack stack = new Stack();
stack.push("A");
stack.push("B");

Mystack my = new Mystack(3);
System.out.println(my.isEmpty());
my.push("A");
my.push("B");
System.out.println(my.toString());
System.out.println(my.pop());
System.out.println(my.toString());
my.push(1);
System.out.println(my.toString());
my.push(2);
System.out.println(my.toString());
	}
}
~~~