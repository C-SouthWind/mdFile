# SpringMVC

2021-12-17 20:33

ssm：mybatis + Spring +SpringMVC **MVC三层架构**

JavaSE：

JavaWeb：

SSM框架：研究官方文档，锻炼自学能力，锻炼笔记能力，锻炼项目能力



SpringMVC + Vue + SpringBoot + SpringCloud + Linux



SSM = JavaWeb做项目

Spring：IOC 和 AOP

SpringMVC ： SpringMVC的执行流程！

SpringMVC ：SSM框架整合！



MVC：模型（dao，service） 视图（jsp） 控制器（servlet）



dao

service

servlet：转发，重定向

jsp/html



**JSP：本质就是一个Servlet**



假设：你的项目的架构，是设计好的，还是演进的？

- Alibaba	PHP
- 随着用户大，Java
- 王坚 去 IOE  **MySQL**
- MySQL：MySQL ----> AliSQL、AliRedis
- All in one --->微服务



MVVM  :   M   V    VM  ViewModel：双向绑定



## MVC

**Controller：控制器**

1. 取得表单数据
2. 调用业务逻辑
3. 转向指定的页面

**Model：模型**

1. 业务逻辑
2. 保存数据的状态

**View：视图**

1. 显示页面

## JspServlet

### 导入jar包

```pom
 <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jsp-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
```



### HelloServlet

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取前端参数
        String method = req.getParameter("method");
        if (method.equals("add")) {
            req.getSession().setAttribute("msg","执行了add方法");
        }
        if (method.equals("delete")) {
            req.getSession().setAttribute("msg","执行了delete方法");
        }
        //2.调用业务层

        //3.视图转发或者重定向
        req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

### 测试jsp

```jsp

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${msg}
</body>
</html>

```

### 表单提交jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="/hello" method="post">
    <input type="text" name="method">
    <input type="submit">
</form>
</body>
</html>

```

### 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.chj.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>


</web-app>
```

### 配置tomcat，启动测试

http://localhost:8080/

http://localhost:8080/hello?method=add



## MVC框架要做哪些事情

1. 将url映射到java类或java类的方法
2. 封装用户提交的数据
3. 处理请求 -- 调用相关的业务处理 -- 封装响应数据
4. 将响应的数据进行渲染 .jsp/html 等表示层数据

# 1、SpringMvc特点

1. 轻量级，简单易学
2. 高效，基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4. 约定优于配置
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活



Spring的web框架围绕DispatcherServlet设计

DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5 开始，使用Java5 或者以上版本的用户可以采用基于注解形式进行开发，十分简洁

正因为SpringMvc好，简单，便捷，易学，天生和Spring无缝集成（使用SpringIoc和Aop）,使用约定优于配置，能够进行简单的junit测试，支持Restful风格，异常处理，本地化，国际化，数据验证，类型转换，拦截器 等等..... 所以我们要学习

**最重要的一点还是用的人多，使用的公司多**



# 	2、SpringMvc： Hello，SpringMVC

#### 配置版

##### 1、新建一个Moudle，添加web的支持！

​	项目右键 Add Frameworks Support

##### 2、确认导入了SpringMvc的依赖

```xml
<!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jsp-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
```



