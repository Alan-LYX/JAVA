# 会话控制

讲师：尚硅谷-张春胜

email：zhangchunsheng_it@163.com

***

## 第1章 Cookie的介绍

### 1.1 为什么需要Cookie

HTTP是无状态协议，服务器不能记录浏览器的访问状态，也就是说服务器不能区分两次请求是否由一个客户端发出。这样的设计严重阻碍了Web程序的设计。如：在我们进行网购时，买了一条裤子，又买了一个手机。由于HTTP协议是无状态的，如果不通过其他手段，服务器是不能知道用户到底买了什么。而Cookie就是解决方案之一。

### 1.2 Cookie是什么

- Cookie，翻译是小饼的意思。实际上就是服务器保存在浏览器上的一段信息。浏览器有了Cookie之后，每次向服务器发送请求时都会同时将该信息发送给服务器，服务器收到请求后，就可以根据该信息处理请求。

- 例如：我们上文说的网上商城，当用户向购物车中添加一个商品时，服务器会将这个条信息封装成一个Cookie发送给浏览器，浏览器收到Cookie，会将它保存在内存中（注意这里的内存是本机内存，而不是服务器内存），那之后每次向服务器发送请求，浏览器都会携带该Cookie，而服务器就可以通过读取Cookie来判断用户到底买了哪些商品。当用户进行结账操作时，服务器就可以根据Cookie的信息来做结算。

- Cookie的用途：
  - 网上商城的购物车
  - 保持用户登录状态

- 总结一句话：Cookie，是一种服务器告诉浏览器以键值对形式存储小量信息的技术。

### 1.3 Cookie的工作原理

- 总的来看Cookie像是服务器发给浏览器的一张“会员卡”，浏览器每次向服务器发送请求时都会带着这张“会员卡”，当服务器看到这张“会员卡”时就可以识别浏览器的身份。

- 实际上这个所谓的“会员卡”就是服务器发送的一个响应头：

  ![1558667076701](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558667076701.png)

  如图Set-Cookie这个响应头就是服务器在向浏览器发“会员卡”，这个响应头的名字是Set-Cookie，后边JSESSIONID=95A92EC1D7CCB4ADFC24584CB316382E和 Path=/Test_cookie，是两组键值对的结构就是服务器为这个“会员卡”设置的信息。浏览器收到该信息后就会将它保存到内存或硬盘中。

- 当浏览器再次向服务器发送请求时就会携带这个Cookie信息：

  ![1558667127766](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558667127766.png)

  这是浏览器发送的请求报文，中间画红框的就是Cookie信息，这里可以理解为浏览器这次带着“会员卡”再次访问服务器。

- 于是服务器就可以根据Cookie信息来判断浏览器的状态。

- Cookie的缺点

  - Cookie因为请求或响应报文发送，无形中增加了网络流量。

  - Cookie是明文传送的安全性差。

  - 各个浏览器对Cookie有限制，使用上有局限。

## 第2章 Cookie的使用

### 2.1 Cookie的创建与设置

1. 在Servlet中创建Cookie对象，并添加到Response中。
2. 然后打开浏览器访问Servlet程序，服务器将Cookie信息发送给浏览器。
3. 浏览器收到Cookie后会自动保存，然后我们可以在下次浏览器发送请求时读取Cookie信息。

> 说明：按下F12查看Cookie内容

**图解Cookie的创建过程：**

![1558626326421](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626326421.png)

**Cookie的创建代码：**

```java
/**
 * Cookie的代码
 */
public class CookieServlet extends BaseServlet {
	private static final long serialVersionUID = 1L;

	protected void createCookie(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		
		// Cookie的创建
		Cookie cookie = new Cookie("cookie-name", "cookie-Value");
		Cookie cookie2 = new Cookie("cookie-name2", "cookie-Value2");

		// 告诉浏览器保存
		response.addCookie(cookie);
		response.addCookie(cookie2);
		response.getWriter().write("已创建Cookie……");
	}
}

```

**web.xml文件中的配置**

