### 暂缺
******
&emsp;&emsp;只展示常用命令，详细的命令见：https://www.cnblogs.com/miracle77hp/articles/11163532.html

&emsp;&emsp;此外，git学习地址推荐：https://www.zsythink.net/archives/category/%e8%bf%90%e7%bb%b4%e7%9b%b8%e5%85%b3/git

# 0、git的仓库结构如下：
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all/2021-11-07-15-55-14.png" width = "60%" /></div>

# 1、新建代码库
``` bash
# 在当前目录新建一个Git代码库
$ git init
# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]
# 下载一个项目和它的整个代码历史
$ git clone [url]
```

# 2、config
``` bash
# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

# 3、add
``` bash
# 添加当前目录的所有文件到暂存区 
$ git add .
```

# 4、commit
``` bash
# 提交暂存区到仓库区
$ git commit -m [message]
# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a
# 提交时显示所有diff信息
$ git commit -v
```

# 5、branch
``` bash
# 列出所有本地分支，加上-r就是远程分支，-a包括所有
$ git branch
# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]
# 新建一个分支，并切换到该分支
$ git checkout -b [branch]
# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]


# 合并指定分支到当前分支
$ git merge [branch]
# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支，加上-r就是删除远程分支
$ git branch -d [branch-name]

```

# 6、tag
``` bash
# 列出所有tag
$ git tag
# 新建一个tag在当前commit
$ git tag [tag]
# 新建一个tag在指定commit
$ git tag [tag] [commit]
```

# 7、status和log
``` bash
# 显示有变更的文件
$ git status
# 显示当前分支的版本历史
$ git log
# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat
# 搜索提交历史，根据关键词
$ git log -S [keyword]
# 显示指定文件是什么人在什么时间修改过
$ git blame [file]
# 显示暂存区和工作区的差异
$ git diff
# 显示当前分支的最近几次提交，Git提供了一个命令git reflog用来记录你的每一次命令
$ git reflog
```

# 8、push
``` bash
# 下载远程仓库的所有变动
$ git fetch [remote]
# 显示某个远程仓库的信息
$ git remote show [remote]
# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]
# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]
# 上传本地指定分支到远程仓库
$ git push [remote] [branch]
# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force
# 推送所有分支到远程仓库
$ git push [remote] --all
```

# 9、checkout和reset
``` bash
# 切换分支
$ git checkout [branch]
# 将本地仓库的文件置于工作空间,可用于撤销工作区间的修改
$ git checkout -- [file]
# 取消暂存区的文件
# mixed是默认选项，保留工作区间的更改；soft只修改Head指针和commitID；hard会将工作区和暂存区的文件全部重置 
# 此外 ，HEAD^表示上一个版本  
git reset [--soft | --mixed | --hard] [版本号]  
``` 




