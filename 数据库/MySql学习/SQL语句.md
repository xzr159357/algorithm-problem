# 一、DOS登录MySql

1. 进入MySql安装目录下的bin目录。

   ```
   cd D:\software\mysql-5.7.22\bin
   ```

2. 通过以下命令连接数据库，并输入密码

   ```
   mysql -h localhost -u root -p
   ```

   

# 二、书写规则

1. SQL语句要以分号**;**结尾。
2. SQL语句**不区分大小写**，SELECT和select解释都一样。可采取以下规则：
   1. 关键字大写
   2. 数据库名、表名、列名小写
3. 字符串、常数、日期书写：
   1. 字符串用**单引号**'，如：'abcd'
   2. SQL语句包含日期时，同样用单引号'，如：'2021-05-13'
   3. 数字直接书写，如：2021
4. 不能使用全角空格（**中文空格**）作为单词的分隔符，否则会发生错误，出现无法预期的结果。SQL 语句中的标点符号必须都是英文状态下的，即半角字。



# 三、问题

**问题**：ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

**分析**：在DOS下操作数据库，输入任何指令都出现此问题，通过问题原因可知是密码的问题。

**解决**：运行以下指令即可。

```
alter user 'root'@'localhost' identified by '123456';
```



# 四、数据创建、修改

1. show databases;	列出当前用户可查看的所有数据库

2. CREATE DATABASE test_db;    创建一个名为 test_db 的数据库，可以用IF NOT EXISTS修饰避免存在而报错

3. like语句。如：SHOW DATABASES LIKE ‘test%’;    可以匹配test开头的数据库

4. show create database test_db;    查看 test_db 数据库的定义声明

5. ALTER DATABASE test_db DEFAULT CHARACTER SET gb2312 DEFAULT COLLATE gb2312_chinese_ci;

   使用命令行工具将数据库 test_db 的指定字符集修改为 gb2312，默认校对规则修改为 gb2312_unicode_ci

6. drop database if exists db_test;    删除数据库

7. use test_db;    选择数据库，如果出现“Database changed”提示，则表示选择数据库成功。



# 五、MySql注释

## 单行注释

- `#`注释，如：#从结果中删除重复行
- 单行注释可以使用`--`注释符，`--`注释符后需要加一个空格，注释才能生效。如：-- 从结果中删除重复行

`#`和`--`的区别就是：`#`后面直接加注释内容，而`--`的第 2 个破折号后需要跟一个空格符在加注释内容。



## 多行注释

多行注释使用`/* */`注释符。和C语言一样。

```
/*
  第一行注释内容
  第二行注释内容
*/
```



# 六、MySql数据类型

MySQL 的数据类型有大概可以分为 5 种，分别是整数类型、浮点数类型和定点数类型、日期和时间类型、字符串类型、二进制类型等。

## 6.1 整数类型

MySQL 主要提供的整数类型有 **TINYINT**、**SMALLINT**、**MEDIUMINT**、**INT**、**BIGINT**

| 类型名称     | 说明           | 存储需求 |
| ------------ | -------------- | -------- |
| TINYINT      | 很小的整数     | 1个字节  |
| SMALLINT     | 小的整数       | 2个字节  |
| MEDIUMINT    | 中等大小的整数 | 3个字节  |
| INT(INTEGHR) | 普通大小的整数 | 4个字节  |
| BIGINT       | 大整数         | 8个字节  |



## 6.2 小数类型

[MySQL](http://c.biancheng.net/mysql/) 中使用浮点数和定点数来表示小数。

浮点类型有两种，分别是单精度浮点数（**FLOAT**）和双精度浮点数（**DOUBLE**）；定点类型只有一种，就是 **DECIMAL**。

| 类型名称           | 说明               | 存储需求  |
| ------------------ | ------------------ | --------- |
| FLOAT              | 单精度浮点数       | 4个字节   |
| DOUBLE             | 双精度浮点数       | 8个字节   |
| DECIMAL(M,D) , DEC | 压缩的“严格”定点数 | M+2个字节 |

DECIMAL 类型不同于 FLOAT 和 DOUBLE。DOUBLE 实际上是以字符串的形式存放的，DECIMAL 可能的最大取值范围与 DOUBLE 相同，但是有效的取值范围由 M 和 D 决定。如果改变 M 而固定 D，则取值范围将随 M 的变大而变大。



## 6.3 日期和时间类型

表示日期的数据类型：**YEAR**、**TIME**、**DATE**、**DTAETIME**、**TIMESTAMP**。

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1个字节  |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3个字节  |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3个字节  |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8个字节  |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:00 UTC ~ 2040-01-19 03:14:07 UTC | 4个字节  |



# 6.4 字符串类型和二进制类型

- [MySQL](http://c.biancheng.net/mysql/) 支持两类字符型数据：文本字符串和二进制字符串。

- [MySQL](http://c.biancheng.net/mysql/) 中的字符串类型有 **CHAR**、**VARCHAR**、**TINYTEXT**、**TEXT**、**MEDIUMTEXT**、**LONGTEXT**、**ENUM**、**SET** 等。

- MySQL 中的二进制字符串有 **BIT**、**BINARY**、**VARBINARY**、**TINYBLOB**、**BLOB**、**MEDIUMBLOB** 和 **LONGBLOB**。





# 七、存储引擎

- 数据库存储引擎是数据库底层软件组件，数据库管理系统使用数据引擎进行创建、查询、更新和删除数据操作。

- 简而言之，存储引擎就是指表的类型。

- `SHOW ENGINES;`语句查看系统所支持的引擎类型
- MySQL 5.7 支持的存储引擎有 InnoDB、MyISAM、Memory、Merge、Archive、CSV、BLACKHOLE 等。（主要是 InnoDB 和 MyISAM ）