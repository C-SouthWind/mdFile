

# 1、Spring

2021-11-19  19:48

## 1.1、简介

- Spring：春天-------> 给软件行业带来了春天！
- 2002年，首次推出了Spring框架的雏形：interface21框架！
- Spring框架即以interface21框架为基础，经过重新设计，并不断丰富其内涵，于2004年3月24日，发布了1.0正式版本。
- **Rod Johnson**，Spring Framework创始人，著名作者。很难想象Rod Johnson的学历，真的让好多人大吃一惊，他是悉尼大学的博士，然而他的专业不是计算机，二十音乐学。
- Spring理念：使现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架！
- SSH：Struct2 + Spring + Hibernate！
- SSM：SpringMvc + Spring + Mybatis！
- 

官网：https://spring.io/projects/spring-framework

官方下载地址：https://docs.spring.io/spring-framework/docs/

GitHub：https://github.com/spring-projects/spring-framework

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.12</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.12</version>
</dependency>

```



## 1.2、优点

- Spring是一个开源的免费的框架（容器）！
- Spring是一个轻量级的、非入侵式的框架！
- 控制反转（IOC）, 面向切面编程（AOP）
- 支持事务的处理，对框架整合的支持！

总结一句话：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！



## 1.3、组成

![image-20211109192443910](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211109192443910.png)

## 1.4、拓展

在Spring的官网中有这个介绍：现代化的java开发！说白就是基于Spring的开发



- SpringBoot
  - 一个快速开发的脚手架
  - 基于SpringBoot可以快速的开发单个微服务
  - 约定大于配置！
- SpringCloud
  - SpringCloud 是基于SpringBoot实现的

因为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Spring及SpringMVC！ 承上启下的作用！



**弊端：发展了太久之后，违背了原来的理念！配置十分繁琐，人称：“ 配置地狱 ”**



# 2、IOC理论推导

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码！如果程序代码量十分大，修改一次的成本代价十分昂贵！

```java
private UserDao userDao;
//利用set进行动态实现值的注入！
public void setUserDao(UserDao userDao){
    this.userDao = userDao;
}
```



- 之前，程序是主动创建对象！控制权在程序员手上！
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接受对象！



这种思想，从本质上解决了问题，我们程序员不用再去管理对象的创建了。系统的耦合性大大降低，可以更加专注的在业务实现！这是IOC的原型！



## IOC本质

**控制反转IOC（Inversion  of Control）,是一种设计思想，DI（依赖注入）是实现IOC的一种方法**，也有人认为DI只是IOC的一种说法。没有IOC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。



采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。



**控制反转是一种通过描述（XML或注解）并通过第三方去生成或获取特定对象的方式。在Spring中实现控制反转的是IOC容器，其实现方法是依赖注入（Dependency Injection，DI）**





# 3、HelloSpring

```java
public class Hello {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "name='" + name + '\'' +
                '}';
    }
}
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 使用Spring来创建对象，在Spring这些都成为Bean

    类型 变量名 = new 类型（）
    bean = 对象  new Hello();

    id = 变量名
    class = new 的对象
    property 相当于给对象中的属性设置一个值
    -->
    <bean id="hello" class="com.chj.pojo.Hello">
        <property name="name" value="Spring"/>
    </bean>
</beans>
```



```java
public class MyTest {
    public static void main(String[] args) {
        //获取Spring的上下文对象！
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在Spring中管理了，我们要使用，直接去里面取出来就可以！
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.toString());
    }
}

```



这个过程就叫控制反转：

控制：谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring后，对象是由Spring来创建的

反转：程序本身不创建对象，而变成被动的接收对象

依赖注入：就是利用set方法来进行注入的

IOC是一种编程思想，由主动的编程变成被动接收

可以通过newClassPathXmlApplicationContext去浏览一下底层源码

**OK，到了现在，我们彻底不用再程序中去改动了，要实现不同的操作，只需要在xml配置文件中进行修改，所谓的IOC，一句话搞定：对象由Spring来创建，管理，装配！**

# 4、IOC创建对象的方式

1.使用无参构造创建对象，默认！

2.假设我们要使用有参构造创建对象。

​	1.下标赋值

```xml
<!--第一种 下标赋值！-->
<bean id="user" class="com.chj.pojo.User">
    <constructor-arg index="0" value="chj学习"/>
