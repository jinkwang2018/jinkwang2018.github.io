---
layout: post
title:  "javascript-Ajax-xmlHttpRequestfunction"
date:   2018-04-02 12:00:00
author: 강진광
categories: [JAVASCRIPT]
comments: true
tags:
  - javascript
  - xmlHttpRequest
  - Ajax
---
# xmlHttpRequestfunction
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
		   }else if(window.ActiveXObject){ //IE6 이하 ...
			   httpReq = new ActiveXObject("Msxml2.XMLHTTP");
		   }else{
		       throw new Error("AJAX 지원하지 않습니다");
		   }
		   return httpReq;
		}
		
		function handlerStateChange(){ //callback 함수
			if(httpReq.readyState == 4) 
		    {
		        if(httpReq.status >= 200  && httpReq.status < 300){
		         document.getElementById("container").innerHTML = httpReq.responseText;
		        }
	    	}
		}
		
		function sendData(){
			httpReq = getInstance();
			//httpReq.onreadystatechange = function(){}
			httpReq.onreadystatechange = handlerStateChange; //handlerStateChange()(x)
			httpReq.open("POST","Ex01_Resource.html"); //비동기 요청
			httpReq.send();
		}
	</script>
</head>
<body>
	<h3>이미지</h3>
	<img src="images/1.jpg" style="width: 150px;height: 150px">
	
	<h3>비동기(Ajax) 처리하기</h3>
	<input type="button" value="비동기처리" onclick="sendData()">
	<div id="container">First data Load .....</div>
 	
 	<h3>동기식 데이터 처리</h3>
 	<a href="Ex01_Resource.html">[서버 데이터 요청]</a>
</body>
</html>
~~~