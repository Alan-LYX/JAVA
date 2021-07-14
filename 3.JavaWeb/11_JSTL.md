# JSTL (JSP Standard Tag Library)

讲师：尚硅谷-张春胜

email：zhangchunsheng_it@163.com

***

## 第1章 JSTL简介

- JSP虽然为我们提供了EL表达式用来替代JSP表达式，但是由于EL表达式仅仅具有输出功能，用于替代JSP中的表达式脚本，而不能替代页面中的JSP代码脚本。

- 为了解决这个问题，JSP为我们提供了可以自定义标签库(Tag Library)的功能，用来替代代码脚本。这样使得整个jsp页面变得更佳简洁。

- 所谓自定义标签库就是指可以在JSP页面中以类似于HTML标签的形式调用Java中的方法。使用方法和我们JSP动作标签类似。

- 而为了方便开发使用，Sun公司又定义了一套通用的标签库名为JSTL(JSP Standard Tag Library)，里面定义很多我们开发中常用的方法，方便我们使用。

- JSTL的标准由Sun公司定制，Apache的Jakarta小组负责实现，是一个不断完善的开放源代码的JSP标签库。

- JSTL由5个不同功能的标签库组成。如下：

  |       功能范围       | 前缀  |                  URI                   |
  | :------------------: | :---: | :------------------------------------: |
  | **核心标签库--重点** | **c** | **http://java.sun.com/jsp/jstl/core**  |
  |     格式化标签库     |  fmt  |    http://java.sun.com/jsp/jstl/fmt    |
  |      函数标签库      |  fn   | http://java.sun.com/jsp/jstl/functions |
  |    数据库(不使用)    |  sql  |    http://java.sun.com/jsp/jstl/sql    |
  |     XML(不使用)      |   x   |    http://java.sun.com/jsp/jstl/xml    |

  > 说明：
  >
  > 函数标签库，需要结合EL表达式使用，里面定义了一些对字符串的操作（对字符串的截取、替换等）。
  >
  > 格式化标签库，主要用来对日期、时间、数字进行国际化的操作。

- 在jsp标签库中使用taglib指令引入标签库

  ```jsp
  CORE 标签库
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  
  XML 标签库
  <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
  
  FMT 标签库 
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
  
  SQL 标签库
  <%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
  
  FUNCTIONS 标签库
   <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
  
  ```

  

## 第2章 使用JSTL标签库步骤

**第一步：先引入JSTL标签库的jar包类库到WEB-INF/lib目录下**

​	![1558539063529](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/10_JSTL_★/尚硅谷_张春胜_JSTL.assets/1558539063529.png)

这两个jar包，和笔记在同一个目录下。

**第二步：在使用的jsp页面中使用taglib指令导入需要的标签库**

- 以core核心标签库为例，需要在jsp页面中导入标签库的引用。

  ![1558539099390](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/10_JSTL_★/尚硅谷_张春胜_JSTL.assets/1558539099390.png)

  - prefix用来指定前缀名，我们通过该名来使用JSTL
  - uri属性为标签库的唯一uri地址。因为JSTL由多个不同的库组成，使用该属性指定要导入哪个库。http://java.sun.com/jsp/jstl/core 

**第三步：使用JSTL**

然后我们就可以在jsp页面中愉快的使用标签库了。

<c:out value="hello">\</c:out>

这个例子标识，调用前缀为c的标签的out方法，向页面中输出value属性中的字符串。

JSTL的使用非常像html标签。

## 第3章 核心标签

- Core标签库，包括了我们最常用的标签。

- 要使用Core标签库需要在JSP页面中加入CORE 标签库：

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

### 3.1 \<c:out>标签

**作用：**\<c:out>用于计算一个表达式并将结果输出到当前页面。功能类似于JSP表达式<%= %>和EL表达式${}

**属性：**

| 属性    | 作用                        | 参数类型 |
| ------- | --------------------------- | -------- |
| value   | 要输出的值                  | Object   |
| default | 当value为null时显示的默认值 | Object   |
| escaXml | 是否对特殊字符进行转义      | boolean  |

**示例：**

```jsp
<c:out value="${user.name}" default="" escapeXml="true"></c:out>
```



### 3.2 \<c:set>标签

**作用：**\<c:set>标签 用于 添加 或 修改 域中的属性。

