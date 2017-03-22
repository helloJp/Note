title: MySQL

---

<!--more-->

# 一、数据库概述
**数据库（DataBase，DB）**：指长期保存在计算机的存储设备上，按照一定规则组织起来，可以被各种用户或应用共享的数据集合。(文件系统)

**数据库管理系统（DataBase Management System，DBMS）**：指一种操作和管理数据库的大型软件，用于建立、使用和维护数据库，对数据库进行统一管理和控制，以保证数据库的安全性和完整性。用户通过数据库管理系统访问数据库中的数据。

数据库软件应该为**数据库管理系统**，数据库是通过数据库管理系统创建和操作的。

数据库：存储、维护和管理数据的集合。

# 二、数据库的配置


### 登录MySQL：
> mysql -u root -p 你的密码


### 修改密码
>
* 停止服务 (ui操作或者 net stop mysql)
* mysqld --skip-grant-tables 启动服务器（不要关闭窗口）
* 另起终端/cmd 输入 mysql -u root -p 不需要密码
  * use mysql;
  * update user set password=password('xxx') WHERE User='root';
* 重启 MySQL服务





### 数据库中一行记录与对象之间的关系。

>* 列：字段
* 行：一条记录(实体)


# 三、sql概述
SQL：Structure Query Language。（结构化查询语言）
SQL被美国国家标准局（ANSI）确定为关系型数据库语言的美国标准，后来被国际化标准组织（ISO）采纳为关系数据库语言的国际标准。
各数据库厂商都支持ISO的SQL标准。
各数据库厂商在标准的基础上做了自己的扩展。

# 四、Sql的分类
**DDL（Data Definition Language）**：数据定义语言，用来定义数据库对象：库、表、列等； 	CREATE、 ALTER、DROP  <br>
**DML（Data Manipulation Language）**：数据操作语言，用来定义数据库记录（数据）；    	INSERT、 UPDATE、 DELETE <br>
**DCL（Data Control Language）**：数据控制语言，用来定义访问权限和安全级别；<br>
**DQL（Data Query Language）**：数据查询语言，用来查询记录（数据）。SELECT

* 注意：sql语句以;结尾

## 4.1 DDL:操作数据库、表、列等
> 使用的关键字：CREATE、 ALTER、 DROP

### 4.1.1操作数据库
```
-- 创建
create database mydb1;
Create database mydb2 character set gbk;
Create database mydb3 character set gbk COLLATE gbk_chinese_ci;
```
```		
-- 查询
查看当前数据库服务器中的所有数据库
show databases;
查看前面创建的mydb2数据库的定义信息
Show  create  database mydb2;
删除前面创建的mydb3数据库
Drop database mydb3;
```		
```
--修改
查看服务器中的数据库，并把mydb2的字符集修改为utf8;
alter database mydb2 character set utf8;
```

```
--删除
drop database mydb3;
```
```				
--其他：
查看当前使用的数据库
select database();
切换数据库
use mydb2;
```

### 4.1.2操作数据表
* 语法：

```
create table 表名(
		字段1 字段类型,
		字段2 字段类型,
		...
		字段n 字段类型
);
```

* 常用数据类型：

```
int：整型
double：浮点型，例如double(5,2)表示最多5位，其中必须有2位小数，即最大值为999.99；
char：固定长度字符串类型； char(10)  'abc       '
varchar：可变长度字符串类型；varchar(10) 'abc'
text：字符串类型;
blob：字节类型；
date：日期类型，格式为：yyyy-MM-dd；
time：时间类型，格式为：hh:mm:ss
timestamp：时间戳类型 yyyy-MM-dd hh:mm:ss  会自动赋值
datetime:日期时间类型 yyyy-MM-dd hh:mm:ss
```


* 当前数据库中的所有表
<BR>SHOW TABLES;

* 查看表的字段信息
<BR>DESC employee;

* 在上面员工表的基本上增加一个image列。
<BR>ALTER TABLE employee ADD image blob;

* 修改job列，使其长度为60。
<BR>ALTER TABLE employee MODIFY job varchar(60);

