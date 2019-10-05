---
title: sql学习知识汇总02
date: 2019-09-12 14:51:21
tags: sql基础知识
categories: 数据库
---

第二部分
<!-- more -->
#### 0x07 创建计算字段
##### 7.1 计算字段
>字段 基本上与列(column)的意思相同，经常互换使用，不过数据库列一般称为列，而术语字段通常用在计算字段的连接上。

##### 7.2 拼接字段
>拼接(concatenate) 将值联结到一起构成单个值。

(1) 使用concat函数来解决
```sql
将vend_name和vend_country拼接为name(location)格式

select concat(vend_name, ' (', vend_country, ')') from vendors order by vend_name;
+--------------------------------------------+
| concat(vend_name, ' (', vend_country, ')') |
+--------------------------------------------+
| ACME (USA)                                 |
| Anvils R Us (USA)                          |
| Furball Inc. (USA)                         |
| Jet Set (England)                          |
| Jouets Et Ours (France)                    |
| LT Supplies (USA)                          |
+--------------------------------------------+
```
##### 7.3 使用别名
>别名(alias)是一个字段或值的替换名。别名用AS关键字赋予。

```sql
select concat(vend_name, '(', vend_country, ')') AS vend_title from vendors order by vend_name;
+------------------------+
| vend_title             |
+------------------------+
| ACME(USA)              |
| Anvils R Us(USA)       |
| Furball Inc.(USA)      |
| Jet Set(England)       |
| Jouets Et Ours(France) |
| LT Supplies(USA)       |
+------------------------+
```
##### 7.4 执行算术计算

下面的SQL语句检索订单号20005中的所有物品：
```sql
select prod_id, quantity, item_price from orderitems where order_num = 20005;
+---------+----------+------------+
| prod_id | quantity | item_price |
+---------+----------+------------+
| ANV01   |       10 |       5.99 |
| ANV02   |        3 |       9.99 |
| TNT2    |        5 |      10.00 |
| FB      |        1 |      10.00 |
+---------+----------+------------+
```
如下是汇总物品的价格(单价乘以订购数量):

```sql
select prod_id, quantity, item_price, quantity*item_price as expanded_price from orderitems where order_num = 20005;
+---------+----------+------------+----------------+
| prod_id | quantity | item_price | expanded_price |
+---------+----------+------------+----------------+
| ANV01   |       10 |       5.99 |          59.90 |
| ANV02   |        3 |       9.99 |          29.97 |
| TNT2    |        5 |      10.00 |          50.00 |
| FB      |        1 |      10.00 |          10.00 |
+---------+----------+------------+----------------+
```
select now(),返回当前的日期和时间。

#### 0x08 使用数据处理函数

##### 8.1 文本处理函数
Upper()函数，将文本转换为大写
```sql
select vend_name, Upper(vend_name) as vend_name_upcase from vendors order by vend_name;
+----------------+------------------+
| vend_name      | vend_name_upcase |
+----------------+------------------+
| ACME           | ACME             |
| Anvils R Us    | ANVILS R US      |
| Furball Inc.   | FURBALL INC.     |
| Jet Set        | JET SET          |
| Jouets Et Ours | JOUETS ET OURS   |
| LT Supplies    | LT SUPPLIES      |
+----------------+------------------+
```
##### 8.2 日期和时间处理函数
常用日期和时间处理函数，如下表所示
|函数|说明|
|:--:|:--:|
|AddDate() |增加一个日期（天、周等）|
|AddTime() |增加一个时间（时、分等）|
|CurDate() |返回当前日期|
|CurTime() |返回当前时间
|Date() |返回日期时间的日期部分|
|DateDiff() |计算两个日期之差|
|Date_Add() |高度灵活的日期运算函数|
|Date_Format() |返回一个格式化的日期或时间串|
|Day() |返回一个日期的天数部分|
|DayOfWeek() |对于一个日期，返回对应的星期几|
|Hour() |返回一个时间的小时部分|
|Minute() |返回一个时间的分钟部分|
|Month() |返回一个日期的月份部分|
|Now() |返回当前日期和时间|
|Second() |返回一个时间的秒部分|
|Time() |返回一个日期时间的时间部分|
|Year() |返回一个日期的年份部分|

需要注意mysql使用的日期格式。无论什么时候指定一个日期，不管是插入或更新表值还是用where字句进行过滤，日期格式最好为yyyy-mm-dd

