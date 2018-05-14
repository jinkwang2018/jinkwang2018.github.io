---
layout: post
title:  "ajax사용하기"
date:   2018-05-11 14:32:00
author: 강진광
categories: [spring]
comments: true
---

## 비동기를 사용하려면 ##

~~~xml
    	<dependency>
    		<groupId>com.fasterxml.jackson.core</groupId>
    		<artifactId>jackson-core</artifactId>
    		<version>2.7.3</version>
		</dependency>
		<dependency>
    		<groupId>org.codehaus.jackson</groupId>
    		<artifactId>jackson-core-asl</artifactId>
    		<version>1.9.13</version>
		</dependency>
		<dependency>
    		<groupId>com.fasterxml.jackson.core</groupId>
    		<artifactId>jackson-databind</artifactId>
    		<version>2.7.3</version>
		</dependency>
~~~

이것을 pom.xml에 적어야한다.

~~~xml
    <context:component-scan base-package="kosta.controller" />
~~~

컨트롤로 사용하지 않고 View Mapping 가능  : /WEB-INF/views/index.jsp
          컨트롤러에서 해줘야할 특정한 작업이 없고, 요청에 대해 단순히 뷰를 보여주고 싶을 때, 사용하는 컨트롤러이다.
          요청된 url이 name="/index.kosta" 의 값과 같으면 viwe를 처리한다.
          return index; 컨트롤러의 return값


# JSON VIEW 방식 #

~~~java
@Autowired
private View jsonview;
/*
jsonview에 자동으로 MappingJackson2JsonView를 주입한다.
<bean name="jsonview" class="org.springframework.web.servlet.view.json.MappingJackson2JsonView" />
servlet-context에서 객체를 생성한다.
*/
@RequestMapping(value="json.kosta")
public View jsonkosta(String command , String name, String[] arr , ModelMap map){

	ArrayList<String> list = new ArrayList<>();

	list.add("치킨맥주");
	list.add("돈까스");
	list.add("치킨피자");
	map.addAttribute("menu", list);

	return jsonview;  //private View jsonview 타입으로 리턴
}
~~~

<br>
private View jsonview 타입으로 리턴 Model, ModelMap, ModelAndView에<br>
데이터를 JSON객체로 담아 놓으면 나머지는 자동으로 처리된다.<br>
MappingJackson2JsonView가  Model, ModelMap, ModelAndView에 들어간 data를 JSON으로 자동 변환 해준다.<br>
받는 곳에서는 json data로 받는다.
<br>
# @ResponseBody 방식 #
<br>
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans "xmlns:mvc="http://www.springframework.org/schema/mvc"
xsi:schemaLocation="http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd">
<mvc:annotation-driven /> <!-- @ResponseBody  messageConverter 동작-->
~~~
servlet-context.xml에 위의 것을 써주어야 한다.

error가 JsonView를 쓰는 것이 적게 난다.
~~~java
    @RequestMapping(value="response.kosta",method=RequestMethod.POST)
	public @ResponseBody Employee add(HttpServletRequest request, HttpServletResponse response)
	{
		
		Employee employee = new Employee();
		employee.setFirstname(request.getParameter("firstName"));
		employee.setLastname(request.getParameter("lastName"));
		employee.setEmail(request.getParameter("email"));
		
		return employee;
	}

	@RequestMapping(value="response2.kosta",method=RequestMethod.POST)
	public @ResponseBody Employee add(Employee emp){
		return emp;
	}
~~~
<br>
예제가 두 종류 이다.

위의 것은 controller에 작성하는 것이다.
<br>
~~~java
$.ajax({
	type: "post",
	url:  "response.kosta",
  	cache: false,				
	data:'firstName=' + $("#firstName").val() + "&lastName=" + $("#lastName").val() + "&email=" + $("#email").val(),
   	success:function(data){ //callback  
   	  
   	$("#menuView").empty();
   	var resv="";  
   	$("#menuView").append("First Name:- " + data.firstname +"</br>Last Name:- " + data.lastname  + "</br>Email:- " + data.email + "<br>");

   	},

   	error: function(){						
   		alert('Error while request..'	);
   	}
});

 var _param = {firstname:$("#firstName").val(), lastname:$("#lastName").val() , email:$("#email").val()};
_data = JSON.stringify(_param); //jsonString으로 변환
alert(_data);

$.ajax({
	  type : 'POST',
	  url : "response2.kosta",
	  cache: false,
   	  dataType: "json",
   	  data: _data,  
   	  processData: false,
   	  contentType: "application/json; charset=utf-8",
   	  success : function(data, status){
   	     alert(data.email);
   	  },
   	  error: function(request, status, error){
   	      //alert("loading error:" + request.status);
	  }
});
~~~
<br>
받는 곳에서는 이렇게 받는다.

두번째 예시는 JSON으로 넘기는 예시인데 값이 넘어가지를 않는다. 그러므로 @RequestBody를 받는 곳에 parameter에 적어주어야한다.
<br>
~~~java
@RequestMapping(value="response2.kosta",method=RequestMethod.POST)
	public @ResponseBody Employee add(@RequestBody Employee emp){   
		return emp;
	}
~~~
