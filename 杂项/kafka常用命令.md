---
title: kafka常用命令
author: hottersquash
date: 2022-11-03 00:00:00
tags: test
---
-- 查询kafka各个分组的待消费数量
kafka-consumer-groups --bootstrap-server cloudera01:9092 --describe --group group.test
-- 获取kafka发送消息
kafka-console-consumer --zookeeper cloudera01:2181/kafka --topic crawler_raw_data