```sql
一个基本的日期操作
select cust_id, order_num from orders where order_date = '2005-09-01';
+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10001 |     20005 |
+---------+-----------+
```
如果字段中的内容为： 2019-09-12 15:33:58
但是却使用order_date = '2019-09-12'，显然会查找失败。这时候可以使用Date()函数

```sql
select cust_id, order_num from orders where Date(order_date) = '2019-09-12';
+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10003 |     20010 |
|   10003 |     20011 |
|   10003 |     20012 |
|   10003 |     20013 |
|   10003 |     20014 |
|   10003 |     20015 |
|   10003 |     20016 |
|   10003 |     20017 |
|   10003 |     20018 |
|   10003 |     20019 |
|   10003 |     20020 |
|   10003 |     20021 |
|   10003 |     20022 |
|   10003 |     20023 |
|   10003 |     20024 |
|   10003 |     20025 |
|   10003 |     20026 |
|   10003 |     20027 |
|   10003 |     20028 |
|   10003 |     20029 |
|   10003 |     20030 |
|   10003 |     20031 |
|   10003 |     20032 |
|   10003 |     20033 |
|   10003 |     20034 |
|   10003 |     20035 |
|   10003 |     20036 |
|   10003 |     20037 |
|   10003 |     20038 |
|   10003 |     20039 |
|   10003 |     20040 |
|   10003 |     20041 |
|   10003 |     20042 |
|   10003 |     20043 |
|   10003 |     20044 |
|   10003 |     20045 |
|   10003 |     20046 |
|   10003 |     20047 |
|   10003 |     20048 |
|   10003 |     20049 |
|   10003 |     20050 |
|   10003 |     20051 |
|   10003 |     20052 |
|   10003 |     20053 |
|   10003 |     20054 |
|   10003 |     20055 |
|   10003 |     20056 |
|   10003 |     20057 |
|   10003 |     20058 |
+---------+-----------+
```
---
如果想要检索处2005年9月下的所有订单，可以使用如下方法：
方法一：
```sql
select cust_id, order_num from orders where Date(order_date) between '2005-09-01' and '2005-09-30';
+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10001 |     20005 |
|   10003 |     20006 |
|   10004 |     20007 |
+---------+-----------+
```
方法二：
不要记住每个月中有多少天或不需要操心闰年2月！
```sql
select cust_id, order_num from orders where year(order_date) = 2005 and month(order_date)=9;
+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10001 |     20005 |
|   10003 |     20006 |
|   10004 |     20007 |
+---------+-----------+
```

#### 0x09 汇总数据
##### 9.1 聚集函数
经常需要汇总数据，而不用把它们实际检索出来，为此MySQL提供了专门你的函数。
- 确定表中行数（或者满足某个条件或包含某个特定值的行数）。
- 获得表中行组的和。
- 找出表列(或所有行或某些特定的行)的最大值、最小值和平均值。

(1) AVG函数，返回某列的平均值
```sql
select AVG(prod_price) as avg_price from products;
+-----------+
| avg_price |
+-----------+
| 16.133571 |
+-----------+
```
返回特定行的平均价格
```sql
select avg(prod_price) as avg_price from products where vend_id=1003;
+-----------+
| avg_price |
+-----------+
| 13.212857 |
+-----------+
```
(2) count()函数
count函数，进行统计
* 使用count(*)对表中行的数目进行计数，不管表列中包含的是空值(NULL)还是非空值
* 使用count(column)对特定列中具有值的行进行计数，忽略NULL值

```sql
select count(*) as num_cust from customers;
+----------+
| num_cust |
+----------+
|        5 |
+----------+
```

```sql
select count(cust_email) as num_cust from customers;
+----------+
| num_cust |
+----------+
|        3 |
+----------+
```
(3) max()函数
max()函数返回指定列中的最大值。max()要求指定列名，如下所示：
```sql
select max(prod_price) as max_price from products;
+-----------+
| max_price |
+-----------+
|     55.00 |
+-----------+
```
(4) min()函数
返回指定列的最小值。与max()函数一样，min()要求指定列名
```sql
select min(prod_price) as min_price from products;
+-----------+
| min_price |
+-----------+
|      2.50 |
+-----------+
```
(5) sum()函数
sum()函数用来返回特定列值的和(总计)
```sql
select sum(quantity) as items_ordered from orderitems where order_num=20005;
+---------------+
| items_ordered |
+---------------+
|            19 |
+---------------+
```