* 删除image列,一次只能删一列。
<BR>ALTER TABLE employee DROP image;

* 表名改为user。
<BR>RENAME TABLE employee TO user;

* 查看表格的创建细节
<BR>SHOW CREATE TABLE user;

* 修改表的字符集为gbk
<BR>ALTER TABLE user CHARACTER SET gbk;

* 列名name修改为username
<BR>ALTER TABLE user CHANGE name username varchar(100);

* 删除表
<BR>DROP TABLE user ;

## 4.2 DML操作(重要)
查询表中的所有数据
SELECT * FROM 表名;

DML是对表中的数据进行增、删、改的操作。不要与DDL混淆了。
INSERT 、UPDATE、 DELETE

* 小知识：
> 在mysql中，字符串类型和日期类型都要用单引号括起来。'tom'  '2015-09-04' 空值：null

### 4.2.1插入操作：INSERT:

* 语法：
> INSERT INTO 表名（列名1，列名2 ...）VALUES(列值1，列值2...);

* 注意：
	* 列名与列值的类型、个数、顺序要一一对应。
	* 可以把列名当做java中的形参，把列值当做实参。
	* 值不要超出列定义的长度。
	* 如果插入空值，请使用null
	* 插入的日期和字符一样，都使用引号括起来。

* 练习 ：

```
create table emp(
	id int,
	name varchar(100),
	gender varchar(10),
	birthday date,
	salary float(10,2),
	entry_date date,
	resume text
);
```

```
INSERT INTO emp(id,name,gender,birthday,salary,entry_date,resume)
 VALUES(1,'zhangsan','female','1990-5-10',10000,'2015-5-5-','good girl');
```

```
INSERT INTO emp(id,name,gender,birthday,salary,entry_date,resume)
VALUES(2,'lisi','male','1995-5-10',10000,'2015-5-5','good boy');
```

```
INSERT INTO emp(id,name,gender,birthday,salary,entry_date,resume)
VALUES(3,'wangwu','male','1995-5-10',10000,'2015-5-5','good boy');
```

* 批量插入：

```
INSERT INTO emp VALUES
(4,'zs','m','2015-09-01',10000,'2015-09-01',NULL),
(5,'li','m','2015-09-01',10000,'2015-09-01',NULL),
(6,'ww','m','2015-09-01',10000,'2015-09-01',NULL);
```

### 4.2.2 修改操作 UPDATE:
* 语法：UPDATE 表名 SET 列名1=列值1，列名2=列值2 。。。 WHERE 列名=值

* 练习：
```
-- 将所有员工薪水修改为5000元。
UPDATE emp SET salary=5000
-- 将姓名为’zs’的员工薪水修改为3000元。
UPDATE emp SET salary=3000 WHERE name=’ zhangsan’;
-- 将姓名为’aaa’的员工薪水修改为4000元,job改为ccc。
UPDATE emp SET salary=4000,gender='female' WHERE name='lisi';
-- 将wu的薪水在原有基础上增加1000元。
UPDATE emp SET salary=salary+1000 WHERE gender='male';
 ```

### 4.2.3 删除操作 DELETE:
> 语法 ： DELETE FROM 表名 【WHERE 列名=值】

* 练习 ：
```
-- 删除表中名称为’zs’的记录。
DELETE FROM emp WHERE name=‘zs’;
-- 删除表中所有记录。
DELETE FROM emp;
-- 使用truncate删除表中记录。
TRUNCATE TABLE emp;
```

> * DELETE 删除表中的数据，表结构还在;删除后的数据可以找回
* TRUNCATE 删除是把表直接DROP掉，然后再创建一个同样的新表。
删除的数据不能找回。执行速度比DELETE快。


## 4.3 DQL操作
**DQL数据查询语言**
数据库执行DQL语句不会对数据进行改变，而是让数据库发送结果集给客户端。
查询返回的结果集是一张**虚拟表**。

* 查询关键字：**SELECT**
* 语法：
> SELECT 列名 FROM表名【WHERE --> GROUP BY -->HAVING--> ORDER BY】