</bean>
```

2.类型

```xml
<!--第二种 通过类型创建 不建议使用！ -->
<bean id="user" class="com.chj.pojo.User">
    <constructor-arg type="java.lang.String" value="chj"/>
</bean>
```

3.参数名

```xml
<!--第三种 直接通过参数名来设置-->
<bean id="user" class="com.chj.pojo.User">
    <constructor-arg name="name" value="chj"/>
</bean>
```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了！



# 5、Spring 配置

## 5.1、别名

```xml
<!--别名，如果添加了别名，我们也可以使用别名获取到这个对象-->
<alias name="user" alias="userNew"/>
```

## 5.2、Bean的配置

```xml
<!--
    id : bean的唯一标识符，也就是相当于我们学的对象名
    class : bean对象所对应的全限定名 ： 包名 + 类型
    name : 也是别名
    -->
<bean id="user" class="com.chj.pojo.UserT" name="u2 u3,u4;u5">
    <property name="name" value="chj"/>
</bean>
```

## 5.3、import

这个import，一般用于团队开发使用，他可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，这三个人复制不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有人的beans.xml合并为一个总的！

- 张三
- 李四
- 王五
- applicationContext.xml

```xml
<import resource="beans.xml"/>
<import resource="beans.xml2"/>
<import resource="beans.xml3"/>
```

使用的时候，直接使用总的配置就可以了



# 6、依赖注入

## 6.1、构造器注入

前面已经记录



## 6.2、Set方式注入【重点】

- 依赖注入：Set注入！
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入！

【环境搭建】

1.复杂类型

```java
public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}

```

2.真是测试对象

```xml
public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

    public Student() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getHobbys() {
        return hobbys;
    }

    public void setHobbys(List<String> hobbys) {
        this.hobbys = hobbys;
    }

    public Map<String, String> getCard() {
        return card;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public Set<String> getGames() {
        return games;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }

    public Student(String name, Address address, String[] books, List<String> hobbys, Map<String, String> card, Set<String> games, String wife, Properties info) {
        this.name = name;
        this.address = address;
        this.books = books;
        this.hobbys = hobbys;
        this.card = card;
        this.games = games;
        this.wife = wife;
        this.info = info;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", address=" + address +
                ", books=" + Arrays.toString(books) +
                ", hobbys=" + hobbys +
                ", card=" + card +
                ", games=" + games +
                ", wife='" + wife + '\'' +
                ", info=" + info +
                '}';
    }
}

```

3.beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.chj.pojo.Address">
        <property name="address" value="中国"/>
    </bean>

    <bean id="student" class="com.chj.pojo.Student">
        <!--普通值注入，value-->
        <property name="name" value="chj"/>
        <!--Bean注入，ref-->
        <property name="address" ref="address"/>
        <!-- 数组注入 -->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>
        <!-- List -->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>
        <!-- map -->
        <property name="card">
            <map>
                <entry key="身份证" value="123456789"></entry>
                <entry key="银行卡" value="987654321"></entry>
            </map>
        </property>
        <!-- set -->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>COC</value>
                <value>BOB</value>
            </set>
        </property>
        <!-- null -->
        <property name="wife">
            <null/>
        </property>

        <!-- Properties -->
        <property name="info">
            <props>
                <prop key="学号">111</prop>
                <prop key="性别">男</prop>
                <prop key="姓名">小明</prop>
            </props>
        </property>
    </bean>
</beans>
```

4.测试类

```java
public class MyTest {
    public static void main(String[] args) {
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student.toString());
        /**
         * Student {
         *      name='chj',
         *      address=Address{
         *          address='中国'
         *      },
         *      books=[
         *          红楼梦,
         *          西游记,
         *          水浒传,
         *          三国演义
         *      ],
         *      hobbys=[
         *          听歌,
         *          敲代码,
         *          看电影
         *      ],
         *      card={
         *          身份证=123456789,
         *          银行卡=987654321
         *      },
         *      games=[
         *          LOL,
         *          COC,
         *          BOB
         *      ],
         *      wife='null',
         *      info={
         *          学号=111,
         *          性别=男,
         *          姓名=小明}
         *     }
         * */
    }
}
```

