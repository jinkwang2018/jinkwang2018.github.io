---
layout: post
title:  "mybatis"
date:   2018-05-07 12:00:00
author: 강진광
categories: [springframework]
comments: true
---
## mybatis 사용하기 ##

>mybatis란 Beans객체를 SQL문에 mapping 시킬 때 사용하는 것이다.

ibatis와 mybatis는 같은 부모를 가졌다. 

### 시작하기 ###

mybatis 를 사용하기 위해서는 mybatis-x.x.x.jar 파일을 클래스패스에 두어야 한다.

메이븐을 사용한다면 pom.xml에 다음의 설정을 추가하자.

~~~xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
~~~

### XML에서 SqlSessionFactory에 빌드하기 ###

모든 마이바티스 애플리케이션은 SqlSessionFactory 인스턴스를 사용한다. SqlSessionFactory인스턴스는 SqlSessionFactoryBuilder를 사용하여 만들수 있다. SqlSessionFactoryBuilder는 XML설정파일에서 SqlSessionFactory인스턴스를 빌드할 수 있다.

~~~xml
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
~~~

위의 코드로 SqlSessionFactoryBUilder와 SqlSessionFactory를 빌드한다. 

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
~~~

가장 간단한 xml설정 예시이다. xml의 가장 윗부분에는 xml문서의 유효성 체크를 
위해 필요한 설정을 작성하고 environment엘리먼트는 트랜잭션과 커넥션 풀링을 위한
환경적인 설정을 나타낸다. 

### XML 을 사용하지 않고 SqlSessionFactory 빌드하기 ###

java를 사용해서 직접 설정하는 방법은

~~~java
DataSource dataSource = BlogDataSourceFactory.getBlogDataSource();
TransactionFactory transactionFactory = new JdbcTransactionFactory();
Environment environment = new Environment("development", transactionFactory, dataSource);
Configuration configuration = new Configuration(environment);
configuration.addMapper(BlogMapper.class);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(configuration);
~~~

위의 설정으로 사용을 하기 위해서는 mapper클래스를 추가하고 매핑을 해주어야 한다.

## SqlSessionFactory 에서 SqlSession 만들기 ##

~~~java
SqlSession session = sqlSessionFactory.openSession();
try {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
} finally {
  session.close();
}
~~~

위의 코드로 session 을 factory에서 생성을 하였다,

## mapping된 sql구문 ##

sqlsession과 mapper클래스가 실행되는 것을 밑의 코드를 통해 알아볼 수 있다.

~~~xml
<!-- mapper xml이다. -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
~~~

한개의 mapper xml에는 여러개의 sql구문을 사용할 수 있다.
org.mybatis.example.BlogMapper를 namespace에서 selectBlog라는 매핑 구문을 정의하고
org.mybatis.example.BlogMapper.selectBlog형태로 명시된다.
그러므로 
~~~java
BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = mapper.selectBlog(101);
~~~
로 select구문이 mapping된다. 

blogmapper interface는 깔끔하고 return type을 정의하지 않아도 되며 parameter에도 적용된다.

### 스코프(Scope) 와 생명주기(Lifecycle) ###

>Scope와 생명주기에 대해서 이해하는 것은 중요하다!

#### SqlSessionFactoryBuilder

SqlSessionFactoryBuilder의 가장 좋은 scope는 method scope이다. 예를 들어 method local variable이다. 
SQLSessionFactory를 build하기 위해서 SqlSessionFactoryBuilder를 재사용할 수도 있지만 유지하지 않는 것이 좋다. 

#### SqlSessionFactory

SqlSessionFactory의 가장 좋은 scope는 애플리케이션 스코프이다. singleton pattern이나 static singleton pattern을 사용하는 것이다. Spring과 같은 의존성 삽입을 통해서 생명주기를 singleton으로 관리 할 수 있다.
>spring에서 bean객체를 만들면 무조건 singleton이다. 

#### SqlSession
SqlSession의 가장 좋은 scope는 method scope이다. SqlSession은 Http요청을 받을 때마다 만들고 응답을 return하면 닫아야 한다. 무조건 finally block에서 close 시켜야한다. 
>SqlSession은 static variable로 사용하면 안된다 !

~~~java
SqlSession session = sqlSessionFactory.openSession();
try {
  // do work
} finally {
  session.close();
}
~~~

#### Mapper 인스턴스 ####

Mapper는 mapping된 구문을 binding하기 위해 만들어야할 interface이다. mapper는 Sqlsession에서 생성하므로 가장 좋은 scope도 SqlSession과 같은 method scope이다. 

~~~java
SqlSession session = sqlSessionFactory.openSession();
try {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  // do work
} finally {
  session.close();
}
~~~





