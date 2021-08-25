

## 第1章 什么是Filter

### 1.1 Filter简介

- Filter中文意思为过滤器。顾名思义，过滤器可在浏览器以及目标资源之间起到一个过滤的作用。例如：水净化器，可以看成是生活中的一个过滤器，他可以将污水中的杂质过滤，从而使进入的污水变成净水。

- 对于WEB应用来说，过滤器是一个驻留在服务器中的WEB组件，他可以截取客户端和WEB资源之间的请求和响应信息。
  - WEB资源可能包括Servlet、JSP、HTML页面等。

![1558767049205](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558767049205.png)

- 当服务器收到特定的请求后，会先将请求交给过滤器，程序员可以在过滤器中对请求信息进行读取修改等操作，然后将请求信息再发送给目标资源。目标资源作出响应后，服务器会再次将响应转交给过滤器，在过滤器中同样可以对响应信息做一些操作，然后再将响应发送给服务器。

- 也就是说过滤器可以在WEB资源收到请求之前，浏览器收到响应之前，对请求和响应信息做一些相应的操作。

- 在一个WEB应用中可以部署多个过滤器，多个过滤器就组成了一个过滤器链，请求和响应必须在经过多个过滤器后才能到达目标；

![1558774993622](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558774993622.png)

- 过滤器不是必须将请求传送到下一个过滤器（或WEB资源），也可以自己来处理请求，发送响应。
- 当配置多个Filter以后就有一个执行顺序的问题，实际执行顺序是按照在web.xml文件中servlet-mapping的顺序决定的，如果顺序越靠前越先被调用。

### 1.2 总结

- Filter是一个接口。
- Filter是Java Web三大组件之一。（JavaWeb三大组件分别是：Servlet小程序、Filter过滤器、Listener监听器）
- Filter是服务器专门用来过滤请求，拦截响应的。 
- **Filter的常见作用：**
  - 检查用户访问权限
  - 设置请求响应编码，解决乱码问题

### 1.3 主要API

- 编写Filter和编写Servlet类似，都需要实现接口。

#### 1.3.1 Filter接口

- 编写Filter需要实现Filter接口，我们来看一下Filter接口的主要方法：

  ![1558775226983](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558775226983.png)

  - init()方法用于初始化Filter
  - doFilter()作用和service()方法类似，是过滤请求和响应的主要方法。
  - destroy()用于在Filter对象被销毁前做一些收尾工作。如：释放资源等。

#### 1.3.2 FilterConfig接口

![1558775582961](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558775582961.png)

- FilterConfig对象在服务器调用init()方法时传递进来。
  - getFilterName() 获取Filter的名字
  - getServletContext() 获取ServletContext对象（即application）
  - getInitParameter() 获取Filter的初始化参数
  - getInitParameterNames() 获取所有初始化参数的名字

#### 1.3.3 FilterChain接口

![1558775624400](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558775624400.png)

- FilterChain对象是在doFilter()方法被调用时作为参数传递进来的。
  - doFilter()方法用于调用Filter链上的下一个过滤器，如果当前过滤器为最后一个过滤器则将请求发送到目标资源。

## 第2章 Filter初体验

### 需求：

现在在WebContent目录下有一个目录admin。这个目录是管理员操作的目录。这个目录里有jsp文件，有html文件，还有图片资源文件。现在我们要让这些资源都在用户登录才能被访问。那么我们要怎么实现这样的需求。

### 思路：

前面我们讲过Session。有同学可能会想，我们可以在用户登录之后把用户的信息保存在Session域对象中。然后在jsp页面里通过Session域对象获取用户的信息。如果用户信息存在，说明用户已登录。否则就重定向到登录页面。这个方案可行。可是html页面呢? html页面是没有Session域对象的。

### 解决方案：

这就需要我们使用Filter过滤器来进行请求的拦截。然后判断Session域对象中是否包含用户的信息。

现在我们以admin目录下user.jsp为例进行讲解。

1、首先，我们需要创建一个类来实现Filter接口，用来检查Session中是否包含用户信息。

2、实现Filter中的doFilter方法

3、然后到web.xml文件中去配置Filter的过滤信息。

4、然后重启服务器访问测试



**操作步骤1：Filter1的类代码**

