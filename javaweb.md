# 一、JavaWeb

## ServletContext

### 1.web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用；

（1）共享数据：设置存储数据

```java
ServletContext context = this.getServletContext();
String username = ""';
context.setAttribute("username",username); // 将数据保存在了servletContext中  里边是一个键值对
```

（2）共享数据：设置拿到数据

```java
ServletContext context = this.getServletContext();
String username = (String)context.getAttribute("username");
resp.setContentType("text/html");
resp.setCharacterEncoding("utf-8");
resp.getWriter().print("名字"+username);
```

### 2.ServletContext实现转发     请求页面不变，但内容会转发到另外一个网页从而显示另外一个网页的内容

```java
ServletContext context = this.getServletContext();
context.getRequestDispatcher("/**").forward(req,resp); //forward是转发的意思，**是在web.xml文件中的mapping路径
```

### 3.ServletContext实现读取properties资源文件

```java
InputStream is = this.getServletContext().getResourceAsStream("/***");
Properties prop = new Properties();
prop.load(is);
prop.getProperty("***");
resp.getWriter().print();
```

### 4.HttpServletResponseweb服务器接收到客户端的http请求

#### 4.1常见应用

```java
1.向浏览器输出信息

2.下载文件

（1）要获取下载文件的路径

this.getServletContext().getReadPath("/1.png");

（2）下载的文件名是啥

realPath.substring(realPath.lastIndexOf("") + 1);

（3）设置想办法让浏览器能够支持下载我们需要的东西

resp.setHeader("Content-Disposition","attachment;filename=" + fileName);   attachment是附件的意思

（4）获取下载文件的输入流

new FileInputStream （realPath）;

（5）创建缓冲区

int len = 0;

byte[]buffer = new byte[1024];

（6）获取OutputStream对象

resp.getOutputStream();

（7）将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端

while((len = in.read(buffer)) > 0)

{

out.write(buffer,0,len);

}

in.close();

out.close();

}
```

#### 4.2验证码功能

验证怎么来的

前端实现

后端实现，需要用到java的图片类

```java
//如何让浏览器五秒自动刷新一次；
resp.setHeader("refresh","3");   键值对
//如何在内存中创建一个图片
    BufferedImage image = new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
//得到图片
Graphics2D g = (Graphics2D)image.getGraphics(); // 笔
//设置图片的背景颜色
g.setColor(Color.white);
g.fillRect(0,0,80,20);
//给图片写数据
g.setColor(Color.blue);
g.setFont(new Font(null,Font.BOLD,20));
g.drawString(makeNum(),0,20);
//告诉浏览器，这个请求用图片的方式打开
      resp.setContentType("image/jpg");
//网站存在缓存，不让浏览器缓存
 resp.setDateHeader("expires",-1);
resp.setHeader("Chache-Control","no-cache");
resp.setHeader("pragma","no-cache");

//把图片写给浏览器
ImageIO.write(image,"jpg",resp.getOutputStream());

//生成随机数
private String makeNum()
{
    new random = new Random();
    String num = random.nextInt(99999999)+"";
    new StringBuffer(); //
    for(int i = 0;i<8-num.lenth();i++)
    {
        sb.append("");
    }
    num = sb.toString() + num;
    return num;
}
```

#### 4.3实现重定向

```java
resp.setHeader("Location","/index.jsp");
resp.setStatus(302);


//重定向的时候一定要注意，路径问题，否则404；
resp.sendRedirect("/index.jsp");    //重定向（实现转发）  url会发生变化  forward不会发生变化
```

#### 4.4在表单中的action提交路径中（重要）

```java
form action="${pageContext.request.contextPath}/*"    /*是web.xml中的mapping里的第二个      代表当前的项目  dollar符号里的
```

### 5.HttpServletRequest 代表客户端的请求

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息

#### 5.1获取前端请求的参数

```java
req.getParameter();
req.getParameterValues(); //获取多选框
```

#### 5.2请求转发

```java
req.getRequestDispather("/").forward(req,resp);    这里的/代表当前的web应用
```

##  Cookie、Session

### 6、会话（session）

会话：用户打开一个浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话

1.服务端给客户端一个 信件，客户端下次访问服务端带上信件就可以了    cookie

2.服务器等级你来过了，下次你来的时候我来匹配你   session

#### 6.1保存会话的两种技术

cookie

客户端技术（响应，请求）

session

服务器技术，利用这个技术，可以保存用户的会话信息？我们可以把信息或者数据放在session中

常见常例：网站登录之后，你下次不用再登录了，第二次访问直接登录上去

```java
//服务器告诉你你来的时间，把这个时间封装成为一个信件，你下次带来我就知道你了
     //解决中文乱码
  req.setCharacterEncoding("utf-8");
  resp.setCharacterEncoding("utf-8");

  resp.getWriter();

//cookie，服务器端从客户端获取
  req.getCookies();   // 这里返回数组，说明Cookie可能存在多个

//判断cookie 是否存在
  if(cookies!=null)
  {
      //如果存在怎么办
       out.write("你上一次访问的时间是：")
           for(int i = 0; i< cookies.length;i++)
           {
               Cookie cookie = cookies[i];
               //获取cookie的名字
               if(cookie.getName().equals("lastLoginTime"))
               {
                   //获取cookie中的值
                   Long.parseLong(cookie.getValue());
                   new Date()
                       out.write(date.toLocaleString());
               }
           }
  }else
  {
      out.write("这是第一次访问");
  }
//服务给客户端响应一个cookie:

new Cookie("lastLoginTime",System.currentTimeMillis()+"");   //只能存String
resp.addCookie(cookie);   //响应给客户端一个cookie

//cookie有效期为1天
cookie.setMaxAge(24*60*60)  

    
    cookie中文乱码问题
    
    
   编码解码：
    URLEncoder.encode("赵家辉","utf-8");
    URLDecoder.decode(cookie.getValue(),"utf-8");
```