* 语法：
  * SELECT selection_list /*要查询的列名称*/
  * FROM table_list /*要查询的表名称*/
  * WHERE condition /*行条件*/
  * GROUP BY grouping_columns /*对结果分组*/
  * HAVING condition /*分组后的行条件*/
  * ORDER BY sorting_columns /*对结果分组*/
  * LIMIT offset_start, row_count /*结果限定*/

创建名：
* 学生表：stu

|   字段名称     | 字段类型       | 说明     |
| ------------- |:-------------:| -----: |
| sid           | char(6)       | 学生学号 |
| sname         | varchar(50)   | 学生姓名 |
| age       		| int           | 学生年龄 |
| gender     		| varchar(50)   | 学生性别 |

```
CREATE TABLE stu (
	sid	CHAR(6),
	sname		VARCHAR(50),
	age		INT,
	gender	VARCHAR(50)
);
```

```
INSERT INTO stu VALUES('S_1001', 'liuYi', 35, 'male');
INSERT INTO stu VALUES('S_1002', 'chenEr', 15, 'female');
INSERT INTO stu VALUES('S_1003', 'zhangSan', 95, 'male');
INSERT INTO stu VALUES('S_1004', 'liSi', 65, 'female');
INSERT INTO stu VALUES('S_1005', 'wangWu', 55, 'male');
INSERT INTO stu VALUES('S_1006', 'zhaoLiu', 75, 'female');
INSERT INTO stu VALUES('S_1007', 'sunQi', 25, 'male');
INSERT INTO stu VALUES('S_1008', 'zhouBa', 45, 'female');
INSERT INTO stu VALUES('S_1009', 'wuJiu', 85, 'male');
INSERT INTO stu VALUES('S_1010', 'zhengShi', 5, 'female');
INSERT INTO stu VALUES('S_1011', 'xxx', NULL, NULL);
```

* 雇员表：emp

|   字段名称     | 字段类型       | 说明     |
| ------------- |:-------------:| -----: |
| empno         | int           | 员工编号 |
| ename         | varchar(50)   | 员工姓名 |
| job       		| varchar(50)   | 员工工作 |
| mgr           | int           | 领导编号 |
| hiredate   		| date     	    | 入职日期 |
| sal     	  	| decimal(7,2)  | 月薪  	|
| comm     	  	| decimal(7,2)  | 奖金  	|
| deptno        | int           | 部门编号 |

```
CREATE TABLE emp(
	empno		INT,
	ename		VARCHAR(50),
	job		VARCHAR(50),
	mgr		INT,
	hiredate	DATE,
	sal		DECIMAL(7,2),
	comm		decimal(7,2),
	deptno		INT
) ;
```

```
INSERT INTO emp values(7369,'SMITH','CLERK',7902,'1980-12-17',800,NULL,20);
INSERT INTO emp values(7499,'ALLEN','SALESMAN',7698,'1981-02-20',1600,300,30);
INSERT INTO emp values(7521,'WARD','SALESMAN',7698,'1981-02-22',1250,500,30);
INSERT INTO emp values(7566,'JONES','MANAGER',7839,'1981-04-02',2975,NULL,20);
INSERT INTO emp values(7654,'MARTIN','SALESMAN',7698,'1981-09-28',1250,1400,30);
INSERT INTO emp values(7698,'BLAKE','MANAGER',7839,'1981-05-01',2850,NULL,30);
INSERT INTO emp values(7782,'CLARK','MANAGER',7839,'1981-06-09',2450,NULL,10);
INSERT INTO emp values(7788,'SCOTT','ANALYST',7566,'1987-04-19',3000,NULL,20);
INSERT INTO emp values(7839,'KING','PRESIDENT',NULL,'1981-11-17',5000,NULL,10);
INSERT INTO emp values(7844,'TURNER','SALESMAN',7698,'1981-09-08',1500,0,30);
INSERT INTO emp values(7876,'ADAMS','CLERK',7788,'1987-05-23',1100,NULL,20);
INSERT INTO emp values(7900,'JAMES','CLERK',7698,'1981-12-03',950,NULL,30);
INSERT INTO emp values(7902,'FORD','ANALYST',7566,'1981-12-03',3000,NULL,20);
INSERT INTO emp values(7934,'MILLER','CLERK',7782,'1982-01-23',1300,NULL,10);
```

