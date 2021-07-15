# JSP

## 第一章 为什么学习Jsp

### 1.1 现有技术不足

​		Servlet可以通过转发或重定向跳转到某个HTML文档。但HTML文档中的内容不受Servlet的控制。比如登录失败时，跳转回登录表单页面无法显示诸如“用户名或密码不正确”的错误消息，所以我们目前采用的办法是跳转到一个错误信息页面。如果通过Servlet逐行输出响应信息则会非常繁琐。

**Servlet输入html页面的程序代码：**

```java
package com.atguigu.servlet;

import java.io.IOException;
import java.io.Writer;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HtmlServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		// 设置返回的数据内容的数据类型和编码
		response.setContentType("text/html; charset=utf-8");
		// 获取字符输出流
		Writer writer = response.getWriter();
		//输出页面内容！
		writer.write("<!DOCTYPE html PUBLIC \"-//W3C//DTD HTML 4.01 Transitional//EN\" \"http://www.w3.org/TR/html4/loose.dtd\">");
		writer.write("<html>");
		writer.write("<head>");
		writer.write("<meta http-equiv=\"Content-Type\" content=\"text/html; charset=UTF-8\">");
		writer.write("<title>Insert title here</title>");
		writer.write("</head>");
		writer.write("<body>");
		writer.write("这是由Servlet程序输出的html页面内容！");
		writer.write("</body></html>");
	}

	protected void doPost(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
	}
}

```

​		上面的代码我们不难发现。通过Servlet输出简单的html页面信息都非常不方便。那我们要输出一个复杂页面的时候，就更加的困难，而且不利于页面的维护和调试。

### 1.2 Servlet与HTML

|      | Servlet                            | HTML               |
| ---- | ---------------------------------- | ------------------ |
| 长处 | 接收请求参数，访问域对象，转发页面 | 以友好方式显示数据 |
| 短处 | 以友好方式显示数据                 | 动态显示数据       |

### 1.3 总结

​		sun公司推出一种叫做JSP的动态页面技术帮助我们实现对页面输出繁锁工作。



## 第二章 Jsp简介

### 2.1 Jsp全称

- Jsp全称Java Server Pages，顾名思义就是运行在java服务器中的页面。由Sun 公司专门为了解决动态生成HTML文档的技术，也就是在我们JavaWeb中的动态页面。
- Jsp能够以HTML页面的方式呈现数据，是一个可以嵌入Java代码的HTML。
- Jsp其本质就是一个Servlet。Servlet能做的事情JSP都能做。
- Jsp必须运行在服务器中，不能直接使用浏览器打开。
- Jsp是Web网页的技术标准,主要语法组成包括：指令，html模板元素，脚本片段（小脚本），表达式，声明，注释，后缀是*.jsp。

### 2.2 Jsp与Html的区别

- Jsp是动态页面，html是静态页面。

  |          | 动态页面                                     | 静态页面                           |
  | -------- | -------------------------------------------- | ---------------------------------- |
  | 运行原理 | 通过服务器解析后，将数据在浏览器中显示       | 直接在浏览器中解析运行             |
  | 维护成本 | 较低，可以修改后台数据，进而影响页面中的数据 | 较高，必须将修改后的页面覆盖原页面 |
  | 数据库   | 可以连接数据库                               | 不可连接数据库                     |
  | 访问速度 | 较慢                                         | 较快                               |
  | 书写代码 | 可以书写java代码                             | 不能书写java代码                   |

### 2.3 Jsp与Servlet分工

- Jsp本质是一个Servlet ，翻译后的文件结构为：class helloworld_jsp : HttpJspBase : HttpServlet。
- Jsp主要负责显示及获取数据，从表面上看，Jsp相对于在html中嵌入java代码：jsp=html+java。
- Servlet主要负责处理业务，从表面上看，Servlet相当于在java中嵌入html代码：Servlet=java+html。
- 总结：相比于Servlet，Jsp更加善于处理显示页面，而Servlet更善于处理业务逻辑，两种技术各有专长，所以一般我们会将Servlet和Jsp结合使用，Servlet负责业务，Jsp负责显示。

