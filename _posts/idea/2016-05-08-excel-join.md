---
layout: post
title: 'Excel 整合报表'
date: 2016-05-08
author: huang
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: excel
---

我们常常需要从日志，数据库中取大量的数，这些记录常常是离散的，但业务需要将他们根据关键字合并，这项任务都有哪些做法呢？

1. SQL join，不是一直都有这样的机会。
1. join，bash 的 join 命令。
2. 写一段python 处理。
3. Excel处理。

join操作或者join命令只能处理比较理想的情形，比如当join key缺失的时候，比如需要从多个key到不同列的尝试匹配。

在整合报表的时候，是一个反复尝试、预览的过程，这个时候命令行需要重复调整参数，并不比GUI更高效，不好调试，也容易遗漏。

Python是万能的，若每次遇到问题都从open,readline,split开始，不停的针对业务写脚本，并逐步抽取成一套工具集，为什么不直接 **Find One** 呢。

## Excel很棒
Excel是非常棒的工具，除非要处理的是一个超大的表格。

Excel用于整合报表的函数主要是 `VLOOKUP` `LOOKUP`，且听我一一道来。

首先，我们的案例是sheet1 和 sheet2 通过 sheet3 连接，说人话大概是**表1**有关键字K1，数据KX，KY，**表2**有关键字K2，数据KX，KY，**表3**有K1、K2的关系。在这里通过K1,K2,KX,KY都不一定能在sheet1找到sheet2的对应。

以上是我之前处理的一个问题的简化，已经算复杂到不想用join了吧，每次写脚本呢又觉得很麻烦，其实看似问题清晰，但写程序的工作非常繁琐，涉及到搜索资料，调试排错，重构等等时间开销，一次性的开发，性价比并不高。

首先，在sheet1表通过`VLOOKUP`在sheet3查找K2：
```
=VLOOKUP(K1, sheet3, index for K2)
```

查找后，发现有些结果是`N/A`，这就需要在sheet1借助KX, KY，在sheet2查找K2：
```
=VLOOKUP(KX, sheet2, index for K2)
=VLOOKUP(KY, sheet2, index for K2)
```

sheet1新增的三列查找结果，都包含一些`N/A`，但所幸总算保证每条记录都找到K2。为了把sheet2的数据搬到sheet1的对应位置，现在要想办法把每条记录的`N/A`，空值，K2合并到一起。这里用到了`LOOKUP`
### tricky
```
=LOOKUP(1, 1/(X1:Z1<>""), X1:Z1)
```
括号里的不等判断返回TRUE,FALSE，所以返回的就是1或者除0，自然能找到结果。

准确求出K2以后，将sheet2的数据取过来很简单，还是用`VLOOKUP`，只不过这次是横向填充，因此需要准备一个sheet4用于index递增。另外第一个参数的列位置要锁定，行值则不用。

```
=VLOOKUP($L2,Sheet2!$A$2:$Q$412,Sheet6!A$1,FALSE)
```