* 部门表：dept

|   字段名称     | 字段类型       | 说明     |
| ------------- |:-------------:| -----: |
| deptno         | int           | 部门编码 |
| dname         | varchar(50)   | 部分名称 |
| loc       		| varchar(50)   | 部分所在地点 |

```
CREATE TABLE dept(
	deptno		INT,
	dname		varchar(14),
	loc		varchar(13)
);
```

```
INSERT INTO dept values(10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO dept values(20, 'RESEARCH', 'DALLAS');
INSERT INTO dept values(30, 'SALES', 'CHICAGO');
INSERT INTO dept values(40, 'OPERATIONS', 'BOSTON');
```

# 1　基础查询
## 1.1　查询所有列
> SELECT * FROM stu;

## 1.2　查询指定列
> SELECT sid, sname, age FROM stu;

# 2　条件查询
## 2.1　条件查询介绍
* 条件查询就是在查询时给出WHERE子句，在WHERE子句中可以使用如下运算符及关键字：
	* =、!=、<>、<、<=、>、>=；
	* BETWEEN…AND；
	* IN(set)；
	* IS NULL； IS NOT NULL
	* AND；
	* OR；
	* NOT；


* 查询性别为女，并且年龄50的记录
```
SELECT * FROM stu WHERE gender='female' AND ge<50;
```

* 查询学号为S_1001，或者姓名为liSi的记录
```
SELECT * FROM stu WHERE sid ='S_1001' OR sname='liSi';
```

* 查询学号为S_1001，S_1002，S_1003的记录
```
SELECT * FROM stu WHERE sid IN ('S_1001','S_1002','S_1003');
```
* 查询学号不是S_1001，S_1002，S_1003的记录
```
SELECT * FROM tab_student WHERE s_number NOT IN ('S_1001','S_1002','S_1003');
```

* 查询年龄为null的记录
```
SELECT * FROM stu WHERE age IS NULL;
```

* 查询年龄在20到40之间的学生记录
```
SELECT *
FROM stu
WHERE age>=20 AND age<=40;
-- 或者
SELECT *
FROM stu
WHERE age BETWEEN 20 AND 40;
```
* 查询性别非男的学生记录
```
SELECT *
FROM stu
WHERE gender!='male';
-- 或者
SELECT *
FROM stu
WHERE gender<>'male';
-- 或者
SELECT *
FROM stu
WHERE NOT gender='male';
```
* 查询姓名不为null的学生记录
```
SELECT *
FROM stu
WHERE sname IS NOT NULL;
-- 或者
SELECT *
FROM stu
WHERE NOT sname IS NULL;
```

# 3 模糊查询

> 当想查询姓名中包含a字母的学生时就需要使用模糊查询了。模糊查询需要使用关键字**LIKE**。
通配符:**_**任意一个字符  	**%**：任意0~n个字符
'%张%'  '张_'



<br> * 查询姓名由5个字母构成的学生记录
```
SELECT *
FROM stu
WHERE sname LIKE '_____';
```
> 模糊查询必须使用LIKE关键字。其中 “_”匹配任意一个字母，5个“_”表示5个任意字母。

*　查询姓名由5个字母构成，并且第5个字母为“i”的学生记录
```
SELECT *
FROM stu
WHERE sname LIKE '____i';
```

*　查询姓名以“z”开头的学生记录
```
SELECT *
FROM stu
WHERE sname LIKE 'z%';
```
> 其中“%”匹配0~n个任何字母。

*　查询姓名中第2个字母为“i”的学生记录
```
SELECT *
FROM stu
WHERE sname LIKE '_i%';
```

*　查询姓名中包含“a”字母的学生记录
```
SELECT *
FROM stu
WHERE sname LIKE '%a%';
```

