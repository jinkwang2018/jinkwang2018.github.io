---
layout: post
title:  "javascript-Ajax-wordsearch"
date:   2018-04-03 12:00:00
author: 강진광
categories: [JAVASCRIPT]
comments: true
tags:
  - javascript
  - Ajax
  - xmlHttpRequest
---
# word_search.jsp
~~~jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
 String[] keyword = {
   "Anna"
  ,"Brittany"
  ,"Cinderella"
  ,"Diana"
  ,"Eva"
  ,"Fiona"
  ,"Gunda"
  ,"Hege"
  ,"Inga"
  ,"Johanna"
  ,"Kitty"
  ,"Linda"
  ,"Nina"
  ,"Ophelia"
  ,"Petunia"
  ,"Amanda"
  ,"Raquel"
  ,"Cindy"
  ,"Doris"
  ,"Eve"
  ,"Evita"
  ,"Sunniva"
  ,"Tove"
  ,"Unni"
  ,"Violet"
  ,"Liza"
  ,"Elizabeth"
  ,"Ellen"
  ,"Wenche"
  ,"Vicky" };

String q = request.getParameter("word");
String hint ="";

// lookup all hints from array if $q is different from "" 
if (q != "") {
    q = q.toLowerCase(); 
    int len = q.length(); //an
    for(String str : keyword){
     //out.print(str.substring(0, len));
     if(str.substring(0, len).toLowerCase().equals(q)){
      if(hint ==""){
       //out.print("data : " + hint);
       hint = str;
      }else{
       hint += "." +str;
      }
     }
    }
}
%>
<%= hint == "" ? "no suggestion" : hint %>
~~~

# xmlHttpRequest_wordsearch.html

~~~html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		/* 
		 1. XMLHttpRequest 객체 얻기
  		 2. onreadystatechange 이벤트 핸들러 구현 (함수 (callback))
  		 3. 요청 정보 ( open() )
  	     4. 요청 보내기 (send() )
         5. 응답 처리 (Text(JSON), xml )
		
		*/
		var httpReq = null;
		
		function getInstance(){
		   if(window.XMLHttpRequest){
			   httpReq = new XMLHttpRequest(); //모든 브라우져 > XMLHttpRequest
		   }else if(window.ActiveXObject){
			   httpReq = new ActiveXObject("Msxml2.XMLHTTP");
		   }else{
		    throw new Error("AJAX 지원하지 않습니다");
		   }
		   return httpReq;
		}
		
		function HandlerStateChange(){
			//alert(httpReq.readyState)
			if(httpReq.readyState == 4){ 
				if(httpReq.status >= 200 && httpReq.status < 300){
					//UI구성
					document.getElementById("txthint").innerHTML= httpReq.responseText;
				}
			}
		}
		
		function sendData(str){
			httpReq = getInstance();
			httpReq.onreadystatechange = HandlerStateChange; //(x)
			//key > 0 , 1 , 2
			httpReq.open("POST","Ex04_word_Search.jsp?word="+str);  //준비
			httpReq.send(); //땅 
		}
	</script>
</head>
<body>
	<h3>단어 검색하기</h3>
	<form action="">
		word:<input type="text" id="txt1" onkeyup="sendData(this.value)">
	</form>
	<fieldset>
		<legend>검색단어</legend>
		<span id="txthint"></span>
	</fieldset>
</body>
</html>
~~~