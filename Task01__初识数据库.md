# Task 01： 初始数据库

![](./img/SQL-Cheat-Sheet-Summary-Full.png)

> `初始数据库`：学习任务包括为什么需要数据库、什么是数据库、数据库的结构、SQL概要、表的创建、表的删除和更新以及学习理解！

**学习重点：**

本章介绍了数据库的结构和基本理论，以及数据库的实际应用。大家可以学习到如何对关系数据库中用来存储数据的表进行创建、更新和删除操作，同时还能掌握关系数据库专用的SQL 语句的书写方法和规则。

## 1.1 为什么需要数据库？

数据库是以一定方式储存在一起、能与多个用户共享、具有尽可能小的冗余度、与应用程序彼此独立的数据集合，可视为电子化的文件柜——存储电子文件的处所，用户可以对文件中的数据进行新增、查询、更新、删除等操作。数据库技术是数据管理的技术，是计算机科学与计算的重要分支，是信息系统的核心和基础。

## 1.2 什么是数据库？

- 数据库的分类
- 什么是DBMS
- DBMS的分类

数据库是将大量数据保存起来，通过计算机加工而成的可以进行高效访问的数据集合。用来管理数据库的计算机系统称为数据库管理系统。关系数据库通过关系数据库管理系统（RDBMS）进行管理。

- DBMS的种类

  - 层次数据库（`Hierarchical Database，HDB`）

  - 关系数据库（`Relational Database，RDB`）

    ![](./img/SQLvsMySQL.png)

    - Oracle Database：甲骨文公司的RDBMS
    - SQL Server ：微软公司的RDBMS
    - DB2：IBM 公司的RDBMS
    - PostgreSQL：开源的RDBMS
    - MySQL ：开源的RDBMS

  - 面向对象数据库（`Object Oriented Database，OODB`）

  - XML数据库（`XML Database，XMLDB`）

  - 键值存储系统（`Key-Value Store，KVS`）

## 1.3 数据库的结构

- RDBMS的常见系统结构

  图1-1 RBMS的系统结构

  ![](./img/1-1RDBMS.png)

- 表的结构

  表存储在由RDBMS 管理的数据库中，如图1-2所示。一个数据库中可以存储多个表。

  图1-2 数据库和表的关系

  ![](./img/1-2DB&Table.png)

  图1-3 商品表的示例

  ![](./img/1-3Product_Table.png)

  图1-4 单元格的使用规范

  ![](./img/1-4Note1.png)

  > **名词解释及注意事项**
  >
  > 字段：表的列（垂直方向）称为字段，它代表了保存在表中的数据项。
  >
  > 记录：表的行（水平方向）称为记录，它相当于一条数据。
  >
  > 单元格：行和列交汇的方格称为单元格。 一个单元格中只能输入一个数据。
  >
  > :exclamation: 注意：关系数据库必须以行为单位进行数据读写！

## 1.4 SQL概要

![](./img/what_is_sql.png)

- 标准SQL

  国际标准化组织（ISO）为SQL 制定了相应的标准，以此为基准的SQL 称为标准SQL。学会标准SQL 就可以在各种RDBMS 中书写SQL 语句了。

- SQL语句及其分类

  - 什么是SQL语句？

    SQL 用关键字、表名、列名等组合而成的一条语句（SQL 语句）来描述操作的内容。

  - SQL语句的分类（SQL 根据功能不同可以分为三类，其中使用最多的是`DML`。）

    - DDL

      DDL（Data Definition Language，数据定义语言） 用来创建或者删除存储数据用的数据库以及数据库中的表等对象。DDL一般包括以下几种指令：

      - CREATE： 创建数据库和表等对象
      - DROP： 删除数据库和表等对象
      - ALTER： 修改数据库和表等对象的结构

    - DML

      DML（Data Manipulation Language，数据操纵语言） 用来查询或者变更表中的记录。DML一般 包含以下几种指令：

      - SELECT：查询表中的数据
      - INSERT：向表中插入新数据
      - UPDATE：更新表中的数据
      - DELETE：删除表中的数据

    - DCL

      DCL（Data Control Language，数据控制语言） 用来确认或者取消对数据库中的数据进行的变更。DCL 一般包含以下几种指令：

      - COMMIT： 确认对数据库中的数据进行的变更
      - ROLLBACK： 取消对数据库中的数据进行的变更
      - GRANT： 赋予用户操作权限
      - REVOKE： 取消用户的操作权限

- SQL的基本书写规则

  - SQL 语句要以分号（;）结尾

  - SQL 语句不区分大小写

    - 关键字一般大写
    - 表名的首字母一般大写
    - 列名一般小写
    - 表中的数据严格区别大小写

  - 常数的书写方式是固定的

    在SQL 语句中直接书写的字符串、日期或者数字等称为常数。

    - 字符串：`'abc'`
    - 日期：`'2020-12-14'`
    - 数字：1024

  - 单词需要用半角空格或者换行来分隔

    CREATE TABLE Product

    > :exclamation: 注意：不能使用全角空格作为单词的分隔符！

  

## 1.5 表的创建

- 表的内容的创建

  ![](./img/1-5Product_Table.png)

  表1-1商品表

  表1-1商品表中最上面一行是数据的项目名，真正的数据是从第2行开始的。

- 数据库的创建（CREATE DATABASE语句）

  语法：

  

  ```sql
  CREATE DATABASE < 数据库名称 > ;
  ```

  注意：在创建表之前，一定要先创建用来存储表的数据库！

  不妨将数据库命名为`shop`，执行以下SQL语句，即可创建一个名为`shop`的数据库：

  ```sql
  CREATE DATABASE shop;
  ```

  注意：数据库名称、表名以及列名都要使用半角字符（英文字母、数字、符号）。