sum()可以用来合计计算值。合计物品的item_price*quantity，得出总得订单金额
```sql
select sum(item_price*quantity) as total_price from orderitems where order_num=20005;
+-------------+
| total_price |
+-------------+
|      149.87 |
+-------------+
```
##### 9.2 聚集不同值

以上5个聚集函数都可以如下使用：
* 对所有的行执行计算，指定ALL参数或不给参数(因为ALL是默认行为)
* 只包含不同的值，指定DISTINCT参数


```sql
返回特定供应商提供的产品的平均价格
select avg(distinct prod_price) as avg_price from products where vend_id=1003;
+-----------+
| avg_price |
+-----------+
| 15.998000 |
+-----------+
```

##### 9.3 组合聚集函数

```sql
select count(*) as num_items, min(prod_price) as price_min, max(prod_price) as price_max, avg(prod_price) as price_avg from products;
+-----------+-----------+-----------+-----------+
| num_items | price_min | price_max | price_avg |
+-----------+-----------+-----------+-----------+
|        14 |      2.50 |     55.00 | 16.133571 |
+-----------+-----------+-----------+-----------+
```

#### 10 分组数据
允许把数据分为多个逻辑组，以便能对每个组进行聚集计算。

##### 10.1 创建分组
通过select语句的Group by 子句中建立的。
```sql
按照vend_id进行分组，并统计每个分组的个数
select vend_id, count(*) as num_prods from products group by vend_id;
+---------+-----------+
| vend_id | num_prods |
+---------+-----------+
|    1001 |         3 |
|    1002 |         2 |
|    1003 |         7 |
|    1005 |         2 |
+---------+-----------+
```

* group by 子句必须出现在where子句之后，order by子句之前。
* 除聚集计算语句外，select语句中的每个列都必须在group by子句中给出

##### 10.2 过滤分组
mysql允许过滤分组，规定包括哪些分组，排除哪些分组。
* where 过滤指定的是行而不是分组，having过滤分组

```sql
select cust_id, count(*) as orders from orders group by cust_id having count(*) >= 2;
+---------+--------+
| cust_id | orders |
+---------+--------+
|   10001 |      2 |
|   10003 |     50 |
+---------+--------+
```

```sql
列出具有2个(含)以上、价格为10(含)以上的产品的供应商
select vend_id, count(*) as num_prods from products where prod_price >= 10 group by 
vend_id having count(*) >= 2;
+---------+-----------+
| vend_id | num_prods |
+---------+-----------+
|    1003 |         4 |
|    1005 |         2 |
+---------+-----------+
```
where子句过滤所有prod_price至少为10的行。然后按照vend_id分组数据，having子句过滤计数为2或2以上的分组。

##### 10.3 分组和排序

```sql
检索总结订单价格大于等于50的订单的订单号和总结订单价格：
select order_num, sum(quantity*item_price) as ordertotal from orderitems group by order_num having 
sum(quantity*item_price) >= 50;

+-----------+------------+
| order_num | ordertotal |
+-----------+------------+
|     20005 |     149.87 |
|     20006 |      55.00 |
|     20007 |    1000.00 |
|     20008 |     125.00 |
+-----------+------------+

为了总计订单价格排序输出，需要添加order by子句，如下所示
select order_num, sum(quantity*item_price) as ordertotal from orderitems group by order_num having 
sum(quantity*item_price) >= 50 order by ordertotal;
+-----------+------------+
| order_num | ordertotal |
+-----------+------------+
|     20006 |      55.00 |
|     20008 |     125.00 |
|     20005 |     149.87 |
|     20007 |    1000.00 |
+-----------+------------+
```

##### 10.4 select子句顺序
由上到下，表示从左到右的顺序

|子句|说明|是否必须使用|
|:--:|:--:|:--:|
|SELECT |要返回的列或表达式 |是|
|FROM |从中检索数据的表 |仅在从表选择数据时使用|
|WHERE |行级过滤 |否|
|GROUP BY |分组说明 |仅在按组计算聚集时使用|
|HAVING |组级过滤 |否|
|ORDER BY |输出排序顺序 |否|
|LIMIT |要检索的行数 |否|