##### 3、配置web.xml，注册DispatcherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--1.注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件【servlet-name】 servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别 1-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!-- / 匹配所有的请求： （不包括.jsp）-->
    <!-- /* 匹配所有的请求： （包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

##### 4、编写SpringgMvc的配置文件！名称：springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

##### 5、springMvc-servlet.xml中 添加处理映射器

```xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
```

##### 6、springMvc-servlet.xml中 添加处理器适配器

```xml
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```

##### 7、springMvc-servlet.xml中 添加视图解析器

```xml
<!--    视图解析器 DispatcherServlet 给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```

##### 8.、我们要操作业务Controller，要么实现Controller接口，要么增加注解，需要返回一个ModelAndView 装数据，封视图

```java
//导入一个Controller接口
public class HelloController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中
        mv.addObject("msg","HelloSpringMVC!");
        //封装视图，放在ModelAndView中
        mv.setViewName("hello");//WEB-INF/jsp/hello.jsp
        return mv;
    }
}

```

##### 9、将自己的类交给SpringIoc容器，注册bean

```xml
    <!--Handler-->
    <bean id="/hello" class="com.chj.controller.HelloController"/>
```

##### 10、写要跳转的jsp页面，显示ModelAndView存放数据

```xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
	${msg}
</body>
</html>

```

##### 11.、配置tomcat  启动测试

http://localhost:8080/hello

如果遇到404

1.查看控制台输出，是不是少了jar包

2.jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖

​	Project Structure ---> Artifacts --->WEB-INF下创建lib文件夹---->在lib中添加Library Files----> 全部添加

3.重启tomcat解决

#### 注解版

##### 1、新建一个Moudle，添加web支持！

##### 2、由于Maven可能存在资源过滤的问题，我们配置

```xml
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

##### 3、在pom.xml文件引入相关的依赖

```xml
<!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jsp-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
```

##### 4、配置web.xml

- 注意web.xml版本问题，要最新版
- 注册DispatcherServlet
- 关联SpringMvc的配置文件
- 启动级别为1
- 映射路径为 / 【不要用/* ，会404】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--1.注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件【servlet-name】 servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别 1-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!-- / 匹配所有的请求： （不包括.jsp）-->
    <!-- /* 匹配所有的请求： （包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

##### 5、添加SpringMvc配置文件

- 让IOC的注解生效
- 静态资源过滤：HTML    JS    CSS  图片  视频 .....
- MVC的注解驱动
- 配置视图解析器

在resource目录下添加springmvc-servlet.xml配置文件，配置的形式与spring容器配置基本类似，为了支持基础注解的IOC，设置了自动扫描包的功能，具体配置信息如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--自动扫描包，让指定该包下的注解生效，由IOC容器统一管理-->
    <context:component-scan base-package="com.chj.controller"/>
    <!--让SpringMvc不处理静态资源-->
    <mvc:default-servlet-handler/>
    <!--
    支持mvc注解驱动
    在spring中一般采用@RequestMapping注解来完成映射关系
    要想使@RequestMapping注解生效
    必须在上下文中注册DefaultAnnotationHandlerMapping
    和一个AnnotationMethodHandlerAdapter实例
    这两个实例分别在类级别和方法级别处理
    而annotation-driven配置帮助我们自动完成上述两个实例的注入
    -->
    <mvc:annotation-driven/>

    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

在视图解析器中我们把所有的视图都存在放 /WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问

##### 6、创建Controller

```java
@Controller
@RequestMapping("/hello")
public class HelloController {

    @RequestMapping("/hello")
    public String hello(Model model){
        model.addAttribute("msg","hello,SpringMvcAnnotation!");
        return "hello";
    }
}
```

- @Controller 是为了让Spring Ioc容器初始化时自动扫描到
- @RequestMapping是为了映射请求路径，这里因为类于方法上都有映射所以访问时应该是 /hello/hello
- 方法中声明Model类型的参数是为了把Action中的数据带到视图中
- 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/hello.jsp

##### 7、创建试图层

在WEB-INF/jsp 目录中创建hello.jsp 视图可以直接取出并展示从controller带回的信息；

可以通过EL表示取出Model中存放的值，或者对象

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${msg}
</body>
</html>
```

##### 8、配置Tomcat运行

http://localhost:8080/hello/hello



#### 小结

- 创建一个web项目
- 导入相关jar包
- 编写web.xml 注册DispatcherServlet
- 编写springmvc配置文件
- 接下来就是去创建对应的控制了 controller
- 最好完成前端视图和controller之间的对应
- 测试运行调试

使用SpringMvc必须配置的三大件

**处理器映射器、处理器适配器、视图解析器**

通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大段的xml配置

# 3、SpringMvc流程

**SpringMvc原理**

![image-20211125194026085](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211125194026085.png)

**SpringMvc流程**

![image-20211125204541248](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211125204541248.png)

1. DIspatcherServlet表示前置控制器，是整个SpringMvc的控制中心。用户发出请求DispatcherServlet接收请求并拦截请求
   - 我们假设请求的url为 ：http://location:8080/SpringMvc/hello
   - 如上url拆分成三部分
   - http://location:8080 服务器域名
   - SpringMvc部署在服务器上的web站点
   - hello表示控制器
   - 通过分析，如上url表示为：请求位于服务器location:8080上的SpringMvc站点的hello控制器
2. HandlerMapping为处理器映射。DispatchServlet调用HandlerMapping，HandlerMapping根据请求查找Handler
3. HandlerExecution表示具体的Handler，其主要作用是根据url查找控制器，如上url被查找控制器为：hello
4. HandlerExecution将解析后的信息传递给DispatcherServlet，如解析控制器映射等
5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler
6. Handler让具体的Controller执行

# 4、Controller

## 控制器Controller

- 控制器复杂提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现
- 控制器负责解析用户的请求并将其转换为一个模型
- 在SpringMVC中一个控制器类可以包含多个方法
- 在SpringMvc中，对于Controller的配置方式有很多

## 实现Controller接口

Controller是一个接口，在org.springframework.web.servlet.mvc包下，接口中只有一个方法

```java
//实现该接口的类获得控制器功能
public interface Controller {
	//处理请求且返回一个模型与视图对象
	ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception;

}
```



测试

1、编写一个Controller类，ControllerHello

```java
//定义控制器
//注意点，不要导错包，实现Controller接口，重写方法
public class HelloController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //返回一个模型视图对象
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg","springMvc!");
        modelAndView.setViewName("hello");
        return modelAndView;
    }
}
```

2、在Spring配置文件中注册请求的bean，name对应请求路径，class对应处理请求的类

```xml
    <bean name="/t1" class="com.chj.controller.HelloController"/>
```

3、编写前端的jsp页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${msg}
</body>
</html>
```

测试成功

- 实现Controller定义控制器是较老的方法
- 缺点一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller，定义方式比价麻烦

## 使用注解@Controller

@Controller 注解类型用于声明Spring类的实例是一个控制器

Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需要在配置文件中声明组件扫描

```xml
<context:component-scan base-package="com.chj.controller"/>
```

增加Controller类

```java
@Controller
public class HelloController2 {

    @RequestMapping("/t2")
    public String test1(Model model){
        model.addAttribute("msg","springMvc-servlet");
        return "hello";
    }
}
```

测试成功

可以发现，我们两个请求都可以指向一个视图，但是页面结果的结果是不一样的，从这里可以看出视图是被复用的，而控制器与视图之间是弱耦合关系

注解方式是平时使用的最多的方式！除了这两种之外还有其他的方法



## RequestMapping

@RequestMapping

- @RequestMapping注解用于映射url到控制器类或一个特定的处理程序方法。可以于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径
- 为了测试结论更加准确，我们可以加上一个项目名测试
- 只注解在方法上面

```java
@Controller
public class HelloController3 {

    @RequestMapping("/h1")
    public String test1(Model model){
        model.addAttribute("msg","test1");
        return "hello";
    }
}
```

- 同时注解类于方法

```java
@Controller
@RequestMapping("/hello")
public class HelloController3 {

    @RequestMapping("/h1")
    public String test1(Model model){
        model.addAttribute("msg","test1");
        return "hello";
    }
}
```



## RestFul风格

**概念**

RestFul 就是一个资源定位及资源操作的风格，不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制

**功能**

- 资源 ： 互联网所有的事务都可以被抽象为资源
- 资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作
- 分别对应 添加、删除、修改、查询

**传统方式操作资源**：通过不同的参数来实现不同的效果！方法单一，post和get

- location:8080/add?a=1&b=1

**使用RestFul 操作资源** 可以通过不同的请求方式来实现不同的效果！

- location:8080/1/1



### 测试

创建一个类RestFulController

```java
@Controller
public class RestFulController {
}
```

在SpringMvc中可以使用 @PathVariable注解，让方法参数的值对应绑定到一个URL模板变量上

```java
@Controller
public class RestFulController {
    @RequestMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a,@PathVariable int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果是："+res);
        return "hello";
    }
}
```



使用method属性指定请求类型

用于约束请求的类型，可以收窄请求范围，指定请求谓词的类型如GET，HEAD，OPTIONS，PUT，PATCH，DELETE，TRACE等

增加一个方法

```java
 @RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.GET)
    public String test2(@PathVariable int a,@PathVariable int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果是："+res);
        return "hello";
    }
```



小结

SpringMvc的@RequestMapping 注解能够处理HTTP请求的方法 比如GET，PUT，POST，DELETE以及PATCH

所有的地址栏请求默认都会是HTTP GET类型的

方法级别的注解变体有如下几个：组合注解

```java
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

@GetMapping是一个组合注解

它所扮演的是@RequestMapping(method = RequestMethod.GET)

使用路径变量的好处

- 使路径变得更加简洁
- 获得参数更加方便，框架会自动进行类型转换
- 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法。



# 5、SpringMvc结果跳转方式

## ModelAndView

设置ModelAndView对象，根据view名称，和视图解析器跳到指定的页面

页面：{视图解析器前缀} + viewName + {视图解析器后缀}

```xml
<!--视图解析器-->    
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
    <!--前缀-->    
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!--后缀-->
    <property name="suffix" value=".jsp"/>
</bean>
```

对应的Controller

```java
public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
    ModelAndView modelAndView = new ModelAndView();

    modelAndView.addObject("msg","HelloSpringMvc!");
    modelAndView.setViewName("hello");

    return modelAndView;
}
```



## ServletAPI

通过设置ServletAPI，不需要视图解析器

1. 通过HttpServletResponse进行输出
2. 通过HttpServletResponse进行重定向
3. 通过HttpServletResponse实现转发

```java
@Controller
public class ResultGo{
    
    //进行输出
    @RequestMapping("/result/t1")
    public void test1(HttpServletRequest req,HttpServletResponse rsp) throw IOException{
        rsp.getWriter().println("Hello,Spring By Servlet API");
    }
    
    //重定向
    @RequestMapping("/result/t2")
    public void test2(HttpServletRequest req,HttpServletResponse rsp) throw IOException{
        rsp.sendRedirect("/index.jsp");
    }
    
    //请求转发
    @RequestMapping("/result/t3")
    public void test3(HttpServletRequest req,HttpServletResponse rsp) throw IOException{
        rsp.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,rsp);
    }
    
}
```

## SpringMvc

**通过SpringMvc来实现转发和重定向 - 无需视图解析器**

测试前，需要将视图解析器注释掉

```java
@Controller
public class ResultSpringMvc{
    @RequestMapping("/rsm/t1")
    public String test1(){
        //转发
        return "/index.jsp";
    }
    @RequestMapping("/rsm/t2")
    public String test2(){
        //转发
        return "forward:/index.jsp";
    }
    @RequestMapping("/rsm/t3")
    public String test3(){
        //重定向
        return "redirect:/index.jsp";
    }
}
```



**通过SpringMvc来实现转发和重定向 - 有视图解析器**

重定向，不需要视图解析器，本质就是重新请求一个新的地方，所有注意路径问题

可以重定向到另一个请求

```java
@Controller
public class ResultSpringMvc{
    @RequestMapping("/rsm/t1")
    public String test1(){
        //转发
        return "test";
    }
    @RequestMapping("/rsm/t2")
    public String test2(){
        //重定向
        return "redirect:/index.jsp";
    }
}
```



# SpringMvc：数据处理

## 处理提交数据

### 1、提交的域名称和处理方法的参数名一致

提交数据:  http://location:8080/hello?name=chj

处理方法

```java
@RequestMapping("/hello")
public String hello(String name){
    System.out,println(name);
    return "hello";
}
```

后台输出 chj

### 2、提交的域名称和处理方法的参数名不一致

提交数据:  http://location:8080/hello?username=chj

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
    System.out.println(name);
    return "hello";
}
```

后台输出 chj

### 3、提交的是一个对象

要求提交的表单域和对象的属性名一致，参数使用对象即可

1.实体类

```java
public class User{
    private int id;
    private String name;
    private int age;
}
```

2.提交数据：  http://location:8080/user?name=chj&id=1&age=15

3.处理方法：

```java
@RequestMapping("/user")
public String user(User user){
    System.out.println(user);
    return "hello";
}
```

后台输出：User{  id =1 , name = 'chj'  , age = 15 }

**如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null**



## 数据显示到前端

### 1、通过ModelAndView

```java
//导入一个Controller接口
public class HelloController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中
        mv.addObject("msg","HelloSpringMVC!");
        //封装视图，放在ModelAndView中
        mv.setViewName("hello");//WEB-INF/jsp/hello.jsp
        return mv;
    }
}

```

### 2、通过ModelMap

ModelMap

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name,ModelMap model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name)
    model.addAttribute("msg",name);
    System.out.println(name);
    return "hello";
}
```

### 3、Model

Model

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name,Model model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name)
    model.addAttribute("msg",name);
    System.out.println(name);
    return "hello";
}
```



```java
LinkedHashMap
    
ModelMap : 继承了LinkedHashMap，所有他拥有LinkedHashMap的全部功能！
    
Model ：精简版 （大部分情况，我们都直接使用Model）
```



### 对比

```text
Model 只有寥寥几个方法只适合用于存储数据，简化了新手对于Model对象的操作和理解

ModelMap 继承LinkedMap 除了实现了自身的一些方法，同样继承LinkedMap的方法和特性

ModelAndView 可以在存储数据的同时，可以进行配置返回的逻辑视图，进行控制展示层的跳转
```

使用80%的时间打好扎实的基础，剩下18%的时间研究框架，2%的时间学点英文，框架的官方文档永远是最好的教程



# SpringMvc：数据乱码

中文返回乱码时 

配置SpringMvc的拦截器  Filter

```xml
 <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

# SpringMvc：JSON

- JSON（JavaScript Object Notation ，JS对象标记） 是一种轻量级的数据交换格式，目前使用特别广泛
- 采用完全独立于编程语言的文本格式来存储和表示数据
- 简洁和清晰的层次结构使得JSON成为理想的数据交换语言
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率

在JavaScript语言中，一切都是对象，因此，任何JavaScript支持的类型都可以通过JSON来表示，例如字符串，数字，对象，数组等。

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON键值对** 是用来保存JavaScript对象的一种方式，和JavaScript对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号“ ” 包裹，使用冒号：分隔，然后紧接着值

- JSON是JavaScript对象的字符串表示法，它使用文本表示一个JS对象的信息，本质是一个字符串

```js
var obj = {a:'Hello',b:'World'};//这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a":"Hello" ,"b":"World"}';//这是一个JSON字符串，本质是一个字符串
```

- **JSON和JavaScript对象互转**

要实现从JSON字符串转换为JavaScript对象，使用JSON.parse()方法

```js
var obj = JSON.parse('{"a":"Hello" ,"b":"World"}');
//结果是：{a:'Hello',b:'World'}
```

要实现JavaScript对象转换为JSON字符串，使用JSON.stringify()方法

```js
var json = JSON.stringify{a:'Hello',b:'World'});
//结果是：'{"a":"Hello" ,"b":"World"}'
```

## Jackson

- Jackson应该是目前比较好的json解析工具
- 阿里巴巴的fastjson

**导入jar包**

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.0</version>
</dependency>

```

配置SpringMvc需要的配置

**web.xml**

```xml

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>

        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

```

**springmvc-servlet.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
        <context:component-scan base-package="com.chj.controller"/>
        <mvc:default-servlet-handler/>
        <mvc:annotation-driven/>

        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
                <property name="prefix" value="/WEB-INF/jsp/"/>
                <property name="suffix" value=".jsp"/>
        </bean>
</beans>
```

**创建实体类**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Accessors
public class User {
    private String name;
    private int age;
    private String sex;
}
```

**controlller测试**

```java
@Controller
public class UserController {
    @RequestMapping("/j1")
    @ResponseBody//它就不会走视图解析器，会直接返回一个字符串
    public String json() throws JsonProcessingException {
        //ObjectMapper
        ObjectMapper mapper = new ObjectMapper();
        User chj = new User("chj", 1, "23");
        return mapper.writeValueAsString(chj);
    }
}
```

**编写工具类**

```java
public class JsonUtils {

    public static String getJson(Object object){
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }

    public static String getJson(Object object,String dataFormat){
        ObjectMapper mapper = new ObjectMapper();
        //不使用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS,false);
        //自定义日期的格式
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(dataFormat);
        mapper.setDateFormat(simpleDateFormat);
        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```



**乱码问题解决**

在springmvc的配置文件中添加 StringHttpMessageConverter转换配置

```xml
<!--JSON乱码问题配置-->
<mvc:annotation-driven>
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

## FastJson

fastjson是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象于JavaBean对象的转换，实现JavaBean对象于json字符串的转换，实现json对象与json字符串的抓换。实现json的转换的方法很多

fasetJson的jar

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.78</version>
</dependency>

```

fastJson有三个主要的类

- JSONObject代表json对象
  - JSONObject实现了Map接口，猜想JSONObject底层操作是由Map实现的
  - JSONObejct对应json对象，通过各种形式的get（）方法可以获取json对象中的数据，也可利用诸如size()，isEmpty()等方法获取键：值对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的
- JSONArray代表json对象数组
  - 内部是由List接口中的方法来完成操作的
- JSON代表JSONObject和JSONArray的转换
  - JSON类源码分析与使用
  - 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化

## 解决json报错

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.13.0</version>
</dependency>


```

application.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <mvc:annotation-driven/>
</beans>
```



# Ajax

- AJAX = Asynchronus JavaScript and XML（异步的JavaScript 和 XML）

- AJAX  是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术

- AJAX不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术

- 在2005年 Google通过AJAX创造出动态性极强的web界面，在谷歌的搜索框输入关键字时JavaScript会把这些字符发送到服务器，服务器会返回一个搜索建议的列表，就像国内的百度搜索框一样

- 传统的网页，想要更新内容或者提交一个表单，都需要重新加载整个网页
- 使用ajax技术的网页，通过后台服务器进行少量的数据交换，就可以实现异步局部更新
- 使用ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的Web用户界面

## jQuery.ajax

- 纯JS原生实现Ajax不去使用，直接使用jquery提供的，方便学习和使用，避免重复造轮子， 可以了解原生XMLHttpRequest
- Ajax的核心是XMLHttpRequest对象（XHR）XHR为向服务器发送请求和解析服务器响应提供了接口，能够以异步的方式从服务器获取新数据
- jQuery提供多个Ajax有关的方法
- 通过jQuery Ajax方法，能够使用Http Get和Http Post从远程服务器上请求文本、HTML、XML或JSON 同时能把这些外部的数据直接载入网页的被选元素种
- jQuery不是生成者，而是大自然的搬运工
- jQueryAjax本质就是XMLHttpRequest 对他进行封装，方便调用

```js
jQuery.ajax(...)
            部分参数：
            	url			: 请求地址
            	type		: 请求方式，GET,POST(1.9.0之后用method)
				headers 	: 请求头
                data		: 要发送的数据
                contentType : 即将发送信息至服务器的内容编码类型（默认："application/x-www-form-urlencoded; charset=UTF-8"）
                async		: 是否异步
                timeout		: 设置请求超时时间（毫秒）
                beforeSend	: 发送请求前执行的函数（全局）
                complete	: 完成之后执行的回调函数（全局）
                success		: 成功之后执行的回调函数（全局）
                error		: 失败之后执行的回调函数（全局）
                accepts		: 通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
                dataType	: 将服务器端返回的数据转换成指定类型
                	"xml" 	: 将服务器返回的数据转换成指定类型
                    "text"	: 将服务器返回的内容转换成普通文本格式
               		"html"	: 将服务器返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行
                    "script":尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
                    "json"	: 将服务器端返回的内容转换成响应的JavaScript对象
                    "jsonp"	: JSONP格式使用JSONP形式调用函数时，如"myurl?callback=?"  jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```



HTML + css + js

js

- 函数 ：闭包（） （）
- DOM
  - id ，name，tag
  - create ，remove
- BOM
  - window
  - document







## 失焦触发搜索

controller

```java
@Controller
public class UserController {

    @RequestMapping("/aaa")
    public String aa(Model model){
        model.addAttribute("aaa","bbb");
        return "hello";
    }

    @RequestMapping("/a1")
    public void a1(String name, HttpServletResponse response) throws IOException {
        System.out.println("name = " + name + ", response = " + response);
        if ("chj".equals(name)) {
            response.getWriter().println("true");
        }else {
            response.getWriter().println("false");
        }
    }
}

```

springMvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

        <mvc:annotation-driven/>
        <mvc:default-servlet-handler/>
        <context:component-scan base-package="com.chj.controller"/>

        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
            <property name="prefix" value="/WEB-INF/jsp/"/>
            <property name="suffix" value=".jsp"/>
        </bean>
</beans>
```

jquery-3.6.0.js文件

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
   <script src="static/js/jquery-3.6.0.js"></script>
  </head>
  <body>
    用户名：<input type="text" id="username" onblur="a()">
  </body>
  <script>
      function a() {
          $.post({
              url:"/a1",
              data:{"name":$("#username").val()},
              success:function (data) {
                  alert(data);
              }
          })
      }
  </script>
</html>

```

## 查询数据

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2021/12/14
  Time: 19:55
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="static/js/jquery-3.6.0.js"></script>
</head>
<body>
    <input type="button" id="btn" value="加载数据">
    <table>
        <thead>
            <tr>
                <td>id</td>
                <td>姓名</td>
                <td>密码</td>
            </tr>
        </thead>
        <tbody id="tbody">

        </tbody>
    </table>
</body>
</html>
<script>
        $("#btn").click(function () {
            $.post("/getUser",function (data) {
                var html = "";
                for (let i = 0;i<data.length;i++){
                    html += "<tr>"+
                        "<td>"+data[i].id+"</td>" +
                        "<td>"+data[i].name+"</td>" +
                        "<td>"+data[i].pwd+"</td>" +
                        "</tr>"
                }
                $("#tbody").html(html);
            })
        });
</script>

```

# SpringMvc  拦截器

SpringMvc 的处理器拦截器类似于Servlet开发中的过滤器Filter，用于处理进行预处理和后处理。开发者可以自己定义一些拦截器来实现特点的功能

**过滤器与拦截器的区别：**拦截器是AOP思想的具体应用

**过滤器**

- servlet规范中的一部分，任何java web工程都可以使用
- 在url-pattern中配置了/*之后，可以对所有要访问的资源进行拦截

**拦截器**

- 拦截器是SpringMvc框架自己的，只有使用了SpringMvc框架的工程才能使用
- 拦截器只会拦截访问的控制器方法，如果访问的是jsp、html、css、image、js是不会进行拦截的



springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

        <mvc:annotation-driven/>
    <mvc:default-servlet-handler/>
    <context:component-scan base-package="com.chj.controller"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

<!--    拦截器配置-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--/**  包括这个请求下面的所有请求-->
            <mvc:mapping path="/**"/>
            <bean class="com.chj.config.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
</beans>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

MyInterceptor

```java
package com.chj.config;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author ：chj
 * @date ：Created in 2021/12/14 20:36
 * @params :
 */
