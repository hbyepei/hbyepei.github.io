---
title: cerberus监控
date: 2016-01-13 18:54:25
tags: 监控
categories: 
---
**参考WiKi**:*http://wiki.corp.qunar.com/pages/viewpage.action?pageId=63243479*

# 一、监控分为两类:
## 业务监控
- Gauge: 		瞬间值——一个最简单的计量，一般用来统计瞬时状态的数据信息。  
- Counter:	增减一个整数值——Counter是Gauge的一个特例，维护一个计数器，可以通过inc()和dec()方法对计数器做修改。使用步骤与Gauge基本类似，在MetricRegistry中提供了静态方法可以直接实例化一个Counter。  
- Meter:		QPS监控——用来度量某个时间段的平均处理次数，每1、5、15分钟的TPS。通过metrics.meter()实例化Meter，然后通过meter.mark()记录请求。统计结果有总的请求数，平均每秒的请求数，以及最近的1、5、15分钟的平均TPS等。  
- Timer:		响应时间及QPS监控——主要是用来统计某一块代码段的执行时间以及其分布情况，需要Cover住一个耗时的上下文。

## 系统监控
- 异常监控:	需要配置异常日志的收集，[见wiki](http://wiki.corp.qunar.com/pages/viewpage.action?pageId=87333222)  
- HTTP监控:	common version>=8.2.6.引入common-web依赖，并配置Spring:`<context:component-scan base-package="qunar.web.spring" />`。  
- SQL监控:	common version>=8.2.11，启用Qtrace  
- Dubbo监控:	common version>=8.2.4，依赖common-rpc,版本同common  
# 二、使用common-core API直接在代码里打监控，具体如下：
	- TODO 以下方法可以封装成QMonitor类似的简洁调用接口。

### 1.记录当前状态时的某个数值：
```java
    Metrics.gauge("key").call(new Gauge() {//“key”即监控名
        @Override
        public double getValue() {
           //在这里给出监控值
        }
    });
```

### 2.记录一个累计值
```java
    Counter count = Metrics.counter("key").get();//“key”即监控名
    	count.inc();//累加，或count.inc(n);
    	count.dec();//递减,或count.dec(n);
```

### 3.记录单次变化量。可以用来监控错误数、异常等，但注意:下面代码的第一句尽量放在static块中，如果放在错误或异常发生处，则未出错时，将不会产生监控图，影响报警的添加
```java
    Counter count = Metrics.counter("key").delta().get();//“key”即监控名
    count.inc();
```

### 4.记录变化率（不清零）
```java
    Counter count = Metrics.counter("key").delta().keep().get();//“key”即监控名
    count.inc();
```

### 5.监控QPS
```java
    Meter meter = Metrics.meter("key").get();
    meter.mark();//在请求发生处调用mark()方法
```

### 6.监控响应时间，也可用于记录QPS，需要Cover耗时上下文
```java
    Timer.Context time = Metrics.timer("key").get().time();//没有tag的时候可以作为常量         
    try {
      //耗时操作
    } finally {
        time.stop();//操作完成
    }		
```

### 7.指标分组监控——tag
调用Metrics./gauge/counter/meter/timer("").tag(key, value).get()，来获取KeyWrapper


# 三、在应用中心-应用详情列表中，开启监控和报警: 应用中心——应用列表——应用详情——服务器列表——启动监控和报警状态
	
# 四、添加报警的步骤：
１.进入应用中心——监控平台——选择对应应用，这时页面上会列出该应用所监控的指标
２.可以通过模糊搜索查询到特定的指标，点击此指标，即可看到指标详情
３.点击“添加报警”——新建配置，在配置页中指定报警参数，如聚合函数、时间表达式、报警方式、通知人员、级别、次数等。
４.单击“保存”，即可。注意，报警方式中包括了“RTX－邮件－短信”和“发送Qmq消息”两项，其实还有微信报警，这是默认开启，只需要关注“QunarMan”公众号即可收到报警(前提:已经添加通知人员)
	
>详细设置:`http://wiki.corp.qunar.com/pages/viewpage.action?pageId=81036319`

# 五、查看监控图：
Wiki:http://wiki.corp.qunar.com/pages/viewpage.action?pageId=81035640	