# 4　字段控制查询
*　去除重复记录
> 去除重复记录（两行或两行以上记录中系列的上的数据都相同），例如emp表中sal字段就存在相同的记录。当只查询emp表的sal字段时，那么会出现重复记录，那么想去除重复记录，需要使用DISTINCT：
* SELECT DISTINCT sal FROM emp;


* 查看雇员的月薪与佣金之和
> 因为sal和comm两列的类型都是数值类型，所以可以做加运算。如果sal或comm中有一个字段不是数值类型，那么会出错。* SELECT \*,sal+comm FROM emp;

> comm列有很多记录的值为NULL，因为任何东西与NULL相加结果还是NULL，所以结算结果可能会出现NULL。下面使用了把NULL转换成数值0的函数IFNULL：
* SELECT \*,sal+IFNULL(comm,0) FROM emp;

* 给列名添加别名
> 在上面查询中出现列名为sal+IFNULL(comm,0)，这很不美观，现在我们给这一列给出一个别名，为total：
SELECT \*, sal+IFNULL(comm,0) AS total FROM emp;
给列起别名时，是可以省略AS关键字的：
SELECT \*,sal+IFNULL(comm,0)  total FROM emp;

# 5　排序  order by 列名 asc(默认) desc
* 查询所有学生记录，按年龄升序排序
```
SELECT \*
FROM stu
ORDER BY sage ASC;
或者
SELECT \*
FROM stu
ORDER BY sage;
```

* 查询所有学生记录，按年龄降序排序
```
SELECT \*
FROM stu
ORDER BY age DESC;
```

* 查询所有雇员，按月薪降序排序，如果月薪相同时，按编号升序排序
```
SELECT \*  FROM emp
ORDER BY sal DESC,empno ASC;
```

# 6　聚合函数  sum avg max min count
聚合函数是用来做纵向运算的函数：
* COUNT()：统计指定列不为NULL的记录行数；
* MAX()：计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算；
* MIN()：计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算；
* SUM()：计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为0；
* AVG()：计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为0；

## 6.1　COUNT
当需要纵向统计时可以使用COUNT()。
* 查询emp表中记录数：
```
SELECT COUNT(\* ) AS cnt FROM emp;
```

* 查询emp表中有佣金的人数：
```
SELECT COUNT(comm) cnt FROM emp;
```
 * 注意，因为count()函数中给出的是comm列，那么只统计comm列非NULL的行数。

* 查询emp表中月薪大于2500的人数：
```
SELECT COUNT(\* ) FROM emp
WHERE sal > 2500;
```

* 统计月薪与佣金之和大于2500元的人数：
```
SELECT COUNT(\* ) AS cnt FROM emp WHERE sal+IFNULL(comm,0) > 2500;
```

* 查询有佣金的人数，有领导的人数：
```
SELECT COUNT(comm), COUNT(mgr) FROM emp;
```

## 6.2　SUM和AVG
> 当需要纵向求和时使用sum()函数。

* 查询所有雇员月薪和：
```
SELECT SUM(sal) FROM emp;
```

* 查询所有雇员月薪和，以及所有雇员佣金和：
```
SELECT SUM(sal), SUM(comm) FROM emp;
```

* 查询所有雇员月薪+佣金和：
```
SELECT SUM(sal+IFNULL(comm,0)) FROM emp;
```

* 统计所有员工平均工资：
```
SELECT AVG(sal) FROM emp;
```

## 6.3　MAX和MIN
* 查询最高工资和最低工资：
```
SELECT MAX(sal), MIN(sal) FROM emp;
```

# 7　分组查询

> 当需要分组查询时需要使用GROUP BY子句，例如查询每个部门的工资和，这说明要使用部门来分组。
注：凡和聚合函数同时出现的列名，一定要写在group by 之后


## 7.1　分组查询
* 查询每个部门的部门编号和每个部门的工资和：
```
SELECT deptno, SUM(sal)
FROM emp
GROUP BY deptno;
```

* 查询每个部门的部门编号以及每个部门的人数：
```
SELECT deptno,COUNT(\* )
FROM emp
GROUP BY deptno;
```