#### 11 使用子查询
##### 11.1 子查询
即嵌套在其他查询中的查询
##### 11.2 利用子查询进行过滤
orders表：存储用户id、订单的num、购买日期
customers表：存储用户的id，以及用户的其他信息
orderitems表：存储订单的num、订单个数、商品的id、单价等。
现在，假如需要列出订购物品TNT2的所有客户。步骤如下：
(1) 检索包含物品TNT2所有订单的编号
```sql
select order_num from orderitems where prod_id='TNT2';
+-----------+
| order_num |
+-----------+
|     20005 |
|     20007 |
+-----------+
```
(2) 检索具有前一步骤列出的订单编号的所有客户的ID。
```sql
select cust_id from orders where order_num in (20005, 20007);
+---------+
| cust_id |
+---------+
|   10001 |
|   10004 |
+---------+
```
(3) 检索前一步骤返回的所有客户ID的客户信息
```sql
select cust_name, cust_contact from customers where cust_id in (10001, 10004);
+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
```
可以将上面三个步骤写到一块儿：
```sql
select cust_name, cust_contact from customers where cust_id in (select cust_id from orders where order_num in (select order_num from orderitems where prod_id="TNT2"));
+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
```

##### 11.3 作为计算字段使用子查询
需要显示customers表中每个客户的订单总数。
(1) 从customers表中检索客户列表
(2) 对于检索出的每个用户，统计其在orders表中的订单数目。
```sql
select cust_name, cust_state, 
        (select count(*) from orders where orders.cust_id = customers.cust_id) as orders
from customers
order by cust_name;

+----------------+------------+--------+
| cust_name      | cust_state | orders |
+----------------+------------+--------+
| Coyote Inc.    | MI         |      2 |
| E Fudd         | IL         |      1 |
| Mouse House    | OH         |      0 |
| Wascals        | IN         |     50 |
| Yosemite Place | AZ         |      1 |
+----------------+------------+--------+
```
分析：
(1) 共返回三列：cust_name、cust_state和orders。其中orders是一个计算字段，它是由圆括号中的子查询建立的。该子查询对检索出的每个客户执行一次。子查询执行了5次，共检索出了5个客户。
(2) 子查询中的where子句使用了完全限定列名，其中
where orders.cust_id = customers.cust_id 为相关子查询(correlated subquery) 涉及外部查询的子查询。


#### 12 连接表
##### 12.1 联接(join)
(1) 关系表
关系表的设计就是要保证把信息分解成多个表，一类数据一个表。各表通过某些常用的值(即关系设计中的关系(relational))互相关联
(2)为什么要使用联接
分解数据为多个表能更有效地存储，更方便地处理，并且具有更大的可伸缩性。现在问题是，如何将数据存储在多个表中，怎样用单条select语句检索出数据，这就是联接的作用。使用特殊的语法，可以联接多个表返回一组输出，联接在运行时关联表中正确的行。

##### 12.2 创建联接
看一个简单的例子
```sql
select vend_name, prod_name, prod_price from vendors, products where vendors.vend_id = products.vend_id order by vend_name, prod_name;
+-------------+----------------+------------+
| vend_name   | prod_name      | prod_price |
+-------------+----------------+------------+
| ACME        | Bird seed      |      10.00 |
| ACME        | Carrots        |       2.50 |
| ACME        | Detonator      |      13.00 |
| ACME        | Safe           |      50.00 |
| ACME        | Sling          |       4.49 |
| ACME        | TNT (1 stick)  |       2.50 |
| ACME        | TNT (5 sticks) |      10.00 |
| Anvils R Us | .5 ton anvil   |       5.99 |
| Anvils R Us | 1 ton anvil    |       9.99 |
| Anvils R Us | 2 ton anvil    |      14.99 |
| Jet Set     | JetPack 1000   |      35.00 |
| Jet Set     | JetPack 2000   |      55.00 |
| LT Supplies | Fuses          |       3.42 |
| LT Supplies | Oil can        |       8.99 |
+-------------+----------------+------------+
```
分析：
from后出现两个表vendors、products;where子句指示mysql匹配vendors表中的vend_id和products表中的vend_id;

(1) where子句的重要性
> 笛卡尔积(cartesian product) 由没有联接条件的表关系返回的结果为笛卡尔积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