**属性：**

| **属性** | **描述**                                                     | **是否必要** | **默认值** |
| :------: | :----------------------------------------------------------- | :----------- | ---------- |
|  value   | 要存储或修改的值                                             | 否           | 主体的内容 |
|  target  | 要修改的域对象的属性名（必须是JavaBean或者Map）              | 否           | 无         |
| property | 指定要修改的对象的属性名                                     | 否           | 无         |
|   var    | 表示域中存放的属性名                                         | 否           | 无         |
|  scope   | var属性的作用域 (page , request , session ,application) ，若不指定则为page | 否           | page       |

如果指定了target属性，那么property属性也需要被指定。

**1）在作用域中添加新属性**

```jsp
<c:set scope="" var="" value=""></c:set>
```

- scope是哪个作用域

- var 是属性名

- value 是属性值

**2）修改已存中的作用域的属性**

```jsp
<c:set target="" property="" value=""></c:set>
```

- target 是要修改的目标对象

- property 是要修改或者添加的属性名

- value 是要修改的属性值

**示例：**

```jsp
<body>
<%
	User user = new User("username姓名",null,null,null);
	session.setAttribute("user", user);
	
	Map<String,String> map = new HashMap<String,String>();
	map.put("abc", "abcValue");
	pageContext.setAttribute("map", map);
%>
	<%-- 在作用域里添加新的属性 --%>
	<c:set scope="page" var="two" value="twovalue"></c:set>
	<%-- 修改在作用域里的对象 --%>
	<c:set target="${ sessionScope.user }" property="username" value="twovalue"></c:set>
	<%-- 给原作用域中的map添加一个key --%>
	<c:set target="${ pageScope.map }" property="bbb" value="twovalue"></c:set>
	${ pageScope.two }<br/> <hr/>
	${ pageScope.map }<br/> <hr/>
	${ sessionScope.user }<br/> <hr/>
</body>

```

**运行结果：**

![1558604309995](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/10_JSTL_★/尚硅谷_张春胜_JSTL.assets/1558604309995.png)

### 3.3 \<c:if>标签

**作用：**\<c:if>标签 判断表达式的值，如果表达式的值为 true 则执行其主体内容。

**属性：**

| **属性** | **描述**               | **是否必要** | **默认值** |
| -------- | ---------------------- | ------------ | ---------- |
| test     | 条件                   | 是           | 无         |
| var      | 用于存储条件结果的变量 | 否           | 无         |
| scope    | var属性的作用域        | 否           | page       |

**示例：**

```jsp
<body>
	<!-- 判断 12 == 12 -->
	<c:if test="${ 12 == 12 }">
		<h1>12 == 12 为真</h1>
	</c:if>
	<!-- 把判断的结果保存到作用域中 -->
	<c:if test="${ 12 == 13 }" scope="page" var="a"></c:if>
	<%-- 输出page作用域中的a对象 --%>
	<c:out value="${ pageScope.a }"></c:out>
</body>

```

**运行结果：**

![1558611135271](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/10_JSTL_★/尚硅谷_张春胜_JSTL.assets/1558611135271.png)

### 3.4 \<c:choose>、\<c:when>、\<c:otherwise>标签(常用)

**作用：**

\<c:choose>标签与Java switch语句的功能一样，用于在众多选项中做出选择。

switch语句中有case，而\<c:choose>标签中对应有\<c:when>，switch语句中有default，而\<c:choose>标签中有\<c:otherwise>。

 

**属性：**

\<c:choose>标签没有属性。

\<c:when>标签只有一个属性，如下：

| **属性** | **描述** | **是否必要** | **默认值** |
| -------- | -------- | ------------ | ---------- |
| test     | 条件     | 是           | 无         |

\<c:otherwise>标签没有属性。

**示例：**

```jsp
<body>
	<%
		// 保存一个分数
		pageContext.setAttribute("score", 90);
	%>
	<%-- 开始判断 --%>
	<c:choose>
		<%-- 如果成绩大于等于 90分 --%>
		<c:when test="${ pageScope.score >= 90 }">
			成绩为A
		</c:when>
		<%-- 如果成绩大于等于 80分 --%>
		<c:when test="${ pageScope.score >= 80 }">
			成绩为B
		</c:when>
		<%-- 如果成绩大于等于 70分 --%>
		<c:when test="${ pageScope.score >= 70 }">
			成绩为C
		</c:when>
		<%-- 如果成绩大于等于 60分 --%>
		<c:when test="${ pageScope.score >= 60 }">
			成绩为D
		</c:when>
		<%-- 其他情况 --%>
		<c:otherwise>
			成绩为E，不及格
		</c:otherwise>
	</c:choose>
</body>

```