```java
package com.atguigu.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class Filter1 implements Filter {

	/**
	 * Filter初始化方法
	 */
	public void init(FilterConfig filterConfig) throws ServletException {

	}

	/**
	 * Filter的过滤方法
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		// 强转
		HttpServletRequest httpRequest = (HttpServletRequest) request;
		HttpServletResponse httpResponse = (HttpServletResponse) response;
		// 获取用户登录信息
		String username = (String) httpRequest.getSession().getAttribute("username");
		if (username != null) {
			// 过滤器中，只要允许用户访问资源，一定要调用chain.doFilter方法，否则用户永远访问不到资源
			chain.doFilter(request, response);
		} else {
			// 如果用户未登录。返回登录页面
			httpResponse.sendRedirect(httpRequest.getContextPath() + "/login.jsp");
		}
	}

	/**
	 * Filter销毁的方法
	 */
	public void destroy() {
	}

}

```

**操作步骤2：web.xml文件中的Filter配置**

```xml
<!-- 配置Filter1 -->
<filter>
	<!-- 给Filter1起一个名字 -->
	<filter-name>Filter1</filter-name>
	<!-- 是哪一个Filter类，即全类名 -->
	<filter-class>com.atguigu.filter.Filter1</filter-class>
</filter>

<filter-mapping>
	<!-- Filter的名字 -->
	<filter-name>Filter1</filter-name>
	<!-- Filter1的过滤地址
		表示过滤http://127.0.0.1:8080/day17/admin/user.jsp
	-->
	<url-pattern>/admin/user.jsp</url-pattern>
</filter-mapping>

```

- 除此之外在filter-mapping还有一个子标签dispatcher，该标签用来指定需要Filter处理的请求类型，该标签可以配置四个值：

  ```xml
  <!-- 用户直接访问资源时，会调用Filter -->
  <dispatcher>REQUEST</dispatcher>
  
  <!-- 通过转发访问时，会调用Filter -->
  <dispatcher>FORWARD</dispatcher>
  
  <!-- 通过动态包含获取时，会调用Filter -->
  <dispatcher>INCLUDE</dispatcher>
  
  <!-- 当通过异常处理访问页面时，会调用Filter -->
  <dispatcher>ERROR</dispatcher>
  ```

  - 这四种情况可以设置一个，也可以同时设置多个，如果不设置那么默认为REQUEST。

## 第3章 Filter的生命周期

**Servlet的生命周期**

1. 先执行构造方法

2. 执行init方法做初始化操作

3. 执行service方法

4. 销毁的时候调用destory方法

 

**Filter生命周期：**

1. 先执行Filter的构造方法

2. 然后执行Filter的init()方法，对象创建后，马上就被调用，对Filter做一些初始化操作

3. 执行Filter的doFilter()方法，每次访问目标资源，只要匹配过滤的地址，就会调用。

4. 执行Filter的destroy()方法，服务器停止时调用，用来释放资源。

***

**创建一个Filter2类。代码如下：**

```java
package com.atguigu.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class Filter2 implements Filter {

	public Filter2() {
		System.out.println("Filter2 构造 方法 被调用");
	}
	
	/**
	 * Filter初始化方法
	 */
	public void init(FilterConfig filterConfig) throws ServletException {
		System.out.println("Filter2 init 方法被调用。初始化……");
	}

	/**
	 * Filter的过滤方法
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		System.out.println("Filter2 doFilter 方法被调用  ");
		// 一定要调用此方法，否则用户访问的资源会访问不到。
		chain.doFilter(request, response);
	}

	/**
	 * Filter销毁的方法
	 */
	public void destroy() {
		System.out.println("Filter2 的destroy方法被调用……");
	}

}

```

**web.xml文件中的配置：**

```xml
<!-- 配置Filter2 -->
	<filter>
		<!-- 给Filter2起一个名字 -->
		<filter-name>Filter2</filter-name>
		<!-- 是哪一个Filter类 -->
		<filter-class>com.atguigu.filter.Filter2</filter-class>
	</filter>
	<filter-mapping>
		<!-- Filter的名字 -->
		<filter-name>Filter2</filter-name>
		<!-- Filter1的过滤地址
			表示过滤http://127.0.0.1:8080/day17/login.jsp
		 -->
		<url-pattern>/login.jsp</url-pattern>
	</filter-mapping>

```

然后打开浏览器访问 http://127.0.0.1:8080/day17/login.jsp

**查看整个控制台的打印如下：**

1. Filter在工程启动的时候初始化。 

2. 在访问过滤的时候调用doFilter()方法

3. Tomcat关闭Filter被销毁的时候调用destory()方法

![1558764250206](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558764250206.png)





## 第4章 FilterConfig类

- 作用：FilterConfig类和ServletConfig类是一样的。可以获取Filter在web.xml文件中的配置信息，做初始化之用。

- 我们可以在web.xml文件中给Filter添加初始化参数。然后在init初始化方法中使用FilterConfig类获取到初始化的参数。