- 表的创建（CREATE TABLE语句）

  数据库创建好以后，接下来将使用CREATE TABLE 语句在其中创建表。

  语法：

  ```sql
  CREATE TABLE <表名>
  （<列名1> <数据类型> <该列所需约束>，
    <列名2> <数据类型> <该列所需约束>，
    <列名3> <数据类型> <该列所需约束>，
    <列名4> <数据类型> <该列所需约束>，
    ......
    <该表的约束1>， <该表的约束2>，……）；
  ```

  不妨在数据库中创建表1-1中的商品表（Product 表）的CREATE TABLE 语句，创建表的语句如下：

  ```sql
  CREATE TABLE Product
  (product_id CHAR(4) NOT NULL,
  product_name VARCHAR(100) NOT NULL,
  product_type VARCHAR(32) NOT NULL,
  sale_price INTEGER ,
  purchase_price INTEGER ,
  regist_date DATE ,
  PRIMARY KEY (product_id));
  ```

- 命名规则

  只能使用半角英文字母、数字、下划线（_）作为数据库、表和列的名称，且名称必须以半角英文字母开头！

  注意：在同一个数据库中不能创建两个相同名称的表，在同一个表中也不能创建两个名称相同的列！

  ![](./img/1-6Product_Table.png)

  表1-2 商品表和Product表列名的对应关系

- 数据类型的指定

  Product 表所包含的列，定义在CREATE TABLE Product()的括号中。列名右边的INTEGER 或者CHAR 等关键字，是用来声明该列的`数据类型`的，所有的列都必须指定数据类型！

  数据类型表示数据的种类，包括数字型、字符型和日期型，且每一列都不能存储与该列数据类型不符的数据！

  - INTEGER型

    用来指定存储整数的列的数据类型（数字型），不能存储小数。

  - CHAR型

    用来存储定长字符串，当列中存储的字符串长度达不到最大长度的时候，使用半角空格进行补足，由于会浪费存储空间，所以一般不使用。

  - VARCHAR型

    用来存储可变长度字符串，定长字符串在字符数未达到最大长度时会用半角空格补足，但可变长字符串不同，即使字符数未达到最大长度，也不会用半角空格补足。

  - DATE型

    用来指定存储日期（`YYYY-M-D`）的列的数据类型（日期型）。

- 约束的设置

  约束是除了数据类型之外，对列中存储的数据进行限制或者追加条件的功能。

  - `NOT NULL`是非空约束，即该列必须输入数据。

    ```sql
    product_id CHAR(4) NOT NULL,
    product_name VARCHAR(100) NOT NULL,
    product_type VARCHAR(32) NOT NULL,
    ```

  - `PRIMARY KEY`是主键约束，代表该列是唯一值，可以通过该列取出特定商品数据。

    ```sql
    PRIMARY KEY (product_id)
    ```

## 1.6 表的删除和更新

- 表的删除（DROP TABLE语句）

  语法格式：

  ```sql
  DROP TABLE < 表名 > ;
  ```

  删除 product 表：（一旦删除，无法恢复）

  ```sql
  DROP TABLE product;
  ```

- 表定义的更新（ALTER TABLE语句）

  ![](./img/SQL_Keywords.png)

  添加列的 ALTER TABLE 语句：

  ```sql
  ALTER TABLE < 表名 > ADD COLUMN < 列的定义 >;
  ```

  添加一列可以存储100位的可变长字符串的 product_name_pinyin 列：

  ```sql
  ALTER TABLE product ADD COLUMN product_name_pinyin VARCHAR(100);
  ```

  删除列的 ALTER TABLE 语句：

  ```sql
  ALTER TABLE < 表名 > DROP COLUMN < 列名 >;
  ```

  删除 product_name_pinyin 列：

  ```sql
  ALTER TABLE product DROP COLUMN product_name_pinyin;
  ```

  注意：ALTER TABLE 语句和DROP TABLE 语句一样，执行之后无法恢复。误添的列可以通过ALTER TABLE 语句删除，或者将表全部删除之后重新再创建！

- 向Product表中插入数据

  ```sql
  -- DML：插入数据
  
  -- SQL Server or Postgre SQL
  BEGIN TRANSACTION; 
  
  -- MySQL
  -- START TRANSACTION;
  
  -- Oracle or DB2 可以省略
  
  INSERT INTO Product VALUES ('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20');
  INSERT INTO Product VALUES ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11');
  INSERT INTO Product VALUES ('0003', '运动T恤', '衣服', 4000, 2800, NULL);
  INSERT INTO Product VALUES ('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');
  INSERT INTO Product VALUES ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
  INSERT INTO Product VALUES ('0006', '叉子', '厨房用具', 500, NULL, '2009-09-20');
  INSERT INTO Product VALUES ('0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28');
  INSERT INTO Product VALUES ('0008', '圆珠笔', '办公用品', 100, NULL,'2009-11-11');
  
  COMMIT;
  ```

## 1.7 学习理解

1. 编写一条CREATE TABLE 语句，用来创建一个包含表1-A中所列各项的表 Addressbook（ 地址簿），并为 regist_no（ 注册编号）列设置主键约束。

   ![](./img/1-7Addressbook.png)

   表1-A 表Addressbook（地址簿）中的列


2. 假设在创建练习1.1 中的Addressbook 表时忘记添加如下一列postal_code（邮政编码）了，请把此列添加到Addressbook 表中。

   列名 ：postal_code
   数据类型 ：定长字符串类型（长度为8）
   约束 ：不能为NULL


3. 编写SQL 语句来删除Addressbook 表。


4. 编写SQL 语句来恢复删除掉的Addressbook 表。


   
