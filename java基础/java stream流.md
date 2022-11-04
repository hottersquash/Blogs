---
title: java stream流
author: hottersquash
date: 2022-11-03 00:00:00
tags: test
---
1、流(Stream)是java8引入的api，可替换大部分集合操作

2、流的处理逻辑如下：

获取流对象 -> 一系列中间操作 -> 终端操作
![image.jpeg](https://upload-images.jianshu.io/upload_images/26760127-d9463f13faa6fd9f.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


3、流的特点：

          1>没有存储，中间不需要变量进行存储
    
            2>不改变原对象的值

3>懒加载，即后一个中间操作涉及的变量值不会在前一个中间操作中被计算

4>可能大小无限，例如limit(n) 和 findFirst()是“短路机制”, 无限大小在有限时间里查找

5>消耗的，就是流里面的每个元素在一个中间操作里只能被消耗一次

4、常见的流的获取：

1>从Collection中获取，collection.stream()，collection.paralleStream()

2>Arrays.stream(object []) , 根据数组构造array然后获取流

3>从stream的静态工厂方法获取，Stream.of(object []), IntStream.range(int,int) ,Stream.iterate(Object,UnaryOperator)

4>BufferedReader.lines()

5>Files类的一些方法

6>Random.ints()

5、