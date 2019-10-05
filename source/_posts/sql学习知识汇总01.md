---
title: sql学习知识汇总01
date: 2019-09-09 11:39:43
tags: sql基础知识
categories: 数据库
---

mysql必知必会，读书笔记！
<!--more-->

[toc]

---

建表语句：[create.sql](/download/sql学习知识总结/create.sql)
插入数据：[populate.sql](/download/sql学习知识总结/populate.sql)
数据库名字：learnmysql
#### 0x01 检索数据
##### 1.1 检索不同的行，即输出的子段不重复

```sql
select distinct 字段名 from 表名;
```

现有表：t_user
```sql
+----+----------+----------+
| id | username | password |
+----+----------+----------+
|  1 | jack     | 520      |
|  2 | rose     | 521      |
|  3 | justnow  | 123      |
|  4 | justnow  | 123      |
|  5 | justnow  | 123      |
|  6 | justnow  | 123      |
|  7 | justnow  | 123      |
|  8 | justnow  | 123      |
|  9 | justnow  | 123      |
+----+----------+----------+
```
查找username列
```sql            
select username from t_user;
+----------+
| username |
+----------+
| jack     |
| rose     |
| justnow  |
| justnow  |
| justnow  |
| justnow  |
| justnow  |
| justnow  |
| justnow  |
+----------+
```
不重复的查找username列
```sql            
select distinct username from t_user;
+----------+
| username |
+----------+
| jack     |
| rose     |
| justnow  |
+----------+
```
##### 1.2 限制结果
* 限制行数
```sql
select * from 表名 limit m,n;
```
(1) 如果limit后面只有一个数字m的话，表明查找前m行
如：检索前5行的内容
```sql
select * from t_user limit 5;
+----+----------+----------+
| id | username | password |
+----+----------+----------+
|  1 | jack     | 520      |
|  2 | rose     | 521      |
|  3 | justnow  | 123      |
|  4 | justnow  | 123      |
|  5 | justnow  | 123      |
+----+----------+----------+
```
(2) 如果limit后面有两个数字m,n的话，表明查找第m行后面的n行
如：检索第5行后面的2行
```sql
select * from t_user limit 5,2;
+----+----------+----------+
| id | username | password |
+----+----------+----------+
|  6 | justnow  | 123      |
|  7 | justnow  | 123      |
+----+----------+----------+
```
##### 1.3 使用完全限定的表名
即：表名.列名来引用列
```sql
select t_user.id, t_user.password from t_user;
+----+----------+
| id | password |
+----+----------+
|  1 | 520      |
|  2 | 521      |
|  3 | 123      |
|  4 | 123      |
|  5 | 123      |
|  6 | 123      |
|  7 | 123      |
|  8 | 123      |
|  9 | 123      |
+----+----------+
```
#### 0x02 排序检索数据
##### 2.1 排序数据
使用order by子句，使用order by子句取一个或多个列的名子，据此对输出进行排序。
未排序前：
```sql
select prod_name from products;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Detonator      |
| Bird seed      |
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
```
使用order by子句后：
```sql
select prod_name from products order by prod_name;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Bird seed      |
| Carrots        |
| Detonator      |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
```
与没有使用order by子句对比，可以看出，结果会根据子母和数字进行排序。

##### 2.2 按照多个列排序
如果产品的价格相同，但是名字不同，这时候可以使用多个列排序的方法
```sql
select prod_id, prod_price, prod_name from products order by prod_price, prod_name;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| FC      |       2.50 | Carrots        |
| TNT1    |       2.50 | TNT (1 stick)  |
| FU1     |       3.42 | Fuses          |
| SLING   |       4.49 | Sling          |
| ANV01   |       5.99 | .5 ton anvil   |
| OL1     |       8.99 | Oil can        |
| ANV02   |       9.99 | 1 ton anvil    |
| FB      |      10.00 | Bird seed      |
| TNT2    |      10.00 | TNT (5 sticks) |
| DTNTR   |      13.00 | Detonator      |
| ANV03   |      14.99 | 2 ton anvil    |
| JP1000  |      35.00 | JetPack 1000   |
| SAFE    |      50.00 | Safe           |
| JP2000  |      55.00 | JetPack 2000   |
+---------+------------+----------------+
```
注意：仅在多个行具有相同的prod_price值时才对产品按prod_name进行排序。如果prod_price列中所有的值都是唯一的，则不会按prod_name排序

