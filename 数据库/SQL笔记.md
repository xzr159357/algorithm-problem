# 一、SQL入门

## 1.1 SQL简介

SQL（Structed Query Language：结构化查询语句）是用于管理关系数据库管理低系统（RDBMS）。

SQL的范围包括：数据插入、查询、更新和删除，数据库模式创建和修改，数据访问控制。

SQL在1986年成为ANSI的一项标准，在1987年成为国际标准化组织（ISO）标准。

## 1.2 区别

虽然SQL是一门ANSI标准的计算机语言，但是仍然存在着多种不同版本的SQL语言。

然而，为了与ANSI标准兼容，他们必须以相似的方式共同支持一些主要的命令：

- SELECT
- UPDATA
- DELETE
- INSERT
- WHERE

绝大多数SQL数据库都有自己的扩展。

## 1.3 RDBMS

RDBMS，关系型数据库管理系统，Relational Database Management System

RDBMS是SQL的基础，也是所有现代数据库的基础。

RDBMS中的数据存储在被称为**表**的数据库对象中。表是相关的数据项的集合，它由列和行组成。



## 1.4 执行sql语句

（1）使用test_db数据库

```
use test_db;
```

（2）设置编码，防止乱码

```
set names utf8;
```

（3）执行sql文件：source 路径;

```
source D:\software\mysql-5.7.22\data\test_db\websites.sql;
```



## 1.5 最重要的SQL命令

- select —— 从数据库中提取数据
- updata —— 更新数据库中的数据
- delete —— 从数据库中删除数据
- insert into —— 向数据库中插入新数据
- create database —— 创建新数据库
- alter database —— 修改数据库
- create table —— 创建新表
- alter table —— 改变数据库表
- drop table —— 删除表
- create index —— 创建索引
- drop index —— 删除索引



# 二、SQL命令

## 2.1 select

> - select column_name, column_name from table_name;
>
> - select * from table_name;



比如，

```
select name,url,country from websites;
```



## 2.2 select distinct

distinct关键字用于返回唯一不同的值。每个列可能有重复的值，distinct可以去除相同的值。

例如，显示country中不同的值。

```
select distinct country from websites;
```

结果：只显示出USA和CN，其他重复的不显示

![image-20210626210948476](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626210948476.png)

然后，如果column多几项时，需要取多项column的都没有重复的，就有可能都显示了。

如，就都显示出来了。

```
select name, distinct country from websites;
```

![image-20210626211141984](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626211141984.png)



# 2.3 where

语法：

> select column_name, column_name from table_name where column_name operator value;



如：下面的 SQL 语句从 "Websites" 表中选取国家为 "CN" 的所有网站：

```
select * from websites where country = 'CN';
```

![image-20210626211931645](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626211931645.png)

SQL 使用**单引号**来环绕文本值（大部分数据库系统也接受双引号）。

如果是数字字段，不需加引号：

```
select * from websites where id = 2;
```

![image-20210626212613489](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626212613489.png)



### WHERE 子句中的运算符

| 运算符  |            描述            |
| :-----: | :------------------------: |
|    =    |            等于            |
|   <>    |  不等于。有些版本也可：!=  |
|    >    |            大于            |
|    <    |            小于            |
|   >=    |          大于等于          |
|   <=    |          小于等于          |
| between |        在某个范围内        |
|  like   |        搜索某种模式        |
|   in    | 指定针对某个列的多个可能值 |



## 2.4 and & or运算符

AND & OR 运算符用于基于一个以上的条件对记录进行过滤。

如：

```
select * from websites where country = 'CN' and alexa > 19;
```

![image-20210626213802040](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626213802040.png)





## 2.5 order by

ORDER BY 关键字用于对结果集进行排序。

语法：

> select column_name, column_name from table_name order by column_name, column_name ASC | DESC;



（1）例如：按照alexa排序。默认升序排列。

```
select * from websites order by alexa;
```

![image-20210626215001989](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626215001989.png)



（2）也可以按降序排列，加上DESC：

```
select * from websites order by alexa DESC;
```

![image-20210626215059685](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626215059685.png)



（3）order by 多列

```
select * from websites order by country, alexa;
```

![image-20210626220541902](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210626220541902.png)

先按country排列，如果country相同则按alexa排列。



## 2.6 insert into

INSERT INTO 语句用于向表中插入新记录。

```cpp
insert into websites (name, url, alexa, country) values ('百度', 'https://www.baidu.com', '4', 'CN');
```

![image-20210702233623933](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210702233623933.png)



### 在指定的列插入数据

id 字段会自动更新

```
insert into websites (name, url, country) values ('stackoverflow', 'http://stackoverflow.com/', 'IND');
```

![image-20210702233828451](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210702233828451.png)





## 2.7 update

UPDATE 语句用于更新表中的记录。

```
update websites set alexa='5000', country='USA' where name='菜鸟教程';
```

![image-20210702235702121](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210702235702121.png)



