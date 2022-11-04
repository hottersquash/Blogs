### "尊严，只在剑锋之上！真理，只在大炮射程之内！    ——艾公"
***
环境：linux mint19.3

##### 1、安装搜狗输入法
&emsp;&emsp;首先直接在软件管理器中搜索Fcitx，点击安装
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/2021-11-07-12-47-09.png" width = "60%" /></div>

&emsp;&emsp;去搜狗的官网下载linux版本的输入法安装包,双击安装即可，下载地址：https://pinyin.sogou.com/linux/，
或者可以使用命令安装

```bush
//命令1
cd 安装包所在路径
//命令2，展示当前路径下的文件和文件夹，复制目的安装包名（右键复制）
ll
//命令3，安装软件包
sudo dpkg -i 包名(注意要加上后缀名)
```
安装完成后在启动栏搜索fcitx，启动fcitx，这时候可以看到状态栏有个小键盘的图标
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/2021-11-07-12-49-17.png"/></div>

然后注销用户，重新登陆即可，看到状态栏有小键盘的图标，然后按shift即可切换到搜狗输入法。
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/2021-11-07-12-49-44.png"/></div>


##### 2、chrome安装：直接去官网下载linux版本的deb安装包，其他步骤同上。