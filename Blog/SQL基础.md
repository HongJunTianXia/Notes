---
title: SQL基础
date: 2017-09-13 20:32:49
categories:
    - 数据库
tags:
	- 数据库
---


**一.sql入门**

**1.创建并使用数据库**
<!--more-->
~~~
create database studentdb;
use studentdb;// mysql刚打开时需要有一句使用某个数据库的指令。
create database wishwall charset utf8;
use wishwall;
create table messagebroad(
user_id int not null auto_increment,
user_name char not null,
artcle_id int not null,
com_title char not null,
comment text not null,
datetime timestamp not null,
primary key(user_id)
)engine=InnoDB default character set utf8;
~~~
**2.确认表是否存在**



~~~
show tables;(显示该数据库所有表)

show columns from users(表名);  显示某个表的所有列值

~~~

**3.插入记录**

~~~
insert into tablename(表名)

(列名1，列名2…)   //改行可以不写，按表的默认顺序全部赋值

values(列1对应值，列2对应值);
~~~

**插入多条记录**

~~~
insert into tablename(表名)

(列名1，列名2…)

values(列1对应值，列2对应值)，

(列1对应值，列2对应值)，

(列1对应值，列2对应值)；2.更新数据

update users（表名）

set pass（某列）="password";

~~~

**该指令的功能是将该列里的值全部赋值为’password’;**

**4.查询显示列表**

~~~
select *
from users
where last_name='wenyan';
~~~

**where为条件语句**

**5.**

**查询满足条件的元组**

**①**.字符匹配

like和not like

他们与两个通配符一起使用

下划线（_）用于匹配单个字符

和百分号(%)用于匹配0个或多个字符

例如:

~~~
select *

from users

where last_name=’sim%’;
~~~

该查询会显示出所有last_name为sim开头的用户的所有信息。

**②.比较大小**

**=，>,<,>=,<=,!=,<>,!<,!>;**

**③.**确定范围

BETWEEEN AND,**NOT**BETWEEN AND

**④.确定集合**

IN,NOT IN

~~~
SELECT Sname,Sdept,Sage

FROM Student

WHERE Sdept IN ('CS','MA','IS')
~~~

**⑤.空值**

IS NULL, IS NOT NULL

**⑥.多重条件**

AND,OR,NOT

**6.排序查询结果**

~~~
select *
from tablename
where age>15
order by column desc;
~~~

**默认为从小到大(asc)，从大到小的指令为（desc）;**

**7.消除重复行(distinct)**

~~~
SELECT DISTINCT Sno

FROM SC;
~~~

**8.限制查询结果的数量**

~~~
select *

from users

where age>15

order by age

limit 3;    查询显示3个年龄大于15岁的用户。按年龄升序的前三个
~~~

**9.更新数据**

~~~
update tablename

set column=value;
~~~

 ~~~
UPDATE Student

SET Sage=Sage+1;
 ~~~

在使用mysql执行update的时候，如果不是用主键当where语句，会报如下错误，使用主键用于where语句中正常。这是因为MySql运行在safe-updates模式下，该模式会导致非主键条件下无法执行update或者delete命令，执行命令SET SQL_SAFE_UPDATES = 0;修改下数据库模式

 

**10.删除某条记录**

如不确定删哪条，先找到要删记录的主键

~~~
select user_id

from users

where last_name='lishan' andfirst_name='zhang';

~~~

执行删除

 ~~~
delete

from users

where user_id=2622;
 ~~~

**11.连接查询**

两张表通过公共属性进行连接

~~~
SELECT Student.,SC.

FROM Student,SC

WHERE Student.Sno=SC.Sno;

~~~

**12.嵌套查询**

~~~
SELECT Sname

FROM Student

WHERE Sno IN

(

SELECT Sno

FROM SC

WHERE Cno='2';)

~~~

**13.GROUP BY子句**

~~~
SELECT Sno

FROM SC

GROUP BY Sno

HAVING COUNT(*)>3;

~~~

WHERE子句和HAVING短语的区别在于作用对象不同.WHERE子句作用于基本表或视图，从中找出满足条件的元组。HAVING短语作用于组，从中选择满足条件的组。

**14.视图**

~~~
CREATE VIEW IS_Student

AS

SELECT Sno,Sname,Sage

FROM Student

WHERE Sdept='IS'

WITH CHECK OPTION;
~~~

创建视图并进行查询，定义视图时加上WITH CHECK OPTION子句，以后对该视图进行插入，修改和删除操作时，关系数据库管理系统会自动加上Sdept='IS'的条件。

**15.SQL的数据定义语句**

