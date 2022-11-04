### "尊严，只在剑锋之上！真理，只在大炮射程之内！    ——艾公"
***
环境：linux mint19.3
# 1、下载对应版本jdk，解压

# 2、jdk环境变量的配置
###利用vim或其他的文本编辑器修改
终端输入：vim etc/profile, 然后按insert键进入编辑模式,然后跳转到文件尾, 插入以下代码
```bush
// 这一行是设置系统变量的java_home
export JAVA_HOME=/home/software/jdk1.8.0_51
// 这一行是设置编译时jar包的路径
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOMEgit/lib/tools.jar
// 这一行是把java_home添加到path里
export PATH=$PATH:$JAVA_HOME
```

保存，效果图如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-74fac73c13868486.png" width = "60%" /></div>

# 3、eclipse安装：
进入解压文件夹，双击图标即可启动eclipse

# 4、导入工程
打开eclipse -->  file --> open Projects from File System

<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-e6c23f9b25005bb6.png" width = "60%" /></div>

然后点击Dictionary选择项目文件夹

<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-deec441fabf0cc7f.png" width = "60%" /></div>

选中后点击open --> finish

# 5、eclipse的Maven设置
打开eclipse  -->状态栏中 的Window  -->Preferences -->在搜索框中输入Maven  --> 点击Maven子目录下的User Settings

<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-3d79e6de9172561b.png" width = "60%" /></div>

点击Browse，将Grobal Settings和User Settings的路径分别改成Maven文件夹下以及user文件下的两个setting.xml文件。

 修改项目运行的jdk版本，具体步骤详见：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/2021-11-07-11-47-14.png" width = "60%" /></div>

6、修改maven本地仓库路径
        打开步骤5中的（2）中的两个xml文件，修改其中的localRepository路径，具体如下：
```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
<localRepository>/home/software/maven-3.0.5/repository/repository</localRepository>
<servers>
.......
</settings>
```

(如果导入后，porm.xml文件全是红色的波浪线，可以试试在porm.xml的最后加上)
```xml
<dependency>  
    <groupId>jdk.tools</groupId>  
    <artifactId>jdk.tools</artifactId>  
    <version>1.8</version>  
    <scope>system</scope>  
    <systemPath>${JAVA_HOME}/lib/tools.jar</systemPath>  
</dependency>
```


