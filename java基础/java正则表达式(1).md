### “人的一生应当这样度过：当他回首往事的时候，他不因虚度年华而悔恨，也不应碌碌无为而羞愧。——保尔·柯察金”
***
正则表达式的学习思维图如下：

<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/屏幕截图.png" width = "60%" /></div>

一般的正则表达式的模式是（定位符）+ 字符 + 量词 + 匹配模式 +（定位符）

### 1、字符
   此处的字符只表示那些代表文本的字符，不包括匹配数量的限定符和匹配为止的定位符，正则表达式中的字符分为普通字符和特殊字符。

###### 1>普通字符，即大写和小写字母、所有数字、所有标点符号和一些其他符号。即直接能和待匹配文本“相同”的部分，例如：

  ```java
        String str="hello world hello nihao";
        // 构造一个正则匹配
        Pattern p = Pattern.compile("hello");
        Matcher m = p.matcher(str);
        while(m.find()){
            System.out.println(m.group());
        }
  ```


运行结果如下：

<div align="center"><img src="D:\workspace\Git\Notes\imgs/24004096-48cd29b0d4dd0087.png" width = "60%" /></div>

可以看到当表达式是为“hello”时，直接匹配出两个相同的“hello”。

###### 2>特殊字符，包括一些转义字符，特殊含义字符。常见的特殊字符如下表所示：
首先是常见转义字符：
| 字符 | 代码中的表示| 作用|
| ------ | ------ |--------|
| \n | \\\n| 匹配换行符|
| \t | \\\t | 匹配制表符|

其次是正则中的一些需要记的专门字符
| 字符 | 代码中的表示| 作用|
| ------ | ------ |--------|
| \s | \\\s| 匹配单个空白字符|
| \S| \\\S | 匹配单个非空白字符|
| \d | \\\d| 匹配单个0-9的数字字符|
| \D| \\\D | 匹配单个非数字字符|
| \w | \\\w| 匹配单个数字、字母、下划线，等价于 [A-Za-z0-9_]|
| \W | \\\W| 匹配非‘\\\w’，即单个除了数字、字母、下划线之外的字符|
| . |.| 匹配单个除了换行符之外的任意字符|


值得注意的是，在其他语言中，\\\ 表示：我想要在正则表达式中插入一个普通的（字面上的）反斜杠，请不要给它任何特殊的意义。

在 Java 中，\\\ 表示：我要插入一个正则表达式的反斜线，所以其后的字符具有特殊的意义。