**运行结果：**

![1558611318535](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/10_JSTL_★/尚硅谷_张春胜_JSTL.assets/1558611318535.png)

### 3.5 \<c:forEach>标签

**作用：**

\<c:forEach>标签是迭代一个集合中的对象-可以是数组，也可以是list，也可以是map对象。

**属性**：

| **属性**  | **描述**                                   | **是否必要** | **默认值**   |
| --------- | ------------------------------------------ | ------------ | ------------ |
| items     | 要被循环的数据集合-可以使用el表达式        | 否           | 无           |
| begin     | 开始的索引（0=第一个元素，1=第二个元素）   | 否           | 0            |
| end       | 最后一个索引（0=第一个元素，1=第二个元素） | 否           | Last element |
| step      | 每一次迭代的步长                           | 否           | 1            |
| var       | 代表当前条目的变量名称                     | 否           | 无           |
| varStatus | 代表循环状态的变量名称                     | 否           | 无           |

**varStatus状态：**

- 作用：指定保存迭代状态的对象的名字，该变量引用的是一个LoopTagStatus类型的对象

- 通过该对象可以获得一些遍历的状态，如下：
  - begin       获取begin属性里的值
  - end           获取end属性里的值
  - count        获取当前遍历的个数
  - index         获取当前索引值
  - first           获取是否是第一个元素
  - last            获取是否是最后一个元素
  - current     获取当前遍历的元素对象



**需求1：遍历一个List集合，遍历一个Map**

**需求2：使用 map 模拟用户信息。包含编号，用户名，密码，年龄，电话信息。创建5个放到list集合中，再把list集合存放到pageContext域中，然后遍历输出**

表格的格式：

```css
<style type="text/css">	table{		width: 700px;		border: 1px solid red;		border-collapse: collapse;	}	th , td{		border: 1px solid red;	}</style>
```

**示例：**

```jsp
<%@page import="java.util.ArrayList"%><%@page import="java.util.List"%><%@page import="java.util.HashMap"%><%@page import="java.util.Map"%><%@ page language="java" contentType="text/html; charset=UTF-8"    pageEncoding="UTF-8"%><%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>   <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"><html><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><title>Insert title here</title><style type="text/css">	table{		width: 700px;		border: 1px solid red;		border-collapse: collapse;	}	th , td{		border: 1px solid red;	}</style></head><body><%	// 模拟创建5个学生的信息	List<Map<String,Object>> list = new ArrayList<Map<String,Object>>();	for (int i = 0; i < 5; i++) {		Map<String,Object> item = new HashMap<String,Object>();		item.put("id", (i + 1));		item.put("username", "用户名" + i);		item.put("password", "密码" + i);		item.put("age", (12 + i) );		item.put("phone", "18615041888");		list.add(item);	}	// 保存学生的信息到pageContext作用域中	pageContext.setAttribute("stus", list);%><table>	<tr>		<th>编号</th>		<th>姓名</th>		<th>密码</th>		<th>年龄</th>		<th>电话</th>		<th>遍历的个数</th>		<th>当前索引</th>		<th>是否第一个</th>		<th>是否最后一个</th>	</tr>	<%-- 		begin表示从索引0开始，		end表示到索引10结束，		step表示迭代的步长		items是遍历的集合		var是当前遍历到的对象名		varStatus是当前遍历的状态对象，是LoopTagStatus对象实例	 --%>	<c:forEach begin="0" end="10" step="1" items="${ pageScope.stus }" var="item" varStatus="status">		<tr>			<td>${ item.id }</td>			<td>${ item.username }</td>			<td>${ item.password }</td>			<td>${ item.age }</td>			<td>${ item.phone }</td>			<td>${ status.count }</td>			<td>${ status.index }</td>			<td>${ status.first }</td>			<td>${ status.last }</td>		</tr>	</c:forEach></table></body></html>
```

**运行结果：**

