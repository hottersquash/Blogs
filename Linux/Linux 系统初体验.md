### "尊严，只在剑锋之上！真理，只在大炮射程之内！    ——艾公"
***
环境: linux mint19.3
# 1、切换软件源。
<div align="center"><img src="https://gitee.com/hottersquash/pictures_byan/raw/master/all//24004096-1ee6c0562e53e8f3.png" width = "60%" /></div>


# 2、更新软件
```bash 
//   源列表中服务器的软件列表，保存展示到我们的电脑，作用类似360软件管家。
sudo apt-get update
// 更新本地文件
sudo apt-get upgrade
```

# 3、安装ssh
```bush
// 安装
sudo apt-get install openssh-server
// 开启服务
service sshd start
// scp传输文件命令
// scp [发送文件名] 接收用户名@对方ip:接收方存放文件文件夹。eg:
scp /home/space/music/1.mp3 root@www.runoob.com:/home/root/others/music 
```

# 4、查看ip：
```bush
ifconfig
```

# 5、修改权限