## 6.3、拓展方式注入

我们可以使用p命名空间和c命名空间进行注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--p命名空间注入，可以直接注入属性的值：property-->
    <bean id="user" class="com.chj.pojo.User" p:name="chj" p:age="18"/>

    <!-- c命名空间注入，通过构造器注入：construct-args-->
    <bean id="user2" class="com.chj.pojo.User" c:age="18" c:name="chj"/>
</beans>
```

测试：

```java
@Test
public void test(){
    ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
    User user = context.getBean("user2", User.class);
    System.out.println(user.toString());
}
```

注意点：p命名和c命名空间不能直接使用，需要导入xml约束！

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

## 6.4、bean的作用域

![image-20211112204059370](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211112204059370.png)

1.代理模式（Spring默认机制）

```xml
<bean id="user2" class="com.chj.pojo.User" c:age="18" c:name="chj" scope="singleton"/>
```

2.原型模式：每次从容器中get的时候，都会产生一个新对象

```xml
<bean id="user2" class="com.chj.pojo.User" c:age="18" c:name="chj" scope="prototype"/>
```

3. 其余的 request、session、application、这些个只能在web开发中使用到！

# 7、Bean的自动装配

- 自动装配是Spring满足bean依赖一种方式
- Spring会在上下文中自动寻找，并自动给bean装配属性！



Spring中有三种装配方式

1. 在xml中显示的配置
2. 在java中显示配置
3. 隐式的自动装配bean【重要】



## 7.1、测试

环境搭建：一个人有两个宠物！



## 7.2、ByName自动装配

```xml
<bean id="cat" class="com.chj.pojo.Cat"/>
<bean id="dog" class="com.chj.pojo.Dog"/>
<!--
        byName：会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid！
    -->
<bean id="people" class="com.chj.pojo.People" autowire="byName">
    <property name="name" value="chj"/>
</bean>
```

## 7.3、ByType自动装配

```xml
<bean  class="com.chj.pojo.Cat"/>
<bean  class="com.chj.pojo.Dog"/>
<!--
            byName：会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid！
            byType：会自动在容器上下文中查找，和自己对象属性类型相同的bean！
        -->
<bean id="people" class="com.chj.pojo.People" autowire="byType">
    <property name="name" value="chj"/>
</bean>
```



小结：

- byName的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致！
- byType的时候，需要保证所有的bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！

## 7.4、使用注解实现自动装配

jdk1.5支持的注解，Spring2.5就支持注解了！

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML. 

要使用注解须知：

 1.  导入约束 context约束

 2.  配置注解的支持  <context:annotation-config/>

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
             https://www.springframework.org/schema/beans/spring-beans.xsd
             http://www.springframework.org/schema/context
             https://www.springframework.org/schema/context/spring-context.xsd">
     
         <context:annotation-config/>
     
     </beans>
     ```

     

     **@Autowired**

     直接在属性上使用即可！也可以在set方式上使用！

     使用Autowired我们可以不用编写Set方法了，前提是你这个自动装配的属性在 IOC（Spring）容器中存在，且符合名字byName!

     ```xml
     @Nullable 字段标记了这个注解,说明这个字段可以为null;
     ```

     ```java
     public @interface Autowired {
         boolean required() default true;
     }
     ```

     ```java
     public class People {
         //如果显示定义Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
         @Autowired(required = false)
         private Cat cat;
         @Autowired
         private Dog dog;
         private String name;
     }
     ```

     如果@Autowired自动装配的环境比价复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用@Qualifier（value = "XXX"）去配合@Autowired的使用，指定一个唯一的bean对象注入！

```java
public class People {
    @Autowired
    @Qualifier(value = "cat1")
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog2")
    private Dog dog;
    private String name;
}
```

**@Resource注解**

```java
public class People {
   	@Resource(name="cat2")
    private Cat cat;
    @Resource(name="dog2")
    private Dog dog;
    private String name;
}
```





小结：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求这个对象存在！【常用】
- @Resource默认通过byName的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@Autowired 通过byType的方式实现



# 8、使用注解开发



在Spring4之后，要使用注解开发，必须要保证aop的包导入了

![image-20211116195115272](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211116195115272.png)



使用注解需要导入context约束，增加注解支持！