![1558611541959](D:/0JAVAstudy/尚硅谷1.07/3.阶段三 Web开发与实战应用/尚硅谷_JavaWeb课件_张春胜/10_JSTL_★/尚硅谷_张春胜_JSTL.assets/1558611541959.png)

### 3.6 \<c:remove>标签

**作用：**用于移除域中的属性



**属性：**

| 属性  | 作用                                                     | 参数类型 |
| ----- | -------------------------------------------------------- | -------- |
| var   | 设置要移除的属性的名字                                   | String   |
| scope | 设置要移除属性所在的域，若不指定则删除所有域中的对应属性 | String   |

**示例：**

```
移除所有域中key属性：<c:remove var="key"/>移除request中的key属性: <c:remove var="key" scope="request"/>
```



### 3.7 \<c:url>标签

**作用：**主要用来重写URL地址

**属性：**

| 属性  | 作用                                   | 参数类型 |
| ----- | -------------------------------------- | -------- |
| value | 设置要处理的URI地址，注意这里要以/开头 | String   |
| var   | 修改后存储到域对象中的uri属性名        | String   |
| scope | l  设置修改后uri存放的域               | String   |

**示例：**

```jsp
使用相对路径：<c:url value="index.jsp" var="uri" scope="request">	<c:param name="name" value="张三"></c:param></c:url>会生成如下地址：index.jsp?name=%E5%BC%A0%E4%B8%89使用绝对路径会自动在路径前加上项目名：<c:url value="/index.jsp" var="uri" scope="request">	<c:param name="name" value="张三"></c:param></c:url>会生成如下地址：/Test_JSTL/index.jsp?name=%E5%BC%A0%E4%B8%89
```



### 3.8 \<c:redirect>标签

**作用：**用于将请求重定向到另一个资源地址

**属性：**

| 属性 | 作用                                                         | 参数类型 |
| ---- | ------------------------------------------------------------ | -------- |
| url  | 指定要重定向到的目标地址，注意这里指定绝对路径会自动加上项目名 | String   |

**示例：**

```jsp
<c:redirect url="/target.jsp"></c:redirect>
```



## 第4章 JSTL函数

- 函数标签库是在JSTL中定义的标准的EL函数集。

- 函数标签库中定义的函数基本上都是对字符串的操作。

- 引入：

  ```
  <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
  ```



### 4.1 fn:contains和fn:containsIgnoreCase

作用：用于判断字符串中是否包含指定字符串，containsIgnoreCase忽略大小写。

语法：

```
fn:contains(string,subString) --> boolean
```

参数和返回值：

| 参数      | 类型    | 作用                                             |
| --------- | ------- | ------------------------------------------------ |
| string    | String  | 源字符串                                         |
| subString | String  | 要查找的字符串                                   |
| 返回值    | boolean | 若String中包含subString则返回true，否则返回false |

例：

```
${fn:contains("hello","HE")}  --> false${fn:containsIgnoreCase("hello","HE")} --> true
```



### 4.2 fn:startsWith和fn:endsWith

作用：判断一个字符串是否以指定字符开头（startsWith）或结尾（endsWith）

语法：

```
fn:startsWith(string , prefix) --> booleanfn:endsWith(string,suffix) --> boolean
```

参数和返回值：

| 参数             | 类型    | 作用                            |
| ---------------- | ------- | ------------------------------- |
| string           | String  | 源字符串                        |
| prefix 或 suffix | String  | 要查找的前缀或后缀字符串        |
| 返回值           | boolean | 符合要求返回true，否则返回false |

例：

```
${fn:startsWith("hello","he") } --> true${fn:endsWith("hello","he") } --> false
```



### 4.3 fn:indexOf

作用：在一个字符串中查找指定字符串，并返回第一个符合的字符串的第一个字符的索引。

语法：

```
fn:indexOf(string,subString) --> int
```

参数和返回值：

| 参数      | 类型   | 作用                                                         |
| --------- | ------ | ------------------------------------------------------------ |
| string    | String | 源字符串                                                     |
| subString | String | 要查找的字符串                                               |
| 返回值    | int    | 若在string中找到subString则返回第一个符合的索引，若没有符合的则返回-1 |

例：

```
${fn:indexOf("hello",'e') } --> 1
```



### 4.4 fn:replace

作用：将一个字符串替换为另外一个字符串，并返回替换结果

