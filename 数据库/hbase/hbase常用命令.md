#### 1、开启hbase交互界面

```bash
hbase shell
```



#### 2、查表操作

```bash
# scan命令
# scan 全表
scan 'student'
# scan 加过滤器
scan 'student',{LIMIT=>10,FILTER=>"PrefixFilter('zhangsan')"}
# scan 起始行、结束行
scan 'zy_comment',{STARTROW=>'b',STOPROW='e',LIMIT=>10}
# 其他命令详见 url = https://www.jianshu.com/p/0ccfd59d73f4


# 查询表的行数
count 'student'
```





#### 3、库指令

