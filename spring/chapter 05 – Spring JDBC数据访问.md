## Spring JDBC 数据访问

相关源代码：（创作不易，点个star呗，感谢🙏）<https://github.com/MicahKong/spring_source_code>

**Spring知识精简版梳理系列**

- chapter 01 – Spring概述  
  <https://blog.csdn.net/qq_35843514/article/details/114287046?spm=1001.2014.3001.5501>
- chapter 02 – Spring IoC和DI <https://blog.csdn.net/qq_35843514/article/details/114291994?spm=1001.2014.3001.5501>
- chapter 03 – Spring Beans <https://blog.csdn.net/qq_35843514/article/details/114299601?spm=1001.2014.3001.5501>
- chapter 04 – Spring AOP <https://blog.csdn.net/qq_35843514/article/details/114358536?spm=1001.2014.3001.5501>
- chapter 05 – Spring JDBC数据访问 <https://blog.csdn.net/qq_35843514/article/details/114436797?spm=1001.2014.3001.5501>
- chapter 06 – Spring 事务管理 <https://blog.csdn.net/qq_35843514/article/details/114442323?spm=1001.2014.3001.5501>  
  chapter 07 – Spring 5新特性 <https://blog.csdn.net/qq_35843514/article/details/114547076>

### 文章目录

