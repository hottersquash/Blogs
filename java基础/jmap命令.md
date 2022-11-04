---
title: jmap命令
author: hottersquash
date: 2022-11-03 00:00:00
tags: test
---
```bash
jmap命令
```
1、查看堆内存(histogram)中的对象数量及大小。
执行命令： jmap -histo 3331

2、将内存使用的详细情况输出到文件，
执行命令： jmap -dump:format=b,file=heapDump 6900

3、查看java 堆（heap）使用情况,
执行命令： jmap -heap 31846