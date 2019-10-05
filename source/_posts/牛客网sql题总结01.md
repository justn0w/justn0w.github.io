---
title: 牛客网sql题总结01
date: 2019-09-20 20:55:40
tags: sql
categories: "数据库"
---

#### 1. 聚集函数的位置
> 聚集函数：运行在行组上，计算和返回单个值的函数

* 平均值：avg
* 最小值：min
* 最大值：max
* 总和：  sum 
* 计数：  count

聚集函数的位置在，select 和 from之间。
如：
```sql
select count(*) from username;
```

#### 2. mysql语句的执行顺序

```sql
<SELECT clause> [<FROM clause>] [<WHERE clause>] [<GROUP BY clause>] [<HAVING clause>] [<ORDER BY clause>] [desc | asc] [<LIMIT clause>]   
```


#### 3. 区分内联结和外联结，讲的非常好！
https://blog.csdn.net/plg17/article/details/78758593


