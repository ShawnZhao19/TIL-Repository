# 1、Spring

## 1.1、简介

- Spring：给软件行业带来了春天！            

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.2</version>
</dependency>
```

## 1.2、优点

- Spring是一个开源的免费的框架（容器）
- Spring是一个轻量级的、非入侵式的框架
- **控制反转（IOC），面向切面编程（AOP）**
- 支持事务的处理，对框架整合的支持

## 1.3、拓展

- Spring Boot
- 一个快速开发的脚手架
- 基于SpringBoot可以快速的开发单个微服务
- 约定大于配置





- Spring Cloud
- 是基于SpringBoot实现的



因为现在大多数公司都在使用SpringBoot进行快速的开发，学习SpringBoot的前提，需要完全掌握Spring以及SpringMVC！承上启下的作用！



# 2、IOC理论推导

1.UserDao接口

2.UserDaoImpl实现类

3.UserService业务接口

4.UserServiceImpl业务实现类

- 之前，程序是主动创建对象！控制权在程序员的受伤
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接收对象



这种思想，从本质上解决了问题，我们程序员不用再去管理对象的创建了



## 2.1、IOC本质

**控制反转IOC，是一种设计思想，Dl（依赖注入）是实现IoC的一种方法。**

**控制反转是一种通过描述（XML或者注解）并通过第三方去生产或获取特定对象的方式，在Spring中实现控制反转的IoC容器，其实现方法是依赖注入**



## 2.2、HelloSpring

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
    id=变量名
    class = new 的对象
    property 相当于给对象的属性赋值
    -->
    <bean id="hello" class="com.shawn.pojo.Hello" >
        <!--ref: 引用Spring容器中创建好的对象-->
        <property name="str" value="Spring" ref="/"/>
    </bean>

</beans>
```

**实体类.java**

```java
package com.shawn.pojo;

public class Hello {

    private String str;

    public Hello() {
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    public Hello(String str) {
        this.str = str;
    }
}

```

测试类.java

```java
import com.shawn.pojo.Hello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //获取Spring的上下文对象！  获取ApplicationContext对象，拿到Spring的容器
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在Spring中的管理了，我们要使用，直接去里面取出来就可以！
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.toString());
    }
}

```

# 3、IOC创建对象的方式

## 1.使用无参构造创建对象，默认

```java
package com.shawn.pojo;

public class User {
    private String name;

    public User() {
        System.out.println("User的无参构造");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("name=" + name);
    }
}
```

## 2.使用有参构造来创建对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--第一种，下标来赋值-->
    <bean id="user" class="com.shawn.pojo.User" >
        <constructor-arg index="0" value="肖恩呀" />
    </bean>
    
    <!--第二种，直接通过参数名来赋值-->
    <bean id="user" class="com.shawn.pojo.User" >
        <constructor-arg name="name" value="肖恩哦" />
    </bean>
</beans>
```



**总结：在配置文件加载的时候，容器中管理的对象就已经初始化了！**

# 4、Spring的配置

## 4.1、别名（alias）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--第二种，直接通过参数名来赋值-->
    <bean id="user" class="com.shawn.pojo.User" >
        <constructor-arg name="name" value="肖恩哦" />
    </bean>

    <!--别名，如果添加了别名，我们也可以通过别名来获取这个对象-->
    <alias name="user" alias="userNew" />
</beans>
```

## 4.2、Bean的配置

```xml
    <!--
    id:bean的唯一标识符，相当于对象名字
    class:bean对象所对应的全限定名：包名+类型
    name：也是别名，可以同时取多个别名
    -->
    <bean id="userT" class="com.shawn.pojo.User" name="user2" />
```

## 4.3、import

一般用于团队开发使用，他可以将多个配置文件，导入合并为一个

# 5、Di依赖注入

## 5.1、构造器注入

前面已经学过了

## 5.2、set注入（重点）

- 依赖注入：set注入！
  - 依赖：bean对象的创建依赖于容器！
  - 注入：bean对象中的所有属性，由容器来注入！

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="student" class="com.shawn.pojo.Student">
        <!--第一种，普通值注入，value-->
        <property name="name" value="肖恩" />
    </bean>

</beans>
```

```java
package com.shawn.pojo;

import java.util.*;

public class Student {

