# 1、简介

SSM框架：配置文件的。最好的方式：看官网文档；

## 1.1、什么是Mybaits？

- MyBatis是一款优秀的持久层框架，它支持定制化SQL、存储过程以及高级映射。MyBatis避免了几乎所有的JDBC代码。它可以使用简单的XML或注解来配置和映射原生类型、接口和JAVA的POLO为数据库中的记录

- 如何获得Mybatis？

- maven仓库:

```xml

```

- Github：https://github.com/mybatis/mybatis-3/releases

## 1.2、持久层

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程

- 数据库（jdbc）、io文件持久化

- 为什么需要持久化？

- 有一些对象不能让它丢掉

  

## 1.3、持久层

Dao层，Service层，Controller层

- 完成持久化工作的代码块
- 层界限是十分明显的



# 2、第一个MyBatis程序

思路：搭建环境-->导入Mybatis-->编写代码-->测试！

## 2.1、搭建环境

搭建数据库

- 创建项目（maven）
- 删除src目录
- 导入maven的依赖（mybatis、junit、sql）

```xml
    <dependencies>

        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>


        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>


        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

```

## 2.2、创建一个模块

- 编写mybatis的核心配置文件

```xml
<!--核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

- 编写mybatis的工具类

```java
//sqlSessionFactory-->sqlsession
public class MybatisUtils {
    private static  SqlSessionFactory sqlSessionFactory;

