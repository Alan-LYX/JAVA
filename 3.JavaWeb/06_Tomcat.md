## Tomcat

### 1. 服务器

- 硬件：电脑，提供服务供其它客户电脑访问
- 软件：电脑上安装的服务器软件，安装后能提供服务给网络中的其他计算机，**将本地文件映射成一个虚拟的url地址供网络中的其他人访问。**

### 2. JavaWeb常用服务器

- **Tomcat（Apache）**：当前应用最广的JavaWeb服务器。官网：http://tomcat.apache.org/

- JBoss（Redhat红帽）：支持JavaEE，应用比较广EJB容器 –> SSH轻量级的框架代替
- GlassFish（Orcale）：Oracle开发JavaWeb服务器，应用不是很广
- Resin（Caucho）：支持JavaEE，应用越来越广
- Weblogic（Orcale）：要钱的！支持JavaEE，适合大型项目
- Websphere（IBM）：要钱的！支持JavaEE，适合大型项目

### 3. Tomcat使用步骤

- 使用Tomcat，需要配置JAVA_HOME环境
- 解压apache-tomcat-7.0.79-windows-x64.zip到**非中文无空格**目录中
- 当前环境下只有一个jdk

### 4. Tomcat目录结构

- **bin：**该目录下存放的是二进制可执行文件，如果是安装版，那么这个目录下会有两个exe文件：tomcat6.exe、tomcat6w.exe，前者是在控制台下启动Tomcat，后者是弹出GUI窗口启动Tomcat；如果是解压版，那么会有startup.bat和shutdown.bat文件，startup.bat用来启动Tomcat，但需要先配置JAVA_HOME环境变量才能启动，shutdawn.bat用来停止Tomcat；
- **conf：**这是一个非常非常重要的目录，这个目录下有四个最为重要的文件：
  - **server.xml：**配置整个服务器信息。例如修改端口号。默认HTTP请求的端口号是：8080
  - **tomcat-users.xml：**存储tomcat用户的文件，这里保存的是tomcat的用户名及密码，以及用户的角色信息。可以按着该文件中的注释信息添加tomcat用户，然后就可以在Tomcat主页中进入Tomcat Manager页面了；
  - **web.xml：**部署描述符文件，这个文件中注册了很多MIME类型，即文档类型。这些MIME类型是客户端与服务器之间说明文档类型的，如用户请求一个html网页，那么服务器还会告诉客户端浏览器响应的文档是text/html类型的，这就是一个MIME类型。客户端浏览器通过这个MIME类型就知道如何处理它了。当然是在浏览器中显示这个html文件了。但如果服务器响应的是一个exe文件，那么浏览器就不可能显示它，而是应该弹出下载窗口才对。MIME就是用来说明文档的内容是什么类型的！
  - **context.xml：**对所有应用的统一配置，通常我们不会去配置它。

- **lib：**Tomcat的类库，里面是一大堆jar文件。如果需要添加Tomcat依赖的jar文件，可以把它放到这个目录中，当然也可以把应用依赖的jar文件放到这个目录中，这个目录中的jar所有项目都可以共享之，但这样你的应用放到其他Tomcat下时就不能再共享这个目录下的jar包了，所以建议只把Tomcat需要的jar包放到这个目录下；

- **logs：**这个目录中都是日志文件，记录了Tomcat启动和关闭的信息，如果启动Tomcat时有错误，那么异常也会记录在日志文件中。

- **temp：**存放Tomcat的临时文件，这个目录下的东西可以在停止Tomcat后删除！

- **webapps：存放web项目的目录，其中每个文件夹都是一个项目**；如果这个目录下已经存在了目录，那么都是tomcat自带的项目。其中ROOT是一个特殊的项目，在地址栏中访问：http://127.0.0.1:8080，没有给出项目目录时，对应的就是ROOT项目。<http://localhost:8080/examples，进入示例项目。其中examples>就是项目名，即文件夹的名字。

- **work：**运行时生成的文件，最终运行的文件都在这里。通过webapps中的项目生成的！可以把这个目录下的内容删除，再次运行时会生再次生成work目录。当客户端用户访问一个JSP文件时，Tomcat会通过JSP生成Java文件，然后再编译Java文件生成class文件，生成的java和class文件都会存放到这个目录下。

- LICENSE：许可证。

- NOTICE：说明文件。

### 5. 启动tomcat

1. 单击bin目录下的startup.bat文件，启动服务器

2. 打开浏览器，输入网址：http://localhost:8080或http://127.0.0.1:8080或http://192.168.xx.xx:8080

3. 使用上述网址，访问路径本质是：tomcat下webapps下ROOT下的index.jsp

   - 服务器默认会访问index系列页面，原因服务器web.xml中配置如下：

     ```xml
     <welcome-file-list>
         <welcome-file>index.html</welcome-file>
         <welcome-file>index.htm</welcome-file>
         <welcome-file>index.jsp</welcome-file>
     </welcome-file-list>
     ```

### 6. 修改Tomcat端口号

- 进入tomcat的conf目录
- 打开server.xml
- 找到8080字样，修改端口【建议修改后的端口号在8080附近】

### 7. 将Tomcat整合到IDEA中

### 8. 在IDEA中创建动态工程【JavaEE工程】，部署Tomcat中启动