| 操作对象 | 创建            | 删除          | 修改          |
| ---- | ------------- | ----------- | ----------- |
| 模式   | CREATE SCHEMA | DROP SCHEMA |             |
| 表    | CREATE TABLE  | DROP TABLE  | ALTER TABLE |
| 视图   | CREATE VIEW   | DROP VIEW   |             |
| 索引   | CREATE INDEX  | DROP INDEX  | ALTER INDEX |

**16.用户授权与取回**

**①.GRANT**

~~~
GRANT SELECT

ON TABLE Student

TO U1;
~~~

结尾如果加上WITH GRANT OPTION;表示允许用户传播该权限，但不允许循环授权。

ALL PRIVILEGES表示全部权限。

PUBLIC表示全部用户。

**②.REVOKE**

~~~
REVOKE SELECT

ON TABLE SC

FROM U2;
~~~

**③.创建角色**

~~~
CREATE ROLE U1;
~~~

**④.将这个角色授予其他角色**

~~~
GRANT U1 

TO 王平，张明，赵玲;
~~~

**⑤.通过U1收回王平得到的U1的权限。**

~~~
REVOKE U1

FROM 王平;
~~~

**17.索引**

①.建立索引

~~~
CREATE INDEX Stusno ON Student(Sno);
~~~

①.修改索引

~~~
ALTER INDEX SCno RENAME SCSno;
~~~

**15.使用函数**

**①.文本函数**

Concat(t1,t2,....)连接字符串

Concat_ws(s,t1,t2.....)返回t1st2的字符串

Left(t，y)返回串t左边y个的字符

Right(t，y)返回串t右边y个的字符

Length(t)返回串t的长度

Lower(t)将串转换为小写

Upper(t)将串转换为大写

LTrim()去掉串左边的空格

RTrim()去掉右边的空格

Trim(t)去掉左右两边的空格

Replace（t1，t2，t3）把t1字符串中的t2换为t3

SubString(t，x，y)返回t中始于x的y个字符的串

**②.数字函数**

Abs()返回一个数的绝对值

Cos()返回一个角度的余弦

Exp()返回一个数的指数值

Mod()返回除操作的余数Pi()

返回圆周率Pow（n1，n2）N1的n2次方

Rand()返回一个随机数

Round(n1,n2)返回数n1，它被四舍五入为n2位小数

Sin()返回一个角度的正弦

Sqrt()返回一个数的平方根

Tan()返回一个角度的正切

Ceiling(n)基于n的值的下一个最大的整数

Floor返回n的整数值

Format（n1,n2）

返回格式化为一个数的n1，这个数带有n2位小数，并且每3位之间插入一个逗号

**③.聚合函数**

AVG(column) 返回某列的平均值
BINARY_CHECKSUM  
CHECKSUM  
CHECKSUM_AGG  
COUNT(column) 返回某列的行数（不包括NULL值）
COUNT(*) 返回被选行数
COUNT(DISTINCT column) 返回相异结果的数目
FIRST(column) 返回在指定的域中第一个记录的值（SQLServer2000 不支持）
LAST(column) 返回在指定的域中最后一个记录的值（SQLServer2000 不支持）
MAX(column) 返回某列的最高值
MIN(column) 返回某列的最低值
STDEV(column)  
STDEVP(column)  
SUM(column) 返回某列的总和
VAR(column)  
VARP(column)

**二.数据库设计**

**①.第一范式**

要求最低，每个属性不可再分

**②.第二范式**

属于第一范式，且每一个非主属性完全函数依赖于任何一个候选码。

**③.第三范式**

属于第二范式，且每个非主属性都不传递函数依赖于主关系键。

三.

1.创建数据库使用utf8编码

~~~
create database forum

charset utf8

collate utf8_general_ci;

~~~

2.创建forums表

~~~
create table forums(

forum_id tinyint unsignednot null auto_increment,

name varchar(60) not null,

primary key(forum_id),

unique(name)

)engine=InnoDB;

~~~

3.创建users表

~~~
create table users(

user_id mediumint unsignednot null auto_increment,

username varchar(40) notnull,

pass char(40) not null,

first_name varchar(20) notnull,

last_name varchar(40) notnull,

email varchar(60) not null,

primary key(user_id),

unique(username),

unique(email),

index login(pass,email)

)engine=InnoDB;

~~~

4.创建messages表

~~~
insert intomessages(parent_id,forum_id,user_id,subject,body,date_entered)

values(0,1,123,'phpstudy','i love php.i have studied for few days',utc_timestamp()),

(0,1,1,'php study','i lovephp.i have studied for few days',utc_timestamp()),