    private String name;
    private Address address;
    private String[] book;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;
```

```java
package com.shawn.pojo;

public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

```

测试类.java

```java
import com.shawn.pojo.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {

    public static void main(String[] args) {
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student.getName());
    }
}
```

### 5.2.1、完善以后

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.shawn.pojo.Address">
        <property name="address" value="西安" />
    </bean>

    <bean id="student" class="com.shawn.pojo.Student">
        <!--第一种，普通值注入，value-->
        <property name="name" value="肖恩" />
        <!--第一种，bean注入，ref-->
        <property name="address" ref="address" />

        <!--数组注入-->
        <property name="book">
            <array>
                <value>红楼梦</value>
                <value>三国</value>
                <value>西游记</value>
                <value>水浒传</value>
            </array>
        </property>

        <!--List注入-->
        <property name="hobbys">
            <list>
                <value>看电影</value>
                <value>唱歌</value>
                <value>敲代码</value>
                <value>刷抖音</value>
            </list>
        </property>

        <!--Map注入-->
        <property name="card">
            <map>
                <entry key="身份证" value="11111111111115555" />
                <entry key="银行卡" value="21321312312312312" />
            </map>
        </property>

        <!--Set注入-->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>BOB</value>
                <value>COC</value>
            </set>
        </property>

        <!--Null值注入-->
        <property name="wife">
            <null />
        </property>

        <!--properties注入-->
        <property name="info">
            <props>
                <prop key="学号">22229999911</prop>
                <prop key="性别">男性</prop>
                <prop key="姓名">小恩恩</prop>
            </props>
        </property>

    </bean>

</beans>
```

测试类.java

```java
import com.shawn.pojo.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {

    public static void main(String[] args) {
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student.toString());


        /*
        Student{
        name='肖恩',
        address='西安',
        book=[红楼梦, 三国, 西游记, 水浒传],
        hobbys=[看电影, 唱歌, 敲代码, 刷抖音],
        card={身份证=11111111111115555, 银行卡=21321312312312312},
        games=[LOL, BOB, COC],
        wife='null', i
        nfo={学号=22229999911,
        性别=男性,
        姓名=小恩恩}}


         */
    }
}
```

## 5.3、拓展注入

### 5.3.1、具有p-命名空间的XML快捷方式

```xml
<!--p命名空间注入，可以直接注入属性的值：property-->
xmlns:p="http://www.springframework.org/schema/p"
```

### 5.3.2、具有c-namespace的XML快捷方式

```xml
<!--c命名空间注入，通过构造器注入：construct-args-->
xmlns:c="http://www.springframework.org/schema/c"
```

# 6、Bean的自动装配

- 自动装配是Spring满足bean依赖一种方式！
- Spring会在上下文中自动寻找，并自动给bean装配属性！



在Spring中有三种装配的方式

1.在xml中显示的配置

2.在Java中显示配置

**3.隐式的自动装配【重要】**

## 6.1、测试

环境搭建

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans  xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <bean id="cat" class="com.shawn.pojo.Cat" />
        <bean id="dog" class="com.shawn.pojo.Dog" />
        <bean id="people" class="com.shawn.pojo.People" >
        
        <property name="name" value="小小嗯" />
        <property name="dog" ref="dog" />
        <property name="cat" ref="cat" />
    </bean>

</beans>
```

实体类.java

```java
package com.shawn.pojo;

public class Dog {
    public void shot()
    {
        System.out.println("汪汪汪");
    }
}
```

```java
package com.shawn.pojo;

public class Cat {
    public void shot()
    {
        System.out.println("喵喵喵");
    }
}
```

测试类.java

```java
import com.shawn.pojo.People;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MyTest {
    @Test
    public void test1()
{
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    People people = (People) context.getBean("people");
    people.getDog().shot();
    people.getCat().shot();
}
}
```

## 6.2、ByName自动装配

```xml
 <bean id="people" class="com.shawn.pojo.People" autowire="byName">

        <!--
        byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean Id！
        -->
        
        <property name="name" value="小小嗯"/>
    </bean>
```

## 6.3、ByType自动装配

```xml
    <bean id="people" class="com.shawn.pojo.People" autowire="byType">

        <!--
        byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean Id！
        byType:会自动在容器上下文中查找，和自己对象属性类型相同的bean！
        -->
        
        <property name="name" value="小小嗯"/>
    </bean>
```

## 6.4、使用注解实现自动装配

**基于注释的配置的引入提出了一个问题，即这种方法是否比XML“更好”。简短的答案是“取决于情况”。长远的答案是每种方法都有其优缺点，通常，由开发人员决定哪种策略更适合他们。**

1.导入约束。context约束

2.配置注解的支持

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
         https://www.springframework.org/schema/aop/spring-aop.xsd">

    <context:annotation-config />
</beans>
```



**@AutoWired**

直接在属性上使用即可！也可以在set方式上使用！

使用Autowired我们可以不用编写Set方法了，前提是你这个自动装配的属性在Ioc（Spring）容器中存在，且符合名字byName!

如果**@AutoWired**自动装配的环境比较复杂，自动装配无法通过一个注解【**@AutoWired**】完成的时候，我们可以使用**@Qualifier**（value=“xxxxx”）去配合**@AutoWired**来使用一个唯一的bean对象注入