所以，在其他的语言中（如Perl或者python），一个反斜杠 \ 就足以具有转义的作用，而在 Java 中正则表达式中则需要有两个反斜杠才能被解析为其他语言中的转义作用。也可以简单的理解在 Java 的正则表达式中，两个 \\\ 代表其他语言中的一个 \，这也就是为什么表示一位数字的正则表达式是 \\\d，而表示一个普通的反斜杠是 \\\\\\\\。（详见：[https://www.runoob.com/java/java-regular-expressions.html](https://www.runoob.com/java/java-regular-expressions.html)）

###### 3>自定义字符集合
比如我想匹配以a，b，z开头的部分，此时就可以用“[abz]\w+”来匹配，暂且先不管'\w+'，此时[abz]就代表a、b、z中的其中之一，这里的'[]'代表选择区域，就个人的理解，类似于数学上的‘定义域’的含义。eg:
```java
        String str="hello world hello nihao";
        // 构造一个正则匹配
        Pattern p = Pattern.compile("[hw]\\w+");
        Matcher m = p.matcher(str);
        while(m.find()){
            System.out.println(m.group());
        }
```
匹配结果如下：
<div align="center"><img src="D:\workspace\Git\Notes\imgs/24004096-0f03467fbdf901fc.png" width = "60%" /></div>

这里值得注意的是：
（1）“-”在"[]"中表示“至”的含义，例如“[a-zA-Z]”表示匹配字符范围是大小写英文字母。
（2）“^”在"[]"中表示"非"的含义，例如“[\^\\\d]”表示匹配非数字字符。
（3）其他特殊字符出现在"[]"中均表示本身含义，例如“[.]”就是表示"."这个字符。

### 2、量词
顾名思义，就是表示数量的修饰词，是用来修饰1中的字符，常见的量词见下表所示
| 量词 | 代码中的表示| 作用|
|:------: | :------ | :-------- |
| {m} |表示m次匹配前面的字符或子表达式。|例如，\d[2] 表示匹配数字字符两个。|
| {m,n} | 表示前面的字符或子表达式至少出现m次，至多出现n次。|例如，\d[2，5] 表示匹配的内容数字字符至少出现2次，至多出现5次。|
| {m,} | 表示前面的字符或子表达式至少出现m次|例如，\d[2，] 表示匹配的内容数字字符至少出现2次。|
| * |表示零次或多次匹配前面的字符或子表达式。|例如，zo* 匹配"z"和"zoo"。* 等效于 {0,}。|
| ？|  零次或一次匹配前面的字符或子表达式。|例如，"do(es)?"匹配"do"或"does"中的"do"。? 等效于 {0,1}。|
| +|一次或多次匹配前面的字符或子表达式。|例如，"zo+"与"zo"和"zoo"匹配，但与"z"不匹配。+ 等效于 {1,}。|
eg:
```java
        String str="hello world hello nihao";
        // 构造一个正则匹配
        Pattern p = Pattern.compile(".+");
        Matcher m = p.matcher(str);
        while(m.find()){
            System.out.println(m.group());
        }
```
运行结果如下:
<div align="center"><img src="D:\workspace\Git\Notes\imgs/24004096-f644e971b9977976.png" width = "60%" /></div>

详解：“.”代表匹配任意字符，“+”代表匹配前面的“.”至少一次，故匹配结果是整个str，不过大家可以先想一想为什么匹配到的不是一次而是多次呢？这个问题我们在下面匹配模式中会解释。

### 3、匹配模式
匹配模式分为贪婪匹配和非贪婪匹配，简单地讲，就是当步骤2中的量词出现多种选择时，例如“\d{m,n}”的时候：
###### 1>贪婪模式
如果时贪婪模式，就会尽可能多的匹配，偏向于n个，java默认的是贪婪模式。这就能现在就能解释为什么上面的2中运行结果是匹配出整个字符串而不是单个字符

###### 2>非贪婪模式
但是如果是非贪婪模式，就会尽可能少的匹配，更偏向于m个。
非贪婪模式用“?”来表示，意义和量词中的“？”不同。例如“\w??”中第一个“?”表示"\w"出现零次或者一次，等同于{0,1}。而第二个“？”表示非贪婪模式，表示第一个“？”取零次。（其实感觉上面这个正则表达式没啥含义，仅仅为了说明两个“？”的不同含义）

下面的程序说明贪婪匹配和非贪婪匹配的区别：
```java
        String str="hello world hello nihao";
        // 构造一个正则匹配
        Pattern p = Pattern.compile(".+?");
        Pattern p2 = Pattern.compile(".+");
        Matcher m = p.matcher(str);
        System.out.println("非贪婪匹配结果：");
        while(m.find()){
            System.out.print(m.group()+",");
        }
        System.out.println("\n贪婪匹配结果：");
        m = p2.matcher(str);
        while(m.find()){
            System.out.print(m.group()+",");
        }
```
运行结果如下：
<div align="center"><img src="D:\workspace\Git\Notes\imgs/24004096-5dd014129e9fd330.png" width = "60%" /></div>


### 4、分组匹配
当我们想要同时匹配多个表达式的时候，该如何实现呢，例如对于字符串“1213141##abab”，如果我们想要同时取得“1213141”和“abab”，但是并不想要中间的“##”该怎么办呢？此时我们就可以利用分组匹配来解决

即一个正则表达式由多个“小”的正则表达式组成。具体怎么来实现呢，见下列源码：

```java
        String str="1213141##abab";
        // 构造一个正则匹配
        Pattern p = Pattern.compile("(\\d+)##(\\w+)");
        Matcher m = p.matcher(str);
        while(m.find()) {
            System.out.println(m.group(0));
            System.out.println(m.group(1));
            System.out.println(m.group(2));
        }
```
运行结果：

<div align="center"><img src="D:\workspace\Git\Notes\imgs/24004096-8e5ba66868de409c.png" width = "60%" /></div>


##### 基本的正则入门知识差不多就是这么多，但是还有一些没有涉及到的内容，例如Pattern类和Matcher类以及它们包含的方法，由于大多方法平时用到的并不多，所以没有过多地解释。如果感觉还是不懂，可以去B站看视频，推荐视频入口：[https://www.bilibili.com/video/BV1XW411R7RF?p=1](https://www.bilibili.com/video/BV1XW411R7RF?p=1

