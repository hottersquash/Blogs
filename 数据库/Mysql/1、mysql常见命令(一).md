##### “我们登上并非我们所选择的舞台，演出并非我们所选择的剧本。    —— 爱比克泰德”
********
### 常用命令
```bush
#展示数据库
show DataBases    
# 展示表
show Tables        
# 表结构
show columns from tablename   <==> DESCRIBE tablename
# 用来显示授权用户的安全权限
show GRANTS 
```

### 启动关闭Mysql命令
 ```bush
# 启动mysql服务
net start mysql 
# 停止mysql服务
net stop mysql 
# 移除mysq
sc delete mysql  
```

### 登录mysql
```bush
# 登录mysql
mysql -u root -pxxxxxx   
set password for root@localhost = password('');
```

********

ps:
1、检索数据时只要数据相同，顺序可能是入库顺序也可能不是（当未指定排序方式时）

2、多条SQL语句必须以分号（;）分隔，单条不需要，处理SQL忽视空格

3、代码规范：所有的SQL关键字使用大写，对表的列和表的名使用消协，将SQL语句分成多行更易阅读


