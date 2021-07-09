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

## 3.1 limit命令（TOP）

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





## 3.6 别名

通过使用 SQL，可以为表名称或列名称指定别名。



### 列的别名实例

（1）下面的 SQL 语句指定了两个别名，一个是 name 列的别名，一个是 country 列的别名。**提示：**如果列名称包含空格，要求使用双引号或方括号：

```
select name as n, country as c from websites;
```

![image-20210705092640470](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705092640470.png)



（2）在下面的 SQL 语句中，我们把三个列（url、alexa 和 country）结合在一起，并创建一个名为 "site_info" 的别名：

```
select name, concat(url, ',', alexa, ',', country) as site_info from websites;
```

![image-20210705093017446](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705093017446.png)



### 表的别名实例

（3）下面的 SQL 语句选取 "菜鸟教程" 的所有访问记录。我们使用 "Websites" 和 "access_log" 表，并分别为它们指定表别名 "w" 和 "a"（通过使用别名让 SQL 更简短）：

```
select w.name, w.url, a.count, a.date from websites as w, access_log as a where a.site_id = w.id and w.name = "菜鸟教程";
```

![image-20210705093907202](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705093907202.png)





## 3.7 join——SQL连接

SQL JOIN 子句用于把来自两个或多个表的行结合起来，基于这些表之间的共同字段。

```
select websites.id, websites.name, access_log.count, access_log.date from websites inner join access_log on websites.id = access_log.site_id;
```

![image-20210705100839232](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705100839232.png)



### 不同的SQL JOIN

- **INNER JOIN**：如果表中有至少一个匹配，则返回行
- **LEFT JOIN**：即使右表中没有匹配，也从左表返回所有的行
- **RIGHT JOIN**：即使左表中没有匹配，也从右表返回所有的行
- **FULL JOIN**：只要其中一个表中存在匹配，则返回行



## 3.8 inner join

INNER JOIN 关键字在表中存在至少一个匹配时返回行。



下面的 SQL 语句将返回所有网站的访问记录：

```
select websites.name, access_log.count, access_log.date from websites inner join access_log on websites.id = access_log.site_id order by access_log.count;
```

![image-20210705105706613](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705105706613.png)





## 3.9 left join

LEFT JOIN 关键字从左表（table1）返回所有的行，即使右表（table2）中没有匹配。如果右表中没有匹配，则结果为 NULL。



下面的 SQL 语句将返回所有网站及他们的访问量（如果有的话）。

以下实例中我们把 Websites 作为左表，access_log 作为右表：

```
select websites.name, access_log.count, access_log.date from websites left join access_log on websites.id = access_log.site_id order by access_log.count DESC;
```

![image-20210705120230434](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705120230434.png)

使用`left join`，即使access_log.count = 0也将显示，并将为0的数据值显示为NULL。



## 3.10 right join

RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。



```
select websites.name, access_log.count, access_log.date from websites right join access_log on websites.id = access_log.site_id order by access_log.count DESC;
```

 ![image-20210705121024966](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705121024966.png)



`right join`则是，以右表的所有**行**为主，左表没有的为NULL。



## 3.11 full outer join

FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行.

FULL OUTER JOIN 关键字结合了 LEFT JOIN 和 RIGHT JOIN 的结果。



MySQL中不支持 FULL OUTER JOIN，可以在 SQL Server 测试以下实例。







## 3.12 union——合并

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

请注意，UNION 内部的每个 SELECT 语句必须拥有**相同数量**的**列**。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。



（1）下面的 SQL 语句从 "Websites" 和 "apps" 表中选取所有**不同的**country（只有不同的值）：

```
select country from websites 
union
select country from apps order by country;
```

![image-20210705122617977](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705122617977.png)



（2）union all 可选取所有country

```
select country from websites 
union all
select country from apps order by country;
```

![image-20210705122735803](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705122735803.png)



（3）下面的 SQL 语句使用 UNION ALL 从 "Websites" 和 "apps" 表中选取**所有的**中国(CN)的数据（也有重复的值）：

```
select country, name from websites where country = 'CN'
union all
select country, app_name from apps where country = 'CN' order by country;
```

![image-20210705123040718](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210705123040718.png)







## 3.13 select into

SELECT INTO 语句从一个表**复制**数据，然后把数据插入到另一个新表中。