以下是，没有使用where子句的效果：
```sql
select vend_name, prod_name, prod_price from vendors, products order by vend_name, prod_name;
+----------------+----------------+------------+
| vend_name      | prod_name      | prod_price |
+----------------+----------------+------------+
| ACME           | .5 ton anvil   |       5.99 |
| ACME           | 1 ton anvil    |       9.99 |
| ACME           | 2 ton anvil    |      14.99 |
| ACME           | Bird seed      |      10.00 |
| ACME           | Carrots        |       2.50 |
| ACME           | Detonator      |      13.00 |
| ACME           | Fuses          |       3.42 |
| ACME           | JetPack 1000   |      35.00 |
| ACME           | JetPack 2000   |      55.00 |
| ACME           | Oil can        |       8.99 |
| ACME           | Safe           |      50.00 |
| ACME           | Sling          |       4.49 |
| ACME           | TNT (1 stick)  |       2.50 |
| ACME           | TNT (5 sticks) |      10.00 |
| Anvils R Us    | .5 ton anvil   |       5.99 |
| Anvils R Us    | 1 ton anvil    |       9.99 |
| Anvils R Us    | 2 ton anvil    |      14.99 |
| Anvils R Us    | Bird seed      |      10.00 |
| Anvils R Us    | Carrots        |       2.50 |
| Anvils R Us    | Detonator      |      13.00 |
| Anvils R Us    | Fuses          |       3.42 |
| Anvils R Us    | JetPack 1000   |      35.00 |
| Anvils R Us    | JetPack 2000   |      55.00 |
| Anvils R Us    | Oil can        |       8.99 |
| Anvils R Us    | Safe           |      50.00 |
| Anvils R Us    | Sling          |       4.49 |
| Anvils R Us    | TNT (1 stick)  |       2.50 |
| Anvils R Us    | TNT (5 sticks) |      10.00 |
| Furball Inc.   | .5 ton anvil   |       5.99 |
| Furball Inc.   | 1 ton anvil    |       9.99 |
| Furball Inc.   | 2 ton anvil    |      14.99 |
| Furball Inc.   | Bird seed      |      10.00 |
| Furball Inc.   | Carrots        |       2.50 |
| Furball Inc.   | Detonator      |      13.00 |
| Furball Inc.   | Fuses          |       3.42 |
| Furball Inc.   | JetPack 1000   |      35.00 |
| Furball Inc.   | JetPack 2000   |      55.00 |
| Furball Inc.   | Oil can        |       8.99 |
| Furball Inc.   | Safe           |      50.00 |
| Furball Inc.   | Sling          |       4.49 |
| Furball Inc.   | TNT (1 stick)  |       2.50 |
| Furball Inc.   | TNT (5 sticks) |      10.00 |
| Jet Set        | .5 ton anvil   |       5.99 |
| Jet Set        | 1 ton anvil    |       9.99 |
| Jet Set        | 2 ton anvil    |      14.99 |
| Jet Set        | Bird seed      |      10.00 |
| Jet Set        | Carrots        |       2.50 |
| Jet Set        | Detonator      |      13.00 |
| Jet Set        | Fuses          |       3.42 |
| Jet Set        | JetPack 1000   |      35.00 |
| Jet Set        | JetPack 2000   |      55.00 |
| Jet Set        | Oil can        |       8.99 |
| Jet Set        | Safe           |      50.00 |
| Jet Set        | Sling          |       4.49 |
| Jet Set        | TNT (1 stick)  |       2.50 |
| Jet Set        | TNT (5 sticks) |      10.00 |
| Jouets Et Ours | .5 ton anvil   |       5.99 |
| Jouets Et Ours | 1 ton anvil    |       9.99 |
| Jouets Et Ours | 2 ton anvil    |      14.99 |
| Jouets Et Ours | Bird seed      |      10.00 |
| Jouets Et Ours | Carrots        |       2.50 |
| Jouets Et Ours | Detonator      |      13.00 |
| Jouets Et Ours | Fuses          |       3.42 |
| Jouets Et Ours | JetPack 1000   |      35.00 |
| Jouets Et Ours | JetPack 2000   |      55.00 |
| Jouets Et Ours | Oil can        |       8.99 |
| Jouets Et Ours | Safe           |      50.00 |
| Jouets Et Ours | Sling          |       4.49 |
| Jouets Et Ours | TNT (1 stick)  |       2.50 |
| Jouets Et Ours | TNT (5 sticks) |      10.00 |
| LT Supplies    | .5 ton anvil   |       5.99 |
| LT Supplies    | 1 ton anvil    |       9.99 |
| LT Supplies    | 2 ton anvil    |      14.99 |
| LT Supplies    | Bird seed      |      10.00 |
| LT Supplies    | Carrots        |       2.50 |
| LT Supplies    | Detonator      |      13.00 |
| LT Supplies    | Fuses          |       3.42 |
| LT Supplies    | JetPack 1000   |      35.00 |
| LT Supplies    | JetPack 2000   |      55.00 |
| LT Supplies    | Oil can        |       8.99 |
| LT Supplies    | Safe           |      50.00 |
| LT Supplies    | Sling          |       4.49 |
| LT Supplies    | TNT (1 stick)  |       2.50 |
| LT Supplies    | TNT (5 sticks) |      10.00 |
+----------------+----------------+------------+
```
这个结果只是将第一表中的每个行将第二个表中的每个行配对，而不管它们逻辑上是否可以配在一起。

