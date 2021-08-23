# 第1章 MyBatis简介

## 1.1 MyBatis简介

1） MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架

2） MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集

3） MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录

4） Mybatis 是一个 半自动的ORM（Object  Relation Mapping）框架

## 1.2 如何下载MyBatis

1） 下载网址

​     https://github.com/mybatis/mybatis-3/

2） 官方文档

 	https://mybatis.org/mybatis-3/zh/index.html

## 1.3 MyBatis与现有技术对比

1） JDBC

①  SQL夹在Java代码块里，耦合度高导致硬编码内伤

②  维护不易且实际开发需求中sql有变化，频繁修改的情况多见

2） Hibernate和JPA

①  长难复杂SQL，对于Hibernate而言处理也不容易

②  内部自动生产的SQL，不容易做特殊优化

③  基于全映射的全自动框架，大量字段的POJO进行部分映射时比较困难。导致数据库性能下降

3） MyBatis

①  对开发人员而言，核心sql还是需要自己优化

②  sql和java编码分开，功能边界清晰，一个专注业务、一个专注数据

 

# 第2章 MyBatis HelloWorld

## 2.1 开发环境的准备

1)    导入MyBatis框架的jar包、Mysql驱动包、log4j的jar包

- myBatis-3.5.1.jar  

- mysql-connector-java-5.1.37-bin.jar  
- log4j.jar  

2)    导入log4j 的配置文件

```xml
<?xml  version="1.0" encoding="UTF-8" ?>
<!DOCTYPE  log4j:configuration SYSTEM "log4j.dtd">     
<log4j:configuration  xmlns:log4j="http://jakarta.apache.org/log4j/">      
    <appender name="STDOUT"  class="org.apache.log4j.ConsoleAppender">    
        <param name="Encoding"  value="UTF-8" />    
        <layout  class="org.apache.log4j.PatternLayout">    
            <param  name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}  %m (%F:%L) \n" />
        </layout>   
    </appender>   
    <logger name="java.sql">  
        <level value="debug" /> 
    </logger> 
    <logger  name="org.apache.ibatis">  
        <level value="info" />  
    </logger>  
    <root>    
        <level value="debug" /> 
        <appender-ref ref="STDOUT"  />  
    </root>
</log4j:configuration>  
```

## 2.2 创建测试表

```sql
  -- 创建库  
  CREATE DATABASE test_mybatis; 
  -- 使用库 
  USE test_mybatis; 
  -- 创建表 
  CREATE TABLE tbl_employee(   
      id INT(11) PRIMARY KEY AUTO_INCREMENT,  
      last_name VARCHAR(50),  
      email VARCHAR(50),  
      gender CHAR(1) 
  );  
```

## 2.3 创建javaBean

```java
public class Employee {

	private Integer id ; 
	private String lastName; 
	private String email ;
	private String gender ;
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	@Override
	public String toString() {
		return "Employee [id=" + id + ", lastName=" + lastName + ", email=" + email + ", gender=" + gender + "]";
	}
```

## 2.3 创建MyBatis的全局配置文件

1)    参考MyBatis的官网手册

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 数据库连接环境的配置 -->
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url" value="jdbc:mysql://localhost:3306/mybatis_1129" />
				<property name="username" value="root" />
				<property name="password" value="1234" />
			</dataSource>
		</environment>
	</environments>
	<!-- 引入SQL映射文件,Mapper映射文件 	-->
	<mappers>
		<mapper resource="EmployeeMapper.xml" />
	</mappers>
</configuration>
```

## 2.4 创建Mybatis的sql映射文件 

1)    参考MyBatis的官方手册

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="suibian">
	<select id="selectEmployee" resultType="com.atguigu.myabtis.helloWorld.Employee">
		select id ,last_name lastName ,email ,gender from tbl_employee where id = #{id}
		<!-- select * from tbl_employee  where id = #{id} -->
	</select>
</mapper>
```

## 2.5 测试

1)    参考MyBatis的官方手册

```java
@Test
	public void test() throws Exception {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactory sqlSessionFactory = 
					new SqlSessionFactoryBuilder().build(inputStream);
		System.out.println(sqlSessionFactory);
		
		SqlSession session  = sqlSessionFactory.openSession();
		try {
			Employee employee = 
					session.selectOne("suibian.selectEmployee", 1001);
			System.out.println(employee);
		} finally {
			session.close();
		}
	}
```

 

## 2.6 Mapper接口开发MyBatis HelloWorld

1)    编写Mapper接口

```java
public interface EmployeeMapper {
	
	public Employee getEmployeeById(Integer id );	
	
}
```

2)    完成两个绑定

-  Mapper接口与Mapper映射文件的绑定

​		在Mppper映射文件中的<mapper>标签中的namespace中必须指定Mapper接口的全类名

- Mapper映射文件中的增删改查标签的id必须指定成Mapper接口中的方法名. 


3)    获取Mapper接口的代理实现类对象

```java
	@Test
	public void test()  throws Exception{
		String resource = "mybatis-config.xml";
		InputStream inputStream =
                 Resources.getResourceAsStream(resource);
		SqlSessionFactory sqlSessionFactory = 
				new SqlSessionFactoryBuilder()
              .build(inputStream);		
		SqlSession session = 
                         sqlSessionFactory.openSession();
		
		try {
			//Mapper接口:获取Mapper接口的 代理实现类对象
			EmployeeMapper mapper =
                 session.getMapper(EmployeeMapper.class);		
			Employee employee = 
                  mapper.getEmployeeById(1006);
			System.out.println(employee);
		} finally {
			session.close();
		}
	}
```

 

# 第3章 MyBatis全局配置文件

配置文件详细信息：https://mybatis.org/mybatis-3/zh/configuration.html

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