1. bean

2. 属性如何注入

```java
//等价于 <bean id="user" class="com.chj.pojo.User"/>
//@Component 组件
@Component
public class User {
    //相当于 <property name="name" value="chj"/>
    @Value("chj")
    public String name;
}
```

3. 衍生的注解

   @Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层！

   - dao 【@Repository】

   - service 【@Service】

   - controller 【@Controller】

     这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean

4. 自动装配

   ```xml
   @Autowired :自动装配通过类型。名字
   如果Autowired不能自动装配上属性，则需要通过@Qualifier(value="XXX")
   @Nullable   字段标记了这个注解，说明这个字段可以为null
   @Resource  : 自动装配通过名字。类型
   ```

5. 作用域

   ```java
   @Component
   @Scope("prototype")
   public class User {
       //相当于 <property name="name" value="chj"/>
       @Value("chj")
       public String name;
   }
   ```

6. 小结

   xml与注解：

   - xml更加万能，适用于任何场合！维护简单方便
   - 注解 不是自己类使用不了，维护相对复杂

   xml与注解最佳实践：

   - xml用来管理bean；

   - 注解只复杂完成属性的注入；

   - 我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持

     ```xml
     <!--指定要扫描的包，这个包下的注解就会生效-->
     <context:component-scan base-package="com.chj"/>
     <context:annotation-config/>
     ```

     

# 9、使用Java的方式配置Spring

我们现在要完全不使用Spring的xml配置了，全权交给Java来做！

JavaConfig是Spring的一个子项目，在Spring4之后，它成为了一个核心功能！

![image-20211116205806049](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211116205806049.png)

实体类

```java
//这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中
@Component
public class User {
    @Value("chj")
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置文件

```java
//这个也会Spring容器托管，注册到容器中，因为本来就是一个@Component
//@Configuration代表这是一个配置类，就和我们之前看到的beans.xml
@Configuration
@ComponentScan("com.chj.pojo")
@Import(ChjConfig2.class)
public class ChjConfig {

    //注册一个bean，就相当于我们之前写的一个bean标签
    //这个方法的名字，就相当于bean标签中的id属性
    //这个方法的返回值，就相当于bean标签中的class属性
    @Bean("user")
    public User getUser(){
        return new User();//就是返回要注入到bean的对象！
    }
}
```



测试类

```java
public class MyTest {
    public static void main(String[] args) {
        //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(ChjConfig.class);
        User user = context.getBean("user", User.class);
        System.out.println(user.getName());
    }
}
```



# 10、代理模式

为什么要学习代理模式 ? 因为这就是SpringAOP的底层 【SpringAOP和SpringMVC】

代理模式的分类：

- 静态代理
- 动态代理

![image-20211116230122131](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211116230122131.png)

## 10.1、静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人！



代码步骤：

1. 接口

   ```java
   public interface Rent {
       public void rent();
   }
   ```

2. 真实角色

   ```java
   public class Host implements Rent {
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

3. 代理角色

   ```java
   public class Proxy implements Rent {
       private Host host;
   
       public Proxy() {
       }
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       public void rent() {
           seeHouse();
           host.rent();
           hetong();
           fare();
       }
   
       public void seeHouse(){
           System.out.println("中介带你看房");
       }
   
       public void hetong(){
           System.out.println("签租赁核体");
       }
       public void fare(){
           System.out.println("收中介费");
       }
   }
   
   ```

4. 客户端访问代理角色

   ```java
   public class Client {
       public static void main(String[] args) {
           //房东要租房子
           Host host = new Host();
           //代理，中介帮房东租房子， 代理会有一些附属操作
           Proxy proxy = new Proxy(host);
           //不用面对房东，直接找中介租房即可
           proxy.rent();
       }
   }
   ```

   



代理模式的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公共也就交给代理角色！实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！

缺点：

- 一个真实角色就会产生一个代理角色；代码量会翻倍，开发效率变低

## 10.2、代理

service

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}
```

ServiceImpl

```java
public class UserServiceImpl implements UserService {
    public void add() {
        System.out.println("增加了一个用户");
    }

    public void delete() {
        System.out.println("删除了一个用户");
    }

    public void update() {
        System.out.println("修改了一个用户");
    }