##### 2.3 指定排序方向
按价格以降序排序产品(最贵的排在最前面):
```sql
select prod_id, prod_price, prod_name from products order by prod_price DESC;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| JP2000  |      55.00 | JetPack 2000   |
| SAFE    |      50.00 | Safe           |
| JP1000  |      35.00 | JetPack 1000   |
| ANV03   |      14.99 | 2 ton anvil    |
| DTNTR   |      13.00 | Detonator      |
| TNT2    |      10.00 | TNT (5 sticks) |
| FB      |      10.00 | Bird seed      |
| ANV02   |       9.99 | 1 ton anvil    |
| OL1     |       8.99 | Oil can        |
| ANV01   |       5.99 | .5 ton anvil   |
| SLING   |       4.49 | Sling          |
| FU1     |       3.42 | Fuses          |
| FC      |       2.50 | Carrots        |
| TNT1    |       2.50 | TNT (1 stick)  |
+---------+------------+----------------+
```
---
找出一个列中最高或者最低的值。
```sql
select prod_price from products order by prod_price desc limit 1;
+------------+
| prod_price |
+------------+
|      55.00 |
+------------+
```
**<font color="red" face="微软雅黑">order by子句保证它位于from子句之后；如果使用limit，它必须位于order by子句之后</font>**

#### 0x03 过滤数据
##### 3.1 使用where子句
在select语句中，数据根据where子句中指定的搜索条件进行过滤。where子句在表名(from 子句)之后，给出
```sql
select prod_name, prod_price from products where prod_price = 2.50;
+---------------+------------+
| prod_name     | prod_price |
+---------------+------------+
| Carrots       |       2.50 |
| TNT (1 stick) |       2.50 |
+---------------+------------+
```
**<font color="red" face="微软雅黑">在同时使用ORDER BY和WHERE子句时，应该让ORDER BY位于WHERE之后， 否则将会产生错误</font>**
##### 3.2 使用where子句操作符

|操作符|说明|
|:--:|:--:|
|=|等于|
|<>|不等于|
|!=|不等于|
|<|小于|
|<=|小于等于|
|>|大于|
|>=|大于等于|
|BETWEEN|在指定的两个值之间|
(1) 检查单个值

```sql
select prod_name, prod_price from products where prod_name = 'fuses';
+-----------+------------+
| prod_name | prod_price |
+-----------+------------+
| Fuses     |       3.42 |
+-----------+------------+
```
mysql在执行匹配时默认不区分大小写。
(2) 列出价格小于10美元的所有产品
```sql
select prod_name, prod_price from products where prod_price < 20;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| .5 ton anvil   |       5.99 |
| 1 ton anvil    |       9.99 |
| 2 ton anvil    |      14.99 |
| Detonator      |      13.00 |
| Bird seed      |      10.00 |
| Carrots        |       2.50 |
| Fuses          |       3.42 |
| Oil can        |       8.99 |
| Sling          |       4.49 |
| TNT (1 stick)  |       2.50 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
```
(3) 不匹配检查
不是由供应商1003制造的所有产品：
```sql
select vend_id, prod_name from products where vend_id <> 1003;
select vend_id, prod_name from products where vend_id != 1003;
+---------+--------------+
| vend_id | prod_name    |
+---------+--------------+
|    1001 | .5 ton anvil |
|    1001 | 1 ton anvil  |
|    1001 | 2 ton anvil  |
|    1002 | Fuses        |
|    1005 | JetPack 1000 |
|    1005 | JetPack 2000 |
|    1002 | Oil can      |
+---------+--------------+
```
(4) 范围检查
检索价格在5美元和10美元之间的所有产品：
```sql
select prod_name, prod_price from products where prod_price between 5 and 10;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| .5 ton anvil   |       5.99 |
| 1 ton anvil    |       9.99 |
| Bird seed      |      10.00 |
| Oil can        |       8.99 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
```
(5) 控制检查
在创建表时，可以指定其中的列是否可以不包含值。在一个列不包含值时，称其为包含空值NULL。
检索客户email为null的客户id
```sql
select cust_id from customers where cust_email is null;
+---------+
| cust_id |
+---------+
|   10002 |
|   10005 |
+---------+
```
#### 0x04 数据过滤
##### 4.1 组合where子句
> 操作符(operator)用来联接或改变where子句中的子句的关键字。也称为逻辑操作符(logical operator)
(1) AND 操作符
检索满足所有给定条件的行。
检索由供应商1003制造且价格小于10美元的所有产品的名称和价格
```sql
select prod_id, prod_name, prod_price from products where vend_id = 1003 and prod_price <= 10;
+---------+----------------+------------+
| prod_id | prod_name      | prod_price |
+---------+----------------+------------+
| FB      | Bird seed      |      10.00 |
| FC      | Carrots        |       2.50 |
| SLING   | Sling          |       4.49 |
| TNT1    | TNT (1 stick)  |       2.50 |
| TNT2    | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
```
(2) OR操作符
检索匹配任一条件的行
检索由1002或者1003供应商制造的所有产品的产品名字和价格。
```sql
select vend_id, prod_name, prod_price from products where vend_id=1002 or vend_id=1003;
+---------+----------------+------------+
| vend_id | prod_name      | prod_price |
+---------+----------------+------------+
|    1003 | Detonator      |      13.00 |
|    1003 | Bird seed      |      10.00 |
|    1003 | Carrots        |       2.50 |
|    1002 | Fuses          |       3.42 |
|    1002 | Oil can        |       8.99 |
|    1003 | Safe           |      50.00 |
|    1003 | Sling          |       4.49 |
|    1003 | TNT (1 stick)  |       2.50 |
|    1003 | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
```
(3) 计算次序
AND的优先级要高于OR操作符，所以，如果sql语句为
```sql
select * from products where vend_id=1002 or vend_id=1003 and prod_price >= 10;
```
结果将会出现，price低于10美元的产品
正确的做法是，加上括号
```sql
select * from products where (vend_id=1002 or vend_id=1003) and prod_price >= 10;
+---------+---------+----------------+------------+-------------------------------------------------+
| prod_id | vend_id | prod_name      | prod_price | prod_desc                                       |
+---------+---------+----------------+------------+-------------------------------------------------+
| DTNTR   |    1003 | Detonator      |      13.00 | Detonator (plunger powered), fuses not included |
| FB      |    1003 | Bird seed      |      10.00 | Large bag (suitable for road runners)           |
| SAFE    |    1003 | Safe           |      50.00 | Safe with combination lock                      |
| TNT2    |    1003 | TNT (5 sticks) |      10.00 | TNT, red, pack of 10 sticks                     |
+---------+---------+----------------+------------+-------------------------------------------------+
```
(4) IN操作符
>IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配。 IN取合法值的由逗号分隔的清单，全都括在圆括号中