**configuration（配置）：**

- properties（属性）
- settings（设置）
- typeAliases（类型别名）
- typeHandlers（类型处理器）
- objectFactory（对象工厂）
- plugins（插件）
- environments（环境配置）
  - environment（环境变量）
    - transactionManager（事务管理器）
    - dataSource（数据源）
- databaseIdProvider（数据库厂商标识）
- mappers（映射器）

 **示例：**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 数据库连接环境的配置 -->
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url" value="jdbc:mysql://localhost:3306/mybatis_1129" />
				<property name="username" value="root" />
				<property name="password" value="1234" />
			</dataSource>
		</environment>
	</environments>
	<!-- 引入SQL映射文件,Mapper映射文件 	-->
	<mappers>
		<mapper resource="EmployeeMapper.xml" />
	</mappers>
</configuration>
```

作用：MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。

标签详解：

- 根标签：configuration，所有子标签均书写在根标签内部。

- properties：将数据源中属性提取到外部，使用该标签进行引入外部属性

  - resource：按照类路径检索属性文件
  - url：：按照指定路径【真实路径】检索属性文件

- settings：这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。 

  - mapUnderscoreToCamelCase：是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。
    - 注意：只能将类似last_name与lastName进行映射，不能将其他情况自动映射，如：last_name与lName。

- typeAliases：为POJO设置别名【缩写名】

  ```xml
  <typeAliases>
      <!--局部设置-->
  <!--<typeAlias type="com.atguigu.pojo.Employee" alias="employee"></typeAlias>-->
      <!--全局设置，为当前包下所有pojo,设置别名。别名为类名首字母小写【使用别名时，不区分大小写】--> 
      <package name="com.atguigu.pojo"/>
  </typeAliases>
  ```

  - mybatis内置一些别名

    | int     | Integer |
    | ------- | ------- |
    | list    | List    |
    | map     | Map     |
    | integer | Integer |
    | double  | Double  |
    | string  | String  |

- environments：设置数据库连接环境

  ```xml
  <!-- 设置数据库连接环境-->
  <environments default="development">
      <environment id="development">
          <!-- 设置事务管理器
              this.typeAliasRegistry.registerAlias("JDBC", JdbcTransactionFactory.class);
          -->
          <transactionManager type="JDBC"/>
          <!--设置数据源
              this.typeAliasRegistry.registerAlias("POOLED", PooledDataSourceFactory.class);
          -->
          <dataSource type="POOLED">
              <property name="driver" value="${db.driver}"/>
              <property name="url" value="${db.url}"/>
              <property name="username" value="${db.username}"/>
              <property name="password" value="${db.password}"/>
          </dataSource>
      </environment>
  </environments>
  ```

- mappers：加载映射文件

  ```xml
  <mappers>
      <!-- 设置映射文件路径-->
      <!-- <mapper resource="EmployeeMapper.xml" />-->
      <!-- 设置映射文件包名，将该报名下所有映射文件统一加载
              要求：映射文件包名，与接口包名一致
      -->
      <package name="com.atguigu.mapper"/>
  </mappers>
  ```

总结：Mybatis核心配置文件中，根元素下的子元素，必须按照指定顺序书写，否则报错。

# 第4章 MyBatis 映射文件

## 4.1 Mybatis映射文件简介

1)    MyBatis 的真正强大在于它的映射语句，也是它的魔力所在。由于它的异常强大，映射器的 XML 文件就显得相对简单。如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。MyBatis 就是针对 SQL 构建的，并且比普通的方法做的更好。

2)    SQL 映射文件有很少的几个顶级元素（按照它们应该被定义的顺序）：

- cache – 给定命名空间的缓存配置。

- cache-ref – 其他命名空间缓存配置的引用。

- resultMap – 是最复杂也是最强大的元素，用来描述如何从数据库结果集中来加载对象。

- parameterMap – **已废弃！**老式风格的参数映射。内联参数是首选,这个元素可能在将来被移除，这里不会记录。

- sql – 可被其他语句引用的可重用语句块。

- insert – 映射插入语句

- update – 映射更新语句

- delete – 映射删除语句

- select – 映射查询语


## 4.2 Mybatis完成CRUD

###     4.2.1   select

1)    Mapper接口方法

```java
	public  Employee getEmployeeById(Integer id );
```

2)    Mapper映射文件

```xml
<select id="getEmployeeById" 
          resultType="com.atguigu.mybatis.beans.Employee" 
          databaseId="mysql">
		 select * from tbl_employee where id = ${_parameter}