### 重要警告

没有where，会将所有的数据的alexa改成5000，country='USA'。

```
update Websites set alexa='5000', country='USA';
```



## 2.8 delete

DELETE 语句用于删除表中的行。

```
delete from websites where name='Facebook' and country='USA';
```

![image-20210703000137915](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703000137915.png)



### 删除所有数据

实际上就是没有where语句。

```
delete from table_name;
//或者
delete * from table_name;
```





# 三、SQL高阶命令

## 3.1 limit命令

下面的 SQL 语句从 "Websites" 表中选取头三条记录：

```
select * from websites limit 3;
```

![image-20210703000621421](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703000621421.png)



## 3.2 like操作符

（1）LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

```
select * from websites where name like 'G%';
```

![image-20210703002147752](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703002147752.png)

- "%" 符号用于在模式的前后定义通配符（默认字母）。

- `'%k'`则是以k结尾。



（2）下面的 SQL 语句选取 name 包含模式 "oo" 的所有客户：

```
select * from websites where name like '%oo%';
```

![image-20210703002510420](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703002510420.png)





（2）下面的 SQL 语句选取 name **不包含**模式 "oo" 的所有客户：

```
select * from websites where name not like '%oo%';
```

![image-20210703002557684](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703002557684.png)





## 3.3 SQL通配符

- 通配符可用于替代字符串中的任何其他字符。

- 在 SQL 中，通配符与 **SQL LIKE 操作符一起使用**。

通配符：

|           通配符           |            描述            |
| :------------------------: | :------------------------: |
|             %              |     替代0个或多个字符      |
|             _              |        替代一个字符        |
|         [charlist]         |   字符列中的任何单一字符   |
| [^charlist] 或 [!charlist] | 不在字符列中的任何单一字符 |



（1）下面的 SQL 语句选取 url 以字母 "https" 开始的所有网站：

```
select * from websites where url like "https%";
```

![image-20210703003215652](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703003215652.png)



（2）下面的 SQL 语句选取 name 以一个任意字符开始，然后是 "oogle" 的所有客户：

```
select * from websites where name like '_oogle';
```

![image-20210703003343062](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703003343062.png)



### 使用 SQL [charlist] 通配符

- MySQL 中使用 **REGEXP** 或 **NOT REGEXP** 运算符 (或 RLIKE 和 NOT RLIKE) 来操作**正则表达式**。



（1）下面的 SQL 语句选取 name 以 "G"、"F" 或 "s" 开始的所有网站：

```
select * from websites where name regexp '^[GFs]';
```

![image-20210703003735050](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703003735050.png)



（2）下面的 SQL 语句选取 name 以 A 到 H 字母开头的网站：

```
select * from websites where name regexp '^[A-H]';
```

![image-20210703003915545](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703003915545.png)



（3）下面的 SQL 语句选取 name 不以 A 到 H 字母开头的网站：

```
select * from websites where name regexp '^[^A-H]';
//注意在[]里用^和!都可
```

![image-20210703004035362](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703004035362.png)





## 3.4 in操作符

IN 操作符允许您在 WHERE 子句中规定多个值。

下面的 SQL 语句选取 name 为 "Google" 或 "百度" 的所有网站：

```
select * from websites where name in ('Google', '百度');
```

![image-20210703004431565](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703004431565.png)	



## 3.5 between操作符

BETWEEN 操作符选取介于两个值之间的数据范围内的值。这些值可以是数值、文本或者日期。



（1）下面的 SQL 语句选取 alexa 介于 1 和 20 之间的所有网站（包括1和20）：

```
select * from websites where alexa between 1 and 20;
```

![image-20210703004638237](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703004638237.png)



（2）如需显示不在上面实例范围内的网站，请使用 NOT BETWEEN：

```
select * from websites where alexa not between 1 and 20;
```

![image-20210703004722415](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703004722415.png)



（3）下面的 SQL 语句选取 alexa 介于 1 和 20 之间但 country 不为 USA 和 IND 的所有网站：

```
select * from websites where (alexa between 1 and 20) and country not in ('USA', 'ind');
```

![image-20210703004924733](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703004924733.png)



### 带文本值

（4）下面的 SQL 语句选取 name 以介于 'A' 和 'Z' 之间字母开始的所有网站：

```
select * from websites where name between 'A' and 'Z';
```

![image-20210703005103793](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703005103793.png)



（5）下面的 SQL 语句选取 name 不介于 'A' 和 'H' 之间字母开始的所有网站：

```
select * from websites where name not between 'A' and 'H';
```

![image-20210703005157633](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703005157633.png)



### 带日期值

（6）下面的 SQL 语句选取 date 介于 '2016-05-10' 和 '2016-05-14' 之间的所有访问记录：

```
select * from access_log where date between '2016-05-10' and '2016-05-14';
```

![image-20210703005702295](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210703005702295.png)





## 参考

- https://www.runoob.com/sql/sql-wildcards.html