public class MyInterceptor implements HandlerInterceptor {
    //return true 执行下一个拦截器
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("处理前=======================================");
        return true;
    }

    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("处理后=====================================");
    }

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("清理======================================");
    }
}

```

# 登录判断验证

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>spring</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:appliction.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>spring</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

login.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/goLogin" method="post">
        <input type="text" name="username">
        <input type="text"  name="pwd">
        <input type="submit" value="登录">
    </form>
</body>
</html>

```

main.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
登录成功
<h1> ${msg}</h1>
</body>
</html>

```

index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <h1><a href="/login">登录</a></h1>
  <h1><a href="/main">首页</a></h1>
  </body>
</html>

```

appliction.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <mvc:default-servlet-handler/>
    <mvc:annotation-driven/>
    <context:component-scan base-package="com.chj.controller"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.chj.interceptor.LoginInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>

</beans>
```

LoginInterceptor.java

```java
public class LoginInterceptor implements HandlerInterceptor {

    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        HttpSession session = request.getSession();

        if (request.getRequestURI().contains("login")) {
            return true;
        }
        if (request.getRequestURI().contains("goLogin")) {
            return true;
        }
        if (session.getAttribute("userInfo")!=null) {
            return true;
        }
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
        return false;
    }
}

```

UserController.java

```java
@Controller
public class UserController {
    @RequestMapping("/login")
    public String login(){
        return "login";
    }