</select>
```

### 4.2.2	 insert

1)	Mapper接口方法

```java
public Integer  insertEmployee(Employee employee);
```

2)	Mapper映射文件

```xml
<insert id="insertEmployee" 
		parameterType="com.atguigu.mybatis.beans.Employee"  
			databaseId="mysql">
		insert into tbl_employee(last_name,email,gender) values(#{lastName},#{email},#{gender})
</insert>
```

### 4.2.3  update

1)	Mapper接口方法

```java
public Boolean  updateEmployee(Employee employee);
```

2)	Mapper映射文件

```xml
<update id="updateEmployee" >
		update tbl_employee set last_name = #{lastName},
							    email = #{email},
							    gender = #{gender}
							    where id = #{id}
</update>
```

### 4.2.4  delete

1)	Mapper接口方法

```java
public void  deleteEmployeeById(Integer id );
```

2)	Mapper映射文件

```xml
<delete id="deleteEmployeeById" >
		delete from tbl_employee where id = #{id}
</delete>
```

## 4.3     主键生成方式、获取主键值

###     4.3.1  主键生成方式

1)    支持主键自增，例如MySQL数据库

2)    不支持主键自增，例如Oracle数据库

 需求: 插入一条新数据，立马查询这条数据. 

###     4.3.2 获取主键值

1)	若数据库支持自动生成主键的字段（比如 MySQL 和 SQL Server），则可以设置 useGeneratedKeys=”true”，然后再把 keyProperty 设置到目标属性上。

```xml
<insert id="insertEmployee" 	parameterType="com.atguigu.mybatis.beans.Employee"  
			databaseId="mysql"
			useGeneratedKeys="true"
			keyProperty="id">
		insert into tbl_employee(last_name,email,gender) values(#{lastName},#{email},#{gender})
</insert>
```

2)	而对于不支持自增型主键的数据库（例如 Oracle），则可以使用 selectKey 子元素：selectKey  元素将会首先运行，id  会被设置，然后插入语句会被调用:

```xml
<insert id="insertEmployee" 
		parameterType="com.atguigu.mybatis.beans.Employee"  
			databaseId="oracle">
		<selectKey order="BEFORE" keyProperty="id" 
                                       resultType="integer">
			select employee_seq.nextval from dual 
		</selectKey>	
		insert into orcl_employee(id,last_name,email,gender) values(#{id},#{lastName},#{email},#{gender})
</insert>
```

或者是:

```xml
<insert id="insertEmployee" 
		parameterType="com.atguigu.mybatis.beans.Employee"  
			databaseId="oracle">
		<selectKey order="AFTER" keyProperty="id" 
                                         resultType="integer">
			select employee_seq.currval from dual 
		</selectKey>	
	insert into orcl_employee(id,last_name,email,gender) values(employee_seq.nextval,#{lastName},#{email},#{gender})
</insert>
```

或者是 :

```xml
<insert id="insertEmployee" 
		parameterType="com.atguigu.mybatis.beans.Employee"  
			databaseId="oracle">
		<selectKey order="AFTER" keyProperty="id" 
                                         resultType="integer">
			select last_insert_id()
		</selectKey>	
	insert into orcl_employee(id,last_name,email,gender) values(employee_seq.nextval,#{lastName},#{email},#{gender})
</insert>
```



## 4.4 参数传递

###     4.4.1 参数传递的方式

1)    单个普通类型参数

​     	可以接受基本类型，包装类型，字符串类型等。这种情况MyBatis可直接使用这个参数，不需要经过任何处理。

2)    多个参数

​		任意多个参数，都会被MyBatis重新包装成一个Map传入。

​		Mybatis3.4及以前版本。Map的key是param1，param2...，或者0，1…，值就是参数的值。

​		Mybatis3.5及以后版本。Map的key是param1，param2...，或者arg0，arg1…，值就是参数的值。

3)    命名参数

​		为参数使用@Param起一个名字，MyBatis就会将这些参数封装进map中，key就是我们自己指定的名字。

```java
public Employee selectEmpByNamed(@Param("id") int id, @Param("lastName")String lName);
```

4)    POJO

​		当这些参数属于我们业务POJO时，我们直接传递POJO，此时sql参数为属性名。

5)    Map

​		我们也可以封装多个参数为map，直接传递。sql的参数名为map的key值。

6)    Collection/Array

​		会被MyBatis封装成一个map传入, Collection对应的key是collection,Array对应的key是array. 如果确定是List集合，key还可以是list.

###     4.4.2 参数处理

1)    参数位置支持的属性:

​		javaType、jdbcType、mode、numericScale、resultMap、typeHandler、jdbcTypeName、expression

2)    实际上通常被设置的是：可能为空的列名指定 jdbcType ,例如:

```sql
insert into orcl_employee(id,last_name,email,gender)
values(employee_seq.nextval,#{lastName,jdbcType=NULL },#{email},#{gender})  
```

###     4.4.3 #{key}与${key}

1)    #{key}：获取参数的值，预编译到SQL中。安全。

2)    ${key}：获取参数的值，拼接到SQL中。有SQL注入问题。

​		a)    使用此种方式获取参数时，不能任意书写参数，只能通过_parameter进行获取参数，eg:${_parameter}

​		b)    一般#{key}解决不了的问题，使用${key}获取参数

​		c)    底层原理使用Statement对象，不是占位符方式。

​		d)   占位符?的位置可以使用#{}，除此之外的其他位置，如需传递参数时，使用${key}，比如**列名**。



## 4.5 select查询的几种情况

1)    查询单行数据返回单个对象

```java
  public  Employee getEmployeeById(Integer id );  
```

 SQL：resultType=pojo

2)    查询多行数据返回对象的集合

```java
  public  List<Employee> getAllEmps();  
```

 SQL：resultType=pojo

3)    查询单行数据返回Map集合

```java
  public  Map<String,Object> getEmployeeByIdReturnMap(Integer id );  
```

 SQL：resultType=“map或java.util.Map”

4)    查询多行数据返回Map集合

```java
@MapKey("id")	// 指定使用对象的哪个属性来充当map的key 
public Map<Integer,Employee> selectEmpReturnPojoMap();
```

```xml
<select id="selectEmpReturnPojoMap" resultType="employee">
    SELECT
        id,last_name,email,gender
    FROM
        tbl_employee
</select>
```



## 4.6 resultType自动映射

- 概述：自动将表中的**字段**与类中的**属性**关联映射
- 前提条件
  - 需要表中的字段与类中的属性一致
  - 如表中的字段与类中的属性不一致，在Mybatis的核心文件中，设置settings的mapUnderscoreToCamelCase=true。

- 如自动映射不能满足时，使用自定义映射，如以下情况
  1. 表中的字段与类中的属性不一致，且设置mapUnderscoreToCamelCase=true无效。
  2. 多表联查，结果集中需要多张表中的数据。

1)    autoMappingBehavior默认是PARTIAL，开启自动映射的功能。唯一的要求是结果集列名和javaBean属性名一致

2)    如果autoMappingBehavior设置为NONE则会取消自动映射

## 4.7 resultMap自定义映射

1)    自定义resultMap，实现高级结果集映射

2)    id ：用于完成主键值的映射

3)    result ：用于完成普通列的映射

4)    association ：一个复杂的类型关联;许多结果将包装成这种类型

5)    collection ： 复杂类型的集

###     4.7.1 级联映射 

```java
/**
 * 通过员工id获取员工信息及员工所属部门信息
 */
public Employee selectEmployeeAndDeptByEmpId(int id);
```

```xml
<!--    定义resultMap-->
<resultMap id="deptAndEmpRm" type="employee">
    <id column="eid" property="id"></id>
    <result column="last_name" property="lastName"></result>
    <result column="email" property="email"></result>
    <result column="gender" property="gender"></result>
    <!-- 映射员工中部门关联关系-->
    <result column="did" property="dept.id"></result>
    <result column="dept_name" property="dept.deptName"></result>
</resultMap>
<select id="selectEmployeeAndDeptByEmpId" resultMap="deptAndEmpRm">
     SELECT
        e.id eid,e.`last_name`,e.`email`,e.`gender`,
        d.`id` did,d.`dept_name`
    FROM
        tbl_employee e,tbl_dept d
    WHERE
        e.dept_id = d.id
    AND
        e.id = #{id}
</select>
```

###     4.7.2 association（多对一）

```java
/**
 * 【association】通过员工id获取员工信息及员工所属部门信息（多对一的情况）
 */
public Employee selectEmployeeAndDeptByEmpIdAssociation(int id);
```

```xml
<!--    定义resultMap【association】-->
<resultMap id="deptAndEmpAssociation" type="employee">
    <id column="eid" property="id"></id>
    <result column="last_name" property="lastName"></result>
    <result column="email" property="email"></result>
    <result column="gender" property="gender"></result>
    <!-- association映射员工中部门关联关系
        property:设置员工中关联部门属性
        javaType:设置属性类名【全类名】
    -->
    <association property="dept" javaType="com.atguigu.pojo.Dept">
        <id column="did" property="id"></id>
        <result column="dept_name" property="deptName"></result>
    </association>
</resultMap>
<!--    【association】通过员工id获取员工信息及员工所属部门信息-->
    <select id="selectEmployeeAndDeptByEmpIdAssociation" resultMap="deptAndEmpAssociation">
        SELECT
            e.id eid,e.`last_name`,e.`email`,e.`gender`,
            d.`id` did,d.`dept_name`
        FROM
            tbl_employee e,tbl_dept d
        WHERE
            e.dept_id = d.id
        AND
            e.id = #{id}
    </select>
```

###     4.7.3 association 分步查询

```java
/**
 * 【association分步查询：降低服务器压力】通过员工id获取员工信息及员工所属部门信息
 *      1. 通过员工id员工信息
 *      2. 通过部门id获取部门信息
 */
public Employee selectEmployeeAndDeptByEmpIdAssociationByStep(int id);
```

```xml
<!--    定义resultMap[分布查询]-->
<resultMap id="deptAndEmpAssociationByStep" type="employee">
    <id column="eid" property="id"></id>
    <result column="last_name" property="lastName"></result>
    <result column="email" property="email"></result>
    <result column="gender" property="gender"></result>
    <!-- association映射员工中部门关联关系
        property:设置员工中关联部门属性
        javaType:设置属性类名【全类名】
    -->
    <association property="dept"
                 select="com.atguigu.mapper.DeptMapper.selectDeptById"
                 column="did">
    </association>
</resultMap>
<select id="selectEmployeeAndDeptByEmpIdAssociationByStep" resultMap="deptAndEmpAssociationByStep">
        SELECT
            e.id eid,e.`last_name`,e.`email`,e.`gender`,
            d.`id` did,d.`dept_name`
        FROM
            tbl_employee e,tbl_dept d
        WHERE
            e.dept_id = d.id
        AND
            e.id = #{id}
    </select>
```

###     4.7.4 延迟加载（懒加载）

- 概述：需要数据时，查询【加载】数据，暂时不需要数据，不查询【加载】数据。

- 代码：在核心配置文件的settings中进行如下配置

  ```xml
  <!-- 开启全局延迟加载 
  		lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态。
  -->
  <setting name="lazyLoadingEnabled" value="true"/>
  <!-- 设置加载的数据是按需还是全部
  	true:全部加载
   	false:按需加载【默认值Mybatis3.5.1及以后】
  -->
  <setting name="aggressiveLazyLoading" value="false"/> 
  ```


###     4.7.5 collection（一对多）

```java
/**
 * 通过部门id获取部门信息
 */
public Dept selectDeptById(int id);
```

```xml
<!--    通过部门id获取部门信息-->
    <select id="selectDeptById" resultType="dept">
        select
            id,dept_name
        from
            tbl_dept
        where
            id=#{id}
    </select>
```

collection【外连接】：

```java
/**
 * 通过部门id获取部门信息，及部门中所有员工信息
 * 【如部门中有员工信息，则查询员信息】
 * 【如部门中暂无员工信息，则员工信息用null不全】
 */
public List<Dept> selectDeptAndDeptEmpByDeptIdCollectionLeftJoinOn();
```

```xml
<!--    左外连接练习-->
    <select id="selectDeptAndDeptEmpByDeptIdCollectionLeftJoinOn" resultMap="deptAndEmpRs">
        SELECT
            d.`id` did,d.`dept_name`,
            e.`id` eid,e.`last_name`,e.`email`,e.`gender`,e.`dept_id`
        FROM tbl_dept d LEFT JOIN tbl_employee e
        ON e.`dept_id`=d.`id`
    </select>
```

**总结：**

- 设置延迟加载【懒加载】方式

  - 全局设置

    ```xml
    <!-- 开启延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 设置加载的数据是按需还是全部 -->
    <setting name="aggressiveLazyLoading" value="false"/>
    ```

  - 局部设置，在association或collection中添加fetchType

    - lazy：开启懒加载（默认）

    - eager：关闭懒加载

    - fetchType可以灵活的设置查询是否需要使用延迟加载，而不需要因为某个查询不想使用延迟加载将全局的延迟加载设置关闭。

      ```xml
       <collection property="emps"
           select="com.atguigu.mapper.EmployeeMapper.selectEmpByDeptId"
           column="{dId=did}"
           fetchType="eager"
       >
       </collection>
      ```

###     4.7.6 分步查询多列值的传递

将多个参数，封装Map结构：{key=value,key2=value}

- 具体代码，如下

  ```xml
  <!-- DeptMapper.xml -->
  <resultMap id="deptAndEmpCollectionByStep" type="dept">
       <id column="did" property="id"></id>
       <result column="dept_name" property="deptName"></result>
       <!--
  property: 关联的属性名
  
  -->
       <collection property="emps"
           select="com.atguigu.mapper.EmployeeMapper.selectEmpByDeptId"
           column="{dId=did}"
           fetchType="eager"
       >
       </collection>
   </resultMap>
  
  <!-- EmployeeMapper.xml-->
  <select id="selectEmpByDeptId" resultType="employee">
           SELECT
              e.`id`,e.`last_name`,e.`gender`,e.`email`
           FROM
              tbl_employee e
           WHERE
              e.`dept_id`=#{dId}
      </select>
  ```

# 第5章：MyBatis 动态SQL

## 5.1 MyBatis动态SQL简介

​	1)    动态 SQL是MyBatis强大特性之一。极大的简化我们拼装SQL的操作

​	2)    动态 SQL 元素和使用 JSTL 或其他类似基于 XML 的文本处理器相似

​	3)    MyBatis 采用功能强大的基于 OGNL 的表达式来简化操作

- **常用标签：**

  - if：if判断

  - where标签用于解决SQL语句中where关键字以及条件中第一个and或者or的问题 

  - trim 可以在条件判断完的SQL语句前后 添加或者去掉指定的字符

  - prefix: 添加前缀

  - prefixOverrides: 去掉前缀

  - suffix: 添加后缀

  - suffixOverrides:去掉后缀

  - set 主要是用于解决修改操作中SQL语句中**set**关键字，和可能多出**一个逗号**的问题

  - choose 主要是用于分支判断，类似于java中的switch case,只会满足所有分支中的一个【与jstl中choose用法一致】

  - sql: sql 标签是用于抽取可重用的sql片段，将相同的，使用频繁的SQL片段抽取出来，单独定义，方便多次引用.

  - foreach主要用于循环迭代

    - collection: 要迭代的集合

      item: 当前从集合中迭代出的元素

      open: 开始字符

      close:结束字符

      separator: 元素与元素之间的分隔符

​	4)    OGNL（ Object Graph Navigation Language ）对象图导航语言，这是一种强大的

​			表达式语言，通过它可以非常方便的来操作对象属性。 类似于我们的EL，SpEL等

​			访问对象属性：       person.name

​			调用方法：         person.getName()

​			调用静态属性/方法： @java.lang.Math@PI   

​          									   @java.util.UUID@randomUUID()

​			调用构造方法：       new com.atguigu.bean.Person(‘admin’).name

​			运算符：             +,-*,/,%

​			逻辑运算符：           in,not in,>,>=,<,<=,==,!=

​			**注意：**xml中特殊符号如”,>,<等这些都需要使用转义字符

## 5.2 if where

1)	If用于完成简单的判断.
2)	Where用于解决SQL语句中where关键字以及条件中第一个and或者or的问题 

```xml
	<select id="getEmpsByConditionIf" resultType="com.atguigu.mybatis.beans.Employee">
		select id , last_name ,email  , gender  
		from tbl_employee 
		<where>
			<if test="id!=null">
				and id = #{id}
			</if>
			<if test="lastName!=null &amp;&amp; lastName!=&quot;&quot;">
				and last_name = #{lastName}
			</if>
			<if test="email!=null and email.trim()!=''">
				and email = #{email}
			</if>
			<if test="&quot;m&quot;.equals(gender) or &quot;f&quot;.equals(gender)">
				and gender = #{gender} 
			</if>
		</where>
	</select>
```



## 5.3 trim 

1)	Trim 可以在条件判断完的SQL语句前后 添加或者去掉指定的字符
		prefix: 添加前缀
		prefixOverrides: 去掉前缀
		suffix: 添加后缀
		suffixOverrides: 去掉后缀

```xml
<select id="getEmpsByConditionTrim" resultType="com.atguigu.mybatis.beans.Employee">
		select id , last_name ,email  , gender  
		from tbl_employee 
		<trim prefix="where"  suffixOverrides="and">
			<if test="id!=null">
				 id = #{id} and
			</if>
			<if test="lastName!=null &amp;&amp; lastName!=&quot;&quot;">
				 last_name = #{lastName} and
			</if>
			<if test="email!=null and email.trim()!=''">
				 email = #{email} and
			</if>
			<if test="&quot;m&quot;.equals(gender) or &quot;f&quot;.equals(gender)">
				gender = #{gender}
			</if>
		</trim>
</select>
```



## 5.4 set 

1)    set 主要是用于解决修改操作中SQL语句中可能多出逗号的问题

```xml
<update id="updateEmpByConditionSet">
		update  tbl_employee  
		<set>
			<if test="lastName!=null &amp;&amp; lastName!=&quot;&quot;">
				 last_name = #{lastName},
			</if>
			<if test="email!=null and email.trim()!=''">
				 email = #{email} ,
			</if>
			<if test="&quot;m&quot;.equals(gender) or &quot;f&quot;.equals(gender)">
				gender = #{gender} 
			</if>
		</set>
		 where id =#{id}
</update>

```



## 5.5 choose(when、otherwise) 

1)    choose 主要是用于分支判断，类似于java中的switch case,只会满足所有分支中的一个

```xml
<select id="getEmpsByConditionChoose" resultType="com.atguigu.mybatis.beans.Employee">
		select id ,last_name, email,gender from tbl_employee
		<where>
			<choose>
				<when test="id!=null">
					id = #{id}
				</when>
				<when test="lastName!=null">
					last_name = #{lastName}
				</when>
				<when test="email!=null">
					email = #{email}
				</when>
				<otherwise>
					 gender = 'm'
				</otherwise>
			</choose>
		</where>
</select>

```

 

## 5.6 foreach 

1)    foreach 主要用于循环迭代

collection: 要迭代的集合

item: 当前从集合中迭代出的元素

open: 开始字符

close:结束字符

separator: 元素与元素之间的分隔符

index:

​     迭代的是List集合: index表示的当前元素的下标

​     迭代的Map集合: index表示的当前元素的key

**设置数据库url属性,支持多条Sql执行:allowMultiQueries=true**，否则不支持一次执行多条语句。

**场景一：**循环迭代数据

```java
/**
 * 查询类似id在1-5之间员工信息
 */
public List<Employee> selectEmpByIds(@Param("ids")List<Integer> ids);
```

```xml
<select id="selectEmpByIds" resultType="employee">
    SELECT
        <include refid="emp_col"></include>
    FROM
        tbl_employee
    WHERE
        id IN
            <foreach open="(" close=")" collection="ids" item="id" separator=",">
                #{id}
            </foreach>
</select>
```

**场景二：**

- "批处理"新增数据1

```java
    /**
     * 使用foreach新增员工信息
     */
    public void insertEmpByForEach1(@Param("emps")List<Employee> emps);
```

```xml
    <!--    使用foreach新增员工信息-->
        <insert id="insertEmpByForEach1">
            <foreach collection="emps" item="emp">
                INSERT INTO
                    tbl_employee(last_name,email,gender)
                VALUES(#{emp.lastName},#{emp.email},#{emp.gender});
            </foreach>
        </insert>
```

- 批处理新增数据2

```java
    /**
     * 使用foreach新增员工信息2
     */
    public void insertEmpByForEach2(@Param("emps")List<Employee> emps);
```

```xml
    <insert id="insertEmpByForEach2">
        INSERT INTO
                tbl_employee(last_name,email,gender)
        VALUES
            <foreach collection="emps" item="emp" separator=",">
                (#{emp.lastName},#{emp.email},#{emp.gender})
            </foreach>
    </insert>
```

## 5.7 sql 

1)    sql 标签是用于抽取可重用的sql片段，将相同的，使用频繁的SQL片段抽取出来，单独定义，方便多次引用.

2)    抽取SQL:

```xml
  <sql id="selectSQL">       
      select id , last_name,  email ,gender from tbl_employee 
  </sql>  
```

3)    引用SQL:

```xml
  <include refid="selectSQL"></include>  
```

 

# 第6章：MyBatis 缓存机制

## 6.1 缓存机制简介

1)    MyBatis 包含一个非常强大的查询缓存特性,它可以非常方便地配置和定制。缓存可以极大的提升查询效率

2)    MyBatis系统中默认定义了两级缓存

​		一级缓存（sqlSession级别缓存，默认开启不能关闭，可以清空）

​		二级缓存（namespace/mapper/sqlSessionFactory级别缓存，sqlSession关闭或提交后生效，默认不开启，pojo需实现Serializable接口）

3)    默认情况下，只有一级缓存（SqlSession级别的缓存，也称为本地缓存）开启。

4)    二级缓存需要手动开启和配置，他是基于namespace级别的缓存。

5)    为了提高扩展性。MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

## 6.2 一级缓存的使用

1)    一级缓存(local cache), 即本地缓存, 作用域默认为sqlSession。当 Session flush 或 close 后, 该 Session 中的所有 Cache 将被清空。

2)    本地缓存不能被关闭, 但可以调用 clearCache() 来清空本地缓存, 或者改变缓存的作用域.

3)    在mybatis3.1之后, 可以配置本地缓存的作用域. 在 mybatis.xml 中配置

4)    一级缓存的工作机制

​		同一次会话期间只要查询过的数据都会保存在当前SqlSession的一个Map中

​		key: hashCode+查询的SqlId+编写的sql查询语句+参数

## 6.3 一级缓存失效的几种情况

1)    不同的SqlSession对应不同的一级缓存

2)    同一个SqlSession但是查询条件不同

3)    同一个SqlSession两次查询期间执行了任何一次增删改操作

4)    同一个SqlSession两次查询期间手动清空了缓存

## 6.4 二级缓存的使用

1)    二级缓存(second level cache)，全局作用域缓存

2)    二级缓存默认不开启，需要手动配置

3)    MyBatis提供二级缓存的接口以及实现，缓存实现要求POJO实现Serializable接口

4)    二级缓存在 SqlSession 关闭或提交之后才会生效

5)    二级缓存使用的步骤:

​		①  全局配置文件中开启二级缓存

```xml
<setting name="cacheEnabled" value="true"/>
```

​		②  需要使用二级缓存的映射文件处使用cache配置缓存

```xml
<cache />
```

​		③  注意：POJO需要实现Serializable接口

6)    二级缓存相关的属性

​		①  eviction=“FIFO”：缓存回收策略：

​			LRU – 最近最少使用的：移除最长时间不被使用的对象。（默认）

​			FIFO – 先进先出：按对象进入缓存的顺序来移除它们。

​			SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。

​			WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。

​		②  flushInterval：刷新间隔，单位毫秒

​			默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新

​		③  size：引用数目，正整数

​			代表缓存最多可以存储多少个对象，太大容易导致内存溢出

​		④  readOnly：只读，true/false

​			true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。

​			false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是 false。

## 6.5 二级缓存缓存机制

- 基于相同SqlSessionFactory，多次查询相同数据时，首先从二级缓存中获取数据
  - 二级缓存中有数据【开启二级缓存且关闭或提交sqlSession时】
    - 直接使用二级缓存中数据，即可。
  - 二级缓存中没有数据
    - 优先从一级缓存中获取数据
      - 一级缓存中有数据，直接获取数据，使用即可。
      - 一级缓存中没有数据，执行sql从数据库中获取数据
        - 默认缓存至一级缓存
        - 开启二级缓存且关闭或提交sqlSession时，将数据缓存至二级缓存

## 6.6 缓存的相关属性设置

1)    全局setting的cacheEnable：

​		配置二级缓存的开关，一级缓存一直是打开的。

2)    select标签的useCache属性：

​		配置这个select是否使用二级缓存。一级缓存一直是使用的

3)    sql标签的flushCache属性：

​		增删改默认flushCache=true。sql执行以后，会同时清空一级和二级缓存。

​		查询默认 flushCache=false。

4)    sqlSession.clearCache()：只是用来清除一级缓存。

 

## 6.7 整合第三方缓存 

1)    为了提高扩展性。MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

2)    EhCache 是一个纯Java的进程内缓存框架，具有快速、精干等特点，是Hibernate中默认的CacheProvider

3)    整合EhCache缓存的步骤:

​		①  导入ehcache包，以及整合包，日志包

​				ehcache-core-2.6.8.jar、mybatis-ehcache-1.0.3.jar

​				slf4j-api-1.6.1.jar、slf4j-log4j12-1.6.2.jar

​		②  编写ehcache.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
 <!-- 磁盘保存路径 -->
 <diskStore path="D:\atguigu\ehcache" />
 
 <defaultCache 
   maxElementsInMemory="1000" 
   maxElementsOnDisk="10000000"
   eternal="false" 
   overflowToDisk="true" 
   timeToIdleSeconds="120"
   timeToLiveSeconds="120" 
   diskExpiryThreadIntervalSeconds="120"
   memoryStoreEvictionPolicy="LRU">
 </defaultCache>
</ehcache>
 
<!-- 
属性说明：
l diskStore：指定数据在磁盘中的存储位置。
l defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache便会采用<defalutCache/>指定的的管理策略
 
以下属性是必须的：
l maxElementsInMemory - 在内存中缓存的element的最大数目 
l maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大
l eternal - 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断
l overflowToDisk - 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上
 
以下属性是可选的：
l timeToIdleSeconds - 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置时间无穷大
l timeToLiveSeconds - 缓存element的有效生命期，默认是0.,也就是element存活时间无穷大
 diskSpoolBufferSizeMB 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认是30MB.每个Cache都应该有自己的一个缓冲区.
l diskPersistent - 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false。
l diskExpiryThreadIntervalSeconds - 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作
l memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时候， 移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出）
 -->

```

​		③  配置EhCache标签

```
<cache type="org.mybatis.caches.ehcache.EhcacheCache" eviction="FIFO" flushInterval="60000" size="512" readOnly="true" />
```



# 第7章：MyBatis 逆向工程

## 7.1 逆向工程简介

- MyBatis Generator: 简称MBG，是一个专门为MyBatis框架使用者定制的代码生成器，可以快速的根据表生成对应的映射文件，接口，以及bean类。

- 支持基本的增删改查，以及QBC（Query By Criteria）风格的条件查询。

- 但是表连接、存储过程等这些复杂sql的定义需要我们手工编写。

官方文档地址

http://www.mybatis.org/generator/

官方工程地址

https://github.com/mybatis/generator/releases

## 7.2 逆向工程的配置

1)    导入逆向工程的jar包

​		mybatis-generator-core-1.3.2.jar

2)    编写MBG的配置文件（重要几处配置）,可参考官方手册

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
  <!-- 
		id:表名
	  	targetRuntime: 执行生成的逆向工程的版本
	  			MyBatis3Simple: 生成基本的CRUD
	  			MyBatis3: 生成带条件的CRUD
   -->
  <context id="DB2Tables" targetRuntime="MyBatis3">
  
    <jdbcConnection driverClass="com.mysql.jdbc.Driver"
        connectionURL="jdbc:mysql://localhost:3306/mybatis_1129"
        userId="root"
        password="1234">
    </jdbcConnection>
	<!-- javaBean的生成策略-->
    <javaModelGenerator targetPackage="com.atguigu.mybatis.beans" targetProject=".\src">
      <property name="enableSubPackages" value="true" />
      <property name="trimStrings" value="true" />
    </javaModelGenerator>
	<!-- SQL映射文件的生成策略 -->
    <sqlMapGenerator targetPackage="com.atguigu.mybatis.dao"  targetProject=".\conf">
      <property name="enableSubPackages" value="true" />
    </sqlMapGenerator>
	
	<!-- Mapper接口的生成策略 -->
    <javaClientGenerator type="XMLMAPPER" targetPackage="com.atguigu.mybatis.dao"  targetProject=".\src">
      <property name="enableSubPackages" value="true" />
    </javaClientGenerator>
	<!-- 逆向分析的表 -->
    <table tableName="tbl_dept" domainObjectName="Department"></table>
    <table tableName="tbl_employee" domainObjectName="Employee"></table>
  </context>
</generatorConfiguration>

```

 

3)    运行代码生成器生成代码

```java
//代码生成器
try {
    List<String> warnings = new ArrayList<String>();
    boolean overwrite = true;
    File configFile = new File("mbg.xml");	//配置文件
    ConfigurationParser cp = new ConfigurationParser(warnings);
    Configuration config = cp.parseConfiguration(configFile);
    DefaultShellCallback callback = new DefaultShellCallback(overwrite);
    MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
    myBatisGenerator.generate(null);
} catch (IOException e) {
    e.printStackTrace();
} catch (XMLParserException e) {
    e.printStackTrace();
} catch (InvalidConfigurationException e) {
    e.printStackTrace();
} catch (SQLException e) {
    e.printStackTrace();
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

 

## 7.3 逆向工程的使用

1)    基本查询的测试

```java
@Test
	public void testSelect() throws Exception {
		SqlSessionFactory ssf = getSqlSessionFactory();
		SqlSession session = ssf.openSession();
		
		try {
			EmployeeMapper mapper = session.getMapper(EmployeeMapper.class);
			List<Employee> emps = mapper.selectAll();
			for (Employee employee : emps) {
				System.out.println(employee);
			}
		} finally {
			session.close();
		}
}

```

2)    带条件查询的测试

```java
@Test
	public void testSelect() throws Exception {
		SqlSessionFactory ssf = getSqlSessionFactory();
		SqlSession session = ssf.openSession();
		try {
			EmployeeMapper mapper = session.getMapper(EmployeeMapper.class);
			//条件查询: 名字中带有'张' 并且 email中'j'  或者 did = 2 
			EmployeeExample example =  new EmployeeExample();
			Criteria criteria = example.createCriteria();
			criteria.andLastNameLike("%张%");
			criteria.andEmailLike("%j%");
			//or 
			Criteria criteriaOr = example.createCriteria();
			criteriaOr.andDIdEqualTo(2);
			example.or(criteriaOr);
			List<Employee> emps = mapper.selectByExample(example);
			for (Employee employee : emps) {
				System.out.println(employee);
			}
		} finally {
			session.close();
		}
}

```



# 第8章：扩展-PageHelper分页插件

## 8.1 PageHelper 分页插件简介

1)    PageHelper是MyBatis中非常方便的第三方分页插件

2)    官方文档：

https://github.com/pagehelper/Mybatis-PageHelper/blob/master/README_zh.md

3)    我们可以对照官方文档的说明，快速的使用插件

## 8.2 PageHelper的使用步骤

1)    导入相关包pagehelper-x.x.x.jar 和 jsqlparser-0.9.5.jar

2)    在MyBatis全局配置文件中配置分页插件

