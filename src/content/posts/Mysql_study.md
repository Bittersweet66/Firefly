---

title: MySQL数据库学习日记
published: 2026-06-24
description: 谨此记录学习SQL的过程。
tags: [Java, SQL，开发，MySQL]
category: SQL
author: Nomain
draft: false
slug: learning-sql

---
# Mysql数据库学习日记-基础篇
## day1

#### chapter 1 Mysql的数据库基本概念

关系数据库通过DBMS（DataBase Manage System），使用SQL语句进行操作，一个数据库里可以有多张表。

Mysql启动语句（命令行）

```cmd
mysql -u root -p
```
键入密码即可启动。


#### chapter 2 SQL通用语法和分类

SQL通用语法：
1. sql语句可以单行或者多行书写，**以分号结尾**。
2. sql语句可以用空格/缩进来增强语句可读性。
3. mysql数据库的sql语句不区分大小写，关键字建议大写。
4. 注释：
    单行注释： -- 注释内容 或者 # 注释内容（Mysql特有）
    多行注释： /* 注释内容 */

SQL分类：
1. DDL，数据库定义语言，涉及定义数据库（库、表、字段）
2. DML，数据库操作语言，涉及数据库增删改。
3. DQL，数据库查询语言，涉及查询。
4. DCL，数据库控制语言，用来创建数据库用户、控制数据库访问权限。


#### chapter 3 DDL语句

DDL-数据库操作
1. 查询
    查询所有数据库
    ```sql
    show databases;
    ```
    查询当前数据库
    ```sql
    select database();
    ```
2. 创建
    ```sql
    create database [if not exists] 数据库名 [default charset 字符集] [collate 排序规则];
    ```
3. 删除
     ```sql
    drop database [if exists] 数据库名;
    ```
4. 使用
    ```sql
    use 数据库名;
    ```

**注意Tips!**
1. []的内容为可选，if not exists指的是存在则不创建，添加这个可以防止报错。
2. “default charset 字符集”指的是存储数据的字符集规则，例如utf-8、utf-8mb4（4占位符utf8）。

DDL-表操作-查询
1. 查询当前数据库所有表
    ```sql
    show tables;
    ```
2. 查询表结构
    ```sql
    desc 表名;
    ```
3. 查询指定表的建表语句
    ```sql
    show create table 表名;
    ```

DDL-表操作-创建
```sql
create table 表名（
字段1 字段1类型 [COMMENT 字段1注释],
字段2 字段2类型 [COMMENT 字段2注释],
字段3 字段3类型 [COMMENT 字段3注释],
字段n 字段n类型 [COMMENT 字段n注释]
)[COMMENT 表注释];
```
**注意Tips!**
最后一段字段句不用加分号。

DDL-表操作-数据类型
1. 数值类型
    int           4 Bytes          大整数
    float         4 Bytes          单精度浮点数
    double        8 Bytes          双精度浮点数
    decimal          \             小数值（精确定点数）（依赖于精度 M 和标度 D ）

2. 字符串类型
    char          0-255 Bytes      定长字符串             例如：char(10)-----> 性能高
    varchar       0-65535 Bytes    变长字符串             例如：varchar(10)----> 性能差
    blob          0-65535 Bytes    二进制形式的长文本数据
    text          0-65535 Bytes    长文本数据  

3. 日期时间类型
    date          3                 1000-01-01 至 9999-12-31         YYYY-MM-DD     日期值
    time          3                 -838：59：59 至 838：59：59       HH：MM：SS     时间
    year          1                 1901 至 2155                     YYYY           年份
    datetime      8                 1000-01-01 00:00:00 至 9999-12-31 23:59:59       略
    timestamp     4                 1970-01-01 00:00:00 至 2038-01-19 03:14:07      时间戳

**注意区分有符号（signed）和无符号（unsigned）**

练习：创建员工表
 ```sql
create table emp(
    id int comment 'id',
    worknum varchar(10) comment 'num',
    name varchar(10) comment 'num',
    gender char(1) comment 'gender',
    age tinyint unsigned comment 'age',
    idcard char(18) comment 'idcard',
    entrydate date comment 'date'
) comment 'table';
 ```

DDL-表操作-修改
1. 添加字段
 ```sql
alter table 表名 add 字段名 类型（长度）[COMMENT 注释] [约束];
  ```
**案例：为emp添加一个新字段“昵称”为nickname，类型为varchar（20）。**
 ```sql
alter table emp add nickname varchar(20) COMMENT 'Nickname';
 ```