    @RequestMapping("/main")
    public String main(Model model){
        model.addAttribute("msg","登录成功");
        return "main";
    }
    @RequestMapping("/goLogin")
    public String goLogin(HttpSession session, String username, String pwd){
        System.out.println("username = " + username + ", pwd = " + pwd);
        session.setAttribute("userInfo",username);
        return "main";
    }
}

```

# SpringMvc 文件上传和下载

文件上传是项目开发中最常见的功能之一，SpringMvc可以很好的支持文件上传，但是SpringMvc上下文中默认没有装配MultipartResolver，因此默认情况下其不能处理文件上传工作。如果想使用Spring的文件上传功能，则需要在上下文中配置MultipartResolver。

前端表单要求：为了能上传文件，必须将表单的method设置为post，并将enctype设置为multipart/form-data。只有在这样的情况下，浏览去才会把用户选择的文件以二进制数据发送给服务器

**对表单中的enctype属性做个详细的说明：**

- application/x-www=form-urlencoded：默认方式，只处理表单域中的value属性值，采用这种编码方式的表单会将表单域中的值处理成URL编码方式
- multipart/from-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码
- text/plain：除了把空格转换为+号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件

### 文件上传

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>spring</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:application.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>spring</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <form action="/file2" enctype="multipart/form-data" method="post">
    <input type="file" name="file">
    <input type="submit" value="upload">
  </form>
  </body>
</html>

```