(2) 内部联接

看语句
```sql 
select vend_name, prod_name, prod_price from vendors inner join products on vendors.vend_id = products.vend_id;

+-------------+----------------+------------+
| vend_name   | prod_name      | prod_price |
+-------------+----------------+------------+
| Anvils R Us | .5 ton anvil   |       5.99 |
| Anvils R Us | 1 ton anvil    |       9.99 |
| Anvils R Us | 2 ton anvil    |      14.99 |
| LT Supplies | Fuses          |       3.42 |
| LT Supplies | Oil can        |       8.99 |
| ACME        | Detonator      |      13.00 |
| ACME        | Bird seed      |      10.00 |
| ACME        | Carrots        |       2.50 |
| ACME        | Safe           |      50.00 |
| ACME        | Sling          |       4.49 |
| ACME        | TNT (1 stick)  |       2.50 |
| ACME        | TNT (5 sticks) |      10.00 |
| Jet Set     | JetPack 1000   |      35.00 |
| Jet Set     | JetPack 2000   |      55.00 |
+-------------+----------------+------------+
```
与之前使用where子句时返回的结果一致。<br>
两个表之间的关系时from子句的组成部分，以inner join指定。<br>
在使用这种语法时，联接条件用特定的on子句，而不是where子句给出。<br>
传递给on的实际条件与传递给where的相同。

(3) 联接多个表

```sql
select prod_name, vend_name, prod_price, quantity from orderitems, products, vendors where products.vend_id = vendors.vend_id and orderitems.prod_id = products.prod_id and order_num = 20005;
+----------------+-------------+------------+----------+
| prod_name      | vend_name   | prod_price | quantity |
+----------------+-------------+------------+----------+
| .5 ton anvil   | Anvils R Us |       5.99 |       10 |
| 1 ton anvil    | Anvils R Us |       9.99 |        3 |
| TNT (5 sticks) | ACME        |      10.00 |        5 |
| Bird seed      | ACME        |      10.00 |        1 |
+----------------+-------------+------------+----------+
```
分析：
此例子显示编号为20005的订单中的物品。订单物品存储在orderitems表中。<br>
匹配的是：订单编号为20005的订单，然后匹配20005对应的产品id(orderitems.prod_id)与products表中的id，通过这些条件将订单编号为20005的订单、其对应的产品的id、产品的id所对应的供应商id全部匹配出来。
<br>
返回订购产品TNT2(为orderitems中的prod_id字段)的客户列表：

```sql
select cust_name,cust_contact from orderitems, orders, customers where customers.cust_id = orders.cust_id and orders.order_num = orderitems.order_num and prod_id = "TNT2";

+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
```

分析：倒着看，就很好理解

#### 13 创建高级联结

##### 13.1 使用表别名
如：
```sql
select cust_name, cust_contact from customers as c, orders as o, orderitems as oi
where c.cust_id = o.cust_id 
and oi.order_num = o.order_num
and prod_id='TNT2';

+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
```
```orders as o```，以o作为orders的别名。与列别民不一样，表别名不返回到客户机

##### 13.2 使用不同类型的联结
(1) 自联结

查询生产prod_id为DTNTR的供应商提供了的全部产品
```sql
select prod_id, prod_name from products where vend_id = (select vend_id from products where prod_id = 'DTNTR');

+---------+----------------+
| prod_id | prod_name      |
+---------+----------------+
| DTNTR   | Detonator      |
| FB      | Bird seed      |
| FC      | Carrots        |
| SAFE    | Safe           |
| SLING   | Sling          |
| TNT1    | TNT (1 stick)  |
| TNT2    | TNT (5 sticks) |
+---------+----------------+
```

使用自联结
```sql
select p1.prod_id, p1.prod_name from products as p1, products as p2 where p1.vend_id = p2.vend_id
and p2.prod_id = 'DTNTR';

+---------+----------------+
| prod_id | prod_name      |
+---------+----------------+
| DTNTR   | Detonator      |
| FB      | Bird seed      |
| FC      | Carrots        |
| SAFE    | Safe           |
| SLING   | Sling          |
| TNT1    | TNT (1 stick)  |
| TNT2    | TNT (5 sticks) |
+---------+----------------+
```
相同表的联结

