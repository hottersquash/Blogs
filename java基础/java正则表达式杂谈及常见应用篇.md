### “人的一生应当这样度过：当他回首往事的时候，他不因虚度年华而悔恨，也不应碌碌无为而羞愧。——保尔·柯察金”
***
思维导图如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/屏幕截图.png" width = "60%" /></div>

##一、零散知识点

######1、python和java中正则表达式的区别：
&emsp;&emsp;python中的转义字符用一个“\”表示就可以，而java中的却要用两个“\”表示，例如：同样是表示任意数字，python中的是“\d”，而java中则需要用“\\\d”表示。

######2、matcher.matches()和matcher.find()的区别:
 1.matcher.find()进行的局部匹配，只要出现匹配即返回true；
2.matcher.matches()进行的是全局匹配，只有当正则规则匹配到整个待匹配的输入字符串整体时候，才会返回true。

演示代码如下
```java
public static void main(String[] args) {
        String str = "竹杖芒鞋轻胜马，谁怕？一蓑烟雨任平生。";
        String pattern1 = "，.*?？";
        Pattern p = Pattern.compile(pattern1);
        Matcher m = p.matcher(str);

        /**
         * 可以看到同样的正则，
         * find()进行的是局部匹配
         * 而matches()进行的是全局匹配
         */
        System.out.println("pattern1的find()匹配结果:" + m.find());
        System.out.println("pattern1的matches()匹配结果:" + m.matches());

        String pattern2 = "竹.*?。";
        p = Pattern.compile(pattern2);
        m = p.matcher(str);
        System.out.println("pattern2的()匹配结果:" + m.find());
        System.out.println("pattern2的()匹配结果:" + m.matches());
    }
```
运行结果如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/24004096-3de6efcfbe0c1703.png" width = "60%" /></div>

######3、java中的spilt()和replaceAll()和正则表达式搭配“食用”，效果更佳

######4、利用java正则表达式去除首尾的空字符
&emsp;&emsp;由于笔者涉及的业务中有这一部分，而且常见的空字符的种类非常多而且杂，利用[\\\s]和trim()有时候并不能完全去除，故总结如下:
```java
/**
 * 去掉开头的空字符，使得第一个字符不为空
 * 返回一个首字符不为空字符的字符串
 * @param oldstr 
 * @return newStr
 * @throws Exception 
 */
private static String trimBigger(String oldStr) throws Exception {
	if (oldStr==null) {
		throw new Exception("输入字符串为空，请检查输入");
	}
	String result =  "";
	// 匹配开头空白字符
	String pattern1 = "^[\\u00A0|\\u0020|\\u3000|\\pZ|\\t|\\n|\\r|\\s]*";
	// 匹配末尾空白字符
	String pattern2 =  "[\\u00A0|\\u0020|\\u3000|\\pZ|\\t|\\n|\\r|\\s]$";
	result = oldStr.replaceAll(pattern1, "");
	Pattern p2 = Pattern.compile(pattern2);
	Matcher m = p2.matcher(result);
	while (m.find()) {
		result = result.replaceAll(pattern2, "");
		m = p2.matcher(result);
	}
	return result;
}
```
如果读者有更好的办法或者上述程序出现bug，欢迎下方留言指出，感激不尽。


##二、常见的正则表达式
##### 1、匹配国内电话号：
^(\\(\\d{3,4}-)|\\d{3,4}-)?\\d{7,8}$
转载于：[https://www.cnblogs.com/fozero/p/7868687.html](https://www.cnblogs.com/fozero/p/7868687.html)


##### 2、匹配电子邮箱：
^[a-z]([a-z0-9]*[-_]?[a-z0-9]+)*@([a-z0-9]*[-_]?[a-z0-9]+)+[\\.][a-z]{2,3}([\\.][a-z]{2})?$
转载于：[https://www.cnblogs.com/fozero/p/7868687.html](https://www.cnblogs.com/fozero/p/7868687.html)


##### 3、匹配URL：
([hH][tT]{2}[pP]://|[hH][tT]{2}[pP][sS]://|[wW]{3}.|[wW][aA][pP].|[fF][tT][pP].|[fF][iI][lL][eE].)[-A-Za-z0-9+&@#/%?=~_|!:,.;]+[-A-Za-z0-9+&@#/%=~_|]
转载于：[https://blog.csdn.net/qq569699973/article/details/94636893](https://blog.csdn.net/qq569699973/article/details/94636893)

##### 4、匹配日期：
^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$
转载于:[https://segmentfault.com/a/1190000017834831](https://segmentfault.com/a/1190000017834831)

##### 5、匹配任意字符
\[\\\\s\\\\S](这里注意“.”不能匹配换行符)

**其他的应用请参考大佬的博客，里面归纳的是非常全滴，务必请果断收藏：
[https://www.cnblogs.com/fozero/p/7868687.html](https://www.cnblogs.com/fozero/p/7868687.html)