@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求这个对象存在！
- @Resource默认通过byName的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！



# 7、Spring注解开发

**在Spring4之后，使用注解开发必须配置AOP的jar包**

使用注解需要导入context约束，增加注解支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
         https://www.springframework.org/schema/aop/spring-aop.xsd">
    <context:annotation-config />


</beans>
```

1.bean

```java
@Component
public class User {
```

```xml
<context:component-scan base-package="com.shawn.pojo" />
```

2.属性如何注入

```java
public class User {

    @Value("小小嗯")
    public String name;
}
```

3.衍生的注解

@Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层~

- dao 【@Repository】
- service 【@Service】
- controller 【@Controller】

功能都是一样的都是将某个类注册到Spring中来装配

4.自动装配置

5.作用域

```java
@Scope("") 单例模式或者原生模式
```



# 8、使用Java的方式配置Spring

我们现在要完全不使用Spring的xml配置了，全权交给Java来做

JavaConfig是Spring的一个子项目，它是核心功能！

```java
package com.shawn.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;


//这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中
@Component
public class User {
    public String name;
    @Value("小小嗯")
    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

```java
package com.shawn.config;
import com.shawn.pojo.User;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

//这个也会Spring容器托管，注册到容器中，因为它本来就是一个@Component
//这个@Configuration代表这是一个配置类，就是我们之前看的beans.xml
@Configuration
public class shawnConfig {

    //注册一个bean，相当于我们之前写的一个bean标签
    //这个方法的名字，就相当于bean标签中的id属性
    //这个方法的返回值，就相当于bean标签中的class属性
    @Bean
    public User getUser()
    {
        return new User();//就是返回要注入到bean的对象！
    }
}
```

测试类.java

```java
import com.shawn.config.shawnConfig;
import com.shawn.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //如果完全使用了配置类的方式去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象来加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(shawnConfig.class);
        User getUser = (User) context.getBean("getUser");
        System.out.println(getUser.getName());
    }
}
```

# 9、代理模式

**为什么要学习代理模式？因为这就是SpringAOP的底层！ 【SpringAOP 和 SpringMVC】**面试

代理模式的分类：

## 9.1、静态代理

代码步骤：

1.接口

```java
package com.shawn.demo01;

//租房
public interface Rent {

    public void rent();
}
```

2.真实角色

```java
package com.shawn.demo01;

//房东
public class Host implements Rent{


    @Override
    public void rent() {
        System.out.println("房东要出租房子");
    }
}
```

3.代理角色

```java
package com.shawn.demo01;

public class Proxy implements Rent{
    private Host host;

    public Proxy() {
    }

    public Proxy(Host host) {
        this.host = host;
    }

    @Override
    public void rent() {
        seeHouse();
        host.rent();
        hetong();
        fare();

    }

    //看房
    public void seeHouse()
    {
        System.out.println("中介带你看房");
    }
    //收中介费
    public void fare()
    {
        System.out.println("收中介费");
    }
    //签合同
    public void hetong()
    {
        System.out.println("签租赁合同");
    }
}

```

4.客户端访问代理角色

```java
package com.shawn.demo01;

public class Client {
    public static void main(String[] args) {
        //房东要租房子
        Host host = new Host();
        //代理，中介帮房东租房子，代理角色一般会有一些附属操作！
        Proxy proxy = new Proxy(host);

        //你不用面对房东，直接找中介租房即可
        proxy.rent();
    }
}
```

另外一个实例！！！

```java
package com.shawn.demo2;

public class UserServiceProxy implements UserService{

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    UserServiceImpl userService;

    public void add() {

        log("add");
        userService.add();

    }

    public void delete() {
        log("delete");
        userService.add();
    }

    public void update() {
        log("update");
        userService.add();
    }

    public void query() {
        log("query");
        userService.add();
    }

    //日志方法
    public void log(String msg)
    {
        System.out.println("使用了"+msg+"方法");
    }

}
```

```java
package com.shawn.demo2;

//真实对象
public class UserServiceImpl implements UserService{

    public void add()
    {
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

```java
package com.shawn.demo2;

public class Client {

    public static void main(String[] args) {

        UserServiceImpl userService = new UserServiceImpl();
        UserServiceProxy proxy = new UserServiceProxy();
        proxy.setUserService(userService);
        proxy.add();
    }
}
```

## 9.2、动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的！
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口--JDK动态代理 【我们在这里使用】
  - 基于类：cglib
  - java字节码实现：javassist（重要）



需要了解两个类： proxy；invocationHandler

**invocationHandler**

```java
package com.shawn.demo04;

import com.shawn.demo03.Rent;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

//我们用这个类自动生成代理类
public class ProxyInvocationHandler implements InvocationHandler {