(2) 自然联结
自然联结排除多次出现，使每个列只返回一次。

```sql
select c.*, o.order_num, o.order_date, oi.prod_id, oi.quantity, oi.item_price from 
customers as c, orders as o, orderitems as oi
where c.cust_id = o.cust_id
and oi.order_num = o.order_num
and prod_id = 'FB';
+---------+-------------+----------------+-----------+------------+----------+--------------+--------------+-----------------+-----------+---------------------+---------+----------+------------+
| cust_id | cust_name   | cust_address   | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email      | order_num | order_date          | prod_id | quantity | item_price |
+---------+-------------+----------------+-----------+------------+----------+--------------+--------------+-----------------+-----------+---------------------+---------+----------+------------+
|   10001 | Coyote Inc. | 200 Maple Lane | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com |     20005 | 2005-09-01 00:00:00 | FB      |        1 |      10.00 |
|   10001 | Coyote Inc. | 200 Maple Lane | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com |     20009 | 2005-10-08 00:00:00 | FB      |        1 |      10.00 |
+---------+-------------+----------------+-----------+------------+----------+--------------+--------------+-----------------+-----------+---------------------+---------+----------+------------+
```
在这个列子中，虽然我们使用的是c的通配符，但是c的每一列只出现了一次

(3) 外部联结
先来一个内联结,表示检索有订单的客户
```sql
select customers.cust_id, orders.order_num from customers inner join orders on customers.cust_id = orders.cust_id;

+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10001 |     20005 |
|   10001 |     20009 |
|   10003 |     20006 |
|   10003 |     20010 |
|   10003 |     20011 |
|   10003 |     20012 |
|   10003 |     20013 |
|   10003 |     20014 |
|   10003 |     20015 |
|   10003 |     20016 |
|   10003 |     20017 |
|   10003 |     20018 |
|   10003 |     20019 |
|   10003 |     20020 |
|   10003 |     20021 |
|   10003 |     20022 |
|   10003 |     20023 |
|   10003 |     20024 |
|   10003 |     20025 |
|   10003 |     20026 |
|   10003 |     20027 |
|   10003 |     20028 |
|   10003 |     20029 |
|   10003 |     20030 |
|   10003 |     20031 |
|   10003 |     20032 |
|   10003 |     20033 |
|   10003 |     20034 |
|   10003 |     20035 |
|   10003 |     20036 |
|   10003 |     20037 |
|   10003 |     20038 |
|   10003 |     20039 |
|   10003 |     20040 |
|   10003 |     20041 |
|   10003 |     20042 |
|   10003 |     20043 |
|   10003 |     20044 |
|   10003 |     20045 |
|   10003 |     20046 |
|   10003 |     20047 |
|   10003 |     20048 |
|   10003 |     20049 |
|   10003 |     20050 |
|   10003 |     20051 |
|   10003 |     20052 |
|   10003 |     20053 |
|   10003 |     20054 |
|   10003 |     20055 |
|   10003 |     20056 |
|   10003 |     20057 |
|   10003 |     20058 |
|   10004 |     20007 |
|   10005 |     20008 |
+---------+-----------+
```

外部联结，检索所有客户，包括哪些没有订单的客户（也就是在orders中没有出现的cust_id或者customers.cust_id != orders.cust_id的那一部分）
```sql
select customers.cust_id, orders.order_num from customers left outer join orders on customers.cust_id = orders.cust_id;
```
outer join表明是外部联结，其中left outer join指出的是左边的表。上边的例子是选择customers表的所有行。
##### 13.3 使用带聚集函数的联结

检索所有客户及每个客户所下的订单数，可以使用count()函数来完成
```sql
select customers.cust_name, customers.cust_id, count(orders.order_num) as num_ord from 
customers inner join orders on 
customers.cust_id = orders.cust_id group by customers.cust_id;

+----------------+---------+---------+
| cust_name      | cust_id | num_ord |
+----------------+---------+---------+
| Coyote Inc.    |   10001 |       2 |
| Wascals        |   10003 |      50 |
| Yosemite Place |   10004 |       1 |
| E Fudd         |   10005 |       1 |
+----------------+---------+---------+
```