```xml
  <plugins>      
      <plugin interceptor=*"com.github.pagehelper.PageInterceptor"*></plugin> 
  </plugins>  
```

3)    使用PageHelper提供的方法进行分页

4)    可以使用更强大的PageInfo封装返回结果

## 8.3 Page对象的使用

1)    在查询之前通过PageHelper.startPage(页码，条数)设置分页信息，该方法返回Page对象

```java
	@Test
	public void testPageHelper()  throws Exception{
		SqlSessionFactory ssf = getSqlSessionFactory();
		SqlSession session = ssf.openSession();
		try {
			EmployeeMapper mapper = 
                      session.getMapper(EmployeeMapper.class);
			//设置分页信息
			Page<Object> page = PageHelper.startPage(9, 1);
			List<Employee> emps = mapper.getAllEmps();
			for (Employee employee : emps) {
				System.out.println(employee);
			}
			System.out.println("=============获取分页相关的信息=================");
			System.out.println("当前页: " + page.getPageNum());
			System.out.println("总页码: " + page.getPages());
			System.out.println("总条数: " + page.getTotal());
			System.out.println("每页显示的条数: " + page.getPageSize());
		} finally {
			session.close();
		}
	}

```

 

## 8.4 PageInfo对象的使用 

1)    在查询完数据后，使用PageInfo对象封装查询结果，可以获取更详细的分页信息以及可以完成分页逻辑

