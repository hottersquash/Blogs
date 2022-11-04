### “人的一生应当这样度过：当他回首往事的时候，他不因虚度年华而悔恨，也不应碌碌无为而羞愧。——保尔·柯察金”
***
正则表达式的学习思维图如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/屏幕截图.png" width = "60%" /></div>


###1、捕获组与非捕获组
捕获组应用场景是：利用一次匹配，同时匹配出多个不同的结果。例如对于字符串“AA22BB”，我想同时匹配出“AA”和“22”，但是不想它们以“AA22”的形式被匹配出，而是以“AA”，“22”这种分开的形式被同时匹配出来，这时候就可以利用匹配组来解决。

捕获组按照调用形式上可以分为两种：普通捕获组和命名捕获组。普通捕获组按照序号进行调用的，而命名捕获组是对每个捕获组进行命名，通过名称来进行调用kk

&emsp;<1>普通捕获组
 ```java
        String strTest = "AA22BB";
        Pattern pattern = Pattern.compile("([A-Z]{2})(\\d+)");
        Matcher matcher = pattern.matcher(strTest);
        if (matcher.find()) {
             /**
               匹配的结果中，序号“0”的匹配结果是匹配到的整个结果，而要访问其他的匹配结果，
               序号需要从“1”开始，可以从下面的结果中看出
                当访问越界时，报错是当然的啦
            */
            System.out.println("序号为0的匹配结果：" + matcher.group(0));
            System.out.println("序号为1的匹配结果：" + matcher.group(1));
            System.out.println("序号为2的匹配结果：" + matcher.group(2));
            try{
                System.out.println("序号为3的匹配结果：" + matcher.group(3));
            }catch(Exception e){
                System.out.println("访问元素不存在");
                e.printStackTrace();
            }
```
运行结果如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-d543634bd398cacb.png" width = "60%" /></div>

&emsp;<2>命名捕获组
```java
        String strTest = "AA22BB";
        /**
        相信聪明的你已经看出命名捕获组的玄妙啦
        没错，命名捕获组就仅仅是在左括号后面加上"?<名称>"，然后匹配时直接根据“名称”匹配即可
        */
        Pattern pattern = Pattern.compile("(?<hello>[A-Z]{2})(?<world>\\d+)");
        Matcher matcher = pattern.matcher(strTest);
        if (matcher.find()) {
            System.out.println("序号为0的匹配结果：" + matcher.group(0));
            System.out.println("命名为hello的匹配结果：" + matcher.group("hello"));
            System.out.println("命名为world的匹配结果：" + matcher.group("world"));
            try{
                System.out.println("序号为3的匹配结果：" + matcher.group(3));
            }catch(Exception e){
                System.out.println("访问元素不存在");
                e.printStackTrace();
            }
        }
 ```

运行结果：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-172c73d736dda88d.png" width = "60%" /></div>