session

服务器会给每一个用户（浏览器）创建一个session对象

一个session独占一个浏览器，只要浏览器没有关闭，这个session就存在

用户登录之后，每个网页都可以访问而不是每次访问一个网页都要登录验证---------保存信息（如购物车）



session和cookie的区别

cookie是把用户的数据写给用户的浏览器，浏览器保存

session把用户的数据写到用户独占session中，服务器端保存（重要的信息）

session对象由服务创建



session使用场景：

保存一个登录用户的信息

购物车信息

在整个网站中经常会使用的数据，我们将它帮存在session

```java
//解决乱码问题
resp.setCharacterEncoding("UTF-8");
req.setCharacterEncoding("UTF-8");
resp.setContentType("text/html;charset=utf-8");

//得到session
req.getSession();
//给session中存入东西
session.setAttribute("name","赵家辉");
//获取session的ID
new sessionId = session.getId();
//判断session是不是新创建
if(session.isNew())
{
    resp.getWriter().write("session创建成功,ID:"+sessionId);
}else
{
     resp.getWriter().write("session以前在服务器中已经存在了,ID:"+sessionId);
}

//session创建的时候做了什么事情
new Cookie("JESSIONID",sessionId);
resp.addCookie(cookie);








//解决乱码问题
resp.setCharacterEncoding("UTF-8");
req.setCharacterEncoding("UTF-8");
resp.setContentType("text/html;charset=utf-8");

//得到session
req.getSession();
//获取session的数据
session.getAttribute("name");
```

```xml
//设置session默认的失效时间(以分钟为单位
)
<session-config>
<session-timeout>15</session-timeout>
</seesion-config>
```

###  7.JSP

java Server Pages： Java服务器端页面，也和Servlet一样，用于动态web技术

最大的特点：

写JSP就像是在写HTML

区别：

HTML只给用户提供静态的数据

JSP页面中可以嵌入JAVA代码，为用户提供动态的数据

JSP本质上就是一个Servlet

#### 7.1JSP的基础语法

任何语言都有自己的语法，JSP作为Java技术的一种应用，它拥有一些自己扩充的语法，Java所有语法都支持

<%  这里可以直接写Java代码     输出为 out.println   %>

<%!  这里可以写web代码  %>

<%----%>为JSP的注释

#### 7.2JSP的指令

1.自己定制404、500的错误页面

```xml
<error-page>
    <error-code>404</error-codeerror-code>
    <location>/error/404.jsp</location>
</error-page
```

#### 7.3JSP的9大内置对象

1.PageContext

2.Request

3.Response

4.Session

5.Application   作用域在服务器 很大的一个作用域  服务器关闭它才失效

6.config

7.out

8.Page  不用了解

9.exception
我们要从pageContext取出，我们通过寻找的方式来进行  pageContext.findAttribute("");

scope是作用域的意思~

#### 7.4前端页面和后端页面实现转发跳转

前端 pageContext.forward("/***");

后端request.getRequestDispatcher("/***").forward(request.response);

### 7.5JSP标签、JSTL标签、EL表达式

用EL需要导入两个包

EL表达式：${}

获取数据

执行运算

获取web开发的常用对象

```jsp
<jsp:forward page="/****">
```

JSTL表达式

JSTL标签库的使用就是为了弥补HTML标签的不足；它自定义了许多的标签，可以供我们使用，标签的功能和java代码一样

核心标签：必须要引入一个头

在tomcat中也要引入JSTL的包，否则会报错：JSTL解析错误

```jsp
ArrayList<String> people = new ArrayList<>();
people.add();
<c:forEach var="people" items="${list}">
<c:out value="${people}" />   <br>
</c:forEach>
```

## 8.Java Bean

实体类

JavaBean有特定的写法：

必须要有一个无参构造

属性必须私有化

必须有对应的get/set方法

一般用来和数据库的字段做映射ORM

ORM：对象关系映射

表--->类

字段--->属性

行记录--->类的对象

## 9.Filter 过滤器

用来过滤网站的数据

处理中文乱码

### 9.1Filter开发步骤

1.导包

2.implements Filter 一定导servlet这个包  在java 类中

chain是链的意思，chain.doFilter（req，resp）；让我们的请求继续走

web服务器关闭的时候，过滤会销毁

3.在web.xml中配置filter过滤器

## 10.监听器

实现一个监听器的接口；（有N种）

1.编写一个监听器

2.实现监听器的接口

3.在web.xml中注册监听器

```java
//统计网站在线人数：  统计session
public class OnlineCountListener implements HttpSessionListener
{
    //一旦创建session就会触发这个事件
    public void sessionCreated(HttpSessionEvent se)
    {
        se.getSession().getServletContext();
        getAttrihbute("")
           if(***=null)
           {
               
           }else
           {
               
           }
        
        setAttribute("",);
        
    }
    public void sessionDestroyed(HttpSessionEvent se)
    {        
    }
}

```

## 11 事务

要么都成功，要么都失败

ACID原则是为了保证数据的安全

```java
1.开启事务
2.事务提交 commit（）
3.事务回滚 rollback（）
4.关闭事务
```

### 单元测试 Junit

1.添加相关依赖

2.@test注解 只要被加上就可以直接运行

##  12 smbms超市订单管理系统（实训）

###  1.项目搭建

1.搭建一个maven web项目

2.配置tomcat

3.测试项目能否能够跑起来

4.导入项目中会遇到的jar包

jsp，servlet，mysql，jstl，standard

5.创建项目包结构

6.编写实体类：表-类映射

7.编写基础公共类



