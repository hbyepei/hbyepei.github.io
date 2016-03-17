---
title: git换行符之autoCRLF配置的意义
date: 2016-03-17 10:12:17
tags:
categories: git
---

* 关于git换行符处理的问题，我查了一查，自己的设置中，global-config中设了autocrlf=false，systemwide中将autocrlf设成了true.  
* 关于配置的作用域，systemwide>global>local。local没有配置，global会覆盖systemwide的配置，因此最终生效的是“autocrlf=false”。  
* 这句的意思是“在提交与检出代码的时候均不对换行符进行转换”，这个设置是在当时入职时，参照wiki上配置的，然后并没有深入了解其作用。现在把根换行符相关的配置说明列在这里：  


 首先crlf是windows下的换行符，而lf是unix下的换行符。  
>     autocrlf =true 表示要求git在提交时将crlf转换为lf，而在检出时将crlf转换为lf。  
>     autocrlf = false表示提交和检出代码时均不进行转换  
>     autocrlf = input 表示在提交时将crlf转换为lf，而检出时不转换  


* 我又检查了一下idea右下角那儿关于文件的换行符，使用的地crlf，，这表明当前使用的是CRLF换行符，而autocrlf又是false，这会使得代码仓库中出现混合换行符，git中另有一个配置项，名叫“safecrlf”默认值是“warn”，因此git在提交时会弹出关于这个问题的提示，然而我无知的忽略了它。。。——在某次操作中顺手将其设为了“不再提示”，这也是导致后来问题的一个原因。因为这么做的话，safecrlf的值就被改为了false.  

* 因此，可以单击右下角这个换行符标志，将其改为“LF”，同时把全局配置改为autocrlf=input，以使仓库中只保留Unix换行符。  

* 另外，git有一个safecrlf，默认是false，可以改成true，表示拒绝提交混合换行符的代码。当然这可能会造成烦人的提示，但对于windows下的特殊情况可以这样设置。它的具体配置含义如下：  
> safecrlf = true 表示 拒绝提交包含混合换行符的文件  
> safecrlf = false 表示 允许...  
> safecrlf = warn 表示警告  