    static
    {

        try {
            //使用mybatis第一步：获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
    //你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句

    public static SqlSession getSqlSession()
    {
        return sqlSessionFactory.openSession();
    }
}

```

## 2.3、编写代码

- 实体类

```java
public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

- Dao接口

```java
public interface UserDao {
    List<User> getUserList();
}
```

- 接口实现类由原来的UserDaoImpl转换为一个Mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace=绑定一个对应的Mapper接口-->
<mapper namespace="com.shawn.dao.UserDao">

    <!--查询语句-->
    <select id="getUserList" resultType="com.shawn.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```

## 2.4、测试（junit）

```java
@Test
    public void test()
    {
        //第一步：获取sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //执行SQL
        //方式一:getMapper
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        List<User> userList = mapper.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭sqlsession
        sqlSession.close();
    }
```

- maven资源导出失败

```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
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



# 3、CRUD

## 3.1、namespace

namespace中的包名要和Dao/mapper接口的包名一致！

## 3.2、select

选择、查询语句：

- id：就是对应的namespace中的方法名
- resultType：Sql语句执行的返回值
- paramterType：参数的类型
- 增删改查必须要提交事务！！  sqlsession.commit() sqlsession.close()

1、编写接口

```java
//查询全部用户
    List<User> getUserList();

    //根据ID查询用户
    User getUserById(int id);

    //添加一个用户
    int addUser(User user);

    //修改一个用户
    int updateUser(User user);

    //删除一个用户
    int deleteUser(int id);

```

2、编写对应的SQL语句

```xml
    <!--查询语句-->
    <select id="getUserList" resultType="com.shawn.pojo.User">
        select * from mybatis.user
    </select>
    
    <select id="getUserById" parameterType="int" resultType="com.shawn.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>
    
    <insert id="addUser" parameterType="com.shawn.pojo.User">
        insert into mybatis.user(id,name,pwd) values(#{id},#{name},#{pwd})
    </insert>

    <update id="updateUser" parameterType="com.shawn.pojo.User">
        update mybatis.user set name=#{name},pwd=#{pwd} where id = #{id}
    </update>

    <delete id="deleteUser" parameterType="com.shawn.pojo.User">

        delete from mybatis.user where id = #{id}
    </delete>
```

3、测试

```java
	    @Test
    public void test()
    {
        //第一步：获取sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //执行SQL
        //方式一:getMapper
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = userMapper.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭sqlsession
        sqlSession.close();
    }

    @Test
    public void getUserById()
    {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        User user = mapper.getUserById(2);
        System.out.println(user);
        sqlSession.close();
    }

    @Test
    public void addUser()
    {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.addUser(new User(4,"赵家辉","666777"));
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void updateUser()
        {
            SqlSession sqlSession = MybatisUtils.getSqlSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            mapper.updateUser(new User(4,"咚咚咚","131313"));
            sqlSession.commit();
            sqlSession.close();
        }

        @Test
    public void deleteUser()
        {
            SqlSession sqlSession = MybatisUtils.getSqlSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            mapper.deleteUser(4);
            sqlSession.commit();
            sqlSession.close();
        }
```

## 3.3、insert

## 3.4、update

## 3.5、delete

## 3.6、万能的Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们可以适当使用map来存数据

```java
  //万能的Map
    int addUser2(Map<String,Object> map);
```

```xml
 <insert id="addUser2" parameterType="map">
        insert into mybatis.user (id,name,pwd) values(#{userid},#{username},#{password})

 </insert>
```

```java
     @Test
    public void addUser2()
        {
            SqlSession sqlSession = MybatisUtils.getSqlSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            Map<String, Object> map = new HashMap<String, Object>();
            map.put("userid",5);
            map.put("username","张艳萍");
            map.put("password","999999");
            mapper.addUser2(map);
            sqlSession.commit();
            sqlSession.close();
        }
```

# 4、配置解析

## 4.1、核心配置文件

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

## 4.2、环境配置（environments）

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

学会使用配置多套运行环境

Mybatis默认的事务管理是JDBC，连接池：pooled

## 4.3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置传递

【db.properties】

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true;useUnicode=true;characterEncoding=UTF-8
username=root
password=123456
```

在核心配置文件中引入

```xml
<!--核心配置文件-->
<configuration>


    <properties resource="db.properties" />
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}" />
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/shawn/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

## 4.4、别名

- 类型别名可为 Java 类型设置一个缩写名字
- 它仅用于 XML 配置，意在降低冗余的全限定类名书写。

```xml
<!--可以给实体类起别名-->
    <typeAliases>
        <typeAlias type="com.shawn.pojo.User" alias="User"></typeAlias>
    </typeAliases>
```

- 也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

## 4.5、设置

- 这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为

## 4.6、映射器

- MapperRegistry：注册绑定我们的mapper文件

```xml
    <mappers>
        <mapper resource="com/shawn/dao/UserMapper.xml"/>
    </mappers>
```

# 5、解决属性名和字段名不一致的问题

## 5.1、resultMap

- 结果集映射

```xml
<resultMap id="UserMap" type="User">
    <!--column对应数据库中的字段，property实体类中的属性-->
    <result column="id" property="id" />
    <result column="name" property="name" />
    <result column="pwd" property="password" />
</resultMap>
    <select id="getUserById" resultMap="UserMap">
           select * from user where id = #{id}
    </select>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

# 6、日志

## 6.1、日志工厂

如果一个数据库操作，出现了异常，我们需要拍错，日志就是最好的助手！

Mybatis 通过使用内置的日志工厂提供日志功能。内置日志工厂将会把日志工作委托给下面的实现之一：

- SLF4J
- Apache Commons Logging
- Log4j 2
- Log4j
- JDK logging

MyBatis 内置日志工厂会基于运行时检测信息选择日志委托实现。它会（按上面罗列的顺序）使用第一个查找到的实现。当没有找到这些实现时，将会禁用日志功能。

```xml
<configuration>
  <settings>
    ...
    <setting name="logImpl" value="LOG4J"/>
    ...
  </settings>
</configuration>
```

## 6.2、log4j（控制日志的输出）

1.导入jar包

```xml
<dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
</dependency>
```

2.log4j.properties在百度上找比较详细的配置文件

3.配置log4j为日志的实现

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

# 7、分页

## 7.1、使用limit分页

```sql
select * from ** limit startIndex,pageSize
```

## 7.2、使用mybatis实现分页，核心SQL

1.接口

```java
//分页
    List<User> getUserByLimit(HashMap<String, Object> map);
```

2.Mapper.xml

```xml
<select id="getUserByLimit" parameterType="map" resultMap="UserMap">
        select * from user limit #(startIndex),#(pageSize)
    </select>
```

3.测试

```java
@Test
    public void getUserByLimit()
    {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("startIndex",0);
        map.put("pageSize",2);
        List<User> userList = mapper.getUserByLimit(map);
        for (User user : userList) {
            System.out.println(user);
        }
    }
```

# 8、使用注解开发（只能简单使用）

## 8.1、面向接口编程（解耦） 

## 8.2、注解开发

# 9、lombok

## 9.1、在IDEA中安装Lombok插件

## 9.2、在项目中导入lombok的jar包

```xml
<!--lombok-->
        <!--https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.16</version>
            <scope>provided</scope>
        </dependency>
```

```java
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor    有参无参构造方法
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
experimental @var
@UtilityClass
```

# 10、多对一处理

## 10.1、测试环境搭建

- 导入lombok
- 新建实体类Teacher，Student
- 建立Mapper接口
- 建立Mapper.xml文件
- 在核心配置文件中绑定注册我们的Mapper接口或者文件
- 测试查询能否成功

## 10.2、按照查询嵌套处理

```xml
<!--namespace=绑定一个对应的Mapper接口-->
<mapper namespace="com.shawn.dao.StudentMapper">


    <!--
    1.查询所有的学生信息
    2.根据查询出来的学生的tid，寻找对应的老师
    3.复杂的属性我们需要单独处理 对象：association 集合：collection


    -->

    <select id="getStudent" resultMap="StudentTeacher" >
        select * from student
    </select>

    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id" />
        <result property="name" column="name" />
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resultMap>
    <select id="getTeacher" resultType="Teacher">
        select * from teacher where id = #{id}
    </select>
    
</mapper>
```

## 10.3、按照结果嵌套处理

# 11、一对多处理

## 11.1、一个老师拥有多个学生

```java
private List<> 集合
```

集合中的泛型类型在xml文件中用ofType而不是javaType

# 12、动态SQL

什么是动态SQL：动态SQL就是指根据不同的条件生成不同的SQL语句





利用动态 SQL，可以彻底摆脱这种痛苦。

```xml
如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

if
choose (when, otherwise)
trim (where, set)
foreach
```

## 12.1、搭建环境

**创建SQL中随机的ID而不是按照固定顺序的**

**需要编写一个工具类IDutils**

```java
public class IDutils
{
    public static String getId()
    {
        return UUID.randomUUID().toString().replaceAll("-","");
    }
}
```

**编写BlogMapper.xml文件**

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title = #{title}
  </if>
  <if test="author != null and author.name != null">
    AND author_name = #{author.name}
  </if>
</select>
```

## 12.2、choose (when, otherwise)

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>
```

## 12.3、trim(where、set)

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  WHERE
  <if test="state != null">
    state = #{state}
  </if>
  <if test="title != null">
    AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
    AND author_name like #{author.name}
  </if>
</select>
```

```xml
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>
```

### set

**用于动态更新语句的类似解决方案叫做 *set*。*set* 元素可以用于动态包含需要更新的列，忽略其它不更新的列。比如：**

```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```

### forEach

**动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）**

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```

动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以

建议现在Mysql中去测试代码，成功之后再去xml文件中拼接

# 13、缓存

**重点：开启日志，方便查看**

MyBatis 内置了一个强大的事务性查询缓存机制，它可以非常方便地配置和定制。 为了使它更加强大而且易于配置

## 13.1、一级缓存

默认情况下，只启用了本地的会话缓存，它仅仅对一个会话中的数据进行缓存。 要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：

```xml
<cache/>
```

## 13.2、二级缓存

 **缓存只作用于 cache 标签所在的映射文件中的语句。如果你混合使用 Java API 和 XML 映射文件，在共用接口中的语句将不会被默认缓存。你需要使用 @CacheNamespaceRef 注解指定缓存作用域。**

这些属性可以通过 cache 元素的属性来修改

```xml
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

##  13.3、自定义缓存

**你也可以通过实现你自己的缓存，或为其他第三方缓存方案创建适配器，来完全覆盖缓存行为。**

**首先就要导包！ 搜索Ehcache maven，然后在搜索框中输入mybatis-Ehcahce**

如何使用一个自定义的缓存实现。type 属性指定的类必须实现 org.apache.ibatis.cache.Cache 接口，且提供一个接受 String 参数作为 id 的构造器。 这个接口是 MyBatis 框架中许多复杂的接口之一，但是行为却非常简单。

```java
public interface Cache {
  String getId();
  int getSize();
  void putObject(Object key, Object value);
  Object getObject(Object key);
  boolean hasKey(Object key);
  Object removeObject(Object key);
  void clear();
}
```

为了对你的缓存进行配置，只需要简单地在你的缓存实现中添加公有的 JavaBean 属性，然后通过 cache 元素传递属性值，例如，下面的例子将在你的缓存实现上调用一个名为 `setCacheFile(String file)` 的方法：

```xml
<cache type="com.domain.something.MyCustomCache">
  <property name="cacheFile" value="/tmp/my-custom-cache.tmp"/>
</cache>
```