(0,1,2,'Database design','im creating a new database and having problems',utc_timestamp()),

(2,1,2,'Databasedesign','the number of tables your database includes',utc_timestamp()),

(0,1,3,'Databasedesign','okay,thanks',utc_timestamp()),

(0,2,3,'font end','i amusing the scripts from chapter',utc_timestamp());
~~~

四.使用PHP和Mysql

```
1.<?php
DEFINE ('DB_USER','root');
DEFINE ('DB_PASSWORD','05100510');
DEFINE ('DB_HOST','localhost');
DEFINE ('DB_NAME','sitename');

$dbc=@mysqli_connect(DB_HOST,DB_USER,DB_PASSWORD,DB_NAME) OR die('Could not to connect to Mysql:'.mysqli_connect_error());

mysqli_set_charset($dbc, 'utf8');
?>
```

将该段代码作为一个文件，如果成功连接到mysql，则mysqli_connect()函数将返回对应于打开链接的资源链接，这里暂时打开空白页面。

如果该函数不能返回有效的资源链接，那么就会执行语句OR die()部分，输出错误报告。

~~~
 include ('mysqli_connect.php');//使用上述文件
~~~

五.高级sql和mysql

1.联结是使用两个或更多表的sql查询，它产生虚拟的结果表。任何需要同时从多个表检索信息时，都可以使用联结。

它的基本语法是:

①.内联结

~~~
SELECTm.message_id,m.subject,f.name

FROM messages AS m INNERJOIN forums AS f ON m.forum_id=f.forum.id

WHERE  f.name=’mysql’;
~~~

m.forum_id=f.forum.id称为等值连接，作为一种替代语法，如果在等值比较中两个表的列具有相同的名称，可以使用using简化查询

~~~
SELECTm.message_id,m.subject,f.name

FROM messages AS m INNERJOIN forums AS f USING(forum_id)

WHERE  f.name=’mysql’;
~~~

②.外联结

他将会返回两个表都匹配的记录和不匹配的记录。换句话说，内联记录是排他的，但外部联结是包含的。外联结有三个子类型：左联结，右联结，全联结，左联结是目前为止最重要的类型。

 ~~~
SELECT f.*,m.subject

FROM  forums AS f  LEFT JOIN  message  AS  mUSING(forum_id);
 ~~~

2.执行FULLTEXT查找

弥补了like的局限性。

3.执行事务

为了用MySql执行事务，必须用InnoDB表类型。

为了在MYSQL客户端中开始一个新事务，可以输入:

~~~
START TRANSACTION;

update users
set email='100001@qq.com'
where username='mike';
~~~

回滚事务

~~~
ROLLBACK;
~~~

确认结果

~~~
SELECT * FROM users;
~~~

提交事务并确认结果

~~~
COMMIT;
SELECT * FROM users;
~~~

一旦输入COMMIT，整个事务就会永久生效。COMMIT还会结束事务，将MySql返回到自动提交模式。

 

为了改变MySql的自动提交特性，可以输入

~~~
SET AUTOCOMMIT=0;
~~~

然后你不必输入STARTTRANSACTION,并且在输入COMMIT（或者使用ALTER,CREATE等查询）前任何查询都不会生效。

可以在事务中创建保存点

~~~
SAVEPOINT savepoint_name; 
~~~

4.数据库加密

SHA1()并不能提供真正的加密：SHA1()函数返回了一个值的代表（称为散列），而不是一个加密的值。

MYSQL有几个内置的加密和解密函数，如果你需要以可以解密的加密形式存储数据，则要使用AES_ENCRYPT和AES_DECRYPT。AES_ENCRYPT()函数被认为是最安全的加密选项。

要向表中添加纪录的同时对数据进行加密，需要用到类似以下的语句:

~~~
INSERT INTOusers(username,pass)

VALUES(‘trouter’,AES_ENCRYPT(‘mypass’,’nac456464fa’))
~~~

五. 让MySQL支持中文

1，创建table的时候就使用utf8编码

~~~
create table entries2 (
         id    int auto_increment,
         title text,
         content  text,
         posted_on  datetime,
         primary key (id)  
 ) character set = utf8;
~~~

在每次创建表的时候都在最后加上

character set = utf8;  就可以很好的支持中文。

2.修改已经有的table的编码

当使用默认编码创建了一个table的时候，是不能支持中文的，这时候使用如下语句对table_name进行修改：

```
alter table table_name convert to character set utf8;
```

此后再往这个table插入中文的时候，就可以正常存储和读取了，但不知道为什么之前的乱码还是不能纠正，只能新插入的数据没有问题。