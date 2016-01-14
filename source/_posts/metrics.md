---
title: Metrics-Java版的指标度量工具
date: 2016-01-13 19:07:14
tags: 监控
categories: 
---

## 介绍
Metrics是一个给JAVA服务的各项指标提供度量工具的包，在JAVA代码中嵌入Metrics代码，可以方便的对业务代码的各个指标进行监控，同时，Metrics能够很好的跟Ganlia、Graphite结合，方便的提供图形化接口。基本使用方式直接将core包（目前稳定版本3.0.1）导入pom文件即可，配置如下：  

```xml 
    <dependency>
        <groupId>com.codahale.metrics</groupId>
        <artifactId>metrics-core</artifactId>
        <version>3.0.1</version>
    </dependency>
```  
    
core包主要提供如下核心功能：

#### Metrics Registries类似一个metrics容器，维护一个Map，可以是一个服务一个实例。
#### 支持五种metric类型：Gauges、Counters、Meters、Histograms和Timers。
#### 可以将metrics值通过JMX、Console，CSV文件和SLF4J loggers发布出来。
#### 五种Metrics类型：

1. Gauges
Gauges是一个最简单的计量，一般用来统计瞬时状态的数据信息，比如系统中处于pending状态的job。
2. Counter
Counter是Gauge的一个特例，维护一个计数器，可以通过inc()和dec()方法对计数器做修改。使用步骤与Gauge基本类似，在MetricRegistry中提供了静态方法可以直接实例化一个Counter。
3. Meters
Meters用来度量某个时间段的平均处理次数（request per second），每1、5、15分钟的TPS。比如一个service的请求数，通过metrics.meter()实例化一个Meter之后，然后通过meter.mark()方法就能将本次请求记录下来。统计结果有总的请求数，平均每秒的请求数，以及最近的1、5、15分钟的平均TPS。
4. Histograms
Histograms主要使用来统计数据的分布情况，最大值、最小值、平均值、中位数，百分比（75%、90%、95%、98%、99%和99.9%）。例如，需要统计某个页面的请求响应时间分布情况，可以使用该种类型的Metrics进行统计。
5. Timers
Timers主要是用来统计某一块代码段的执行时间以及其分布情况，具体是基于Histograms和Meters来实现的。

## Health Checks
Metrics提供了一个独立的模块：Health Checks，用于对Application、其子模块或者关联模块的运行是否正常做检测。该模块是独立metrics-core模块的，使用时则导入metrics-healthchecks包。  

```xml 
	<dependency>                                    
		<groupId>com.codahale.metrics</groupId>       
		<artifactId>metrics-healthchecks</artifactId> 
		<version>3.0.1</version>         
	</dependency>
```  

使用起来和与上述几种类型的Metrics有点类似，但是需要重新实例化一个Metrics容器HealthCheckRegistry，待检测模块继承抽象类HealthCheck并实现check()方法即可，然后将该模块注册到HealthCheckRegistry中，判断的时候通过isHealthy()接口即可。

## 其他支持

metrics提供了对Ehcache、Apache HttpClient、JDBI、Jersey、Jetty、Log4J、Logback、JVM等的集成，可以方便地将Metrics输出到Ganglia、Graphite中，供用户图形化展示。

## 参考资料

*http://metrics.codahale.com/*

*https://github.com/dropwizard/metrics*

*http://blog.csdn.net/scutshuxue/article/details/8350135*

*http://blog.synyx.de/2013/09/yammer-metrics-made-easy-part-i/*

*http://blog.synyx.de/2013/09/yammer-metrics-made-easy-part-ii/*

*http://wiki.apache.org/hadoop/HADOOP-6728-MetricsV2*