    public void setTarget(Object target) {
        this.target = target;
    }

    //被代理的接口
    private Object target;

    //生成得到代理类
    public Object getProxy()
    {
       return  Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this );
    }
    //处理代理实例并返回结果:
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        log(method.getName());
        //动态代理的本质，就是使用反射机制实现的！
        Object result = method.invoke(target, args);
        return  result;
    }

    public void log(String msg)
    {
        System.out.println("执行了"+msg+"的方法");
    }
}
```

```java
package com.shawn.demo04;

import com.shawn.demo2.UserService;
import com.shawn.demo2.UserServiceImpl;

public class Client {
    public static void main(String[] args) {
        //真实角色
        UserServiceImpl userService = new UserServiceImpl();
        //代理角色，不存在

        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        pih.setTarget(userService);//设置要代理的对象
        //动态生成代理类
        UserService proxy = (UserService) pih.getProxy();
        proxy.delete();
    }
}
```

# 10、AOP

1.**导入依赖**

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
    <scope>runtime</scope>
</dependency>
```



方式一：使用Spring的API接口【主要SpringAIP接口实现】

方式二：使用自定义来实现AOP【主要是切面定义】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
">


    <!--注册bean-->
    <bean id="userService" class="com.shawn.service.UserServiceImpl" />
    <bean id="log" class="com.shawn.log.Log" />
    <bean id="afterLog" class="com.shawn.log.AfterLog" />

<!--方式三-->
    <bean id="annotationPointCut" class="com.shawn.diy.AnnotationPointCut"/>
<!--开启注解支持-->
    <aop:aspectj-autoproxy/>
    <!--方式一：使用原生Spring API接口-->
    <!--配置aop:需要导入aop的约束-->
    <!--    <aop:config>-->
    <!--        &lt;!&ndash;切入点：expression:表达式,execution(要执行的位置！******)&ndash;&gt;-->
    <!--        <aop:pointcut id="pointcut" expression="execution(* com.shawn.service.UserServiceImpl.*(..))"/>-->

    <!--        &lt;!&ndash;执行环绕增加！&ndash;&gt;-->
    <!--        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>-->
    <!--        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut" />-->
    <!--    </aop:config>-->

    <!--方式二：自定义类-->
    <bean id="diy" class="com.shawn.diy.DiyPointCut"/>
    <aop:config>
        <!--自定义切面，ref为要引用的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <!--execution表达式第一个*号代表返回值类型为所有，一般如此；第二个为包名到类名，service后可以为*表示为所有类，后边*（。。）表示为类的所有方法-->
            <aop:pointcut id="point" expression="execution(* com.shawn.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>

</beans>
```

```java
package com.shawn.diy;

//方法三：使用注解方式实现AOP

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
public class AnnotationPointCut {

    @Before("execution(* com.shawn.service.UserServiceImpl.*(..))")
    public void before()
    {
        System.out.println("=======方法执行前=======");
    }
    @After("execution(* com.shawn.service.UserServiceImpl.*(..))")
    public void after()
    {
        System.out.println("=======方法执行后=======");
    }

    //在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点;
    @Around("execution(* com.shawn.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {

        System.out.println("========环绕前===========");

        Signature signature = jp.getSignature();//获得签名
        System.out.println("signature:" + signature);
    //执行方法
        Object proceed = jp.proceed();

        System.out.println("环绕后");

        System.out.println(proceed);
    }
}
```

```java
import com.shawn.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        //动态代理代理的是接口：注意点
        UserService userService = (UserService) context.getBean("userService");
        userService.add();
    }
}
```



# 11、整合Mybatis

步骤：

1.导入相关jar包

- junit

```xml
<dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>
```

- mybatis

```xml
<dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>
```

- mysql数据库

```xml
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
```

- spring相关的

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.2</version>
</dependency>
<!--Spring操作数据库的话，还需要一个spring-jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>4.3.12.RELEASE</version>
        </dependency>
```

- aop织入

```xml
<dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.6</version>
        </dependency>
```

- mybatis-spring【new】

```xml
<dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.6</version>
        </dependency>
```

- lombok

```xml
<dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.16</version>
        </dependency>
```

2.编写配置文件

3.测试

## 11.1、回忆mybatis

## 11.2、Mybatis-spring

1.编写数据源配置

2.sqlSessionFactory

3.sqlSessionTemplate

4.需要给接口添加实现类

5.将自己写的实现类注入到Srping中

6.测试使用

# 12、声明式事务

- 要么都成功，要么都失败
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题
- 确保完整性和一致性



## 12.1、Spring中的事务管理

- 声明式事务：AOP（一般需要使用）
- 编程式事务：