jar

```xml
 <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.4</version>
        </dependency>
```

application.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <mvc:annotation-driven/>
    <mvc:default-servlet-handler/>
    <context:component-scan base-package="com.chj.controller"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--文件上传配置-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!--请求的编码格式，必须和jsp的pageEncoding属性一致，以便正确读取表单的内容 默认为 ISO-8859-1 -->
        <property name="defaultEncoding" value="utf-8"/>
        <!--文件大小上限，单位为字节 （10485760=10M）-->
        <property name="maxUploadSize" value="10485760"/>
        <property name="maxInMemorySize" value="40960"/>
    </bean>
</beans>
```

FileController.java

```java
@Controller
public class FileController {

    //@RequestParam 将name=file控件得到的文件封装成CommonsMultipartFile对象
    //批量上传CommonsMultipartFile则为数据即可
    @RequestMapping("/file")
    public String file(@RequestParam("file")CommonsMultipartFile file, HttpServletRequest request) throws IOException {
        //获取文件名 file.getOriginalFilename();
        String uploadFileName = file.getOriginalFilename();
        //如果文件名为空，直接返回到首页
        if ("".equals(uploadFileName)) {
            return "redirect:/index";
        }
        System.out.println("上传文件名："+uploadFileName);
        //上传路径保存位置
        String path = request.getServletContext().getRealPath("/upload");
        //如果路径不存在，创建一个
        File realPath = new File(path);
        if (!realPath.exists()) {
            realPath.mkdir();
        }
        System.out.println("上传文件保存地址："+realPath);

        InputStream is = file.getInputStream();//文件输入流
        FileOutputStream os = new FileOutputStream(new File(realPath, uploadFileName));//文件输出流

        //读取写出
        int len=0;
        byte[] buffer = new byte[1024];
        while ((len = is.read(buffer))!=-1){
            os.write(buffer,0,len);
            os.flush();
        }
        os.close();
        is.close();

        return "redirect:/index";
    }