聚集函数也可以方便地与其他联结一起使用,如外部联结

```sql
select customers.cust_name, customers.cust_id, count(orders.order_num) as num_ord
from customers left outer join orders on customers.cust_id = orders.cust_id
group by customers.cust_id;

+----------------+---------+---------+
| cust_name      | cust_id | num_ord |
+----------------+---------+---------+
| Coyote Inc.    |   10001 |       2 |
| Mouse House    |   10002 |       0 |
| Wascals        |   10003 |      50 |
| Yosemite Place |   10004 |       1 |
| E Fudd         |   10005 |       1 |
+----------------+---------+---------+

```
##### 13.4 使用联结和联结条件

- 注意所使用的联结类型。一般我们使用内部联结，但使用外部联结也是有效的
- 保证使用正确的联结条件，否则将返回不正确的数据
- 应该总是提供联结条件，否则会得出笛卡儿积。
- 在一个联结中可以包含多个表，甚至对于每个联结可以采用不同的联结类型。虽然这样做是合法的，一般也很有用，但应该在一起测试它们前，分别测试每个联结。这将使故障排除更为简单。

#### 14 组合查询

> 组合查询：执行多个查询(多条select语句)，并将结果作为单个查询结果集返回。这些组合查询通常称为并(union)或是符合查询(compound query)

使用组合查询的情形：
* 在单个查询中从不同的表返回类似结构的数据；
* 对单个表执行多个查询，按单个查询返回数据。

##### 14.1 创建组合查询
用union操作符来组合数条SQL查询。
(1) 使用union

假如需要价格小于等于5的所有物品的一个列表，而且还想包括供应商1001和1002生产的所有物品（不考虑价格）。当然，可以利用where子句来完成此工作。
先分开查询：
条件一：价格小于等于5的所有物品
```sql
select vend_id, prod_id, prod_price from products where prod_price <= 5;
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
+---------+---------+------------+
```
条件二：供应商为1001和1002的所有物品
```sql
select vend_id, prod_id, prod_price from products where vend_id in (1001, 1002);

+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+

```
组合起来：
```sql
select vend_id, prod_id, prod_price from products where prod_price <= 5 union 
select vend_id, prod_id, prod_price from products where vend_id in (1001, 1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+
```
使用union将两个查询的结果合并起来，去除掉相同的结果。
(2) union规则
- union必须由两条或两条以上的select语句组成，语句之间用关键字union分割
- union中的每个查询必须包含相同的列、表达式或聚集函数(不过各个列不需要以相同的次序列出)
- 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含地转换的类型

(3) 包含或取消重复的行

默认的情况下，重复的行会被去除。如果想返回所有匹配的行，可使用union all而不是union
```sql
select vend_id, prod_id, prod_price from products where prod_price <= 5 union all
select vend_id, prod_id, prod_price from products where vend_id in (1001, 1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+
```
(4) 对组合查询结果排序

* 只能有一条order by子句
* order by子句必须出现在最后一条select语句之后

```sql
select vend_id, prod_id, prod_price from products where prod_price <= 5 union all
select vend_id, prod_id, prod_price from products where vend_id in (1001, 1002)
order by vend_id, prod_price;
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | FU1     |       3.42 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
|    1003 | FC      |       2.50 |
|    1003 | TNT1    |       2.50 |
|    1003 | SLING   |       4.49 |
+---------+---------+------------+
```

#### 15 组合查询
##### 15.1 
使用通配符和正则表达式搜索时的限制
- 性能——通配符和正则表达式匹配通常要求MySQL尝试匹配表中所有行（而且这些搜索极少使用表索引）。因此，由于被搜素行数不断增加，这些搜索可能非常耗时。
- 明确控制——使用通配符和正则表达式匹配，很难（而且并不总是能）明确地控制匹配什么和不匹配什么。例如，指定一个词必须匹配，一个词必须不匹配，而一个词仅在第一个词确实匹配的情况下才可以匹配或者才可以不匹配。
- 智能化的结果——虽然基于通配符和正则表达式的搜索提供了非常灵活的搜索，但它们都不能提供一种智能化的选择结果的方法。例如，一个特殊词的搜索将会返回包含该词的所有行，而不区分包含单个匹配的行和包含多个匹配的行（按照可能是更好的匹配来排列它们）。类似，一个特殊词的搜索将不会找出不包含该词但包含其他相关词的行。

所有这些限制以及更多的限制都可以用全文本搜索来解决。

##### 15.2