```sql
select prod_name, prod_price from products where vend_id in (1002, 1003) order by prod_name;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| Bird seed      |      10.00 |
| Carrots        |       2.50 |
| Detonator      |      13.00 |
| Fuses          |       3.42 |
| Oil can        |       8.99 |
| Safe           |      50.00 |
| Sling          |       4.49 |
| TNT (1 stick)  |       2.50 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
```
使用```or```可以得到相同的效果
```sql
select prod_name, prod_price from products where vend_id = 1002 or vend_id = 1003 order by prod_name;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| Bird seed      |      10.00 |
| Carrots        |       2.50 |
| Detonator      |      13.00 |
| Fuses          |       3.42 |
| Oil can        |       8.99 |
| Safe           |      50.00 |
| Sling          |       4.49 |
| TNT (1 stick)  |       2.50 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
```
使用in操作符的优点：
* 在使用长的合法选项清单时，IN操作符更清楚更直观。
* 在使用IN时，计算的次序更容易管理(因为使用的操作符更少)。
* IN操作符一般比OR操作符清单执行更快。
* IN的最大优点是可以包含其他select语句，使得能够更动态地建立where子句。
(5) NOT操作符
>否定它之后所跟的任何条件
列出除了1003和1002之外的所有供应商制造的产品。可编写如下的代码：
```sql
select prod_name, prod_price from products where vend_id not in (1003, 1002) order by prod_name;
+--------------+------------+
| prod_name    | prod_price |
+--------------+------------+
| .5 ton anvil |       5.99 |
| 1 ton anvil  |       9.99 |
| 2 ton anvil  |      14.99 |
| JetPack 1000 |      35.00 |
| JetPack 2000 |      55.00 |
+--------------+------------+
```

#### 0x05 用通配符进行过滤
>通配符 用来匹配值的一部分的特殊字符，例如：百分号通配符、下划线通配符等。

为在搜索子句中使用通配符，必须使用LIKE操作符。LIKE指示MySQL，后跟的搜索模式利用通配符匹配而不是直接相等匹配进行比较。

##### 5.1 百分号(%)通配符
> %表示任何字符出现任意次数

(1) 找到所有以jet开头的产品

```sql
select prod_id, prod_name from products where prod_name like 'jet%';
+---------+--------------+
| prod_id | prod_name    |
+---------+--------------+
| JP1000  | JetPack 1000 |
| JP2000  | JetPack 2000 |
+---------+--------------+
```
(2) 匹配任何位置包含文本anvil的值，不论它之前或之后出现什么字符

