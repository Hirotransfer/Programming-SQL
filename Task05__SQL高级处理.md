# Task 05：SQL高级处理

## 5.1 窗口函数

- 什么是窗口函数

OLAP 是OnLine Analytical Processing 的简称，意思是对数据库数据进行实时分析处理。而窗口函数就是为了实现OLAP 而添加的标准SQL 功能。

- 窗口函数的语法

窗口函数的语法的语法格式：

**能够作为窗口函数使用的函数**
（1）能够作为窗口函数的聚合函数（SUM、AVG、COUNT、MAX、MIN）
（2）RANK、DENSE_RANK、ROW_NUMBER 等专用窗口函数

- 语法的基本使用方法——使用RANK函数


> Note：窗口函数兼具分组和排序两种功能。通过PARTITION BY分组后的记录集合称为“窗口”。

- 无需指定PARTITION BY

- 专用窗口函数的种类

**RANK函数**

**DENSE_RANK函数**

**ROW_NUMBER函数**

由于专用窗口函数无需参数，因此通常括号中都是空的。

- 窗口函数的适用范围

原则上窗口函数只能在SELECT子句中使用。

- 作为窗口函数使用的聚合函数

**指定框架（汇总范围）**

**将当前记录的前后行作为汇总对象**

- 计算移动平均

- 两个ORDER BY

—— Note：将聚合函数作为窗口函数使用时，会以当前记录为基准来决定汇总对象的记录。

## 5.2 GROUPING运算符

- 同时计算出合计值

- ROLLUP——同时得出合计和小计

GROUPING 运算符包含以下3 种A。
- ROLLUP
- CUBE
- GROUPING SETS

**ROLLUP的使用方法：**

**将“登记日期”添加到聚合键当中：**

> Note：ROLLUP可以同时得出合计和小计，是非常方便的工具。

- GROUPING函数——让NULL更加容易分辨

- CUBE——用数据来搭积木

> 可以把CUBE理解为将使用聚合键进行切割的模块堆积成一个立方体。

- GROUPING SETS——取得期望的积木

## 5.3 学习理解

1. 请说出针对本章中使用的Product（商品）表执行如下SELECT 语句所能得到的结果。

2. 继续使用Product表，计算出按照登记日期（regist_date）升序进行排列的各日期的销售单价（sale_price）的总额。排序是需要将登记日期为NULL 的“运动T 恤”记录排在第1 位（也就是将其看作比其他日期都早）。