* 查询每个部门的部门编号以及每个部门工资大于1500的人数：
```
SELECT deptno,COUNT(\* )
FROM emp
WHERE sal>1500
GROUP BY deptno;
```

## 7.2　HAVING子句

* 查询工资总和大于9000的部门编号以及工资和：
```
SELECT deptno, SUM(sal)
FROM emp
GROUP BY deptno
HAVING SUM(sal) > 9000;
```
　　
	> 注：having与where的区别:
	* 1.having是在分组后对数据进行过滤.
		  where是在分组前对数据进行过滤
	* 2.having后面可以使用聚合函数(统计函数)
          where后面不可以使用聚合函数。
WHERE是对分组前记录的条件，如果某行记录没有满足WHERE子句的条件，那么这行记录不会参加分组；而HAVING是对分组后数据的约束。


## 8　LIMIT  
> LIMIT用来限定查询结果的起始行，以及总行数。

* 查询5行记录，起始行从0开始
```
SELECT \*  FROM emp LIMIT 0, 5;
```
 > 注意，起始行从0开始，即第一行开始！

* 查询10行记录，起始行从3开始
```
SELECT \*  FROM emp LIMIT 3, 10;
```

* 分页查询
如果一页记录为10条，希望查看第3页记录应该怎么查呢？
	* 第一页记录起始行为0，一共查询10行；
	* 第二页记录起始行为10，一共查询10行；
	* 第三页记录起始行为20，一共查询10行；
* 查询代码的书写顺序和执行顺序
>
	查询语句书写顺序：select – from- where- group by- having- order by-limit
	查询语句执行顺序：from - where -group by - having - select - order by-limit

# 六、数据的完整性
> 作用：保证用户输入的数据保存到数据库中是正确的。
确保数据的完整性 = 在创建表时给表中添加约束
完整性的分类：
* 实体完整性
* 域完整性
* 引用完整性

## 1、实体完整性
> 实体：即表中的一行(一条记录)代表一个实体（entity）
实体完整性的作用：标识每一行数据不重复。
约束类型： 主键约束（primary key）  唯一约束(unique)  自动增长列(auto_increment)

### 1.1主键约束（primary key）
特点：数据唯一，且不能为null
例：
第一种添加方式：
```
CREATE TABLE student(
Id int primary key,
Name varchar(50)
);
```
第二种添加方式：此种方式优势在于，可以创建联合主键
```
CREATE TABLE student(
id int,
Name varchar(50),
Primary key(id)
);

CREATE TABLE student(
id int,
Name varchar(50),
Primary key(id，name)
);
```
第三种添加方式：
```
CREATE TABLE student(
Id int,
Name varchar(50)
);
ALTER TABLE student
ADD  PRIMARY KEY (id);
```

### 1.2唯一约束(unique)：
```
	CREATE TABLE student(
Id int primary key,
Name varchar(50) unique
);
```


### 1.3自动增长列(auto_increment)

> 给主键添加自动增长的数值，列只能是整数类型，但是如果删除之前增长的序号，后面再添加的时候序号不会重新开始，而是会接着被删除的那一列的序号

```
CREATE TABLE student(
Id int primary key auto_increment,
Name varchar(50)
);

INSERT INTO student(name) values(‘tom’);
```

## 2、域完整性
>域完整性的作用：限制此单元格的数据正确，不对照此列的其它单元格比较
域代表当前单元格
域完整性约束：数据类型 非空约束（not null） 默认值约束(default)
Check约束（mysql不支持） check();

* 1.1 数据类型:（数值类型、日期类型、字符串类型）
* 1.2 非空约束：not null
```
CREATE TABLE student(
Id int pirmary key,
Name varchar(50) not null,
Sex varchar(10)
);

INSERT INTO student values(1,’tom’,null);
```
* 1.3 默认值约束 default
```
CREATE TABLE student(
Id int pirmary key,
Name varchar(50) not null,
Sex varchar(10) default ‘男’
);

insert into student1 values(1,'tom','女');
insert into student1 values(2,'jerry',default);
```

## 3、引用完整性
>要有外键必须先有主键，主键和外键的类型必须一致


  --感谢黑马教程🤣
