## chapter01 - Spring概述

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

- - [chapter01 - Spring概述](#chapter01__Spring_0)
  - - [一、什么是Spring？](#Spring_16)
    - - [1、==什么是spring\?（面试题）==](#1spring_17)
      - - [注：POJO和JavaBean的区别](#POJOJavaBean_22)
      - [2、==Spring有哪些优点？（面试题）==](#2Spring_27)
      - [3、Spring 有两个核心部分：IoC 和AOP](#3Spring_IoC_AOP_43)
      - [4、Spring 特点](#4Spring__47)
      - [5、目前使用Spring 版本 5.x](#5Spring__5x_55)
      - [6、==Spring架构图，Spring由哪些模块组成？（面试题）==](#6SpringSpring_57)
    - [二、第一个Spring项目](#Spring_64)
    - - [1、下载Spring，目前版本5.3.4](#1Spring534_66)
      - [2、打开Idea创建一个Java工程，并新建一个普通的Java Model](#2IdeaJavaJava_Model_71)
      - [3、进入下载的Spring压缩包下面的libs目录中，将其中的核心jar包导入项目](#3Springlibsjar_73)
      - [4、创建一个\`User.java\`类](#4Userjava_76)
      - [5、在src目录下创建Spring配置文件，\`bean1.xml\`](#5srcSpringbean1xml_90)
      - [6、测试，\`BeanTest.java\`](#6BeanTestjava_104)

### 一、什么是Spring？

#### 1、什么是spring\?（面试题）

Spring是一个开源的Java EE开发[框架](https://so.csdn.net/so/search?q=%E6%A1%86%E6%9E%B6&spm=1001.2101.3001.7020)。Spring框架的核心功能可以应用在任何Java应用程序中，但对Java EE平台上的Web应用程序有更好的扩展性。Spring框架的目标是使得Java EE应用程序的开发更加简捷，通过使用POJO为基础的编程模型促进良好的编程风格。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302162718962.png)

##### 注：POJO和JavaBean的区别

（1）POJO 和JavaBean是我们常见的两个关键字，一般容易混淆，POJO全称是Plain Ordinary Java Object / Pure Old Java Object，中文可以翻译成：普通Java类，具有一部分getter/setter方法的那种类就可以称作POJO，但是JavaBean则比 POJO复杂很多， Java Bean 是可复用的组件，对 Java Bean 并没有严格的规范，理论上讲，任何一个 Java 类都可以是一个 Bean 。但通常情况下，由于 Java Bean 是被容器所创建（如 Tomcat\) 的，所以 Java Bean 应具有一个无参的构造器。  
（2）通常 Java Bean 还要实现 Serializable 接口用于实现 Bean 的持久性。 Java Bean 是不能被跨进程访问的。JavaBean是一种组件技术，就好像你做了一个扳子，而这个扳子会在很多地方被拿去用，这个扳子也提供多种功能\(你可以拿这个扳子扳、锤、撬等等\)，而这个扳子就是一个组件。一般在web应用程序中建立一个数据库的映射对象时，我们只能称它为POJO。POJO\(Plain Old Java Object\)这个名字用来强调它是一个普通java对象，而不是一个特殊的对象，其主要用来指代那些没有遵从特定的Java对象模型、约定或框架（如EJB）的Java对象。理想地讲，一个POJO是一个不受任何限制的Java对象（除了Java语言规范）。

#### 2、Spring有哪些优点？（面试题）

**轻量级**：Spring在大小和透明性方面绝对属于轻量级的，基础版本的Spring框架大约只有2MB。

**控制反转\(IoC\)**：Spring使用[控制反转](https://so.csdn.net/so/search?q=%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC&spm=1001.2101.3001.7020)技术实现了松耦合。依赖被注入到对象，而不是创建或寻找依赖对象。

**面向切面编程\(AOP\)**： Spring支持面向切面编程，同时把应用的业务逻辑与系统的服务分离开来。

**容器**：Spring包含并管理应用程序对象的配置及生命周期。

**MVC框架**：Spring的web框架是一个设计优良的web MVC框架，很好的取代了一些web框架。

**事务管理**：Spring对下至本地业务上至全局业务\(JAT\)提供了统一的事务管理接口。

**异常处理**：Spring提供一个方便的[API](https://so.csdn.net/so/search?q=API&spm=1001.2101.3001.7020)将特定技术的异常\(由JDBC, Hibernate, 或JDO抛出\)转化为一致的、Unchecked异常。

#### 3、Spring 有两个核心部分：[IoC](https://so.csdn.net/so/search?q=IoC&spm=1001.2101.3001.7020) 和AOP

```
    （1）	IoC：控制反转，把创建对象过程交给 Spring进行管理
    （2）	AOP：面向切面，不修改源代码进行功能增强
```

#### 4、Spring 特点

```
    （1）	方便解耦，简化开发
    （2）	Aop编程支持
    （3）	方便程序测试
    （4）	方便和其他框架进行整合
    （5）	方便进行事务操作
    （6）	降低 API开发难度
```

#### 5、目前使用Spring 版本 5.x

#### 6、Spring架构图，Spring由哪些模块组成？（面试题）

Spring 总共大约有 20 个模块， 由 1300 多个不同的文件构成。 而这些组件被分别整合在**核心容器**（Core Container） 、 **AOP**（Aspect Oriented Programming）**和设备支持**（Instrmentation） 、**数据访问与集成**（Data Access/Integeration） 、 **Web**、 **消息**（Messaging） 、 **Test**等 6 个模块中。 以下是 Spring 5 的模块结构图：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302160946473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)  
图来源于官网，以下各个模块的介绍。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030216110828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)

### 二、第一个Spring项目

#### 1、下载Spring，目前版本5.3.4

<https://repo.spring.io/release/org/springframework/spring/>  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302155552290.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)  
下载完成后，解压如下：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302155622439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)

#### 2、打开Idea创建一个Java工程，并新建一个普通的Java Model

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030215581587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)

#### 3、进入下载的Spring压缩包下面的libs目录中，将其中的核心jar包导入项目

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302161502546.png)  
并导入一个`commons-logging-1.2.jar`，下载链接：<http://commons.apache.org/proper/commons-logging/download_logging.cgi>

#### 4、创建一个`User.java`类

```java
/**
 * @Author m.kong
 * @Date 2021/2/23 上午11:45
 * @Version 1.0
 */
public class User {
    public void add(){
        System.out.println("add.......");
    }
}
```

#### 5、在src目录下创建Spring配置文件，`bean1.xml`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302162037825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--配置User对象创建-->
    <bean id="user" class="com.micah.spring.User"/>
</beans>
```

#### 6、测试，`BeanTest.java`

```java
@Test
    public void testAdd() {
        //1 加载 spring 配置文件 
        ApplicationContext context =
                new ClassPathXmlApplicationContext("bean1.xml");
        //2 获取配置创建的对象 
        User user = context.getBean("user", User.class);
        System.out.println(user);
        user.add();
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302162523421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1ODQzNTE0,size_16,color_FFFFFF,t_70)  
这样就实现了将对象的创建过程，交给了Spring。