MySQL 数据库不支持 SELECT ... INTO 语句，但支持 [INSERT INTO ... SELECT](https://www.runoob.com/sql/sql-insert-into-select.html) 。



## 3.14 insert into select

INSERT INTO SELECT 语句从一个表复制数据，然后把数据插入到一个已存在的表中。目标表中任何已存在的行都不会受影响。



（1）复制 "apps" 中的数据插入到 "Websites" 中：

```
insert into websites (name, country) select app_name, country from apps;
```

（2）只复制 QQ 的 APP 到 "Websites" 中：

```
insert into websites (name, country) select app_name, country from apps where id = 1;
```





## 3.15 create database——创建数据库

CREATE DATABASE 语句用于创建数据库。



下面的 SQL 语句创建一个名为 "my_db" 的数据库：

```
create database my_db;
```



## 3.16 create talbe——创建数据库中的表

CREATE TABLE 语句用于创建数据库中的表。

表由行和列组成，每个表都必须有个表名。



（1）现在我们想要创建一个名为 "Persons" 的表，包含五列：PersonID、LastName、FirstName、Address 和 City。

我们使用下面的 CREATE TABLE 语句：

```
create table Persons
(
	PersonId int,
	LastName varchar(255),
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255)
);
```



## 3.17 SQL约束

- SQL 约束用于规定表中的数据规则。如果存在违反约束的数据行为，行为会被约束终止。

- 约束可以在：
  - 创建表时规定（通过 **CREATE TABLE** 语句）
  - 或者在表创建之后规定（通过 **ALTER TABLE** 语句）。
- 在 SQL 中，我们有如下约束：
  - **NOT NULL** - 指示某列不能存储 NULL 值。
  - **UNIQUE** - 保证某列的每行必须有唯一的值。
  - **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
  - **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性。
  - **CHECK** - 保证列中的值符合指定的条件。
  - **DEFAULT** - 规定没有给列赋值时的默认值。



之后详细介绍。。。



## 3.18 not null——约束

在默认的情况下，表的列接受 NULL 值，NOT NULL 约束强制列不接受 NULL 值。

NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

### 实例1——创建表时约束

下面的 SQL 强制 "ID" 列、 "LastName" 列以及 "FirstName" 列不接受 NULL 值：

```
create table Persons (
	ID int not null,
	LastName varchar(255) not null,
	FirstName varchar(255) not null,
	Age int
);
```

### 实例2——创完表之后添加约束

在一个已创建的表的 "Age" 字段中添加 NOT NULL 约束如下所示：

```
alter table Persons 
modify Age int not null;
```



### 实例3——创完表之后删除约束

在一个已创建的表的 "Age" 字段中删除 NOT NULL 约束如下所示：

```
alter table Persons
modify Age int NULL;
```

实际上就是修改描述罢了，加上约束和不加上约束代码编写即可。



## 3.19 unique——约束

- UNIQUE 约束**唯一标识**数据库表中的每条记录。
- UNIQUE 和 PRIMARY KEY 约束：均为列或列集合提供了唯一性的保证。
- PRIMARY KEY 约束拥有自动定义的 UNIQUE 约束。
- 请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。



### 实例1——创建表时添加unique约束

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 UNIQUE 约束：

```
create table Persons (
	id int not null,
	LastName varchar(255) not null,
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255),
	unique (id)
);
```

可以发现，在MySql中，unique在最后的部分添加。



如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法：

```
create table Persons (
	id int not null,
	LastName varchar(255) not null,
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255),
	constraint personID unique (id, LastName)
);
```

personID即为表名，且定义了多个列。



### 实例2——已经有表后修改

当表已被创建时，如需在 "id" 列创建 UNIQUE 约束，请使用下面的 SQL：

```
alter table Persons
add unique (id);
```



如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法：

```
alter table Persons
add constraint personID unique (id, LastName);
```



### 实例3——撤销unique约束

如需撤销 UNIQUE 约束，请使用下面的 SQL：

```
alter table Persons
drop index personID;
```





## 3.20 primary key——约束

PRIMARY KEY 约束唯一标识数据库表中的每条记录。

- 主键必须包含唯一的值。
- 主键列不能包含 NULL 值。
- 每个表都应该有一个主键，并且每个表只能有一个主键。



### 实例1——创建表时的primary key

（1）下面的 SQL 在 "Persons" 表创建时在 "id" 列上创建 PRIMARY KEY 约束：

```
create table Persons
(
	id int not null,
	LastName varchar(255) not null,
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255),
	primary key (id)
);
```

（2）如需命名 PRIMARY KEY 约束，并定义多个列的 PRIMARY KEY 约束，请使用下面的 SQL 语法：

```
create table Persons
(
	id int not null,
	LastName varchar(255) not null,
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255),
	constraint PersonID primary key (id, LastName)
);
```

​	

### 实例2——已有表时添加primary key约束

（1）当表已被创建时，如需在 "id" 列创建 PRIMARY KEY 约束，请使用下面的 SQL：

```
alter table Persons
add primary key (id);
```

（2）如需命名 PRIMARY KEY 约束，并定义多个列的 PRIMARY KEY 约束，请使用下面的 SQL 语法：

```
alter table Persons
add constraint PersonID primary key (id, LastName);
```

**注释：**如果您使用 ALTER TABLE 语句添加主键，必须把主键列声明为不包含 NULL 值（在表首次创建时）。



### 实例3——撤销primary key约束

如需撤销 PRIMARY KEY 约束，请使用下面的 SQL：

```
alter table Persons
drop primary key;
```



## 3.21 foreign key——约束

外键约束，可以说就是某个项（如id）是受外部某个表的项（id）约束。

- FOREIGN KEY 约束用于预防破坏表之间连接的行为。
- FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。



### 实例1——创建表时foreign key约束

（1）下面的 SQL 在 "Orders" 表创建时在 "P_Id" 列上创建 FOREIGN KEY 约束：

```
create table olds
(
	O_id int not null,
	OrderNo int not null,
	P_Id int,
	primary key (O_id),
	foreign key (P_Id) references Persons(P_Id)
);
```

（2）如需命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束，请使用下面的 SQL 语法：

```
create table olds
(
	O_id int not null,
	OrderNo int not null,
	P_Id int,
	primary key (O_id),
	constraint PerOrders foreign key (P_Id) references Persons(P_Id)
);
```



### 实例2——已有表时的foreign key约束

（1）当 "Orders" 表已被创建时，如需在 "P_Id" 列创建 FOREIGN KEY 约束，请使用下面的 SQL：

```
alter table olds
add foreign key (P_Id) references Persons(P_Id);
```

（2）如需命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束，请使用下面的 SQL 语法：

```
alter table olds
add constraint PerOrders
foreign key (P_Id) references Persons(P_Id);
```



### 实例3——撤销primary key约束

如需撤销 FOREIGN KEY 约束，请使用下面的 SQL：

```
alter table olds
drop foreign key PerOrders;
```



## 3.22 check——约束

CHECK 约束用于限制列中的值的范围。

- 如果对单个列定义 CHECK 约束，那么该列只允许特定的值。
- 如果对一个表定义 CHECK 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。



（1）下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 CHECK 约束。CHECK 约束规定 "P_Id" 列必须只包含大于 0 的整数。

```
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    CHECK (P_Id>0)
);
```

（2）如需命名 CHECK 约束，并定义多个列的 CHECK 约束，请使用下面的 SQL 语法：

```
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
);
```

（3）当表已被创建时，如需在 "P_Id" 列创建 CHECK 约束，请使用下面的 SQL：

```
ALTER TABLE Persons
ADD CHECK (P_Id>0);
```

（4）如需命名 CHECK 约束，并定义多个列的 CHECK 约束，请使用下面的 SQL 语法：

```
ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes');
```

（5）如需撤销 CHECK 约束，请使用下面的 SQL：

```
ALTER TABLE Persons
DROP CHECK chk_Person;
```



## 3.23 default——约束

- DEFAULT 约束用于向列中插入默认值。
- 如果没有规定其他的值，那么会将默认值添加到所有的新记录。



（1）下面的 SQL 在 "Persons" 表创建时在 "City" 列上创建 DEFAULT 约束：

```
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
);
```

（2）通过使用类似 GETDATE() 这样的函数，DEFAULT 约束也可以用于**插入系统值**：

```
CREATE TABLE Orders
(
    O_Id int NOT NULL,
    OrderNo int NOT NULL,
    P_Id int,
    OrderDate date DEFAULT GETDATE()
);
```

（3）当表已被创建时，如需在 "City" 列创建 DEFAULT 约束，请使用下面的 SQL：

```
ALTER TABLE Persons
ALTER City SET DEFAULT 'SANDNES';
```

（4）如需撤销 DEFAULT 约束，请使用下面的 SQL：

```
ALTER TABLE Persons
ALTER City DROP DEFAULT;
```





## 3.24 create index——创建索引

- CREATE INDEX 语句用于在表中创建索引。
- 在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。



### 索引

- 在表中创建索引，以便更加快速高效地查询数据。
- 用户无法看到索引，它们只能被用来加速搜索/查询。

**注释：**更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。



### 分类

- create index：在表上创建一个简单的索引
- create unique index：在表上创建一个唯一的索引。不允许使用重复的值：唯一的索引意味着两个行不能拥有相同的索引值。



### 实例

（1）下面的 SQL 语句在 "Persons" 表的 "LastName" 列上创建一个名为 "PIndex" 的索引：

```
CREATE INDEX PIndex
ON Persons (LastName);
```

（2）如果您希望索引不止一个列，您可以在括号中列出这些列的名称，用逗号隔开：

```
CREATE INDEX PIndex
ON Persons (LastName, FirstName);
```



## 3.25 drop——撤销

通过使用 DROP 语句，可以轻松地删除索引、表和数据库。



### drop index —— 删除表中的索引

```
ALTER TABLE table_name DROP INDEX index_name;
```



### drop table——删除表

```
DROP TABLE table_name;
```



### drop database——删除数据库

```
DROP DATABASE database_name;
```



### truncate table——删除数据，不删除表

```
TRUNCATE TABLE table_name;
```



## 3.26 alter——修改

ALTER TABLE 语句用于在已有的表中添加、删除或修改列。



### 添加列

如需在表中添加列，请使用下面的语法:

```
ALTER TABLE table_name
ADD column_name datatype;
```

### 删除列

如需删除表中的列，请使用下面的语法（请注意，某些数据库系统不允许这种在数据库表中删除列的方式）：

```
ALTER TABLE table_name
DROP COLUMN column_name;
```

### 修改列的数据类型

要改变表中列的**数据类型**，请使用下面的语法：

```
ALTER TABLE table_name
MODIFY COLUMN column_name datatype;
```



### 实例

（1）现在，我们想在 "Persons" 表中添加一个名为 "DateOfBirth" 的列。

```
alter table persons 
add DateOfBirth Date;
```

（2）现在，我们想要改变 "Persons" 表中 "DateOfBirth" 列的数据类型。

```
ALTER TABLE Persons
MODIFY COLUMN DateOfBirth year;
```

（3）接下来，我们想要删除 "Person" 表中的 "DateOfBirth" 列。

```
ALTER TABLE Persons
DROP COLUMN DateOfBirth;
```





## 3.27 auto increment

- Auto-increment 会在新记录插入表中时生成一个唯一的数字。
- 我们通常希望在每次插入新记录时，自动地创建主键字段的值。我们可以在表中创建一个 auto-increment 字段。



（1）下面的 SQL 语句把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段：

```
create table Persons
(
	id int not null auto_increment,
	LastName varchar(255) not null,
	FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    primary key (id)
);
```

MySQL 使用 AUTO_INCREMENT 关键字来执行 auto-increment 任务。

默认地，AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。



（2）要让 AUTO_INCREMENT 序列以其他的值起始，请使用下面的 SQL 语法：

```
alter table Persons auto_increment = 100;
```





## 3.28 views——视图

视图是可视化的表。在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。

视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。

您可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，也可以呈现数据，就像这些数据来自于某个单一的表一样。



## 3.29 Date——日期

当我们处理日期时，最难的任务恐怕是确保所插入的日期的格式，与数据库中日期列的格式相匹配。

只要您的数据包含的只是日期部分，运行查询就不会出问题。但是，如果涉及**时间部分**，情况就有点复杂了。

| 函数                                                         | 描述                                |
| :----------------------------------------------------------- | :---------------------------------- |
| [NOW()](https://www.runoob.com/sql/func-now.html)            | 返回当前的日期和时间                |
| [CURDATE()](https://www.runoob.com/sql/func-curdate.html)    | 返回当前的日期                      |
| [CURTIME()](https://www.runoob.com/sql/func-curtime.html)    | 返回当前的时间                      |
| [DATE()](https://www.runoob.com/sql/func-date.html)          | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.runoob.com/sql/func-extract.html)    | 返回日期/时间的单独部分             |
| [DATE_ADD()](https://www.runoob.com/sql/func-date-add.html)  | 向日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.runoob.com/sql/func-date-sub.html)  | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.runoob.com/sql/func-datediff-mysql.html) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.runoob.com/sql/func-date-format.html) | 用不同的格式显示日期/时间           |



## 3.30 NULL

NULL 值代表遗漏的未知数据。

默认地，表的列可以存放 NULL 值。



如果表中的某个列是可选的，那么我们可以在不向该列添加值的情况下插入新记录或更新已有的记录。这意味着该字段将以 NULL 值保存。

NULL 值的处理方式与其他值不同：

- NULL 用作未知的或不适用的值的占位符。
- 无法使用比较运算符来测试 NULL 值，比如 =、< 或 <>。
- 我们必须使用 IS NULL 和 IS NOT NULL 操作符。



（1）我们必须使用 IS NULL 操作符：

```
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NULL;
```

（2）我们必须使用 IS NOT NULL 操作符：

```
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NOT NULL;
```



## 3.31——NULL函数

MySQL 也拥有类似 ISNULL() 的函数。不过它的工作方式与微软的 ISNULL() 函数有点不同。

在 MySQL 中，我们可以使用 IFNULL() 函数，如下所示：

```
SELECT ProductName,UnitPrice*(UnitsInStock+IFNULL(UnitsOnOrder,0))
FROM Products
```

或者我们可以使用 COALESCE() 函数，如下所示：

```
SELECT ProductName,UnitPrice*(UnitsInStock+COALESCE(UnitsOnOrder,0))
FROM Products
```













## 参考

- https://www.runoob.com/sql/sql-wildcards.html