```sql
select prod_name from products where prod_name like "%anvil%";
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
| 1 ton anvil  |
| 2 ton anvil  |
+--------------+
```
(3) 找出以s开头以e结尾的所有产品

```sql
select prod_name from products where prod_name like 's%e';
+-----------+
| prod_name |
+-----------+
| Safe      |
+-----------+
```

##### 5.2 下划线(_)通配符
>下划线只匹配单个字符而不是多个字符
```sql
select prod_id, prod_name from products where prod_name like '_ ton anvil';
+---------+-------------+
| prod_id | prod_name   |
+---------+-------------+
| ANV02   | 1 ton anvil |
| ANV03   | 2 ton anvil |
+---------+-------------+
```
总结：
* 不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。
* 在确定需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来时最慢的
* 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据

#### 0x06 用正则表达式进行搜索
##### 6.1 基本的字符匹配
一个简单的正则表达式
```sql
select prod_name from products where prod_name REGEXP '.000' order by prod_name
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
```
```.``` 表示匹配任意一个字符。因此1000和2000都匹配且返回。
like和regexp的区别是：
* LIKE匹配整个列。如果被匹配的文本在列值中出现，like将不会找到它，相应的行也不被返回(除非使用通配符)
* REGEXP在列值内进行匹配，如果被匹配的文本在列值中出现，REGEXP将会找到它，相应的行将被返回。
##### 6.2 进行OR匹配
为了搜索两个串之一，使用|
```sql
select prod_name from products where prod_name regexp '1000|2000' order by prod_name;
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
```

##### 6.3 匹配几个字符之一
[123]匹配1或2或3
```sql
select prod_name from products where prod_name regexp '[123] ton' order by prod_name;
+-------------+
| prod_name   |
+-------------+
| 1 ton anvil |
| 2 ton anvil |
+-------------+
```
##### 6.4 匹配范围
[0-9]匹配数字0到9；
[a-z]匹配字母a到9；

```sql
select prod_name from products where prod_name regexp '[1-5] Ton' order by prod_name;
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
| 1 ton anvil  |
| 2 ton anvil  |
+--------------+
```

##### 6.5 匹配特殊字符
为了匹配特殊字符，需要用\\为前导。\\-表示查找-，\\.表示查找.
```sql
select vend_name from vendors where vend_name regexp '\\.' order by vend_name;
+--------------+
| vend_name    |
+--------------+
| Furball Inc. |
+--------------+
```
##### 6.6 匹配字符类
|类|说明|
|:--:|:--:|
|[:alnum:] |任意字母和数字（同[a-zA-Z0-9]）|
[:alpha:] |任意字符（同[a-zA-Z]）|
[:blank:] |空格和制表（同[\\t]）|
[:cntrl:] |ASCII控制字符（ ASCII 0到31和127）|
[:digit:] |任意数字（同[0-9]）|
[:graph:] |与[:print:]相同，但不包括空格|
[:lower:] |任意小写字母（同[a-z]）|
[:print:] |任意可打印字符|
[:punct:] |既不在[:alnum:]又不在[:cntrl:]中的任意字符|
[:space:] |包括空格在内的任意空白字符（同[\\f\\n\\r\\t\\v]）|
[:upper:] |任意大写字母（同[A-Z]）|
[:xdigit:] |任意十六进制数字（同[a-fA-F0-9]）|
##### 6.7 匹配多个实例
比如，寻找一个单词并且还能够适应一个尾随的s(如果存在)
```sql
select prod_name from products where prod_name regexp '\\([0-9] sticks?\\)';
+----------------+
| prod_name      |
+----------------+
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
```
其中```s?```，表示0个或1个s匹配
下标是正则表达式重复元字符
|元字符|说明|
|:--:|:--:|
|* |0个或多个匹配|
|+ |1个或多个匹配（等于{1,}）|
|? |0个或1个匹配（等于{0,1}）|
|{n} |指定数目的匹配|
|{n,} |不少于指定数目的匹配|
|{n,m} |匹配数目的范围（ m不超过255）|

```sql
select prod_name from products where prod_name regexp '[[:digit:]]{4}' order by prod_name;
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
```

##### 6.8 定位符
为了匹配特定位置的文本，需要使用下表中的定位符
|元字符|说明|
|:--:|:--:|
|^ |文本的开始|
|$ |文本的结尾|
|[\[:<:]] |词的开始|
|[\[:>:]] |词的结尾|

```sql
找出以一个数(包括以小数点开始的数)开始的所有产品。

select prod_name from products where prod_name regexp '^[0-9\\.]' order by prod_name;

+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
| 1 ton anvil  |
| 2 ton anvil  |
+--------------+
```