2. 修改字段
    2.1 修改数据类型
    ```sql
    alter table 表名 modify 字段名 新数据类型（长度）;
    ```
    2.2 修改字段名和字段类型
    ```sql
    alter table 表名 change 旧字段名 新字段名 类型（长度）[COMMENT 注释] [约束];
    ```
**案例：将emp表的nickname字段修改为username，类型为varchar（30）**
 ```sql
alter table emp change nickname username varchar(30) COMMENT 'Username';
 ```

3. 删除字段
 ```sql
alter table 表名 drop 字段名;
  ```

4. 修改表名
 ```sql
alter table 表名 rename to 新表名;
  ```


DDL-表操作-删除
1. 删除表
 ```sql
drop table [if exists] 表名;
  ```

2. 删除指定表，并重新创建该表
 ```sql
truncate table 表名;
  ```
**注意，在删除表时候，表中数据也会删除**


## day2
#### chapter 1 Mysql的图形化界面Datadrip

下载安装datadrip（略）

使用datadrip编写sql代码，测试。


#### chapter 2 DML语句
关键字：
1. 添加数据（insert）
2. 修改数据（update）
3. 删除数据（delete）

DML-添加数据
1. 给指定字段添加数据
 ```sql
insert into 表名(字段1,字段2……) values(value1,value2,……);
  ```

2. 给全部字段添加数据
 ```sql
insert into 表名 values(value1,value2,……);
  ```

3. 批量添加数据
 ```sql
insert into 表名(字段1,字段2……) values(value1,value2,……),(value1,value2,……),(value1,value2,……);
  ```

 ```sql
insert into 表名 values(value1,value2,……),(value1,value2,……),(value1,value2,……);
  ```

**注意Tips**
1. 插入数据时，指定字段顺序和值的顺序一一对应。
2. 字符串和日期型数据应该包含在引号中。
3. 插入数据大小应该在字段规定范围内。例如，你无法在int unsigned类型里插入-1.

DML-修改数据
 ```sql
update 表名 set 字段1=值1,字段2=值2, [where 条件];
  ```
注意：如果没有条件where，则会修改整张表的语句。

DML-删除数据
 ```sql
delete from 表名 [where 条件];
  ```
注意：
1. 如果没有条件where，则会删除整张表的所有数据。
2. delete不能删除某个字段的值（用update）。


#### chapter 3 DQL语句
关键字：select

DQL-语法
 ```sql
select
    字段列表
from
    表名列表
where
    条件列表
group by
    分组字段列表
having
    分组后条件列表
order by
    排序字段列表
limit
    分页参数
  ```

DQL-基本查询
1. 查询多个字段
 ```sql
select 字段1,字段2,字段3... from 表名;
select * from 表名;
  ```
2. 设置别名
 ```sql
select 字段1[as 别名1],字段2[as 别名2]... from 表名;
  ```
3. 去除重复记录
 ```sql
select distinct 字段列表 from 表名;
  ```

DQL-条件查询
 ```sql
select 字段列表 from 表名 where 条件列表;
  ```

**其中，条件有：>, <, >=, <=, =, <>或者!=, between and(在两值之间), in(列表), like 占位符(_匹配单个字符，%匹配任意个字符),**
**is NULL(为NULL), &&, ||, !**


## day3
#### chapter 1 DQL语句（续）

DQL-聚合函数
1. 介绍：将一列数据作为一个整体，进行纵向计算。
2. 常见聚合函数：
函数               功能
count            统计数量
max               最大值
min               最小值
avg               平均值
sum                求和

3. 语法
 ```sql
select 聚合函数(字段列表) from 表名;
  ```
注意：null值不参与聚合函数运算。

DQL-分组查询
1. 语法
 ```sql
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
  ```

**注意Tips**
where与having区别
> 执行时机不同：where是分组之前进行过滤（不满足where条件，不参与分组），而having是分组之后对结果进行过滤。
> 判断条件不同：where不能对聚合函数进行判断，而having可以。

案例：
1. 根据性别分组，统计男性员工和女性员工的数量
 ```sql
select gender, count(*) from emp group by gender;
  ```

2. 根据性别分组，统计男性员工和女性员工的平均年龄
 ```sql
select gender, avg(age) from emp group by gender;
  ```

3. 查询年龄小于45的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址
 ```sql
select workaddress, count(*) from emp where age<45 group by workaddress having count(*)>=3;
  ```

**注意Tips**
1. 执行顺序：where> 聚合函数> having。
2. 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。

DQL-排序查询
1. 语法
 ```sql
select 字段列表 from 表名 order by 字段1 排序方式1, 字段2 排序方式2;
  ```

