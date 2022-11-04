### APP 逆向思路整理

参考示例《Crack App _ 某新闻 app 参数 sn 加密逻辑分析.pdf》



1、Charles + Potern 抓包**->**定位到请求和需要分析的加密参数

2、jadx反编译 apk **->**发现jadx中啥都没有**->** 怀疑有壳

3、查壳**->**发现是腾讯加固

4、BlackDex进行APP脱壳

5、重新用jadx打开脱壳后的文件**->**发现sn参数的加密函数

6、Frida hook动态调试**->**分析so参数

7、逆向还原