```xml
<servlet>
	<servlet-name>CookieServlet</servlet-name>
	<servlet-class>com.atguigu.servlet.CookieServlet</servlet-class>
</servlet>

<servlet-mapping>
	<servlet-name>CookieServlet</servlet-name>
	<url-pattern>/cookieServlet</url-pattern>
</servlet-mapping>

```

**修改html页面中连接的访问地址为：**

```html
<li><a href="cookieServlet?action=createCookie" target="target">Cookie的创建</a></li>
```

然后点击访问。记住，访问的时候，一定不是把html的页面托到浏览器中访问，而是在浏览器里输出地址，通过访问Tomcat服务器访问页面。

**浏览器工具--查看结果：**

谷歌浏览器，直接按下F12功能键，会弹出调试工具，选择Resource-----Cookies----localhost查看localhost域名下的cookie。

![1558626500878](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626500878.png)

如果是火狐浏览器。同样按下F12功能键，弹出调试工具（一定要记住启用所有窗口）。选择Cookies选择卡

![1558626536006](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626536006.png)

### 2.2 Cookie的读取

读取Cookie主要指读取浏览器中携带的Cookie。

1. 服务器端获取浏览器传过来的Cookie代码：request.getCookies()
2. 遍历Cookie数组，获取所有Cookie信息
3. 修改html连接，点击访问
4. 打开浏览器工具查看HTTP协议内容
5. 查看服务器代码获取Cookie后的输出

**图解Cookie的获取过程**

![1558626636876](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626636876.png)

**获取Cookie的代码：**

```java
protected void getCookie(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// 获取所有cookie对象
		Cookie[] cookies = request.getCookies();
		// 如果没有cookie，则返回null。
		if (cookies != null) {
			// 有cookie则遍历
			for (Cookie cookie : cookies) {
				response.getWriter().write("Cookie名：" + cookie.getName() 
						+ "<br/>Cookie值：" + cookie.getValue() + "<br/><br/>");
			}
		} else {
			response.getWriter().write("没有Cookie");
		}
		
	}

```

**修改html页面中的连接访问地址为：**

```html
<li><a href="cookieServlet?action=getCookie" target="target">Cookie的获取</a></li>
```

修改完之后，点击连接访问服务器。

**通过浏览器查看请求头：**

![1558626743021](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626743021.png)

**页面输出：**

![1558626783286](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626783286.png)

### 2.3 Cookie值的修改

1. 在Servlet中添加修改Cookie值的代码

2. 修改html页面中修改cookie的连接，并访问

3. 打开浏览器的调试工具查看，请求头和响应头中Cookie的信息

**图解修改Cookie值的过程：**

![1558626892380](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626892380.png)

**修改Cookie值的代码：**

```java
protected void updateCookie(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		
		// 创建一个已存在key的Cookie对象
		Cookie cookie = new Cookie("cookie-name", null);
		// 修改Cookie的值
		cookie.setValue("this is new value");
		// 通知浏览器保存修改
		response.addCookie(cookie);
		response.getWriter().write("Cookie…已修改值");
	}

```

**修改html页面中update修改cookie的访问地址为：**

```html
<li><a href="cookieServlet?action=updateCookie" target="target">Cookie值的修改</a></li>
```

修改完Cookie更新的连接访问之后，点击访问。

**打开浏览器调试工具查看请求头和响应头中Cookie的信息：**

![1558626979002](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558626979002.png)

**在Resource页签中，查看修改后Cookie的内容：**

![1558627009024](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627009024.png)

### 2.4 Cookie的有效时间

- 经过上边的介绍我们已经知道Cookie是存储在浏览器中的，但是可想而知一般情况下浏览器不可能永远保存一个Cookie，一来是占用硬盘空间，再来一个Cookie可能只在某一时刻有用没必要长久保存。所以我们还需要为Cookie设置一个有效时间。

