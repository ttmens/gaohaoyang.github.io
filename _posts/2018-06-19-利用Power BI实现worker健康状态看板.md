---
layout: post
title:  "利用Power BI实现Worker健康状态看板"
date:   2018-06-19 10:36:54
categories: work
tags: software PowerBI
---

* content
{:toc}

本文主要介绍使用Power BI实现工作过程中的一个需求。在选择使用Power BI之前，我们考虑过搭建一个前台架构用来展示我们所需要的数据，但代价很大  鉴于有现成的Power BI服务器，我们直接使用该工具。




```
限于网络原因，无法上传图片，以下内容只能通过文字表述。
```

## 背景

日常情况下，需要管理2000+台PC，由于这些PC分布在不同地域的实验室，且PC经常发生无法PING通、无法SSH、变为文件只读系统等故障。需要实时了解所有PC的状态，故有了想要做一个小工具，能够随时展现所有PC状态的想法。

具体的原始数据获取过程，简述如下：从两个数据库取原始数据，清洗后将所有PC的IP送到ansible服务器，利用ansible逐个判断每一台PC的状态，是否PING通？是否SSH上？是否文件只读？得到PC的健康状态数据后，将数据整合到一起，上传至固定路径的服务器上。整个过程通过Python和Shell脚本完成。

## Power BI的呈现过程

上文中已经提过，鉴于现有Power BI服务器，切小工具的定位是只需要展示数据，故没有考虑Django，Flask等框架。

### 数据获取

这一块没什么好说的，直接csv导入。

### 数据建模分析

点击“编辑查询”，按照业务逻辑对数据进行各种分析。举例如下：实际业务中，我们需要统计每台PC的运行频率，采用最后构建时间这个指标来衡量，python从数据库中取数据时已经计算得到每一台PC的最后构建时间，为了便于展现，我们按照业务需要对其进行分组。

```
新建自定义列
列名：Last_Build
公式：
if [#"last_build_time - Copy.1"]>=0 and [#"last_build_time - Copy.1"]<7 then "A一周内构建" else if [#"last_build_time - Copy.1"]>=7 and [#"last_build_time - Copy.1"]<15 then "B一周到半个月内构建"  else if [#"last_build_time - Copy.1"]>=15 and [#"last_build_time - Copy.1"]<30 then "C半个月到一个月内构建" else if [#"last_build_time - Copy.1"]>=30 and [#"last_build_time - Copy.1"]<60 then "D一个月到两个月内构建" else if [#"last_build_time - Copy.1"]>=60 and [#"last_build_time - Copy.1"]<180 then "E两个月到半年内构建" else "F半年内无构建"
```

完成所有的数据处理后，关闭&更新。回到报表界面。

### 数据可视

按照业务逻辑，添加适合自己数据表达形式的图表，按照业务需要适当设置数据的钻取以及关系展示。


## 总结

上面主要介绍了一个实际工作过程中使用Power BI的案例，相比于Tableau，Power BI的建模过程其实更复杂一些，且对于各种展示图表，相对较少。在实际的工作中，如果有需要定期做报表展示一些存在一定逻辑关系的数据时，使用Power BI或Tableau这类软件实现是非常值得考虑的。