    public void query() {
        System.out.println("查询了一个用户");
    }
}

```

测试

```java
public class Client {
    public static void main(String[] args) {
        UserServiceImpl userService = new UserServiceImpl();
       	userService.add();
    }
}
```

添加代理（类似于aop）

ServiceProxy

```java
public class UserServiceProxy implements UserService {
    private UserServiceImpl userService;

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    public void add() {
        log("add");
        userService.add();
    }

    public void delete() {
        log("delete");
        userService.delete();
    }

    public void update() {
        log("update");
        userService.update();
    }

    public void query() {
        log("query");
        userService.query();
    }

    public void log(String name){
        System.out.println("使用了" + name + "方法");
    }
}
```

测试

```java
public class Client {
    public static void main(String[] args) {
        UserServiceImpl userService = new UserServiceImpl();
        
        UserServiceProxy proxy = new UserServiceProxy();
        proxy.setUserService(userService);
        proxy.add();
    }
}
```

![image-20211117205140194](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117205140194.png)

## 10.3、动态代理

- 动态代理和静态代理角色一样

- 动态代理的代理类是动态生成的，不是我们直接写好的！

- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理

  - 基于接口 ----JDK动态代理 
  - 基于类：cglib
  - java字节码实现：javasist

  需要了解两个类：Proxy：代理，InvocationHandler：调用处理程序

动态代理的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公共也就交给代理角色！实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可



# 11、AOP

## 11.1、 什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序和功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生泛型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率



## 11.2、Aop在Spring中的作用

**提供声明式事务：允许用户自定义切面**

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等....
- 切面（ASPECT）：横切关注点被模块化的特殊对象。即 它是一个类
- 通知（Advice）：切面必须要完成的工作。即 他是类中的一个方法
- 目标（Target）：被通知对象
- 代理（Proxy）：向目标对象应用通知之后创建的对象
- 切入点（PointCut）：切面通知执行的 "地点"的定义
- 连接点（JointPoint）：与切入点匹配的执行点



SpringAop中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice;

- 前置通知：方法前：org.springframework.aop.MethodBeforeAdvice
- 后置通知：方法后：org.springframework.aop.AfterReturningAdvice
- 环绕通知：方法前后：org.aopalliance.intercept.MethodInterceptor
- 异常抛出通知：方法抛出异常：org.springframework.aop.ThrowsAdvice
- 引介通知：类中增加新的方法属性：org.springframework.aop.IntroductionInterceptor

即AOP在不改变原有代码的情况下，去新增新的功能

## 11.3、使用Spring实现Aop

使用AOP织入，需要导入一个依赖包！

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.7</version>
</dependency>

```



方式一：使用Spring的API接口【主要是SpringAPI接口实现】

xml：

```xml
 <!--注册bean-->
    <bean id="userService" class="com.chj.service.UserServiceImpl"/>
    <bean id="log" class="com.chj.log.Log"/>
    <bean id="afterLog" class="com.chj.log.AfterLog"/>

    <!--方式一：使用原生Spring API接口-->
    <!--配置aop:需要导入aop的约束-->
    <aop:config>
        <!--切入点：expression:表达式，execution(要执行的位置！ * * * * *)-->
        <aop:pointcut id="pointcut" expression="execution(* com.chj.service.UserServiceImpl.*(..))"/>
        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
```

java：

```java
public class AfterLog implements AfterReturningAdvice {
    //returnValue 返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了" + method.getName()+"方法，返回结果为"+returnValue);
    }
}
```

```java
public class Log implements MethodBeforeAdvice {
    //method :要执行的方法
    //args : 参数
    // target : 目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
}
```

测试：

```java
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();
    }
}
```

方式二：自定义来实现AOP【主要是切面定义】

xml：

```xml
<!--注册bean-->
    <bean id="userService" class="com.chj.service.UserServiceImpl"/>
    <bean id="log" class="com.chj.log.Log"/>
    <bean id="afterLog" class="com.chj.log.AfterLog"/>

    <!--方式二 自定义类：-->
    <bean id="diy" class="com.chj.diy.diy"/>
    <aop:config>
        <!--自定义切面，ref要引用的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.chj.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
```



java：