### 2.4 Jsp基本格式

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	这是我的第一个jsp页面。
</body>
</html>
```



## 第三章 Jsp基本语法

### 4.1指令

- 语法格式：<%@ %>  

- 实例

  ```
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  ```

- 三大指令：<%@ page %> ，<%@ include %> ，| <%@ taglib %> 

### 4.2模板元素

- html&css&js&jQuery等...

### 4.3代码脚本片段（重点）

- 格式 ：<%%>

- 作用：在_jspService()方法中，书写java代码。

- 实例

  ```jsp
  <% int i = 0;%>
  ```

### 4.4表达式（重点）

- 格式： <%=%>

- 作用：将数据显示到页面，与out.print()或out.write()作用相同。

- 实例

  ```jsp
   <%= i %>
  ```

### 4.5声明（了解）

- 格式：<%!%> 
- 作用：在翻译后的class helloworld_jsp这个Servlet类中，书写java代码。

### 4.6注释：Jsp支持三种注释

- java：单行注释：//，多行注释：/**/ 
- html：<!-- -->
- jsp：<%-- --%>
- jsp中三种注释的比较，如下所示：

|          | JSP注释 | Java注释 | HTML注释 |
| -------- | ------- | -------- | -------- |
| JSP页面  | 可见    | 可见     | 可见     |
| Java代码 | 不可见  | 可见     | 可见     |
| 浏览器   | 不可见  | 不可见   | 可见     |



## 第四章 Jsp常用指令

### 5.1 语法格式

- <%@ 指令名   属性=属性值	属性2=属性值2  ... %> 

### 5.2 Jsp常用指令

#### 5.2.1 page指令

- 语法

  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  ```

- 属性

  - language：语言，值为java且仅java。
  - contentType：与response.setContentType()作用一致，设置浏览器字符集。
  - pageEncoding：设置Jsp页面的编码字符集。
  - import：导包
  - isErrorPage：设置当前页面是否为错误页面，默认值"false"。
    - ​    true：设置当前页面为错误页面，可以使用exception内置对象，捕获异常 。
    - ​    false：设置当前页面不是错误页面，不可以使用exception内置对象，捕获异常 。
  - errorPage：设置当前页面错误时的跳转目标页面。错误需要在_jspService()中才可以捕获。

#### 5.2.2 include指令:静态包含

- 语法

  ```jsp
  <%@include file="被包含文件的路径" %> 	
  ```

- 作用：将目标文件包含到当前文件中。

- 特点：被包含的文件不会被翻译&编译。（先包含，再翻译)

#### 5.2.3 taglib指令（略）

- 语法

  ```jsp
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
  ```

- 属性

  - prefix用来指定前缀名，我们通过该名来使用JSTL。
  - uri相当于库的唯一标识，因为JSTL由多个不同的库组成，使用该属性指定要导入哪个库。

- 作用：引入标签库。



## 第六章 Jsp常用动作标签

### 6.1 概述

- JSP动作标签与HTML标签不同，HTML标签由浏览器来解析，而JSP动作标签需要服务器（Tomcat）来运行。

### 6.2 常用的JSP动作标签

#### 6.2.1  转发动作标签

- 语法：\<jsp:forward>\</jsp:forward>

- 作用：在页面中用于转发操作

- 实例

  ```jsp
  <jsp:forward page="target.jsp"></jsp:forward>
  ```

- 转发子标签

  - 语法：<jsp:param value="paramValue" name="paramName"/>

  - 作用：在转发时设置请求参数，通过request.getParameter()在目标页面获取请求参数。

  - 实例

    ```jsp
    <jsp:forward page="target.jsp">
    	<jsp:param value="paramValue" name="paramName"/>
    </jsp:forward>
    ```

  - 注意：如果转发动作标签不需要设置请求参数，该标签开始与结束标签内部，不允许书写任何内容，（包括空格）

#### 6.2.2 动态包含动作标签

- 语法：<jsp:include page=*"target.jsp"*>

- 作用：动态包含，将其他页面包含到当前页面中。

- 实例

  ```jsp
  <jsp:include page="target.jsp"></jsp:include>
  ```

- 特点：被包含的文件同时会被翻译&编译。（先翻译，再包含）

  - 本质原理：当使用动态包含时，Tomcat会在生成的Servlet中加入如下代码：

    ```java
    org.apache.jasper.runtime.JspRuntimeLibrary.include(request, response, "target.jsp", out, false);
    ```

### 6.3 动态包含与静态包含的区别

|                    | @include指令                                                 | <jsp:include>标签                                          |
| ------------------ | ------------------------------------------------------------ | ---------------------------------------------------------- |
| 特点               | 静态包含                                                     | 动态包含                                                   |
| 语法的基本形式     | <%@ include   file=”…”%>                                     | <jsp:include   page=”…”/>                                  |
| 包含动作发生的时机 | 翻译期间                                                     | 请求期间                                                   |
| 是否生成java文件   | 不生成                                                       | 生成                                                       |
| 合并方式           | 代码复制                                                     | 合并运行结果                                               |
| 包含的内容         | 文件实际内容                                                 | 页面输出结果                                               |
| 代码冲突           | 有可能                                                       | 不可能                                                     |
| 编译次数           | 1                                                            | 包含的文件 + 1                                             |
| 适用范围           | 适用包含纯静态内容(CSS,HTML,JS)，或没有非常耗时操作。或少量java代码的jsp | 包含需要传递参数。含有大量java代码，运算，耗时很长的操作。 |



