---
title: mysql字符转义
date: 2016-02-02 15:57:26
tags:
categories: mysql
---

Mysql的转义字符是"\"，即反斜杠，在INSERT语句中，如果被插入的文本中包含反斜杠，那么反斜杠会被吃掉。例如：
>`INSERT INTO tb (id,json) VALUES ('1','"person":"{\"name\":\"yp\",\"age\":\"25\"}"');`  

插入后，数据库中的记录中不会有“\”出现，它神秘的消失了。

解决方法: 在插入之前将字符串中的`"\"`替换成:`"\\"`即可。在Java中可以使用：`str.replaceAll("\\\\","\\\\\\\\");`
##### 参考[Mysql中文册](http://www.yesky.com/imagesnew/software/mysql/manual_Reference.html): http://www.yesky.com/imagesnew/software/mysql/manual_Reference.html