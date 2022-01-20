# Mybatis

github:https://github.com/C-SouthWind/mybatis-stu

2021-11-04 20:05

环境：

- JDK 1.8

- Mysql 5.7

- maven 3.6.1

- IDEA

回顾：

- JDBC
- Mysql
- Java基础
- Maven
- Junit

SSM框架：配置文件的。最好的方式：看官网文档；

# 1、简介

## 1.1、什么是Mybatis

![1634647200301](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1634647200301.png)

-  MyBatis 是一款优秀的**持久层框架**。
- 它支持自定义 SQL、存储过程以及高级映射。
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
-  MyBatis 本是apache的一个[开源项目](https://baike.baidu.com/item/开源项目/3406069)iBatis, 2010年这个[项目](https://baike.baidu.com/item/项目/477803)由apache software foundation 迁移到了[google code](https://baike.baidu.com/item/google code/2346604)，并且改名为MyBatis 。
-  2013年11月迁移到[Github](https://baike.baidu.com/item/Github/10145341)



如何获得Mybatis?

- maven仓库

  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.7</version>
  </dependency>
  ```

- GitHub : https://github.com/mybatis/mybatis-3/releases

- 中文文档 ： https://mybatis.net.cn/getting-started.html

## 1.2、持久化

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程
- 内存：**断电即失**
- 数据库（jdbc），io文件持久化。

**为什么需要持久化？**

- 有一些对象，不能让他丢掉。

- 内存太贵



## 1.3、持久层

Dao层，Service，Controller层.....

- 完成持久化工作的代码块

- 层界限十分明显

  

## 1.4、为什么需要Mybatis?

- 帮助程序员将数据存入道数据库中。
- 方便
- 传统的JDBC代码太复杂了。简化。框架。自动化。
- 不用Mybatis也可以。更容易上手。**技术没有高低之分**
- 优点：
  - 简单易学
  - 灵活
  - sql和代码的分离，提高了可维护性。
  - 提供映射标签，支持对象与数据库的orm字段关系映射
  - 提供对象关系映射标签，支持对象关系组建维护
  - 提供xml标签，支持编写动态sql

**最重要的一点：使用人多！**

Spring 	SpringMVC	SpringBoot



# 2、第一个Mybatis

## 2.1、搭建环境

新建项目

1.新建一个普通的maven项目

2.删除src目录

3.导入maven

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.27</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.8.1</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.22</version>
    </dependency>
</dependencies>

<!--在build中配置resources 来防止我们资源导出失败的问题-->
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```



## 2.2、创建一个模块

- 编写mybatis的核心配置文件

  ```xml
  <?xml version="1.0" encoding="UTF8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <!--configuration 核心配置文件-->
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://115.159.191.57:3306/zln?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT%2B8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="chj0526"/>
              </dataSource>
          </environment>
      </environments>
  
      <!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
      <mappers>
          <mapper resource="com/chj/mapper/UserMapper.xml"></mapper>
      </mappers>
  </configuration>
  ```

- 编写mybatis工具类

  ```java
  /**
   * @author ：chj
   * @date ：Created in 2021/10/20 19:23
   * @params :  sqlSessionFactory ---> sqlSession
   */
  public class MybatisUtils {
      private static SqlSessionFactory sqlSessionFactory  ;
      static {
          try {
              //使用Mybatis第一步：获取sqlSessionFactory对象
              String resource = "mybatis-config.xml";
              InputStream inputStream = inputStream = Resources.getResourceAsStream(resource);
              sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
      //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
      //SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句
      public static SqlSession getSession(){
          return sqlSessionFactory.openSession();
      }
  }
  ```

  

## 2.3、编写代码

- 实体类

  ```java
  /**
   * @author ：chj
   * @date ：Created in 2021/10/20 19:34
   * @params :  user实体
   */
  @Data
  @AllArgsConstructor
  @NoArgsConstructor
  @Accessors
  public class User {
      private int id;
      private String name;
      private String pwd;
  }
  ```

- Mapper接口

  ```java
  /**
   * @author ：chj
   * @date ：Created in 2021/10/20 19:35
   * @params :  UserMapper
   */
  public interface UserMapper {
      List<User> getUserList();
  }
  ```

- 接口实现类

  ```java
  <?xml version="1.0" encoding="UTF8" ?>
      <!DOCTYPE mapper
      PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      
      <!--namespace=绑定一个对应的Mapper接口-->
      <mapper namespace="com.chj.mapper.UserMapper">
  
      <select id="getUserList" resultType="com.chj.pojo.User">
      	select * from user
      </select>
  
      </mapper>
  ```
  



## 2.4、测试

注意点：

org.apache.ibatis.binding.BindingException: Type interface com.chj.mapper.UserMapper is not known 		the MapperRegistry.

**MapperRegistry是什么？**

核心配置文件中注册mappers

- junit测试

```java
/**
 * @author ：chj
 * @date ：Created in 2021/10/20 19:47
 * @params :  user测试类
 */
public class UserMapperTest {

    @Test
    public void test(){
        //第一步：获取SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSession();
        //执行SQL

        //方式一：getMapper
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.getUserList();
        //方式二：
        List<User> userList = sqlSession.selectList("com.chj.mapper.UserMapper.getUserList");

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }
}
```

可能遇到的问题：

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名不对
4. 返回类型不对
5. Maven导出资源问题



# 3、CRUD

## 1、namespace

namespace中的包名要和mapper接口的包名一致！



## 2、select

选择，查询语句

- id : 就是对应的namespace中的方法名；
- resultType : Sql语句执行的返回值！
- parameterType : 参数类型！



1.编写接口

```java
//根据ID查询用户
User getUserById(int id);
```



2.编写对应的mapper中的sql语句

```xml
<select id="getUserById" parameterType="int" resultType="com.chj.pojo.User">
    select * from user where id = #{id}
</select>
```



3.测试

```java
@Test
public void getUserById(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    User userById = mapper.getUserById(1);
    System.out.println(userById);
    session.close();
}
```



## 3、Insert

1.编写接口

```java
//inser一个用户
int addUser(User user);
```



2.编写对应的mapper中的sql语句

```xml
<insert id="addUser" parameterType="com.chj.pojo.User" >
    insert into user (id,name,pwd) values (#{id},#{name},#{pwd})
</insert>

```



3.测试

```java
@Test
public void addUser(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    mapper.addUser(new User(6,"ww","1234"));
    session.commit();
    session.close();
}
```



## 4、update

1.编写接口

```java
//修改用户
int updateUser(User user);
```



2.编写对应的mapper中的sql语句

```xml
<update id="updateUser" parameterType="com.chj.pojo.User">
    update user set name = #{name} ,pwd = #{pwd} where id = #{id}
</update>
```



3.测试

```java
@Test
public void updateUser(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    mapper.updateUser(new User(6,"zz","567"));
    session.commit();
    session.close();
}
```



## 5、delete

1.编写接口

```java
//删除一个用户
int deleteUser(int id);
```



2.编写对应的mapper中的sql语句

```xml
<delete id="deleteUser" parameterType="int">
    delete from user where id = #{id}
</delete>
```



3.测试

```java
@Test
public void deleteUser(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    mapper.deleteUser(6);
    session.commit();
    session.close();
}
```



注意点：

- 增删改需要提交事务！



## 6、万能的Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用Map!

1.编写接口

```java
//万能的Map
int addUser2(Map<String,Object> map);
```



2.编写对应的mapper中的sql语句

```xml
//万能的Map
int addUser2(Map<String,Object> map);
```



3.测试

```java
@Test
public void addUser2(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("id",6);
    map.put("name","ww");
    map.put("pwd","123456");
    mapper.addUser2(map);

    session.commit();
    session.close();
}
```



Map传递参数，直接在sql中取出key即可！【parameterType="map"】

对象传递参数，直接在sql中取出对象的属性即可！【parameterType="Object"】

只有一个基本类型参数的情况下，可以直接在sql中取到！

多个参数用Map,**或者注解！**



## 7、模糊查询

1.Java代码执行的时候，传递通配符%%

```java
List<User> l = mapper.getUserLike("%l%");
```



2.在sql拼接中使用通配符

```java
select * from user where name like "%" #{name} "%"
```



# 4、配置解析

## 1、核心配置文件

- mybatis-config.xml

- Mybatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息

  ```xml
  configuration（配置）
  properties（属性）
  settings（设置）
  typeAliases（类型别名）
  typeHandlers（类型处理器）
  objectFactory（对象工厂）
  plugins（插件）
  environments（环境配置）
  environment（环境变量）
  transactionManager（事务管理器）
  dataSource（数据源）
  databaseIdProvider（数据库厂商标识）
  mappers（映射器）
  ```

## 2、环境配置（environments）

Mybatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个SqlSessionFactory实例只能选择一种环境**

学会使用配置多套运行环境！

Mybatis默认的事务管理器就是JDBC，连接池：POOLED

## 3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。【db.properties】

![image-20211024203931152](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211024203931152.png)

编写一个配置文件

db.properties

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://115.159.191.57:3306/zln?useSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
username=root
password=chj0526

```

在核心配置文件中映入

```java
<properties resource="db.properties"/>
```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，优先使用外部配置文件的！



## 4、类型别名（typeAliases）

- 类型别名可为 Java 类型设置一个缩写名字。 
- 它仅用于 XML 配置，意在降低冗余的全限定类名书写。

```xml
<!-- 可以给实体类起别名-->
<typeAliases>
    <typeAlias type="com.chj.pojo.User" alias="User"/>
</typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名

```xml
<typeAliases>
    <package name="com.chj.pojo"/>
</typeAliases>

```

在实体类比较少的时候，使用第一种方式

如果实体类十分多，建议使用第二章

第一种可以DIY别名，第二种则不行，如果非要改，需要在实体上增加注解

```java
@Alias("user")
public class User {}
```



## 5、设置（settings）

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。

![image-20211024212431250](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211024212431250.png)

![image-20211024212503415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211024212503415.png)



## 6、其他

- [typeAliases（类型别名）](https://mybatis.net.cn/configuration.html#typeAliases)
- [objectFactory（对象工厂）](https://mybatis.net.cn/configuration.html#objectFactory)

- plugins插件
  - mybatis-generator-core
  - mybatis-plus
  - 通用mapper

## 7、映射器（mappers

MapperRegistry：注册绑定我们的Mapper文件；

方式一：【推荐使用】

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
<mappers>
    <mapper resource="com/chj/mapper/UserMapper.xml"/>
</mappers>
```

方式二：

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
<mappers>
    <mapper class="com.chj.mapper.UserMapper"/>
</mappers>
```

方式三：使用扫描包进行注入绑定

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
<mappers>
    <package name="com.chj.mapper"/>
</mappers>
```

注意点：

- 接口和他的Mapper配置文件必须同名！
- 接口和他的Mapper配置文件必须在同一个包下



## 8、作用域（Scope）和生命周期

![image-20211024215223656](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211024215223656.png)

生命周期，和作用域，是至关重要的，因为错误的使用会导致非常严重的**并发问题**

**SqlSessionFactoryBuilder：**

- 一旦创建了SqlSessionFactroy，就不再需要它了
- 局部变量

**SqlSessionFactory：**

- 说白了就是可以想象为：数据库连接池

- SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例。**
- 因此 SqlSessionFactory 的最佳作用域是应用作用域。
- 最简单的就是使用单例模式或者静态单例模式。

**SqlSession：**

- 连接到连接池的一个请求：
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
- 用完之后需要赶紧关闭，否则资源被占用！
- ![image-20211024215938933](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211024215938933.png)

这里的每一个Mapper，就代表一个具体的业务！





# 5、解决属性名和字段名不一致的问题

## 1、问题

数据库中的字段

![1635247313131](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635247313131.png)

新建一个项目，拷贝之前的，测试实体类字段不一致的问题

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Accessors
@Alias("user")
public class User {
    private int id;
    private String name;
    private String password;
}
```

测试出现问题

![1635247816631](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635247816631.png)

```xml
//select * from user
//类型处理器
//select id,name,pwd from user
```



解决方法

- 取别名

```xml
<select id="getUserList" resultType="User">
    select id,name,pwd as password from user
</select>
```





## 2、resultMap

结果集映射	

```
id		name		pwd
id		name		password
```

```xml
<!--结果集映射-->
<resultMap id="UserMap" type="com.chj.pojo.User">
    <!--column数据库中的字段，property实体类中的属性 -->
    <result column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>

<select id="getUserList" resultMap="UserMap">
    select * from user
</select>
```

-  `resultMap` 元素是 MyBatis 中最重要最强大的元素 
-  ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。 
-  `ResultMap` 的优秀之处——你完全可以不用显式地配置它们。 
- 如果世界总是这么简单就好了。

# 6、日志

## 6.1、日志工厂

如果一个数据库操作，出现了异常，我们需要排错。日志就是最好的助手！

曾经：sout、debug

现在：日志工厂！

![1635249527006](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635249527006.png)

-  SLF4J 

-  LOG4J 

-  LOG4J2

-  JDK_LOGGING

-  COMMONS_LOGGING

-  STDOUT_LOGGING 

-  NO_LOGGING 

在Mybatis中具体使用那一个日志实现，在设置中设定 

**STDOUT_LOGGING 标准日志输出**

在mybatis核心配置文件中，配置我们的日志！

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

![1635249961966](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635249961966.png)



## 6.2、Log4j

生命是Log4j

-  Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件 
-  我们也可以控制每一条日志的输出格式 
-  通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程 
-  通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码

1.先导入log4j的包

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

```

2.log4j.properties

```xml
#设置日志的级别 ,多个以,分开(没有给出的，则不会被输出)
log4j.rootLogger=debug,console

#输出到控制台
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.Target=System.out
log4j.appender.console.Threshold=debug
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} [%c]-[%p] %m%n
```

3.配置log4j为日志的实现

```xml
<settings>
    <!--标准的日志工厂实现-->
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

4.log4j的使用！直接测试运行刚才的查询

![1635252208053](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635252208053.png)



**简单使用**

1.在要使用Log4j的类中，导入包import org.apache.log4j.Logger;

2.日志对象，参数为当前类的class

```java
static Logger logger= Logger.getLogger(UserTest.class);
```

3.日志级别

```java
logger.info("info：进入了testLog4j");
logger.debug("debug：进入了testLog4j");
logger.error("error：进入了testLog4j");
```



# 7、分页

**思考：为什么分页**

- 减少数据的处理量



## **7.1使用Limit分页**

```xml
语法：SELECT * FROM USER LIMIT startIndex,pageSize;
SELECT * FROM USER LIMIT 3; #[0,n]
```

使用Mybatis实现分页，核心SQL

1.接口

```java
//分页
List<User> getUserByLimit(Map<String,Integer> map);
```

2.Mapper.xml

```xml
<select id="getUserByLimit" parameterType="map" resultMap="UserMap">
    select * from user limit #{startIndex},#{pageSize}
</select>
```

3.测试

```java
@Test
public void  getUserByLimit(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    Map<String,Integer> map = new HashMap<String,Integer>();
    map.put("startIndex",1);
    map.put("pageSize",2);
    List<User> userByLimit = mapper.getUserByLimit(map);
    for (User user : userByLimit) {
        System.out.println(user);
    }

    session.close();
}
```

## 7.2、RowBounds分页

不再使用SQL实现分页

1.接口

```java
//分页RowBounds
List<User> getUserByRowBounds();
```

2.mapper.xml

```xml
<select id="getUserByRowBounds"  resultMap="UserMap">
    select * from user
</select>
```

3.测试

```java
@Test
public void getUserByRowBounds(){
    SqlSession session = MybatisUtils.getSession();
    RowBounds rowBounds = new RowBounds(1, 2);
    List<User> users = session.selectList("com.chj.mapper.UserMapper.getUserByRowBounds",null,rowBounds);

    System.out.println(users);
    session.close();
}
```



## 7.3、分页插件

![1635254110371](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635254110371.png)

了解即可，万一以后公司的架构师，说要使用，需要知道是什么东西！



# 8、使用注解开发

## 8.1、面向接口编程

大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编**程**

**根本原因：解耦，可拓展，提高复用，分层开发中，上层不用管具体的实现，大家都遵守共同的标准，使得开发变的容易，规范性更好**

在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的，对系统设计人员来讲就不那么重要了；

而各个对象之间的协作关系则成为系统设计的关键。小到不同之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。



**关于接口的理解**

接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

接口的本身反应了系统设计对系统的抽象理解

接口应有两类：

​	第一类是对一个个体的抽象，它可对应为一个抽象体（abstract class）;

​	第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）;

一个体有可能有多个抽象面。抽象体与抽象面是有区别的。





**三个面向区别**

面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法。

面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现

接口设计与非接口是针对复用技术而言，与面向对象（过程）不是一个问题，更多的体现就是对系统整体的架构



## 8.2、使用注解开发

1.注解在接口上实现

```java
@Select("select * from user")
List<User> getUsers();
```

2.需要在核心配置文件中绑定接口！

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
<mappers>
    <mapper class="com.chj.mapper.UserMapper"/>
</mappers>
```

3.测试

```java
@Test
public void getUsers(){
    SqlSession session = MybatisUtils.getSession();
    //底层主要应用反射
    UserMapper mapper = session.getMapper(UserMapper.class);
    List<User> users = mapper.getUsers();
    System.out.println(users);
    session.close();
}
```

本质：反射机制实现

底层：动态代理！

![1635256141015](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635256141015.png)



## **8.3、Mybatis详细的执行流程！**

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\Temp.png" alt="Temp" style="zoom: 50%;" />



## 8.4、CRUD

我们可以在工具类创建的时候实现自动提交事务！

```java
public static SqlSession getSession(){
    return sqlSessionFactory.openSession(true);
}
```

编写接口，增加注解

```java
public interface UserMapper{
    @Select("select * from user")
    List<User> getUsers();
    
    //方法存在多个参数，所有的参数前面必须加上@Param("id")注解
    @Select * from user where id = #{id}
    User getUserById(@Param("id") int id);
    
    @Insert("insert into user(id,name,pwd) valus (#{id},#{name},#{password})")
    int addUser(User user);
    
    @Update("update user set name=#{name},pwd=#{password} where id = #{id}")
    int updateUser(User user);
    
    @Delete("delete from user where id = #{uid}")
    int deleteUser(@Param("uid") int id);
}
```



测试类

【注意：我们必须要将接口注册绑定到我们的核心配置文件中！】



**关于@Param()注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不用加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上！
- 我们在SQL中引用的就是我们这里的@Param("uid")中设定的属性名！



# 9、Lombok



```java
	Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
    Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.

```

- java library
- plugs
- build tools
- with one annotation your class

使用步骤：

1. 在IDEA中安装Lombok插件！
2. 在项目中导入lombok的jar包

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.22</version>
</dependency>
```

3. 注解

   ```java
   @Getter and @Setter
   @FieldNameConstants
   @ToString
   @EqualsAndHashCode
   @AllArgsConstructor
   @NoArgsConstructor
   @Log , @Log4j , @Log4j2 , @Slf4j , @xslf4j , @CommonsLog , @JBossLog , @Flogger
   @Data
   @Builder
   @Delegate
   @Value
   @Accessors
   @Wither
   @SneakyThrows
   ```

   

# 10、多对一处理

多对一：

![1635425007128](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1635425007128.png)

- 多个学生，对应一个老师
- 对于学生这边而言，**关联**  多个学生，关联一个老师【多对一】
- 对于老师而言，**集合**，一个老师，有很多学生【一对多】

SQL：

```sql
CREATE TABLE teacher (
	'id' Int(10) NOT NULL,
    'NAME' VARCHAR(30) DEFAULT NULL,
    PRIMARY KEY('id')
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO teacher('id','name') VALUES (1.'陈老师');

CREATE TABLE student(
    'id' INT(10) NOT NULL,
    'name' VARCHAR(30) DEFAULT NULL,
    'tid' INT(10) DEFAULT NULL,
    PRIMARY KEY ('id'),
    KEY fktid ('tid'),
    CONSTRAINT fktid FOREIGN KEY('tid') REFERECES teacher('id')
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO student (id,name,tid) VALUES('1','小明','1');
INSERT INTO student (id,name,tid) VALUES('2','小红','1');
INSERT INTO student (id,name,tid) VALUES('3','小张','1');
INSERT INTO student (id,name,tid) VALUES('4','小李','1');
INSERT INTO student (id,name,tid) VALUES('5','小王','1');
```



## **测试环境搭建**

1. 导入Lombok
2. 新建实体类Teacher,Studnt
3. 建立Mapper接口
4. 建立Mapper.xml文件
5. 在核心配置文件中绑定注册我们的Mapper接口或者文件！
6. 测试查询是否能够成功！





## 按照查询嵌套处理

```xml
<!--
 思路：
  1.查询所有的学生信息
  2.根据查询出来的学生的tid，寻找对应的老师！
-->

<select id= "getStudent" resultMap="StudentTeacher">
    select * from student;
</select>

<resultMap id="StudentTeacher" type="Student">
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <!-- 复杂的属性，我们需要单独处理 对象：association 集合：collection-->
    <associattion property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
</resultMap>

<select id="getTeacher" resultType="Teacher">
    select * from teacher where id = #{id}
</select>
```



## 按照结果嵌套查询

```xml
<!--按照结果嵌套查询-->

<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname,t.name tname
    from student s,teacher t
    where s.tid = t.id
</select>

<resultMap id="StudentTeacher2" type="Student">
	<result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="Teacher">
    	<result property="name" column="tname"/>
    </association>
</resultMap>
```



回顾Mysql多对一查询方式：

- 子查询
- 联表查询



# 11、一对多处理

比如：一个老师拥有多个学生！

对于老师而言，就是一对多的关系

实体类

```java
@Data
public class Student{
    private int id;
    private String name;
    private int tid;
}
```

```java
@Data
public class Teacher{
    private int id;
    private String name;
    
    //一个老师拥有多个学生
    private List<Student> students;
}
```



## 按照结果嵌套查询

```xml
<!--按照结果嵌套查询-->
<select id="getTeacher" resultMap="TeacherStudent">
    select s.id sid, s.name sname, t.name tname ,t.id tid
    from student s,teacher t
    where s.tid = t.id and t.id = #{id}
</select>

<result id="TeacherStudent" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <!--复杂的属性，我们需要单独处理 对象： association  集合： collection
 javaType="" 指定属性的类型！
 集合中的泛型信息，我们使用ofType获取
 -->
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</result>
```



## 按照查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from teacher where id = #{tid}
</select>
<resultMap id="TeacherStudent2" type="Teacher">
    <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId" column="id"/>
</resultMap>
<select id="getStudentByTeacherId" resultType="Student">
    select * from student where tid = #{tid}
</select>
```

## 小结

1.关联 - association 【多对一】

2.集合 - collection【一对多】

3.javaType & ofType

​		1.JavaType 用来指定实体类中属性的类型

​		2.ofType 用来指定映射到List或者集合中的pojo类型，泛型中的约束类型！



注意点：

- 保证SQL的可读性，尽量保证通俗易懂
- 注意一对多和多对一中，属性名和字段的问题！
- 如果问题不好排查错误，可以使用日志，建议使用Log4j





# 12、动态SQL

什么是动态SQL :动态SQL就是指根据不同的条件生成不同的SQL语句

利用动态SQL这一特性可以彻底摆脱这种痛苦

```xml
动态SQL元素和JSTL或基于类似XML的文本处理器相似。在MyBatis之前的版本中，有很多元素需要花时间了解。MyBatis3大大精简了元素种类，现在只需要原来一半的元素便可。MyBatis采用功能强大的基于OGNL的表达式来淘汰其他大部分元素。

if
choose(when,otherwise)
trim(where,set)
foreach
```

## 搭建环境

```sql
CREATE TABLE blog(
	'id' varchar(50) NOT NULL COMMENT '博客id',
    'title' varchar(100) NOT NULL COMMENT '博客标题',
    'author' varchar(30) NOT NULL COMMENT '博客作者',
    'create_time' datetime NOT NULL COMMENT '创建时间',
    'views' int(30) NOT NULL COMMENT '浏览量'
)ENGINE=InnoDB DEFAULT CHARSET=utf8
```



创建一个基础工程

1.导包

2.编写配置文件

3.编写实体类

```java
@Data
public class Blog{
    private int id;
    private String title;
    private String author;
    private Date createTime;
    private int views;
}
```

4.编写实体类对应Mapper接口和Mapper.XML文件

## IF

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from blog where 1=1
    <if test="title != null">
        and title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</select>
```

## choose（when，otherwise）

```xml
<select id="queryBlogChoose" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```



## trim(where,set)

```xml
select * from blog
<where>
    <if test="title != null">
        title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</where>
```

```xml
<update id="updateBlog" parameterType="map">
    update blog
    <set>
        <if test="title != null">
            title = #{title}
        </if>
        <if test = "author != null">
            author =#{author}
        </if>
    </set>
    where id = #{id}
</update>
```

**所谓的动态SQL，本质还是SQL语句，只是我们可以在SQL层面，去执行一个逻辑代码**

## SQL片段

有的时候，我们可能会将一些公共的部分抽取出来，方便复用！

1.使用SQL标签抽取公共的部分

```xml
<sql id="if-title-author">
    <if test="title != null">
        title = #{title}
    </if>
    <if test="author != null">
        author = #{author}
    </if>
</sql>
```

2.在需要使用的地方使用Include标签引用即可

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <include refid="if-title-author"/>
    </where>
</select>
```



注意事项：

- 最好基于单表来定义SQL片段
- 不要存在where标签

## Foreach

```xml
select * from user where 1=1 and

<foreach item="id" collection="ids" 
         open="(" separator="or" close=")"> 
    #{id}
</foreach>
```

```xml
<!--
 select * from blog where 1=1 and (id=1 or id=2 or id=3)
 我们现在传递一个万能的map 这个map中可以存在一个集合！
-->
<select id="queryBlogForeach" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <foreach collection="ids" item="id" open="(" close=")" separator="or">
            id = #{id}
        </foreach>
    </where>

</select>
```

**动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以了！**

建议：

- 先在Mysql中写出完整的SQL,再对应的去修改成为我们的动态SQL实现通用即可！

# 13、缓存

## 13.1、简介

```
查询 ： 连接数据库，耗资源
	一次查询的结果，给他暂时存在一个可以直接取到的地方！-->内存：缓存
		
我们再次查询相同数据的时候，直接走缓存，就不用走数据库了
```



1.什么是缓存【cache】?

- 存在内存中的临时数据
- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上（关系型数据库数据文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题

2.为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统效率

3.什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据

## 13.2、Mybatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。
- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高拓展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

## 13.3、一级缓存

- 一级缓存也叫本地缓存：

  - 与数据库同一次会话期间查询到的数据会放在本地缓存中
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库；

  测试：

  ​    1.开启日志

  ​	2.测试在一个Session中查询两次相同记录

  ​	3.查看日志输出

  ```verilog
  2021-11-03 20:50:56 [org.apache.ibatis.datasource.pooled.PooledDataSource]-[DEBUG] Created connection 103103526.
  2021-11-03 20:50:56 [com.chj.mapper.UserMapper.getUsers]-[DEBUG] ==>  Preparing: select * from user
  2021-11-03 20:50:57 [com.chj.mapper.UserMapper.getUsers]-[DEBUG] ==> Parameters: 
  2021-11-03 20:50:57 [com.chj.mapper.UserMapper.getUsers]-[DEBUG] <==      Total: 6
  [User(id=1, name=chj, password=null), User(id=2, name=zln, password=null), User(id=3, name=zs, password=null), User(id=4, name=ls, password=null), User(id=5, name=ls, password=null), User(id=6, name=ww, password=null)]
  [User(id=1, name=chj, password=null), User(id=2, name=zln, password=null), User(id=3, name=zs, password=null), User(id=4, name=ls, password=null), User(id=5, name=ls, password=null), User(id=6, name=ww, password=null)]
  true
  2021-11-03 20:50:57 [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-[DEBUG] Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@6253c26]
  2021-11-03 20:50:57 [org.apache.ibatis.datasource.pooled.PooledDataSource]-[DEBUG] Returned connection 103103526 to pool.
  ```

缓存失效的情况：

​	1.查询不同的东西

​	2.增删改查，可能会改变原来的数据，所以必定会刷新缓存！

​	3.查询不同的Mapper.xml

​	4.手动清理缓存！

```java
SqlSession session = MybatisUtils.getSession();
//底层主要应用反射
UserMapper mapper = session.getMapper(UserMapper.class);
List<User> users = mapper.getUsers();
System.out.println(users);

session.clearCache();//手动清理缓存

List<User> users2 = mapper.getUsers();
System.out.println(users2);

System.out.println(users==users2);
session.close();
```

小结：一级缓存默认开启，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间段！

一级缓存就是一个Map

## 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；
- 工作机制
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据会被保存到二级缓存中；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；



步骤：

1.开启全局缓存

```xml
<!-- 显示的开启全局缓存 -->
<setting name="cacheEnabled" value="true"/>
```

2.在要使用二级缓存的Mapper中开启

```xml
<!-- 在当前Mapper.xml中使用二级缓存-->
<cache/>
```

也可以自定义参数

```xml
<!-- 在当前Mapper.xml中使用二级缓存-->
<cache eviction="FIFO"
       flushInterval="60000"
       size="512"
       readOnly="true"/>
```



小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会先放在一级缓存中
- 只有当会话提交，或者关闭的时候，才会提交到二级缓存中





## 13.5 缓存原理

缓存顺序

1.先看二级缓存中有没有

2.再看一级缓存中有没有

3.查询数据库





redis数据库做缓存！ K-V
