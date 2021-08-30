# SSM框架整合

## 1 整合注意事项 

1)    查看不同MyBatis版本整合Spring时使用的适配包； 

![img](file:///C:/Users/11373/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)  

2)    下载整合适配包

https://github.com/mybatis/spring/releases

3)    官方整合示例，jpetstore

https://github.com/mybatis/jpetstore-6

 

## 2       整合思路、步骤

1)    搭建环境

创建一个动态的WEB工程

导入SSM需要使用的jar包

导入整合适配包

导入其他技术的一些支持包 连接池 数据库驱动 日志.... 

2)    Spring + Springmvc 

在web.xml中配置:  Springmvc的前端控制器  实例化Spring容器的监听器  字符编码过滤器 REST 过滤器

创建Spring的配置文件: applicationContext.xml:组件扫描、 连接池、 事务.....

创建Springmvc的配置文件: springmvc.xml : 组件扫描、 视图解析器 <mvc:...>

3)    MyBatis

创建MyBatis的全局配置文件

编写实体类 Mapper接口 Mapper映射文件

4)    Spring + MyBatis ：

MyBatis的 SqlSession的创建 .

MyBatis的 Mapper接口的代理实现类

5)    测试: REST CRUD

查询所有的员工信息,列表显示

增删改

## 3       整合的配置

###    3.1 web.xml

```xml
  <?xml version=*"1.0"* encoding=*"UTF-8"*?> 
<web-app xmlns:xsi=*"http://www.w3.org/2001/XMLSchema-instance"* xmlns=*"http://java.sun.com/xml/ns/javaee"*  xsi:schemaLocation=*"http://java.sun.com/xml/ns/javaee  http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"* id=*"WebApp_ID"* version=*"2.5"*>  
    <!-- 字符编码过滤器 -->  
    <filter>     
        <filter-name>CharacterEncodingFilter</filter-name>  
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
        <init-param>         
            <param-name>encoding</param-name>        
            <param-value>UTF-8</param-value>  
        </init-param>  
    </filter>  
    <filter-mapping>   
        <filter-name>CharacterEncodingFilter</filter-name> 
        <url-pattern>/*</url-pattern> 
    </filter-mapping>  
    <!-- REST 过滤器 -->  
    <filter>     
        <filter-name>HiddenHttpMethodFilter</filter-name>   
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class> 
    </filter> 
    <filter-mapping>   
        <filter-name>HiddenHttpMethodFilter</filter-name>    
        <url-pattern>/*</url-pattern> 
    </filter-mapping> 
    <!-- 实例化SpringIOC容器的监听器 -->    
    <context-param>       
        <param-name>contextConfigLocation</param-name>   
        <param-value>classpath:applicationContext.xml</param-value>  
    </context-param>     
    <listener>   
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class> 
    </listener>  
    <!-- Springmvc的前端控制器 -->   
    <servlet>   
        <servlet-name>springDispatcherServlet</servlet-name>    
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>   
        <init-param>         
            <param-name>contextConfigLocation</param-name>        
            <param-value>classpath:springmvc.xml</param-value>     
        </init-param>      
        <load-on-startup>1</load-on-startup>     
    </servlet>     
    <servlet-mapping>     
        <servlet-name>springDispatcherServlet</servlet-name>      
        <url-pattern>/</url-pattern>     
    </servlet-mapping> 
</web-app>  
```



###     3.2 Spring配置 

```xml
     <?xml version=*"1.0"* encoding=*"UTF-8"*?> 
<beans xmlns=*"http://www.springframework.org/schema/beans"*       xmlns:xsi=*"http://www.w3.org/2001/XMLSchema-instance"*       xmlns:context=*"http://www.springframework.org/schema/context"*       xmlns:tx=*"http://www.springframework.org/schema/tx"*       xmlns:mybatis-spring=*"http://mybatis.org/schema/mybatis-spring"*       xsi:schemaLocation=*"http://mybatis.org/schema/mybatis-spring  http://mybatis.org/schema/mybatis-spring-1.2.xsd*            *http://www.springframework.org/schema/beans  http://www.springframework.org/schema/beans/spring-beans.xsd*            *http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context-4.0.xsd*            *http://www.springframework.org/schema/tx  http://www.springframework.org/schema/tx/spring-tx-4.0.xsd"*>     
    <!-- 组件扫描 -->     
    <context:component-scan base-package=*"com.atguigu.ssm"*>       
        <context:exclude-filter type=*"annotation"*
                                expression=*"org.springframework.stereotype.Controller"*/>   
    </context:component-scan>       
    <!-- 连接池 -->      
    <context:property-placeholder location=*"classpath:db.properties"*/>   
    <bean id=*"dataSource"*  class=*"com.mchange.v2.c3p0.ComboPooledDataSource"*>     
        <property name=*"driverClass"*  value=*"${jdbc.driver}"*></property>    
        <property name=*"jdbcUrl"*  value=*"${jdbc.url}"*></property>      
        <property name=*"user"*  value=*"${jdbc.username}"*></property>     
        <property name=*"password"*  value=*"${jdbc.password}"*></property>     
    </bean>       
    <!-- 事务 -->   
    <bean id=*"dataSourceTransactionManager"*     
          class=*"org.springframework.jdbc.datasource.DataSourceTransactionManager"*>    
        <property name=*"dataSource"*  ref=*"dataSource"*></property>  
    </bean>    
    <tx:annotation-driven transaction-manager=*"dataSourceTransactionManager"*/> 
</beans>  
```



