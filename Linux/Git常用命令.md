#### "竹杖芒鞋轻胜马，谁怕？一蓑烟雨任平生 --苏轼《定风波》"
******
&emsp;&emsp;只展示常用命令，详细的命令见：https://www.cnblogs.com/miracle77hp/articles/11163532.html

### 0、git的仓库结构如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/2021-11-07-15-55-14.png" width = "60%" /></div>

### 1、命令行基本接口
#### 1> 接口基础
``` bash
# 当前目录路径
$ pwd
# 进入目的文件路径下，“.”代表当前路径下,“..“代表上一级路径，“/”代表根路径
$ cd [路径]
```

#### 2> 命令行的目录参数
``` bash
# 显示目标目录下所有子目录和文件
$ ls [目录名]
# echo 输出参数的内容
$ echo helloworld
# which 显示该参数的程序的运行路径
$ which vim
# 常见不带参数的路径
$ pwd, clear
```

#### 3> 命令行的选项参数
``` bash
# 相当于java的函数的重载
$ ls -lsah
# 查看命令说明文档
$ ls --help
```


#### 4>提交代码到仓库的命令
```bash
# 显示暂存区和工作区的差异
git diff
# 显示有变更的文件
git status
# 添加当前目录的所有文件到暂存区
git add .
# 提交暂存区到仓库区
git commit -a -m "message"
# 上传本地指定分支到远程仓库
git push 
``` 