## 第七章 Jsp九大隐式对象（隐含对象|内置对象）

### 7.1 概述

> Jsp共有九大隐式对象，也叫隐含对象或内置对象。JSP隐式对象是JSP容器为每个页面提供的Java对象，开发者可以直接使用它们而不用显式声明。JSP隐式对象也被称为预定义变量。

- 对象详情见：工作原理图二。

### 7.2 对象详情

#### 7.2.1 pageContext

- 类型：PageContext
- 定义：代表页面域对象，用来代表整个JSP页面。
- 作用：
  1. 页面域对象，具体详见：下方四大域对象。
  2. 九大隐式对象的“大哥”，可以直接调用其他八大隐式对象。
- 在Servlet中获取方式：无。

#### 7.2.2 request

- 类型：HttpServletRequest

- 定义：代表浏览器向服务器发送的请求报文，该对象由服务器创建，最终以参数的形式发送到doGet()和doPost()方法中。

  > 每当客户端请求一个JSP页面时，JSP引擎就会制造一个新的request对象来代表这个请求。request对象提供了一系列方法来获取HTTP头信息，cookies，HTTP方法等等。

- 作用（详见Servlet中request对象）

  1.  获取请求参数
  2.  获取url地址参数
  3.  请求转发
  4.  向请求域中保存数据（获取数据&移除数据）
  5.  获取请求头信息

- 在Servlet中获取方式：doGet()或doPost()中直接使用。

#### 7.2.3 session

- 类型：HttpSession

- 定义：代表浏览器与服务器之间的会话。

- 作用

  - 会话域对象，具体详见：下方四大域对象。

  > session对象用来跟踪在各个客户端请求间的会话。

- 在Servlet中获取方式 ：request.getSession();

#### 7.2.4 application

- 类型：ServletContext

- 定义：Servlet上下文，代表当前web应用。

  > Web容器在启动时，它会为**每个Web应用程序都创建一个唯一对应的ServletContext对象**，意思是Servlet上下文，**代表当前Web应用。**

- 作用

  1. 获取项目的上下文路径(带/的项目名)：**getContextPath()**

  2. 获取虚拟路径所映射的本地真实路径：**getRealPath(String path)**

  3. 获取WEB应用程序的全局初始化参数（基本不用）

     - 设置Web应用初始化参数的方式是在web.xml的根标签下加入如下代码

       ```xml
       <web-app>
       	<!-- Web应用初始化参数 -->
       	<context-param>
       		<param-name>ParamName</param-name>
       		<param-value>ParamValue</param-value>
       	</context-param>
       </web-app>
       ```

       

     - 获取Web应用初始化参数

       ```java
       @Override
       public void init(ServletConfig config) throws ServletException {
       	//1.获取ServletContext对象
       	ServletContext application = config.getServletContext();
       	//2.获取Web应用初始化参数
       	String paramValue = application.getInitParameter("ParamName");
       	System.out.println("全局初始化参数paramValue="+paramValue);
       }
       ```

  4. 作为域对象共享数据:具体详见：下方四大域对象。

- 在Servlet中获取方式：使用this.getServletContext()方法获取。

#### 7.2.5 page

- 类型：Object
- 作用：this，当前类对象。

#### 7.2.6 response

- 类型：HttpServletResponse
- 定义：代表服务器向浏览器发送的响应报文，该对象由服务器创建，最终以参数的形式发送到doGet()和doPost()方法中。
- 作用：
  1. 向页面（响应体）中响应数据，数据包括文本、Html等。
  2. 重定向
  3. 设置响应头信息
- 在Servlet中获取方式：doGet()或doPost()中直接使用

#### 7.2.7 config

- 类型：ServletConfig
- 定义：代表当前Servlet的配置信息，每一个Servlet都有一个唯一对应的ServletConfig对象。
- 作用：
  1. 获取Servlet名称：getServletName()
  2. 获取全局上下文ServletContext对象：getServletContext()
  3. 获取Servlet初始化参数：getInitParameter(String) / getInitParameterNames()。
- 在Servlet中获取方式：this.getServletConfig()

#### 7.2.8 out

- 类型：JspWriter
- 定义：代表当前页面的输出流。
- 作用：与Servlet中的PrintWriter功能类似，将数据响应到页面，响应的数据可以是页面、页面片段、字符串等。
- 在Servlet中获取方式：无

#### 7.2.9 exception

- 类型：Throwable
- 定义：代表当前页面的异常对象。
- 作用：捕获处理页面中的异常信息。
- 在Servlet中获取方式：new Throwable()