###     3.3 SpringMVC配置

```xml
  <?xml version=*"1.0"* encoding=*"UTF-8"*?> 
<beans xmlns=*"http://www.springframework.org/schema/beans"*       xmlns:xsi=*"http://www.w3.org/2001/XMLSchema-instance"*       xmlns:context=*"http://www.springframework.org/schema/context"*       xmlns:mvc=*"http://www.springframework.org/schema/mvc"*       xsi:schemaLocation=*"http://www.springframework.org/schema/mvc  http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd*            *http://www.springframework.org/schema/beans  http://www.springframework.org/schema/beans/spring-beans.xsd*            *http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context-4.0.xsd"*> 
    <!-- 组件扫描 -->  
    <context:component-scan base-package=*"com.atguigu.ssm"* use-default-filters=*"false"*>     
        <context:include-filter type=*"annotation"*   
                                expression=*"org.springframework.stereotype.Controller"*/> 
    </context:component-scan>   
    <!--视图解析器 -->   
    <bean class=*"org.springframework.web.servlet.view.InternalResourceViewResolver"*>  
        <property name=*"prefix"*  value=*"/WEB-INF/views/"*></property>   
        <property name=*"suffix"*  value=*".jsp"*></property>   
    </bean>   
    <mvc:default-servlet-handler/>   
    <mvc:annotation-driven/>
</beans>  

 
```



###     3.4 MyBatis配置

1)    全局文件的配置

```xml
  <?xml version=*"1.0"* encoding=*"UTF-8"*  ?> 
<!DOCTYPE configuration  PUBLIC "-//mybatis.org//DTD  Config 3.0//EN"  "http://mybatis.org/dtd/mybatis-3-config.dtd"> 
<configuration>    
    <!-- Spring 整合 MyBatis 后， MyBatis中配置数据源，事务等一些配置都可以    
   迁移到Spring的整合配置中。MyBatis配置文件中只需要配置与MyBatis相关    
   的即可。        -->     
    <!-- settings: 包含很多重要的设置项    -->      
    <settings>     
        <!-- 映射下划线到驼峰命名 -->       
        <setting name=*"mapUnderscoreToCamelCase"* value=*"true"*/>      
        <!-- 设置Mybatis对null值的默认处理 -->     
        <setting name=*"jdbcTypeForNull"* value=*"NULL"*/>       
        <!-- 开启延迟加载 -->     
        <setting name=*"lazyLoadingEnabled"* value=*"true"*/>     
        <!-- 设置加载的数据是按需还是全部 -->      
        <setting name=*"aggressiveLazyLoading"* value=*"false"*/>      
        <!-- 配置开启二级缓存 -->        
        <setting name=*"cacheEnabled"* value=*"true"*/>   
    </settings> 
</configuration>  
```

 

2)    SQL映射文件配置

```xml
  <?xml version=*"1.0"* encoding=*"UTF-8"*  ?> 
<!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD  Mapper 3.0//EN"  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace=*"com.atguigu.ssm.mapper.EmployeeMapper"*>   
    <!-- public List<Employee> getAllEmps();  -->    
    <select id=*"getAllEmps"*  resultMap=*"myEmpsAndDept"* >     
        select e.id eid,  e.last_name,e.email,e.gender, d.id did, d.dept_name   
        from tbl_employee e  ,tbl_dept d      
        where e.d_id = d.id   
    </select>   
    <resultMap type=*"com.atguigu.ssm.beans.Employee"*  id=*"myEmpsAndDept"*>  
        <id column=*"eid"*  property=*"id"*/>      
        <result column=*"last_name"*  property=*"lastName"*/>     
        <result column=*"email"*  property=*"email"*/>     
        <result column=*"gender"*  property=*"gender"*/>       
        <association property=*"dept"*  javaType=*"com.atguigu.ssm.beans.Department"*>    
            <id column=*"did"* property=*"id"*/>    
            <result column=*"dept_name"* property=*"departmentName"*/>     
        </association>    
    </resultMap> 
</mapper>  
```



###     3.5 Spring 整合MyBatis 配置

```xml
  <!--  Spring 整合 Mybatis -->    
<!--1. SqlSession   -->    
<bean class=*"org.mybatis.spring.SqlSessionFactoryBean"*>     
    <!-- 指定数据源 -->       
    <property name=*"dataSource"*  ref=*"dataSource"*></property>    
    <!-- MyBatis的配置文件 -->       
    <property name=*"configLocation"*       
              value=*"classpath:mybatis-config.xml"*></property>    
    <!-- MyBatis的SQL映射文件 -->      
    <property name=*"mapperLocations"*       
              value=*"classpath:mybatis/mapper/\*.xml"*></property>      
    <property name=*"typeAliasesPackage"*     
              value=*"com.atguigu.ssm.beans"*></property>   
</bean>    
<!-- Mapper接口    
        MapperScannerConfigurer 为指定包下的Mapper接口批量生成代理实现类.bean的默认id是接口名首字母小写.     
    -->   
<bean class=*"org.mybatis.spring.mapper.MapperScannerConfigurer"*>       
    <property name=*"basePackage"*  value=*"com.atguigu.ssm.mapper"*></property>   
</bean>     
<!-- <mybatis-spring:scan  base-package="com.atguigu.ssm.mapper"/> -->     
```

 

## 4 整合测试 

1)    编写页面，发送请求：http://localhost:8888/ssm/employees

2)    编写Handler,处理请求，完成响应

3)    在页面中获取数据，显示数据