    @RequestMapping("/file2")
    public String file2(@RequestParam("file") CommonsMultipartFile file,HttpServletRequest request) throws IOException {
       //上传路径保存设置
        String path = request.getServletContext().getRealPath("/upload");
        File realPath = new File(path);
        if (!realPath.exists()) {
            realPath.mkdir();
        }
        //上传文件地址
        System.out.println("上传文件保存地址" + realPath);

        //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
        file.transferTo(new File(realPath+"/"+file.getOriginalFilename()));
        return "redirect:/index";
    }
}

```

### 文件下载

1. 设置response响应头
2. 读取文件 -- InputStream
3. 写出文件 -- OutputStream
4. 执行操作
5. 关闭流（先开后关）

```java
 @RequestMapping("/download")
    public String download(HttpServletResponse response,HttpServletRequest request) throws Exception {
        //要下载的图片地址
        String path = request.getServletContext().getRealPath("/upload");
        String fileName = "1.jpg";

        //设置 response响应头
        response.reset();//设置页面不缓存，情况buffer
        response.setCharacterEncoding("utf-8");//字符编码
        response.setContentType("multipart/form-data");//二进制传输数据
        //设置相应头
        response.setHeader("Content-Disposition",
                "attachment;fileName"+ URLEncoder.encode(fileName,"utf-8"));

        File file = new File(path, fileName);
        //2.读取文件 -- 输入流
        FileInputStream input = new FileInputStream(file);
        //3.写出文件 -- 输入流
        OutputStream out = response.getOutputStream();

        byte[] buff = new byte[1024];
        int index=0;
        //4.执行 写出操作
        while ((index = input.read(buff)) != -1){
            out.write(buff,0,index);
            out.flush();
        }
        out.close();
        input.close();
        return null;
    }
```

index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <form action="/file2" enctype="multipart/form-data" method="post">
    <input type="file" name="file">
    <input type="submit" value="upload">
  </form>
  <a href="static/1.jpg"> 下载</a>
  </body>
</html>

```

