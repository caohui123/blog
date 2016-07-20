---
title: mysql同时使用order by和limit查询时的一个严重隐患 -- 丢失数据
date: 2016-07-20 16:13:25
tags: [mysql,bug,limit,order by]
categories: mysql
---
我经常使用order by和limit来做数据分页显示并排序，一直也没发现过什么问题。但这两天缺遇到一个严重的问题，在按时间戳升序排列并用limit分批读取数据时，却发现在某些记录丢失了，表中明明有的记录确死活读取不到。研究了大半天终于发现了问题所在，记录一下以防忘记，也是给大家提个醒。
<!-- more -->
**问题重现**

_工具和原料_

`数据库：`

Ver 14.14 Distrib 5.6.11, for Linux (x86_64) using EditLine wrapper

`表结构：`

| 字段           | 类型           | 说明               |
| ------------- |:-------------:| ------------------:|
| id            | int(10)       | 主键                |
| pay_time      | int(10)       | 时间戳，有索引        |
| flag          | tinyint(1)    | 类型标识，用于分类筛选 |
`数据`

大概5000条数据， 大部分记录的flag都等于0，pay_time字段时间戳格式都正确

`需求`

筛选出flag=0的记录，按pay_time升序依次读取所有数据。

`处理方式`

使用limit分批读取数据，如： 
```
select id, pay_time from order_customer_new where flag=0 order by pay_time asc, id asc limit 250, 10;
```
`发现问题`

在读取数据的过程中，发现有时间戳相等的记录，分两次读取出来时，可能会丢失一条记录。见下图，id=465的记录就丢失了。


{% qnimg mysql_bug/1.jpg title:图1  alt:图1 'class:class1 class2' extend:?imageView2/2/w/600 %}

{% qnimg mysql_bug/2.jpg title:图2  alt:图2 'class:class1 class2' extend:?imageView2/2/w/600 %}
这里写图片描述

问题分析与猜测

当排序值相等，其先后顺序的不确定的。这里我猜想：当465和466处于limit末尾时466排在前面，而当处于limit开头时，466缺排到后面去了。所以465丢失了，466出现了两次。 
排序值相等时，其顺序的不确定应该是其结果不可预测。但真正进行排序时应该会采取一定的规则以确定唯一的排序结果，也就是说，即使有相等的排序值，多次排序的结果应该是一样的。从以前的使用经历看，mysql是这么做的。但这次遇到的问题似乎说明mysql并不是这样的。不知道mysql本来就是如此，还是一个bug。

**解决办法**

既然猜想此问题是因为排序值相等造成顺序不确定引起的，那么就试试增加排序条件让其排序结果是确定的、唯一的。一试果然OK，如下图所示，465出来了。

这里写图片描述
{% qnimg mysql_bug/3.jpg title:图3  alt:图3 'class:class1 class2' extend:?imageView2/2/w/600 %}
`请求支援`

我对mysql的底层实现和数据库原理不是很了解，完全不明白mysql为什么会出现这种问题。若哪位朋友能解释一二，不胜感激！