- Cookie的实例方法setMaxAge(  ) 控制Cookie存活的时间，接收一个int型参数，单位：秒。
  - 参数设置为0，即：setMaxAge(0)：立即失效，表示浏览器一收到响应后，就马上删除Cookie，下次浏览器发送请求时，将不会再携带该Cookie。
  - 参数设置大于0：比如setMaxAge(60)，表示有效的秒数60秒后，Cookie失效。
  - 参数设置小于0：比如setMaxAge(-1)，表示当前会话有效。也就是关闭浏览器后Cookie失效，被删除。
  - 如果不设置失效时间，默认为当前会话有效，一旦关闭浏览器，Cookie就失效，被删除。

**图解Cookie过期时间被修改的过程：**

![1558627416248](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627416248.png)

**修改Cookie过期时间的代码：**

```java
protected void deleteCookie(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// 获取Cookie
		Cookie[] cookies = request.getCookies();
		Cookie cookie = null;
		if (cookies != null) {
			// 查找出我们需要修改的Cookie对象
			for (Cookie c : cookies) {
				// 获取键为cookie-name的cookie对象
				if ("cookie-name".equals(c.getName())) {
					cookie = c;
					break;
				}
			}
		}
		if (cookie != null) {
//          负数表示浏览器关闭后删除，正数表示多少秒后删除
//			 设置为零，表示立即删除Cookie
			cookie.setMaxAge(0);
			response.addCookie(cookie);
			response.getWriter().write("删除Cookie……");
		}
		
	}

```

**修改html页面中的连接地址：**

```html
<li><a href="cookieServlet?action=deleteCookie" target="target">Cookie立即删除</a></li>
```

修改之后，点击访问。

**通过浏览器调试工具查看请求响应信息。和Cookie信息：**

![1558627434014](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627434014.png)

![1558627444677](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627444677.png)

### 2.5 Cookie的路径Path设置

- Cookie的路径指告诉浏览器访问哪些地址时应该携带该Cookie，我们知道浏览器会保存很多不同网站的Cookie，比如百度的Cookie，新浪的Cookie，腾讯的Cookie等等。那我们不可能访问百度的时候携带新浪的Cookie，也不可能访问每个网站时都带上所有的Cookie，这是不现实的。所以往往我们还需要为Cookie设置一个Path属性，来告诉浏览器何时携带该Cookie。

- 我们通过调用Cookie的实例方法setPath()，来设置Cookie的Path路径。这个路径由浏览器来解析。
  - / ：代表服务器的根目录
  - 如果设置有效路径为：`/day14/abc`，则：下面几个路径能访问到Cookie
    - /day14/abc                             能获取Cookie
    - /day14/xxxx.xxx                     不能获取Cookie
    - /day14/abc/xxx.xxx               能获取Cookie
    - /day14/abc/a/b/c                   能获取Cookie
  - 如果不设置，默认会在访问“`/项目名`”下的资源时携带
    - 如：“/项目名/index.jsp” 、 “/项目名/hello/index.jsp”

**设置Cookie对象Path属性的代码**

```java
protected void setPath(HttpServletRequest request, HttpServletResponse response)			throws ServletException, IOException {				// 创建一个Cookie对象		Cookie cookie = new Cookie("cookie-path", "test");		// 设置Cookie的有效访问路径为/day14/abc/下所有资源		cookie.setPath(request.getContextPath() + "/abc");		// 通知浏览器保存修改		response.addCookie(cookie);		response.getWriter().write("设置Cookie…的path路径");	}
```

**当我们通过浏览器访问上面的代码，响应头中会有下如下信息：**

![1558627560065](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627560065.png)

**分别通过不同的路径去访问，查看浏览器的请求头，是否会携带/day14/abc 路径下Cookie**

访问/day14/cookie.html请求头信息

![1558627601165](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627601165.png)

访问/day14/abc/a/b/c.html请求头信息

![1558627633503](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627633503.png)



## 第3章 Cookie练习：用户免输入

**需求：第一次登录之后，一个星期内免输入用户名登录。**

**1 - 服务器Servlet的代码**