**九大内置对象，都是我们可以在【代码脚本】中或【表达式脚本】中直接使用的对象。**



## 第八章 Jsp四大域对象

## 8.1 域对象概述

> ​	生活中使用“域对象”比较经典的行业，是快递行业。现如今快递行业大体分为，全球快递，全国快递，同城快递和同区快递。需求不同，使用不同“域对象”。
>
> ​	如：外卖一般使用同区快递，给北京朝阳区的朋友邮寄贺卡，一般使用同城快递。在某宝某东上购买外地商品，一般使用全国快递或全球快递。

## 8.2 程序中的域对象

### 8.2.1 域对象概述

> 程序中的域对象，主要负责在不同web资源之间进行数据交换，（如:servlet和jsp之间的数据交换）。由于不同的web资源之间需要共享数据，所以就有了域对象。
>
> 在Jsp中一共有四个域对象，分别是pageContext 、request、session、application。主要作用是能够在一定范围内共享数据。

### 8.2.2 域对象分析

**每个域对象内部都维护了一个Map<String , Object>，域对象的共同方法。**

- 设置属性到域中：void setAttribute(String key , Object value);
- 从域中获取指定的属性：Object  getAttribute(String key);
- 移除域中指定属性：void removeAttribute(String key);

### 8.2.3 域对象有效性

- pageContext: 当前页面中共享数据有效，离开当前页面失效。
  - 每个页面都有自己唯一的一个pageContext对象。
  - 注意servlet中没有该对象。
- request： 当前请求中共享数据有效。
  - 当前请求：转发、直接访问一个页面为当前请求。
  - 不在当前请求：重定向、 打开页面再点击页面中的超链接不在当前请求 。
- session： 一次会话范围中共享数据有效。
  - 当前会话：当前浏览器不关闭&不更换浏览器即为当前会话。
  - 只关心浏览器是否关闭，不关心服务器关闭重启。
  - 不同浏览器不共享会话。
- application： 在服务器运行的一次过程中共享数据有效。
  - 服务器关闭销毁

**小结**：

| 域对象      | 作用范围    | 起始时间    | 结束时间    |
| ----------- | ----------- | ----------- | ----------- |
| pageContext | 当前JSP页面 | 页面加载    | 离开页面    |
| request     | 同一个请求  | 收到请求    | 响应        |
| session     | 同一个会话  | 开始会话    | 结束会话    |
| application | 当前Web应用 | Web应用加载 | Web应用卸载 |

### 8.2.4 四个作用域的测试代码：

- 新建两个jsp页面。分别取名叫：context1.jsp，context2.jsp

  - context1.jsp的页面代码如下：

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"    pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
        <head>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
            <title>Insert title here</title>
        </head>
        <body>
            这是context1页面<br/>	
            <%		
            //设置page域的数据		
            pageContext.setAttribute("key", "pageContext-value");		
            //设置request域的数据		
            request.setAttribute("key", "request-value");		
            //设置session域的数据		
            session.setAttribute("key", "session-value");		
            //设置application域的数据		
            application.setAttribute("key", "application-value");	
            %>	
            <%-- 测试当前页面作用域 --%>
            <%=pageContext.getAttribute("key") %><br/>	
            <%=request.getAttribute("key") %><br/>	
            <%=session.getAttribute("key") %><br/>	
            <%=application.getAttribute("key") %><br/>	
            <%			
            // 测试request作用域
            //request.getRequestDispatcher("/context2.jsp").forward(request, response);	
            %>
        </body>
    </html>
    ```
  
  - context2.jsp的页面代码如下：
  
    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"    pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
        <head>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
            <title>Insert title here</title>
        </head>
        <body>	
            这是context2页面 <br/>	
            <%=pageContext.getAttribute("key") %><br/>	
            <%=request.getAttribute("key") %><br/>	
            <%=session.getAttribute("key") %><br/>	
            <%=application.getAttribute("key") %><br/>
        </body>
    </html>
    ```
  
  - 测试操作：
  
    ```txt
    测试pageContext作用域步骤：直接访问context1.jsp文件
    测试request作用域步骤：
    	1.在context1.jsp文件中添加转发到context2.jsp（有数据）
    	2.直接访问context2.jsp文件 （没有数据）
    测试session作用域步骤：
    	1.访问完context1.jsp文件
    	2.关闭浏览器。但是要保持服务器一直开着
    	3.打开浏览器，直接访问context2.jsp文件
    测试application作用域步骤：
    	1.访问完context1.jsp文件，然后关闭浏览器
    	2.停止服务器。再启动服务器。
    	3.打开浏览器访问context2.jsp文件
    ```