2. 排序方式
> ASC:升序（默认值,可以不写）
> DESC：降序
**注意：如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。**

案例：根据年龄对公司的员工进行升序，年龄相同，再按照入职时间进行降序排序。
 ```sql
select * from emp order by age [asc], entrydate desc;
  ```

DQL-分页查询
1. 语法
 ```sql
select 字段列表 from 表名 limit 起始索引, 查询记录数;
  ```
2. 注意：
> 起始索引从0开始，起始索引=（查询页码-1）*每页显示记录数。
> 分页查询是数据库的方言，不同的数据库有不同的实现，MYSQL中是limit。
> 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10。

**案例练习**
1. 查询年龄为20,21,22,23岁的员工信息。
2. 查询性别为男，并且年龄在20-40岁(含)以内的姓名为三个字的员工。
3. 统计员工表中，年龄小于60岁的，男性员工和女性员工的人数。
4. 查询所有年龄小于等于35岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按入职时间降序排序。
5. 查询性别为男，且年龄在20-40岁(含)以内的前5个员工信息，对查询的结果按年龄升序排序，年龄相同按入职时间升序排序。
附录：建表语句如下：
 ```sql
  insert into emp (id, workno, name, gender, age, idcard, entrydate, workaddress)
    values (1,'1','刘艳','女',20,'123456789012345678','2000-01-01','北京'),
          (2,'2','张无忌','男',18,'123456789112345678','2000-02-01','北京'),
          (3,'3','韦一笑','男',38,'123456789212345678','2000-03-01','上海'),
          (4,'4','赵敏','女',18,'123456789312345678','2000-04-01','北京'),
          (5,'5','小邵','女',16,'123456789412345678','2000-05-01','上海'),
          (6,'6','杨坡','男',28,'12345678951234567X','2000-06-01','北京'),
          (7,'7','范瑶','女',40,'123456789612345678','2000-07-01','天津');
  ```
解答：
 ```sql
select * from emp where age in(20,21,22,23);
select * from emp where gender = '男' and age between 20 and 40 and name like '___';
select gender, count(*) from emp where age<60 group by gender;
select name, age from emp where age<=35 order by age, entrydate desc ;
select * from emp where gender = '男' and age between 20 and 40 order by age,entrydate limit 5;
  ```

DQL-执行顺序
1. from
2. [where]
3. group by [having]
4. select
5. order by
6. limit


#### chapter 2 DCL语句
DCL-介绍
DCL英文全称是DataControl Language(数据控制语言)，用来管理数据库用户、控制数据库的访问权限。

DCL-用户管理
1. 查询用户
 ```sql
use mysql;
select * from user;
  ```
2. 创建用户
 ```sql
create user '用户名'@'主机名' identified by '密码';
  ```
3. 修改用户密码
 ```sql
alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';
  ```
4. 删除用户
 ```sql
drop user '用户名'@'主机名';
  ```

注意：
1. 主机名可以使用 % 通配。
2. 这类sql开发人员操作的比较少，主要是DBA（DatabaseAdministrator数据库管理员）使用。

DCL-权限控制
MySQL中定义了很多种权限，但是常用的就以下几种：
权限                                             说明
ALL,ALLPRIVILEGES                              所有权限
SELECT                                         查询数据
INSERT                                         插入数据
UPDATE                                         修改数据
DELETE                                         删賒数据
ALTER                                           修改表
DROP                                       删除数据库/表/视图
CREATE                                       创建数据库/表

1. 查询权限
 ```sql
show grants for'用户名'@'主机名';
  ```
2. 授予权限
 ```sql
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
  ```
3. 撤销权限
 ```sql
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
  ```


#### chapter 3 函数
函数是指一段可以直接被另一段程序调用的程序或代码。

一、字符串函数

函数                                         功能
CONCAT(S1,S2...Sn)            字符串拼接，将S1,S2,..Sn拼接成一个字符串
LOWER(str)                         将字符串str全部转为小写
UPPER(str)                         将字符串str全部转为大写
LPAD(str,n,pad)            左填充，用字符串pad对str的左边进行填充，达到n个字符串长度
RPAD(str,n,pad)            右填充，用字符串pad对str的右边进行填充，达到n个字符串长度
TRIM(str)                           去掉字符串头部和尾部的空格
SUBSTRING(str,start,len)         返回从字符串str从start位置起的len个长度的字符串

二、数值函数

