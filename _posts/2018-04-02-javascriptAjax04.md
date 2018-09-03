---
layout: post
title:  "javascript-Ajax-xmlHttpRequestparam"
date:   2018-04-02 12:00:00
author: 강진광
categories: [JAVASCRIPT]
comments: true
tags:
  - javascript
  - Ajax
  - xmlHttpRequest
---
# xmlHttpRequest param
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
				    //처리 코드 (가공 사용자 마음)
				    //Text (text , html , json)
				    //XML
				  
				    var TempArray = new Array();
				    TempArray = httpReq.responseText.split(","); //AA,BB,CC
				    //[0] = AA  [1]= BB  [2]=CC
				    
				    var printdata="";
				    for(var i = 0 ; i < TempArray.length ; i++){
				    	printdata+= TempArray[i].trim() + "<br>";
				    }
				  
				    var view = document.getElementById("div_view");
				    view.innerHTML = printdata;
				    
				}
			}
		}
		
		function sendData(){
			httpReq = getInstance();
			httpReq.onreadystatechange = HandlerStateChange; //(x)
			//console.log(document.getElementById("select_menu"));
			var key = document.getElementById("select_menu").selectedIndex;
			//key : 0 or  1 or 2
	        httpReq.open("POST", "Ex03_Server_param.jsp?no="+key);//준비  jsp?id=0
	        httpReq.send();
		}
	</script>
</head>
<body>
	<h3>Ajax param 값 처리하기</h3>
	<div style="background-color: gray;width: 500px;text-align: center;">
		<hr style="color: red">
		<select id="select_menu" onchange="sendData()">
			<option>0번 index</option>
			<option>1번 index</option>
			<option>2번 index</option>
		</select>
	</div>
	<span id="div_view"></span>
</body>
</html>
~~~