- - [Spring JDBC 数据访问](#Spring_JDBC__0)
  - - [一、Spring JDBC概述](#Spring_JDBC_15)
    - - [1、什么是JDBC Template？](#1JDBC_Template_17)
      - [2、Spring JDBC 能做什么？](#2Spring_JDBC__25)
      - [3、JDBC访问数据的方法](#3JDBC_39)
      - [4、Spring框架的JDBC抽象框架由四个不同的软件包组成](#4SpringJDBC_47)
    - [二、使用JBDC Template](#JBDC_Template_60)
    - - [1、新建Module \`chapter05\_jdbc\_template\`，并引入相关jar包](#1Module_chapter05_jdbc_templatejar_61)
      - [2、配置数据库链接池](#2_65)
      - - [（1）代码配置](#1_67)
        - [（2）xml配置](#2xml_76)
      - [3、创建Service和Dao，并在Dao中注入JDBC Template对象](#3ServiceDaoDaoJDBC_Template_103)
      - - [（1）添加数据](#1_224)
        - [（2）修改数据和删除数据](#2_228)
        - [（3）返回某个具体的值](#3_231)
        - [（4）返回一个对象](#4_235)
        - [（5）查询返回集合](#5_241)
        - [（6）实现批量操作](#6_246)

### 一、Spring JDBC概述

#### 1、什么是JDBC Template？

之前我们采用的是手动封装JdbcTemplate，其好处是通过\(sql语句+参数\)模板化了编程。而真正的JdbcTemplate类，是Spring[框架](https://so.csdn.net/so/search?q=%E6%A1%86%E6%9E%B6&spm=1001.2101.3001.7020)为我们写好的。它是 Spring 框架中提供的一个对象，是对原始 Jdbc API 对象的简单封装。除了JdbcTemplate，Spring 框架还为我们提供了很多的操作模板类。

`操作关系型数据的`：JdbcTemplate和HibernateTemplate。  
`操作 nosql 数据库的`：RedisTemplate。  
`操作消息队列的`：JmsTemplate。

#### 2、Spring JDBC 能做什么？

| 操作项目                     | Spring帮你做的 | 你需要做的 |
| ---------------------------- | -------------- | ---------- |
| 定义连接参数                 |                | ✔          |
| 打开连接                     | ✔              |            |
| 指定SQL语句                  |                | ✔          |
| 声明参数并提供参数值         |                | ✔          |
| 准备并运行该语句             | ✔              |            |
| 设置循环以遍历结果（如果有） | ✔              |            |
| 进行每次迭代的工作           |                | ✔          |
| 处理任何异常                 | ✔              |            |
| 处理交易                     | ✔              |            |
| 关闭连接，语句和结果集       | ✔              |            |

#### 3、JDBC访问数据的方法

- `JdbcTemplate`：是经典且最受欢迎的Spring  
  JDBC方法。这种“最低级别”的方法以及其他所有方法都在幕后使用JdbcTemplate。
- `NamedParameterJdbcTemplate`：封装JdbcTemplate以提供命名参数，而不是传统的JDBC ‘\?’ 占位符。当您有多个SQL语句参数时，此方法可提供更好的文档编制和易用性。
- `SimpleJdbcInsert`和`SimpleJdbcCall`：优化数据库元数据以限制必要的配置量。这种方法简化了编码，因此您只需要提供表或过程的名称，并提供与列名称匹配的参数映射即可。仅当数据库提供足够的元数据时，此方法才有效。如果数据库不提供此元数据，则必须提供参数的显式配置。
- `RDBMS对象`：（包括MappingSqlQuery，SqlUpdate和StoredProcedure）要求您在数据访问层初始化期间创建可重用且线程安全的对象。此方法以JDO Query为模型，其中您定义查询字符串，声明参数并编译查询。一旦你这样做，execute\(…\)，update\(…\)，和findObject\(…\)方法可以用不同的参数值多次调用。

#### 4、Spring框架的JDBC抽象框架由四个不同的软件包组成

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306141000348.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)

- `core：`：org.springframework.jdbc.core程序包包含JdbcTemplate该类及其各种回调接口，以及各种相关类。org.springframework.jdbc.core.simple包含SimpleJdbcInsert和  
  SimpleJdbcCall类。另一个名为org.springframework.jdbc.core.namedparam的子程序包包含NamedParameterJdbcTemplate 该类和相关的支持类。
- `datasource`：该org.springframework.jdbc.datasource软件包包含一个易于DataSource访问的实用程序类  
  和各种简单的DataSource实现，可用于在Java  
  EE容器之外测试和运行未修改的JDBC代码。org.springfamework.jdbc.datasource.embedded支持使用Java数据库引擎（例如HSQL，H2和Derby）创建嵌入式数据库。
- `object`：org.springframework.jdbc.object程序包包含将RDBMS查询，更新和存储过程表示为线程安全的可重用对象的类。尽管查询返回的对象自然会与数据库断开连接，但此方法由JDO建模。较高级别的JDBC抽象取决于org.springframework.jdbc.core程序包中的较低级别的抽象。
- `support`：org.springframework.jdbc.support提供了SQLException翻译功能和一些实用程序类。JDBC处理期间引发的异常将转换为org.springframework.dao包中定义的异常。这意味着使用Spring JDBC抽象层的代码不需要实现JDBC或RDBMS特定的错误处理。所有翻译的异常均未选中，这使您可以选择捕获可从中恢复的异常，同时将其他异常传播到调用方。

### 二、使用JBDC Template

#### 1、新建Module `chapter05_jdbc_template`，并引入相关jar包

在源代码里提供了相关jar包，源代码：<https://github.com/MicahKong/spring_source_code>  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306141522128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)

#### 2、配置数据库链接池

新建`bean1.xml`, 配置数据库连接池，并配置JDBC Template对象，注入datasource；最后开启组件扫描

##### （1）代码配置

```java
DriverManagerDataSource dataSource = new DriverManagerDataSource();
dataSource.setDriverClassName("org.hsqldb.jdbcDriver");
dataSource.setUrl("jdbc:hsqldb:hsql://localhost:");
dataSource.setUsername("sa");
dataSource.setPassword("");
```

##### （2）xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--开启组件扫描-->
    <context:component-scan base-package="com.micah"/>

    <!-- 数据库连接池 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          destroy-method="close">
        <property name="url" value="jdbc:mysql:///user_db" />
        <property name="username" value="root" />
        <property name="password" value="root" />
        <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    </bean>

    <!--创建jdbcTemplate对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!--注入-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

#### 3、创建Service和Dao，并在Dao中注入JDBC Template对象

```java
/**
 * @Author m.kong
 * @Date 2021/2/28 下午9:32
 * @Version 1.0
 */
public class Book {
    private int userId;
    private String username;
    private String userStatus;

    public int getUserId() {
        return userId;
    }

    public void setUserId(int userId) {
        this.userId = userId;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getUserStatus() {
        return userStatus;
    }

    public void setUserStatus(String userStatus) {
        this.userStatus = userStatus;
    }
}
```

```java
/**
 * @Author m.kong
 * @Date 2021/2/28 下午9:19
 * @Version 1.0
 */
@Service
public class BookService {
    @Autowired
    private BookDao bookDao;

    public void addBook(Book book){
        bookDao.add(book);
    }

    public void updateBook(Book book){
        bookDao.update(book);
    }

    public void deleteBook(String id){
        bookDao.deleteBook(id);
    }

    public void selectCount(){
        int count = bookDao.selectCount();
        System.out.println(count);
    }

    public void batchAddBook(List<Object[]> books){
        bookDao.batchAddBook(books);
    }

    public void batchUpdateBook(List<Object[]> books) {
        bookDao.batchUpdateBook(books);
    }

    public void batchDelBook(List<Object[]> ids) {
        bookDao.batchDelBook(ids);
    }
}

```

```java
/**
 * @Author m.kong
 * @Date 2021/2/28 下午9:20
 * @Version 1.0
 */
public interface BookDao {
    /**
     * 添加add
     */
    void add(Book book);
    /**
     * update
     */
    void update(Book book);

    /**
     * delete
     */
    void deleteBook(String id);

    /**
     * count(*)
     */
    int selectCount();

    /**
     * batchAddBooks
     * @param books
     */
    void batchAddBook(List<Object[]> books);

    void batchUpdateBook(List<Object[]> books);

    void batchDelBook(List<Object[]> ids);
}

```

##### （1）添加数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306142458805.png)  
第一个参数：sql语句  
第二个参数：\@Nullable允许为空，设置SQL语句中的参数值

##### （2）修改数据和删除数据

方法同上

##### （3）返回某个具体的值

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306142952565.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)  
第一个参数是SQL语句，没什么可多说的，第二个参数是返回值的Class类型，如果是String类型，就是String.class

##### （4）返回一个对象

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306143308919.png)  
⚫ 第一个参数：sql语句  
⚫ 第二个参数：RowMapper 是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装  
⚫ 第三个参数：sql语句值

##### （5）查询返回集合

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306143548748.png) ⚫ 第一个参数：sql语句  
⚫ 第二个参数：RowMapper 是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装  
⚫ 第三个参数：sql语句值

##### （6）实现批量操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306143701919.png)  
第一个参数是SQL语句，第二个参数是要添加的集合数据

```java
/**
 * @Author m.kong
 * @Date 2021/2/28 下午9:20
 * @Version 1.0
 */
@Repository
public class BookDaoImpl implements BookDao{
    /**
     * 批量添加数据
     * @param books
     */
    @Override
    public void batchAddBook(List<Object[]> books) {
        String sql = "Insert into t_user values(?,?,?)";
        int[] res = jdbcTemplate.batchUpdate(sql, books);
        System.out.println(Arrays.toString(res));
    }

    /**
     * 批量删除
     */
    @Override
    public void batchDelBook(List<Object[]> ids) {
        String sql = "DELETE FROM t_user WHERE user_id = ?";
        int[] res = jdbcTemplate.batchUpdate(sql, ids);
        System.out.println(Arrays.toString(res));
    }

    /**
     * 批量更新数据
     * @param books
     */
    @Override
    public void batchUpdateBook(List<Object[]> books) {
        String sql = "Update t_user set username = ?,ustates = ? where user_id = ?";
        int[] res = jdbcTemplate.batchUpdate(sql, books);
        System.out.println(Arrays.toString(res));
    }

    /**
     * 注入JdbcTemplate
     */
    @Autowired
    private JdbcTemplate jdbcTemplate;

    /**
     * 添加方法
     * @param book 书
     */
    @Override
    public void add(Book book) {
        Object[] args = {book.getUserId(),book.getUsername(),book.getUserStatus()};
        int update = jdbcTemplate.update("insert into t_user values (?,?,?)", args);
        System.out.println("update successful!" + update);
    }

    @Override
    public void update(Book book) {
        Object[] args = {book.getUsername(),book.getUserStatus(),book.getUserId()};
        int update = jdbcTemplate.update("update t_user set username = ?,ustates = ? where user_id = ?", args);
        System.out.println("update successful!" + update);
    }

    @Override
    public void deleteBook(String id) {
        int update = jdbcTemplate.update("delete from t_user  where user_id = ?", id);
        System.out.println("delete successful!" + update);
    }

    @Override
    public int selectCount() {
        String sql = "select count(*) from t_user";
        int count = jdbcTemplate.queryForObject(sql,Integer.class);
        return count;
    }
}
```