函数                                         功能
CEIL(X)                                    向上取整
FLOOR(X)                                   向下取整
MOD(xy)                                   返回x/y的模b
RAND()                                 返回0~1内的随机数
ROUND(x,Y)                        求参数x的四舍五入的值，保留y位小数

三、日期函数

函数                                         功能
CURDATE()                                返回当前日期
CURTIME()                                返回当前时间
NOW()                                 返回当前日期和时间
YEAR(date)                            获取指定date的年份
MONTH(date)                           获取指定date的月份
DAY(date)                             获取指定date的日期
DATE_ADD(date, INTERVAL expr type)    返回一个日期/时间值加上一个时间间隔expr后的时间值
DATEDIFF(datel,date2)                 返回起始时间date1和结束时间date2之间的天数

四、流程函数
流程函数也是很常用的一类函数，可以在SQL语句中实现条件筛选，从而提高语句的效率。

函数                                         功能
IF(value,t,f)                      如果value为true，则返回t，否则返回f
IFNULL(valuel,value2)              如果value1不为空，返回value1，否则返回value2
CASE WHEN[vall THEN [resI]..ELSE[default]END          如果val1为true，返回res1,..否则返回default默认值
CASE[expr]WHEN[val1] THEN[res1].ELSE[default]END        如果expr的值等于val1，返回res1,...否则返回default默认值


## day4
#### chapter 1 约束

概述：
1. 概念：约束是作用于表中字段上的规则，用于限制存储在表中的数据。
2. 目的：保证数据库中数据的正确、有效性和完整性。
3. 分类:
约束                                         描述                                 关键字
非空约束                           限制该字段的数据不能为null                      NOT NULL
唯一约束                        保证该字段的所有数据都是唯一、不重复的               UNIQUE
主键约束                       主键是一行数据的唯一标识，要求非空且唯一             PRIMARY KEY
默认约束                  保存数据时，如果未指定该字段的值，则采用默认值             DEFAULT
检查约束                         保证字段值满足某一个条件                           CHECK
外键约束               用来让两张表的数据之间建立连接，保证数据的一致性和完整性       FOREIGN KEY

案例：根据需求构建表
字段名         字段含义          字段类型            约束条件
id           ID唯一标识           int           主键，并且自动增长
name            姓名           varchar(10)        不为空，并且唯一
age             年龄              int         大于0，并且小于等于120
status          状态             char(1)     如果没有指定该值，默认为1
gender          性别             char(1)              无

```sql
create table user(
    id int primary key  auto_increment comment '主键',
    name varchar(10) not null unique comment '姓名',
    age int check ( age > 0 and age <= 120 ) comment '年龄',
    status char(1) default '1' comment '状态',
    gender char(1) comment '性别'
) comment '用户表';
  ```

外键约束：
1. 概念
外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。

2. 语法：
```sql
create table 表名(
字段名 数据类型,
[CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名)
);
 ```
```sql
ALTER TABLE 表名 ADD CONSTRANT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);
  ```

3. 删除更新行为
行为                                                                   说明
NO ACTION       当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。（与RESTRICT一致)
RESTRICT        当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。（与NOACTION一致)
CASCADE         当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有，则也删除/更新外键在子表中的记录。
SET NULL        当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（这就要求该外键允许取null)。
SET DEFAULT     父表有变更时，子表将外键列设置成一个默认的值(Innodb不支持）

语法：
```sql
ALTER TABLE 表名 ADD CONSTRANT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名) on update 行为 on delete 行为;
  ```


## day5
#### chapter 1 多表查询
##### chapter 1.1 多表查询概述

多表关系概述：项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所
以各个表结构之间也存在着各种联系，基本上分为三种：
> 一对多(多对一)
> 多对多
> 一对一

多表关系：
1. 一对多：在多的一方建立外键，指向一的一方的主键。

2. 多对多：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键。
案例：学生与课程的关系。一个学生可以选修多门课程，一门课程也可以供多个学生选择。
代码如下，
表一：
```sql
create table student(
id int auto_increment primary key comment '主键ID',
name varchar(10) comment '姓名',
no varchar(10) comment '学号'
) comment '学生表';
insert into student values(null,'黛绮丝','2808100101'),(null,'谢逊','2080100182'),(null,'股天正','2800100103'),(null
  ```
表二：
```sql
create table course
(
    id   int auto_increment primary key comment '主健ID',
    name varchar(10) comment '课程名称'
) comment '课程表';
insert into course values (null,'Java'),(null,'PHP'),(null,'MySQL'), (null,'Hadoop');
  ```
表三（中间表）：
```sql
create table student_course(
id int auto_increment comment '主键' primary key,
studentid int not null comment '学生ID',
courseid int not null comment '课程ID',
constraint fk_courseid foreign key(courseid) references course(id),
constraint fk_studentid foreign key (studentid) references student(id)
) comment '学生课程中间表';
insert into student_course values (null,1,1),(null,1,2), (null,1,3), (null,2,2), (null,2,3),(null,3,4);
  ```

3. 一对一：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率。

多表查询：指从多张表中查询数据

单表查询：select * from emp;
如果：select * from emp, dept;（即罗列了两张表）
则结果为两表的**笛卡尔积**。

多表查询即为去除无效笛卡尔积。
加入where约束即可。
代码：
```sql
select * from emp, dept where emp.dept_id = dept_id;
  ```
效果为查询两表dept_id相同的数据，即可去除无效笛卡尔积。

多表查询-分类
1. 连接查询
  内连接：相当于查询A、B交集部分数据
  外连接：
    左外连接：查询左表所有数据，以及两张表交集部分数据
    右外连接：查询右表所有数据，以及两张表交集部分数据
  自连接：当前表与自身的连接查询，**自连接必须使用表别名**。
2. 子查询


##### chapter 1.2 内连接
语法：
1. 隐式内连接
```sql
select * from 表一,表二 where 条件;
  ```
2. 显式内连接
```sql
select * from 表一 [inner] join 表二 on 条件;
  ```

##### chapter 1.3 外连接
语法：
1. 左外连接
```sql
select * from 表一 left [outer] join 表二 on 条件;
  ```
2. 右外连接
```sql
select * from 表一 right [outer] join 表二 on 条件;
  ```

##### chapter 1.4 自连接
语法：
```sql
select * from 表A 别名A join 表A 别名B on 条件;
  ```
注意：**自连接查询，可以是内连接查询，也可以是外连接查询。**

##### chapter 1.5 联合查询
对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。
语法：
```sql
select * from 表A ;
union [all]
select * from 表B ;
  ```
注意：
1. 对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。
2. union all直接将两表合并，union先去重再合并。

## day6
#### chapter 1 子查询

概念：SQL语句中嵌套SELECT语句，称为嵌套查询，又称子查询。
语法：
```sql
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
  ```

根据子查询结果不同，分为：
> 标量子查询(子查询结果为单个值)
> 列子查询(子查询结果为一列)
> 行子查询(子查询结果为一行)
> 表子查询(子查询结果为多行多列)

根据子查询位置，分为：WHERE之后、FROM之后、SELECT之后。

1. 标量子查询
子查询返回的结果是单个值（数字、字符串、日期等），最简单的形式，这种子查询成为标量子查询。
常用的操作符：= <> > >= < <=

2. 列子查询
子查询返回的结果是一列（可以是多行），这种子查询称为列子查询。
常用的操作符：IN、NOT IN、ANY、SOME、ALL
操作符                                描述
IN                          在指定的集合范围之内，多选一
NOT IN                         不在指定的集合范围之内
ANY                     子查询返回列表中，有任意一个满足即可
SOME                    与ANY等同，使用SOME的地方都可以使用ANY
ALL                       子查询返回列表的所有值都必须满足

3. 行子查询
子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。
常用的操作符：=、<、IN、NOT IN

4. 表子查询
子查询返回的结果是多行多列，这种子查询称为表子查询。
常用的操作符：IN


#### chapter 2 事务

事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作
请求，即这些操作要么同时成功，要么同时失败。

语法：
1. 查看/设置事务提交方式
```sql
SELECT @@autocommit；
SET @@autocommit=0;
  ```

2. 开启事务
```sql
start transaction; 或者 begin;
  ```
3. 提交事务
```sql
COMMIT;
  ```

4. 回滚事务
```sql
ROLLBACK;
  ```

事务的四大特性-ACID
原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。
隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

并发事务问题
1. 脏读：—个事务读到另外一个事务还没有提交的数据。
2. 不可重复读：一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读。
3. 幻读：一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了
幻影”。

事务隔离级别
隔离级别                        赃读          不可重复读             幻读
Readuncommitted                 √                 √                  √
Read committed                  x                 √                  √
Repeatable Read(默认)           x                 x                  √
Serializable                    x                 x                  x

语法：
-查看事务隔离级别
```sql
SELECT @@TRANSACTION_ISOLATION;
  ```

-设置事务隔离级别
```sql
SET [SESSION|GLOBAL] TRANSACTION ISOLATION LEVEL {READUNCOMMITTED | READCOMMITTED | REPEATABLEREAD | SERIALIZABLE};
  ```