- FilterConfig类，一般有三个作用：
  - 获取Filter在web.xml文件中配置的名称
  - 获取Filter在web.xml文件中配置的初始化参数
  - 通过FilterConfig类获取ServletContext对象实例



**第一步：修改Filter2在web.xml中的配置信息**

```xml
<!-- 配置Filter2 -->
	<filter>
		<!-- 给Filter2起一个名字 -->
		<filter-name>Filter2</filter-name>
		<!-- 是哪一个Filter类 -->
		<filter-class>com.atguigu.filter.Filter2</filter-class>
		<!-- 配置初始化参数 -->
		<init-param>
			<!-- 初始化参数的名称 -->
			<param-name>username</param-name>
			<!-- 初始化参数的值 -->
			<param-value>root</param-value>
		</init-param>
	</filter>

```

**第二步：修改Filter2中init方法的代码**

```java
/**
	 * Filter初始化方法
	 */
	public void init(FilterConfig filterConfig) throws ServletException {
		System.out.println("Filter2 init 方法被调用。初始化……");
		// 获取Filter的名称
		String filterName = filterConfig.getFilterName();
		System.out.println("Filter name ==>>> " + filterName);
		// 获取初始化参数。username的值
		String username = filterConfig.getInitParameter("username");
		System.out.println("username ==>> " + username);
		// 获取ServletContext的对象实例 
		ServletContext ctx = filterConfig.getServletContext();
		System.out.println(ctx);
	}

```

**第三步：重启Tomcat服务器，控制台打印如下：**

![1558764469028](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558764469028.png)



## 第5章 FilterChain过滤器链（重点）

- FilterChain是整个Filter过滤器的调用者。Filter与Filter之间的传递，或者Filter与请求资源之间的传递都靠FilterChain.doFilter方法。

- 一般Filter.doFilter中的代码分为三段：
  - 第一段是FilterChain.doFilter之前的代码。一般用来做请求的拦截，检查用户访问的权限，访问日记的记录。参数编码的设置等等操作。
  - 第二段是FilterChain.doFilter方法。此方法可以将代码的执行传递到下一个Filter中。或者是传递到用户最终访问的资源中。
  - 第三段是FilterChain.doFilter之后的代码。主要用过做一些日志操作。我们很少会在第三段中做太多复杂的操作。

- 在每一个Filter类的doFilter方法中，一定要调用chain.doFilter方法，除非你想要阻止用户继续往下面访问。否则一定要调用FilterChain的doFilter方法。



**5.1 图解：多个Filter过滤器的代码流转**

​	![1558764647060](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558764647060.png)

**5.2 现在我们添加两个Filter类，对同一个资源进行过滤**

第一个Filter类ChainFilter1 代码：

```java
package com.atguigu.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class ChainFilter1 implements Filter {

	public void init(FilterConfig filterConfig) throws ServletException {
	}

	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		System.out.println("资源访问前---ChainFilter1 -- 开始执行");
		// 转发下一个Filter或者请求的资源
		chain.doFilter(request, response);
		System.out.println("资源访问后---ChainFilter1 -- 执行结束");
	}

	public void destroy() {
	}

}

```

第二个Filter类ChainFilter2 代码：

```java
package com.atguigu.filter;import java.io.IOException;import javax.servlet.Filter;import javax.servlet.FilterChain;import javax.servlet.FilterConfig;import javax.servlet.ServletException;import javax.servlet.ServletRequest;import javax.servlet.ServletResponse;public class ChainFilter2 implements Filter {	public void init(FilterConfig filterConfig) throws ServletException {	}	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)			throws IOException, ServletException {		System.out.println("资源访问前---ChainFilter2 -- 开始执行");		// 转发下一个Filter或者请求的资源		chain.doFilter(request, response);		System.out.println("资源访问后---ChainFilter2 -- 执行结束");	}	public void destroy() {	}}
```

**5.3 在web.xml文件中的配置如下：**

```xml
<filter>		<filter-name>ChainFilter1</filter-name>		<filter-class>com.atguigu.filter.ChainFilter1</filter-class>	</filter>	<filter-mapping>		<filter-name>ChainFilter1</filter-name>		<url-pattern>/chainFilter.jsp</url-pattern>	</filter-mapping>	<filter>		<filter-name>ChainFilter2</filter-name>		<filter-class>com.atguigu.filter.ChainFilter2</filter-class>	</filter>	<filter-mapping>		<filter-name>ChainFilter2</filter-name>		<url-pattern>/chainFilter.jsp</url-pattern>	</filter-mapping>
```