语法：

```
fn:replace(str , beforeSubString , afterSubString) --> String
```

参数和返回值：

| 参数            | 类型   | 作用             |
| --------------- | ------ | ---------------- |
| str             | String | 源字符串         |
| beforeSubString | String | 被替换的字符串   |
| afterSubString  | String | 要替换的新字符串 |
| 返回值          | String | 替换后的字符串   |

例：

```
${fn:replace("hello","llo",'e') } --> hee
```

### 4.5 fn:substring

作用：截取字符串

语法：

```
fn: substring (str , beginIndex , endIndex) --> String
```

参数和返回值：

| 参数       | 类型   | 作用                     |
| ---------- | ------ | ------------------------ |
| str        | String | 源字符串                 |
| beginIndex | int    | 开始位置索引(包含该位置) |
| endIndex   | int    | 结束位置索引(不包含自身) |
| 返回值     | String | 返回截取的字符串         |

例：

```
${fn:substring("hello",1,3) } --> el
```



### 4.6 fn:substringBefore和fn:substringAfter

作用：返回一个字符串指定子串之前（substringBefore）之后（substringAfter）的字符串

语法：

```
fn: substringBefore(string,subString) --> Stringfn: substringAfter (string,subString) --> String
```

参数和返回值：

| 参数      | 类型   | 作用                                                  |
| --------- | ------ | ----------------------------------------------------- |
| str       | String | 源字符串                                              |
| subString | int    | 指定str中的一个子串，该串之前或之后的字符串将被返回。 |
| 返回值    | String | 返回截取的字符串                                      |

例：

```
${fn:substringBefore("hello","l") } --> he	${fn:substringAfter("hello","l") } --> lo
```



### 4.7 fn:split

作用：将一个字符串拆分成字符串数组。

语法：

```
fn:split(string,delimiters)--> String
```

参数和返回值：

| 参数       | 类型     | 作用                       |
| ---------- | -------- | -------------------------- |
| str        | String   | 要被拆分的字符串           |
| delimiters | String   | 指定根据什么内容拆分字符串 |
| 返回值     | String[] | 返回拆分后的字符串数组     |

例：

```
${fn:split("a-b-c-d-e-f-g","-")} --> 返回一个数组对象[a,b,c,d,e,f,g]
```



### 4.8 fn:join

作用：将数组中所有元素连接成一个字符串

语法：

```
fn:join(array,sparator) --> String
```

参数和返回值：

| 参数     | 类型     | 作用                         |
| -------- | -------- | ---------------------------- |
| str      | String   | 要被拆分的字符串             |
| sparator | String   | 在结果中每个元素之间的分隔符 |
| 返回值   | String[] | 拼接之后的结果               |

例：

```
<%String[] strs = new String[]{"a","b","c","d","e","f"};pageContext.setAttribute("strs", strs);%>${fn:join(strs,'-') }
```

返回：a-b-c-d-e-f


### 4.9 fn:toLowerCase和fn:toUpperCase

作用：将字符串都转换成大写（toUpperCase）或小写（toLowerCase）字符

语法：

```
fn: toLowerCase (str) --> Stringfn: toUpperCase(str) --> String
```

参数和返回值：

| 参数   | 类型   | 作用                     |
| ------ | ------ | ------------------------ |
| str    | String | 源字符串                 |
| 返回值 | String | 转换为大写或小写的字符串 |

 

例：

```
${fn:toLowerCase("ABCDEFG") } --> abcdefg${fn:toUpperCase("abcdefg") } --> ABCDEFG
```



### 4.10 fn:trim

作用：去掉字符串的前后空格

用法：

```
fn:trim(str) --> String
```

参数和返回值：

| 参数   | 类型   | 作用               |
| ------ | ------ | ------------------ |
| str    | String | 源字符串           |
| 返回值 | String | 去掉前后空格的结果 |

例：

```
${fn:trim("     hello  ") } --> hello
```



### 4.11 fn:length

作用：返回集合或者字符串的长度

用法：

```
fn:trim(input) --> int
```

参数和返回值：

| 参数   | 类型               | 作用               |
| ------ | ------------------ | ------------------ |
| input  | String、集合、数组 | 要计算长度的目标   |
| 返回值 | int                | 集合或字符串的长度 |

例：

```
${fn:length("hello") } --> 5
```