另一方面，对于非捕获组而言，它是实现：去掉正则表达式“工具人规则”的手段之一。（另一种手段在利用零宽断言来实现的），所谓“工具人规则”就是那种必须在正则表达式中出现，只有出现了才能获得匹配结果，但是又不想它在匹配结果中出现，即“只分组、不输出、也不分配组号”。例如：对于字符串“AA22BB，AA22B，AA22BBB”中只想要匹配出“AA22BB”和“AA22B”，不想要匹配出后面的“，”，此时的“，”就属于“工具人规则”
。用法是在“(”后面紧跟“?:”，示例如下：
```java
        String str ="AA22BB，AA22B，AA22BBB";
       \\此处的“(?:\\，)”可以简写成“(?:，)”，加上"\\"为了更容易理解
        Pattern p = Pattern.compile("(\\w+)(?:\\，)");
        Matcher m = p.matcher(str);
        while (m.find()){
            System.out.println(m.group(0));
            System.out.println(m.group(1));
            try {
                System.out.println(m.group(2));
            }catch (Exception e){
                System.out.println("我被触发了，因为访问了不存在的分组");
                continue;
            }
            System.out.println("---------------------------\n");
        }
    }
```
运行结果为：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-964fb45960c44754.png" width = "60%" /></div>

###2、零宽断言
零宽断言的作用和上面的非捕获组的作用相同，“零宽”的含义是指匹配的是位置，而且零宽部分的匹配结果不展示在匹配结果中，说白了就是“工具人规则”。零宽断言一共分为4种（名字十分复杂，记住用法即可），为了记忆方便，记住其中两种就行了，另外两种自然就记住了。
它一般的格式是：
**&emsp;&emsp;&emsp;&emsp;(?<=regex1)regex2(?=regex3)**
其中
1、"?<="部分表示匹配以"regex1"(正则表达式1)开头的位置, 但匹配结果不包含满足"regex1"部分。
2、"?="部分表示匹配以"regex3"(正则表达式3)结尾的位置, 但匹配结果不包含满足"regex3"部分。
示例如下：
```java
       /**
         * 我们这里准备匹配的是“君不见...”，但是我们不想要诗句前面的数字以及诗句后面的出处
          * 这个时候我们就可以用零宽断言来解决这个问题
          */
        String str ="1、君不见，黄河之水天上来，奔流到海不复回。——李白《将进酒》" +
                    "2、仰天长啸出门去，我辈岂是蓬蒿人。——李白《南陵别儿童入京》" +
                    "万里悲秋常作客，百年多病独登台。";
        // 可以看到我们要匹配的内容都是以数字开头，以"》"结尾，因此我们的匹配规则如下:
        String regex = "(?<=\\d、).*?(?=——.*?》)";
        Pattern p = Pattern.compile(regex);
        Matcher m = p.matcher(str);
        while(m.find()){
            System.out.println(m.group());
        }
```
运行结果如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-900dc36a9687aa9b.png" width = "60%" /></div>

&emsp;&emsp;"?<="的反义就是"?<!"则表示不会匹配以"regex1"(正则表达式1)开头的位置，同理"?!"的作用和"?="的相反，用法相同，故不过多赘述。


###3、反向引用
&emsp;&emsp;反向引用更多地应用在java的正则替换部分，配合字符串的切割函数，可以起到一些意想不到的效果，例如去重。话不多说，直接上代码：
 ```java
 public static void main(String[] args) {
        /**
         * 如果我们希望能在下面的诗句前面的数字后面加上制表符，
         * 此时反向引用就派上用场了
         */
        String str = "1、三月七日，沙湖道中遇雨。雨具先去，同行皆狼狈，余独不觉，已而遂晴，故作此。\n" +
                "2、莫听穿林打叶声，何妨吟啸且徐行。竹杖芒鞋轻胜马，谁怕？一蓑烟雨任平生。\n" +
                "3、料峭春风吹酒醒，微冷，山头斜照却相迎。回首向来萧瑟处，归去，也无风雨也无晴。\n";
        /**
         * 此时的$1就代表前面正则匹配的第一个分组，
         * 如果有多个分组，那么$0代表整个匹配结果，
         * 各个分组下标从1开始
          */
        str = str.replaceAll("(\\d、)","$1\t");
        System.out.println(str);
    }
```
运行结果如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-2a2fdf5f02eae348.png" width = "60%" /></div>


&emsp;&emsp;此外，反向引用也可以应用在同一个正则表达式中，示例如下：
```java
public static void main(String[] args) {
        /**
         * 此时我们需求是把每一行的第二个数字去掉
         */
        String str = "1、1三月七日，沙湖道中遇雨。雨具先去，同行皆狼狈，余独不觉，已而遂晴，故作此。\n" +
                "2、2莫听穿林打叶声，何妨吟啸且徐行。竹杖芒鞋轻胜马，谁怕？一蓑烟雨任平生。\n" +
                "3、3料峭春风吹酒醒，微冷，山头斜照却相迎。回首向来萧瑟处，归去，也无风雨也无晴。\n";
        /**
          *此时的\\1也是代表第一个匹配分组滴，和后面的$1的作用一致
          */
        str = str.replaceAll("(\\d)、\\1","$1、\t");
        System.out.println(str);
    }
```
运行结果如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-2d80c0bb9c61b5fa.png" width = "60%" /></div>