```java
protected void login(HttpServletRequest request, HttpServletResponse response)			throws ServletException, IOException {		// 获取请求参数		String username = request.getParameter("username");		String password = request.getParameter("password");		if ("admin".equals(username) && "admin".equals(password)) {			// 创建Cookie			Cookie cookie = new Cookie("username", username);			// 设置过期时间为一个星期			cookie.setMaxAge(60 * 60 * 24 * 7);			// 通知浏览器保存			response.addCookie(cookie);			response.getWriter().write("登录成功！");		} else {			response.sendRedirect(request.getContextPath() + "/login.jsp");		}	}
```

**2 - WebContent/login.jsp页面**

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"	pageEncoding="UTF-8"%><!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"><html><head><meta http-equiv="pragma" content="no-cache" /><meta http-equiv="cache-control" content="no-cache" /><meta http-equiv="Expires" content="0" /><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><title>Insert title here</title></head><body>    <form action="cookieServlet?action=login" method="post">	用户名：<input name="username" type="text" value="${ cookie.username.value }" /><br /> 	密&emsp;码：<input name="password" type="password" value="" /><br /> 	7天免输入:<input type="checkbox" name="ck" value="ck"><br /> 		<input type="submit" value="提 交" /></form></body></html>
```



## 第4章 Session的介绍

### 4.1 为什么需要Session

- 使用Cookie有一个非常大的局限，就是如果Cookie很多，则无形的增加了客户端与服务端的数据传输量。而且由于浏览器对Cookie数量的限制，注定我们不能再Cookie中保存过多的信息，于是Session出现。

- Session的作用就是在服务器端保存一些用户的数据，然后传递给用户一个名字为JSESSIONID的Cookie，这个JESSIONID对应这个服务器中的一个Session对象，通过它就可以获取到保存用户信息的Session。

### 4.2 Session是什么

- 首先Session是jsp中九大内置对象之一

- 其次Session是一个域对象


- 然后Session是在服务器端用来保存用户数据的一种技术。并且Session会话技术是基于Cookie实现的。




## 第5章 Session的使用

### 5.1 Session的创建和获取

- Session的创建时机是在request.getSession()方法第一次被调用时。
  - 补充：request.getSession()之后的调用都是获取已经创建了的Session对象

- Session被创建后，同时还会有一个名为JSESSIONID的Cookie被创建。

- 这个Cookie的默认时效就是当前会话。

![1558627840672](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627840672.png)

**下面是创建Session和获取Session。以及获取Session的 ID编号，获取Session是否是新创建的示例代码：**

```java
package com.atguigu.servlet;import java.io.IOException;import javax.servlet.ServletException;import javax.servlet.http.HttpServletRequest;import javax.servlet.http.HttpServletResponse;import javax.servlet.http.HttpSession;public class SessionServlet extends BaseServlet {	private static final long serialVersionUID = 1L;	public SessionServlet() {	}	protected void getSession(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {		System.out.println(request.getHeader("Cookie"));				// 第一次调用就是创建一个新的Session。如果Session已经创建过。就获取原来的会话。		HttpSession session = request.getSession();		// 输出会话id号，和是否是新创建		// session.getId()返回Session的唯一编号		// session.isNew()返回当前Session是否是刚创建的		response.getWriter().write("session ID:" + session.getId() + "<br/>是否是新的：" + session.isNew());	}}
```

**在web.xml文件中的配置：**

```xml
<servlet>		<servlet-name>SessionServlet</servlet-name>		<servlet-class>com.atguigu.servlet.SessionServlet</servlet-class>	</servlet>	<servlet-mapping>		<servlet-name>SessionServlet</servlet-name>		<url-pattern>/sessionServlet</url-pattern>	</servlet-mapping>
```

**第一次访问的结果：**

![1558627941430](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627941430.png)

**之后每次访问的结果：**

![1558627964822](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558627964822.png)

### 5.2 Session的工作原理

- Session被创建后，对应的Cookie被保存到浏览器中，之后浏览器每次访问项目时都会携带该Cookie。

- 当我们再次调用时会根据该JSESSIONID获取已经存在的Cookie，而不是再创建一个新的Cookie。

- 如果Cookie中有JSESSIONID，但是JSESSIONID没有对应的Session存在，则会重新创建一个HttpSession对象，并重新设置JSESSIONID。

![1558748151080](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558748151080.png)



### 5.3 Session数据的存取

- Session域对象数据的存取和其他三个域对象PageContext、Request、ServletContext是一样的。只需要调用下面两个方法：
  - setAttribute 设置属性
  - getAttribute 获取属性

- 编写下面的java代码去访问，就可以在Session域中设置属性，和获取属性。

```java
protected void setAttribute(HttpServletRequest request, HttpServletResponse response)			throws ServletException, IOException {		// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。		HttpSession session = request.getSession();		// 设置数据		session.setAttribute("abc", "abc value");		response.getWriter().write("设置属性值成功！");	}		protected void getAttribute(HttpServletRequest request, HttpServletResponse response)			throws ServletException, IOException {		// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。		HttpSession session = request.getSession();		// 设置数据		String value = (String) session.getAttribute("abc");		response.getWriter().write("获取abc的属性值：" + value);	}
```

修改session.html 中访问的连接地址，然后点击访问。

```html
<li>	<a href="sessionServlet?action=setAttribute" target="target">Session域数据的存储</a></li><li>	<a href="sessionServlet?action=getAttribute" target="target">Session域数据的获取</a></li>
```

访问后效果图：

![1558628097144](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558628097144.png)

![1558628140230](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558628140230.png)



### 5.4 Session 的有效时间

- **基本原则**

  - Session对象在服务器端不能长期保存，它是有时间限制的，超过一定时间没有被访问过的Session对象就应该释放掉，以节约内存。所以Session的有效时间并不是从创建对象开始计时，到指定时间后释放。而是**从最后一次被访问开始计时，统计其“空闲”的时间。**

- **默认时效**

  - 在tomcat的conf目录下web.xml配置文件中能够找到如下配置：

    ```xml
    <!-- ==================== Default Session Configuration ================= -->  <!-- You can set the default session timeout (in minutes) for all newly   -->  <!-- created sessions by modifying the value below.                       -->    <session-config>        <session-timeout>30</session-timeout>    </session-config>
    ```

    > 说明：Session对象默认的最长有效时间为30分钟。

- **手动设置1：全局**

  - 我们也可以在自己工程的web.xml文件中配置Session会话的超时时间为10分钟。

  - 记住一点，我们在web.xml文件中配置的Session会话超时时间是对所有Session都生效的。

    ```xml
    <!-- 设置Session默认的过期时间  --><session-config>	<!-- 以分钟为单位。10分钟超时  -->    <session-timeout>10</session-timeout></session-config>
    ```

- **手动设置2：局部**

  - int getMaxInactiveInterval()    获取超时时间。以秒为单位。
  - setMaxInactiveInterval (int seconds)  设置用户多长时间没有操作之后就会Session过期。以秒为单位。
    - 如果是正数。表示用户在给定的时间内没有任意操作，Session会话就会过期。
    - 如果是非正数（零&负数）。表示Session永不过期。

- **强制失效**

  - invalidate()

- **示例代码**

```java
Session在3秒之后超时// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。HttpSession session = request.getSession();// 设置过期时间为3秒 session.setMaxInactiveInterval(3);Session在1分钟之后超时// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。 HttpSession session = request.getSession();// 设置过期时间为1分钟session.setMaxInactiveInterval(60);Session在1小时之后超时// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。HttpSession session = request.getSession();// 设置过期时间为1小时session.setMaxInactiveInterval(60 * 60);Session在1天之后超时// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。HttpSession session = request.getSession();// 设置过期时间为1天session.setMaxInactiveInterval(60 * 60 * 24);Session在1周之后超时// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。HttpSession session = request.getSession();// 设置过期时间为1周session.setMaxInactiveInterval(60 * 60 * 24 * 7);Session永远不超时// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。HttpSession session = request.getSession();// 设置永远不超时session.setMaxInactiveInterval(-1);Session马上超时（失效）// 第一个调用就是获取一个新的Session。如果Session已经创建过。就获取原来的会话。HttpSession session = request.getSession();// 让Session对象立即过期session.invalidate();
```

### 5.4 Session对象的释放

- Session对象空闲时间达到了目标设置的最大值，自动释放

- Session对象被强制失效

- Web应用卸载

- 服务器进程停止

### 5.5 Session的活化和钝化

- Session机制很好的解决了Cookie的不足，但是当访问应用的用户很多时，服务器上就会创建非常多的Session对象，如果不对这些Session对象进行处理，那么在Session失效之前，这些Session一直都会在服务器的内存中存在。那么，就出现了Session活化和钝化的机制。

- **Session钝化：**Session在一段时间内没有被使用或关闭服务器时，会将当前存在的Session对象及Session对象中的数据从内存序列化到磁盘的过程，称之为钝化。

- **Session活化：**Session被钝化后，服务器再次调用Session对象或重启服务器时，将Session对象及Session对象中的数据从磁盘反序列化到内存的过程，称之为活化。

- 如果希望Session域中的对象也能够随Session钝化过程一起序列化到磁盘上，则对象的实现类也必须实现java.io.Serializable接口。不仅如此，如果对象中还包含其他对象的引用，则被关联的对象也必须支持序列化，否则会抛出异常：java.io.NotSerializableException

### 5.6 浏览器和Session关联的技术内幕

在前面的演示中我们发现一旦浏览器关闭之后，我们再去获取Session对象就会创建一个新的Session对象。这是怎么回事呢。现在让我们来看一下。这一系列操作过程中的内幕细节。

![1558628349795](D:\0JAVAstudy\尚硅谷1.07\3.阶段三 Web开发与实战应用\尚硅谷_JavaWeb课件_张春胜\11_会话控制_☆\尚硅谷_张春胜_会话控制.assets\1558628349795.png)

通过上图的分析，我们不难发现。当浏览器关闭之后。只是因为浏览器无法再通知服务器，之前创建的Session的会话id是多少了。所以服务器没办法找到对应的Session对象之后，就以为这是第一次访问服务器。就创建了新的Session对象返回。

## 第6章 URL重写（了解）

- 在整个会话控制技术体系中，保持JSESSIONID的值主要通过Cookie实现。但Cookie在浏览器端可能会被禁用，所以我们还需要一些备用的技术手段，例如：URL重写。

- URL重写其实就是将JSESSIONID的值以固定格式附着在URL地址后面，以实现保持JSESSIONID，进而保持会话状态。这个固定格式是：URL;jsessionid=xxxxxxxxx

- 例如：

  ```java
  targetServlet;jsessionid=97120112D5538009334F1C6DEADB1BE7
  ```

- 实现方式：

  - response.encodeURL(String)

  - response.encodeRedirectURL(String)

- 示例代码

  ```java
  //1.获取Session对象HttpSession session = request.getSession();		//2.创建目标URL地址字符串String url = "targetServlet";		//3.在目标URL地址字符串后面附加JSESSIONID的值url = response.encodeURL(url+";jsessionid=97120112D5538009334F1C6DEADB1BE7");		//4.重定向到目标资源response.sendRedirect(url);
  ```

## 第7章 处理表单重复提交问题

- 表单重复提交的危害
  - 可重复注册，对数据库进行批处理攻击。（验证码已解决该问题）
  - 可重复提交已付款表单，用户支付一次订单费用，下了多个订单
  - 等待...
- 解决表单重复提交的步骤
  - 生成一个不可重复（全球唯一）的随机数(uuid)
  - 在提交表单前，将随机数(uuid)分别存放到表单内的隐藏域，和session域对象中
  - 发送“提交表单”请求
  - 判断是否提交表单，具体操作如下：
    - 分别获取隐藏域和session域中的uuid
    - 判断两个域中的数据是否相等
      - 相等：提交表单，并将session域中的uuid移除
      - 不等：不提交表单
- UUID
  - 定义：是一个32位16进制的随机数
  - 特点：全球唯一
  - 使用：java.util.UUID.randomUUID()











