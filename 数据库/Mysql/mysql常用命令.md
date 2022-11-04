---
title: mysql常用命令
author: hottersquash
date: 2022-11-03 00:00:00
tags: test
---
需要总结的问题：
1、synchronized关键字
	synchronized修饰boolean时，
		报错：Incompatible types. Found: 'boolean', required: 'java.lang.Object'
	修改为修饰Boolean时，
		报错：Synchronization on a non-final field 'this.dayFinish'

2、mysql的索引问题，
	what：索引的作用是什么，原理是什么，怎么实现的
	why：为什么用索引，有什么好处
	how：什么时候应该用索引，什么时候不应该用索引，

	参考文档1：https://lazytest.blog.csdn.net/article/details/53395628?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link
	
	参考文档2：https://www.cnblogs.com/zzhAylm/p/14752969.html

3、mysql的各种数据类型的详细比较，存储方式，查询效率

4、insert into... select ... from ... where ...
```sql
insert into ives_cp1.dt_url_list_tmp select *,"" as ChangeReason from ives.dt_url_list_tmp where IsValueable>=0 limit 2000,2000
```

5、查看进程命令
``` sql
show processlist
-- 同上
select * from information_schema.PROCESSLIST p 

-- 查看事务
select * from information_schema.innodb_trx

-- 查看表的锁
select * from information_schema.innodb_locks 
```

6、查看event事件
```sql
select * from performance_schema.events_statements_current 
```

7、查看表占用大小
```sql
SELECT TABLE_NAME,DATA_LENGTH+INDEX_LENGTH,TABLE_ROWS,concat(round((DATA_LENGTH+INDEX_LENGTH)/1024/1024,2), 'MB') as data 
FROM information_schema.TABLES WHERE TABLE_SCHEMA='ives';
```

-- 查询所有索引
```sql
select concat('create index ', index_name, ' on ' , table_name, '(', column_name,')', ' using ', index_type, ';') 
from information_schema.STATISTICS where index_name != 'PRIMARY' and table_schema = 'ives' and TABLE_NAME != 'dt_url_list_tmp_cp';
```

-- 查看所有表的名
```sql
select CONCAT('truncate ',table_name) from information_schema.tables where table_schema='ives' and table_type='base table';
```