```java
public class diy {
    public void after(){
        System.out.println("结束执行了方法");
    }
    public void before(){
        System.out.println("开始执行了方法");

    }
}
```

测试：

```java
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();
    }
}
```

方式三：注解实现

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--注册bean-->
    <bean id="userService" class="com.chj.service.UserServiceImpl"/>
    <bean id="log" class="com.chj.log.Log"/>
    <bean id="afterLog" class="com.chj.log.AfterLog"/>
    <!--方式三-->
    <bean id="annotationPointCut" class="com.chj.diy.AnnotationPointCut"/>
    <aop:aspectj-autoproxy/>
</beans>
```

java：

```java
@Aspect//标注这个类是一个切面
public class AnnotationPointCut {

    @Before("execution(* com.chj.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("方法执行前");
    }
    @After("execution(* com.chj.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("方法执行后");
    }

    //在环绕增强种，我们可以给定一个参数，代表我们要获取处理切入的点
    @Around("execution(* com.chj.service.UserServiceImpl.*(..))")
    public void arount(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前");

        Signature signature = jp.getSignature();//获得签名
        System.out.println("signature:"+signature);
        //执行方法
        Object proceed = jp.proceed();
        System.out.println("环绕后");
        System.out.println(proceed);
    }
}
```

测试：

```java
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();
    }
}
```



# 12、整合Mybatis

步骤：

1.导入jar包

​		junit

​		mybatis

​		mysql

​		spring

​		aop

​		mybatis-spring

2.编写配置文件

3.测试



## 12.1、回忆Mybatis

1.编写实体类

```java
@Data
@Accessors
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

2.编写核心配置文件

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

3.编写接口

```java
public interface UserMapper {
    public List<User> selectUser();
}

```

4.编写Mapper.xml

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.chj.mapper.UserMapper">
    <select id="selectUser" resultType="com.chj.pojo.User">
        select * from user
    </select>
</mapper>
```

5. 测试

```java
public class Mytest {
    @Test
    public void selectUser() throws IOException {
        String path = "mybatis-config.xml";
        InputStream resourceAsStream = Resources.getResourceAsStream(path);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        System.out.println(mapper.selectUser());
        sqlSession.close();
    }
}
```



## 12.2、Mybatis-spring

### 方式一：

#### 1.编写数据源配置

```xml
<?xml version="1.0" encoding="utf8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--DataSource:使用Spring的数据源替换Mybatis的配置 -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://115.159.191.57:3306/zln?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT%2B8"/>
        <property name="username" value="root"/>
        <property name="password" value="chj0526"/>
    </bean>
</beans>
```

#### 2.SqlSessionFactory

```xml
 <!--SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定Mybatis配置文件-->
        <property name="configLocation" value="mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/chj/mapper/*.xml"/>
    </bean>
```

#### 3.SqlSessionTemplate

```xml
    <!--SqlSessionTemplate 就是我们使用SqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory ，因为它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
```



#### 4.需要给接口加实现类

接口

```java
public interface UserMapper {
    public List<User> selectUser();
}
```



接口实现类

```java
public class UserMapperImpl implements UserMapper {
    private SqlSessionTemplate sqlSessionTemplate;