```java
@Test
	public void testPageHelper1()  throws Exception{
		SqlSessionFactory ssf = getSqlSessionFactory();
		SqlSession session = ssf.openSession();
		try {
			EmployeeMapper mapper = session.getMapper(EmployeeMapper.class);
			//设置分页信息
			Page<Object> page = PageHelper.startPage(9, 1);
			List<Employee> emps = mapper.getAllEmps();
			// 
			PageInfo<Employee> info  = new PageInfo<>(emps,5);
			for (Employee employee : emps) {
				System.out.println(employee);
			}
			System.out.println("=============获取详细分页相关的信息=================");
			System.out.println("当前页: " + info.getPageNum());
			System.out.println("总页码: " + info.getPages());
			System.out.println("总条数: " + info.getTotal());
			System.out.println("每页显示的条数: " + info.getPageSize());
			System.out.println("是否是第一页: " + info.isIsFirstPage());
			System.out.println("是否是最后一页: " + info.isIsLastPage());
			System.out.println("是否有上一页: " + info.isHasPreviousPage());
			System.out.println("是否有下一页: " + info.isHasNextPage());
			
			System.out.println("============分页逻辑===============");
			int [] nums = info.getNavigatepageNums();
			for (int i : nums) {
				System.out.print(i +" " );
			}
		} finally {
			session.close();
		}
	}

```


