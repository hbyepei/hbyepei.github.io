---
title: Linux统计某文件夹下文件、文件夹的个数
date: 2016-01-14 22:36:04
tags: 
categories: linux
---

- 统计一组目录下的单词出现情况


```bash 
	for i in {5..8}; 
     do zgrep 'There are 1 records need to update, 0 records update successfully.' l-insurance${i}.f.cn6/201506/insurance-provider.log.2015-06-14-*.gz; 
   done
```


- 遍历查询多台服务器上的日志

```bash 
	atnodes 'zgrep /home/q/www/hotel-tts/logs/request.log*' l-hs[01-02].wap.cn1 | sort -t '#' -k1,1
```

- Top-N统计：  
`sort +awk+uniq` 统计文件中出现次数最多的前10个单词  

实例:  
>`cat logt.log|sort -s -t '-' -k1n |awk '{print $1;}'|uniq -c|sort -k1nr|head -100`

1. 使用linux命令或者shell实现：文件words存放英文单词，格式为每行一个英文单词（单词可以重复），统计这个文件中出现次数最多的前10个单词。  

>`cat words.txt | sort | uniq -c | sort -k1,1nr | head -10`
>

2. 主要考察对sort、uniq命令的使用，相关解释如下，命令及参数的详细说明请自行通过man查看，简单介绍下以上  

3. 指令各部分的功能：

`sort`:  对单词进行排序  
`uniq -c`:  显示唯一的行，并在每行行首加上本行在文件中出现的次数  
`sort -k1,1nr`:  按照第一个字段，数值排序，且为逆序  
`head -10`:  取前10行数据  

- 统计某文件夹下文件的个数

	>`ls -l |grep "^-"|wc -l`

- 统计某文件夹下目录的个数

	>`ls -l |grep "^ｄ"|wc -l`

- 统计文件夹下文件的个数，包括子文件夹里的

	>`ls -lR|grep "^-"|wc -l`

- 如统计/home/han目录(包含子目录)下的所有js文件则：

	>`ls -lR /home/han|grep js|wc -l 或 ls -l "/home/han"|grep "js"|wc -l`

- 统计文件夹下目录的个数，包括子文件夹里的

	>`ls -lR|grep "^d"|wc -l`

- 说明：

	>`ls -lR`

- 长列表输出该目录下文件信息(R代表子目录注意这里的文件，不同于一般的文件，可能是目录、链接、设备文件等)

	>`grep "^-"`

- 这里将长列表输出信息过滤一部分，只保留一般文件，如果只保留目录就是 ^d

	>`wc -l`

- 统计输出信息的行数，因为已经过滤得只剩一般文件了，所以统计结果就是一般文件信息的行数，又由于一行信息对应一个文件，所以也就是文件的个数。
 
 
＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝  
如果只查看文件夹  
`ls -d ` 只能显示一个.  
`find -type d `   可以看到子文件夹  
`ls -lF |grep / `   或 `ls -l |grep '^d' ` 只看当前目录下的文件夹，不包括往下的文件夹