    public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
        this.sqlSessionTemplate = sqlSessionTemplate;
    }

    public List<User> selectUser() {
        UserMapper mapper = sqlSessionTemplate.getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
```

xml

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.chj.mapper.UserMapper">
    <select id="selectUser" resultType="com.chj.pojo.User">
        select * from user
    </select>
</mapper>
```



#### 5.将自己写的实现类，注入到Spring 中

```xml
<bean id="userMapper" class="com.chj.mapper.UserMapperImpl">
    <property name="sqlSessionTemplate" ref="sqlSession"/>
</bean>
```

#### 6.测试使用即可

```java
public class Mytest {
    @Test
    public void selectUser() throws IOException {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapperImpl userMapper = context.getBean("userMapper", UserMapperImpl.class);
        System.out.println(userMapper.selectUser());
    }
}
```



### 方式二：

#### 1.编写数据源配置

```xml
<?xml version="1.0" encoding="utf8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--DataSource:使用Spring的数据源替换Mybatis的配置 -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://115.159.191.57:3306/zln?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT%2B8"/>
        <property name="username" value="root"/>
        <property name="password" value="chj0526"/>
    </bean>

    <!--SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定Mybatis配置文件-->
        <property name="configLocation" value="mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/chj/mapper/*.xml"/>
    </bean>
```

#### 2.实体类

```java
@Data
@Accessors
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

#### 3.需要给接口加实现类

接口

```java
public interface UserMapper {
    public List<User> selectUser();
}
```

实现类

```java
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
    public List<User> selectUser() {
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
```

xml

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.chj.mapper.UserMapper">
    <select id="selectUser" resultType="com.chj.pojo.User">
        select * from user
    </select>
</mapper>
```



#### 4.注册bean

```xml
<?xml version="1.0" encoding="utf8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
  <import resource="spring-dao.xml"/>

    <bean id="userMapper2" class="com.chj.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
</beans>
```

5.测试

```java
public class Mytest {
    @Test
    public void selectUser() throws IOException {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper2", UserMapper.class);
        System.out.println(userMapper.selectUser());
    }
}
```

# 13、声明式事务

## 1、回顾事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败！
- 事务在项目开发中，十分重要，涉及到数据的一致性问题，不能马虎！
- 确保完整性和一致性！



事务ACID原则

- 原子性
- 一致性
- 隔离性
  - 多个业务可能操作同一个资源，防止数据损坏
- 持久性
  - 事务一旦提交，无论系统发生什么问题，结果都不会再被影响，被持久化的写入存储器中！



## 2、Spring中的事务管理

- 声明式事务：AOP
- 编程式事务：需要在代码中，进行事务的管理

### mapper

```java
public interface UserMapper {
    List<User> selectUser();

    //添加一个用户
    Integer addUser(User user);

    //删除一个用户
    Integer deleteUser(@Param("id") Integer id);
}
```

```java
public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {



    public List<User> selectUser() {
        User user = new User(7, "小李", "21312");
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        addUser(user);
        deleteUser(7);
        return mapper.selectUser();
    }

    public Integer addUser(User user) {
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        return mapper.addUser(user);
    }

    public Integer deleteUser(Integer id) {
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        return mapper.deleteUser(id);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.chj.mapper.UserMapper">
    <select id="selectUser" resultType="com.chj.pojo.User">
        select * from user
    </select>

    <insert id="addUser" parameterType="com.chj.pojo.User">
        insert into user (id,name,pwd) values (#{id},#{name},#{pwd});
    </insert>

    <delete id="deleteUser" parameterType="int">
        deletes from user where id = #{id}
    </delete>
</mapper>
```

### Pojo

```java
@Data
@Accessors
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
```



### 配置

applicationContext.xml

```xml
<?xml version="1.0" encoding="utf8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
  <import resource="spring-dao.xml"/>

    <bean id="userMapper" class="com.chj.mapper.UserMapperImpl">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
</beans>
```

Spring-dao.xml

```xml
<?xml version="1.0" encoding="utf8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--DataSource:使用Spring的数据源替换Mybatis的配置 -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://115.159.191.57:3306/zln?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT%2B8"/>
        <property name="username" value="root"/>
        <property name="password" value="chj0526"/>
    </bean>

    <!--SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定Mybatis配置文件-->
        <property name="mapperLocations" value="classpath:com/chj/mapper/*.xml"/>
    </bean>
    <!--SqlSessionTemplate 就是我们使用SqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory ，因为它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
    <bean id="userMapper" class="com.chj.mapper.UserMapperImpl">
        <property name="sqlSessionTemplate" ref="sqlSession"/>
    </bean>

    
    <!--配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource"/>
    </bean>
    <!--结合AOP实现事务的织入-->
    <!--配置事务通知：-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给那些方法配置事务-->
        <!--配置事务的传播特性-->
        <tx:attributes>
            <tx:method name="add" />
            <tx:method name="delete"/>
            <tx:method name="update"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPonintCut" expression="execution(* com.chj.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPonintCut"/>
    </aop:config>
</beans>
```



为什么使用事务？

- 如果不配置事务，可能存在数据提交不一致的情况；
- 如果我们不在Spring中去配置声明式事务，我们就需要在代码中手动配置事务
- 事务在项目的开发中十分重要，设计到数据的一致性和完整性问题，不容马虎！