**5.4 WebContent/chainFilter.jsp文件的内容如下：**

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"    pageEncoding="UTF-8"%><!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"><html>	<head>		<meta http-equiv="pragma" content="no-cache" />		<meta http-equiv="cache-control" content="no-cache" />		<meta http-equiv="Expires" content="0" />		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">		<title>Insert title here</title>	</head>	<body>		<%			System.out.println("这是请求资源的代码");		%>		这是ChainFilter.jsp	</body></html>
```

**5.5 打开浏览器输入http://127.0.0.1:8080/day17/chainFilter.jsp回车访问：**

![1558764839164](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558764839164.png)

> **千万要注意：**在Filter类的doFilter方法中，除非你要拦截请求的资源，否则一定要调用FilterChain参数的doFilter方法让代码的执行传递到下一个Filter或访问的资源中。



## 第6章 Filter的拦截路径（目标资源的配置）

- Filter的目标资源指的是需要调用Filter来进行过滤处理的资源，例如上文我们配置的/index.html就是我们的目标资源，当我们访问项目根目录下的index.html时就会调用HelloFilter来进行过滤。

- 目标资源的配置方式主要有以下两大种：

  - 第一种：通过filter-mapping的url-pattern来配置（与Servlet的url-pattern的规则相同）

    - **精确匹配：/路径/资源名**

      比如：/index.html、/hello/index.jsp 、 /client/LoginServlet 等，只要在请求地址完全一样时才会调用Filter

    - **目录匹配：/路径名/*** 

      比如1：/abc/* 表示可以拦截abc目录下的所有资源，甚至是abc目录下的其他目录。其中：/* 表示访问 当前工程下所有资源

      比如2：/* 表示只要访问项目根目录下的资源就会调用Filter

    - **后缀名匹配：*.后缀名** 

      比如：*.jsp 表示拦截所有后缀为jsp文件资源

  - 第二种：通过filter-mapping中的servlet-name来指定要过滤的Servlet

    - 如：以下是一个项目中的web.xml配置文件，在项目中有一个Filter加做HelloFilter，一个Servlet叫做HelloServlet。在Filter的filter-mapping中增加了一个servlet-name标签，将该标签的值设置成Servlet的名字，在访问Servlet时就会调用该过滤器过滤请求。

      ```xml
      <filter>    <filter-name>HelloFilter</filter-name>    <filter-class>com.atguigu.web.filter.HelloFilter</filter-class>  </filter>  <filter-mapping>    <filter-name>HelloFilter</filter-name>    <servlet-name>HelloServlet</servlet-name>  </filter-mapping>  <servlet>    <servlet-name>HelloServlet</servlet-name>    <servlet-class>com.atguigu.web.servlet.HelloServlet</servlet-class>  </servlet>  <servlet-mapping>    <servlet-name>HelloServlet</servlet-name>    <url-pattern>/HelloServlet</url-pattern>  </servlet-mapping>
      ```

针对于第一种情况：

精确匹配前面 ，我们已经演示过了。

下面我们以目录匹配为示例展示代码。大家可以在此基础上修改web.xml文件中的\<url-pattern>标签来测试自己想要的路径。

**1. Filter的代码如下**

```java
package com.atguigu.filter;import java.io.IOException;import javax.servlet.Filter;import javax.servlet.FilterChain;import javax.servlet.FilterConfig;import javax.servlet.ServletException;import javax.servlet.ServletRequest;import javax.servlet.ServletResponse;public class FilterPath implements Filter {	@Override	public void init(FilterConfig filterConfig) throws ServletException {	}	@Override	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)			throws IOException, ServletException {		System.out.println("filter path 执行了");		// 将代码执行传递到下一个Filter或者是请求资源		chain.doFilter(request, response);	}	@Override	public void destroy() {	}}
```

**2. web.xml文件中的配置内容：**

```xml
<filter>	<filter-name>FilterPath</filter-name>	<filter-class>com.atguigu.filter.FilterPath</filter-class></filter><filter-mapping>	<filter-name>FilterPath</filter-name>	<url-pattern>/admin/*</url-pattern></filter-mapping>
```



## 第7章 HttpFilter

- 回想Servlet中学习，我们发现：实现Servlet接口，不如继承HttpServlet应用方便。所以，我们想到需要继承HttpFilter。

- 如果tomcat类库没有提供HttpFilter,就需要我们自己设计一个HttpFilter。

  - 类比最终创建Servlet的方式，我们发现设计HttpFilter大体分为以下几个步骤：

    1. 提供getFilterConfig()和getServletContext()

       ![1558764839164](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558775624401.png)

    2. 将doFilter()重载并抽象化处理

    3. 原有doFilter()方法需要将参数转换类型后，调用重载doFilter()方法

       ![1558764839164](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/12_Filter/尚硅谷_张春胜_Filter.assets/1558775624402.png)





