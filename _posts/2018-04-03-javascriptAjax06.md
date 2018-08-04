---
layout: post
title:  "javascript-Ajax-empsearch"
date:   2018-04-03 12:00:00
author: 강진광
categories: [JAVASCRIPT]
comments: true
tags:
  - javascript
  - Ajax
  - xmlHttpRequest
---
# EmpSearch.jsp
~~~jsp
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
		Class.forName("oracle.jdbc.OracleDriver");
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		conn = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:XE","bituser","1004");
		String sql="select empno, ename, sal , job from emp where empno=?";
		pstmt = conn.prepareStatement(sql);
		pstmt.setInt(1, Integer.parseInt(request.getParameter("empno")));
		rs = pstmt.executeQuery();
		
		/*  
		코드 구현
		<table>
			<tr><td>empno</td><td>7788</td></tr>
			<tr><td>ename</td><td>SCOTT</td></tr>
			<tr><td>sal</td><td>1000</td></tr>
			<tr><td>job</td><td>SALES</td></tr>
		</table>
		작성된 것  out.print() 하세요
		*/
		out.print("<table border='1'>");
		while(rs.next()){
			out.print("<tr><th>empno</th><td>"+ rs.getInt(1) +"</td></tr>");
			out.print("<tr><th>ename</th><td>"+ rs.getString(2) +"</td></tr>");
			out.print("<tr><th>sal</th><td>"+ rs.getInt(3) +"</td></tr>");
			out.print("<tr><th>job</th><td>"+ rs.getString(4) +"</td></tr>");
		}
		out.print("</table>");
		if(pstmt != null) try{pstmt.close();}catch(Exception e){}
		if(rs != null) try{rs.close();}catch(Exception e){}
		if(conn != null) try{conn.close();}catch(Exception e){}
	%>
~~~

# xmlHttpRequest_EmpTable.html

~~~html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		/*  
			1. XMLHttpRequest 객체 얻기
  			2. onreadystatechange 이벤트 핸들러 구현
  			3. 요청 정보 ( open() )
  			4. 요청 보내기 (send() )
  			5. 응답 처리 (Text(JSON), xml )
		*/
		var httpReq=null;
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
		function sendData(){
			httpReq = getInstance();
			httpReq.onreadystatechange = Handlerstatechange; //(X) 생략
			var keys = document.getElementById("emp").value;
			httpReq.open("post", "Ex05_EmpSearch.jsp?empno="+keys);
			httpReq.send();
		}
		//이벤트에 달아줄 핸들러 함수 구현(onreadystatechange)
		function Handlerstatechange(){
		    if(httpReq.readyState == 4){
	        	if(httpReq.status >= 200  && httpReq.status < 300){
	         		  //받는 데이터가지고 UI 구성하기
	         		  document.getElementById("txtdata").innerHTML = httpReq.responseText;
	        		  
	        		}
	    		}
		}
		
	</script>
	
</head>
<body>
<!--  
문제 )
Ex05_EmpSearch.jsp 파일을 만드시고
아래 select 태그에서 사원선택시 그 사원에 해당하는 
사번,이름,급여,직종 데이터를 Ajax(xmlHttpRequest)를 사용하여
div 태그 txtdata 에 innerHTML 을 사용하여 넣으세요

Ex05_EmpSearch.jsp 에서는 사번을 parameter 로 받아서
DB연결 select empno ,ename, sal , job from emp where empno=?
하고 그 결과를 Table태그를 만들어 넣어서 요청 페이지로 전달하세요
EX)
responseText 데이터 형식
<table>
	<tr><td>empno</td><td>7788</td></tr>
	<tr><td>ename</td><td>SCOTT</td></tr>
	<tr><td>sal</td><td>1000</td></tr>
	<tr><td>job</td><td>SALES</td></tr>
</table>
-->
	<select name="emp" id="emp" onchange="sendData()">
		<option value="0000">SELECT EMPNO</option>
		<option value="7788">SCOTT</option>
		<option value="7902">FORD</option>
		<option value="7521">WORD</option>
	</select>
	<hr>
	<div id="txtdata">EMP DATA LOAD</div>
</body>
</html>
~~~