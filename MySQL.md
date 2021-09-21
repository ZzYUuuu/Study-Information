#  MySQL

## 1.导入数据库

### 1.1dos命令窗口：

```mysql
login:
mysql -uroot -p123456
```

***

### 1.2查看数据库

```mysql
show databases;
```

### 1.3创建数据库

```mysql
create database "name";
```

### 1.4使用“name”数据

```mysql
use "name";
```

### 1.5查询当前目录有哪写表格

```mysql
show tables;
```

### 1.6初始化数据

```mysql
source C:\Users\Administrator\Desktop\MySQL\bjpowernode.sql
```

### 1.7SQL脚本

当一个文件的扩展名是.sql，并且该文件编写了大量的sql语句，我们称为sql脚本。

sql脚本中的数据量太大，无法打开，请使用source命令。

### 1.8删除数据库

```mysql
drop database "name";
```

### 1.9查询表的结构

```mysql
desc "name";
```

***

## 常用命令

```mysql
终止指令：
\c
```

***

```mysql
退出mysql：
exit
```

***

```mysql
查看创建表的语句：
SHOW CREATE TABLE EMP;
```

***

## 2.MySQL语句

### 2.1  SELECT(DQL):

```mysql
SELECT 字段名1，字段名2，字段名3，.....FROM 表名;
```

***

***

查询DEPT表的所有内容：

```mysql
SELECT * FROM DEPT;
```

***

查询EMP表的所有内容：

```mysql
SELECT * FROM EMP;
```

***

查询SALGRADE表的所有内容：

```mysql
SELECT * FROM SALGRADE;
```

***

查询员工的年薪（字段可以参与数学运算）：

```
SELECT ENAME,SAL * 12 FROM EMP;
```

***

给查询结果的列重命名：

```mysql
SELECT ENAME,SAL * 12 AS YEARSAL FROM EMP;
SELECT ENAME,SAL * 12 YEARSAL FROM EMP;
```

注意：标准sql语句要求字符串使用单引号括起来。

```
SELECT ENAME,SAL * 12 AS '年薪' FROM EMP;
```

### 2.2条件查询SELECT:

```mysql
SELECT
	字段,字段...
FROM
	表名
WHERE
	条件;
```

执行顺序:先FROM，然后WHERE，最后SELECT

***

查询工资等于5k的员工姓名

```
SELECT
	ENAME
FROM
	EMP
WHERE
	SAL=5000;
```

***

查询'SMITH'的工资；

```
 SELECT SAL FROM EMP WHERE ENAME = 'SMITH';
```

***

找出工资不等3000的？

```mysql
SELECT ENAME,SAL FROM EMP WHERE SAL<>3000;
SELECT ENAME,SAL FROM EMP WHERE SAL!=3000;
```

***

找出工资在1100和300之间的员工，包括两者

```mysql
SELECT ENAME,SAL FROM EMP WHERE BETWEEN 1100 AND 3000;
SELECT ENAME,SAL FROM EMP WHERE BETWEEN 3000 AND 1100;//查询不到任何数据
```

***

哪些人没有津贴

```mysql
select ename, sal,comm from emp where comm is null;
```

***

找出工作岗位是MANAGER和SALESMAN的员工

```mysql
step1:(AND)
select ename,job from emp where job = 'manager' or job = 'salesman';
step2:
(IN) 在这几个值当中 
(NOT IN)不在这几个值当中
select ename,job from emp where job in ('manager' ,'salesman');
```

***

找出薪资大于3000切部门编号是20或30的员工

```mysql
select ename,sal,deptno from emp where sal>1000 and (deptno = 20 or deptno = 30);
```

### 2.3模糊查询LIKE：

在模糊查询中必须掌握两个特殊符号

1.%代表任意多个字符

2._代表一个字符

***

找出名字中含有o的

```mysql
select ename from emp where ename like '%o%';
```

***

找出第二个字母是A的

```mysql
select ename from emp where ename like '_A%';
```

### 2.4排序：

asc表示升序，desc表示降序

***

按照工资升序，找出员工名和薪资

```mysql
select ename,sal from emp order by sal asc;
select ename,sal from emp order by 2;//按第二列排序
select ename,sal from emp order by sal desc;//降序
```

***

按照工资降序排，工资一样按名字升序排

```mysql
select ename,sal from emp order by sal desc, ename asc;
```

***

找出工作岗位是SALESMAN的员工，并且要求按照薪资降序排列。

```mysql
select ename,job,sal from emp where job = 'salesman' order by sal desc;
```

***

#### 执行顺序

```
select		3
	字段
from		1
	表名
where		2	
	条件
order by	4
	....
```

### 2.5分组函数：

count 计数

sum 求和

avg 平均数

max 最大值

min 最小值

记住：所有的分组函数都是对某一组数据进行操作。

***

count(*) 和 count(具体的某个字段)区别：

​	count(*):不是同居某个字段中的数据的个数，而是统计总统计条数(和某个字段无关)。

​	count(字段):表示统计字段中不为NULL的数据总数量。

***

组合使用：

​		select count(*),sum(sal),avg(sal),max(sal),min(sal) from emp;

***

***

找出工资总和

```mysql
select sum(sal) from emp;
```

找出最高工资

```mysql
select max(sal) from emp;
```

找出最低工资

```mysql
select min(sal) from emp;
```

找出平均工资

```mysql
select avg(sal) from emp;
```

找出总人数

```mysql
select count(*) from emp;
```

***

##### 多行处理函数

特点：*输入多行 输出一行

​			*自动忽略NULL

***

##### 单行处理函数

特点：*输入一行，输出一行

***

**计算每个员工的年薪**

```mysql
select ename,(sal+ifnull(comm,0))*12 as yearsal from emp;
```

ifnull()  空处理函数

***

**找出工资高于平均工资的员工**

(子查询)

```mysql
select ename,sal from emp where sal > 
(select avg(sal) from emp);
```

***

### 2.6 group by 和 having：

group by : **按照某个字段或者某些字段进行分组。**

having：**having是对分组后的数据再次过滤。**

***

分组函数一般和group by联合使用。

**任何一个分组函数都是在group by之后语句执行结束后执行**

**而group by是在where执行之后才会执行的。**

```mysql
select 	 5
	..
from   	 1
	..
where  	 2
	..
group by 3
	..
having   4
	..
order by 6
	..
```

***

案例:找出某个岗位最高薪资

```mysql
 select max(sal),job from emp group by job; 
```

案例:找出每个工作岗位的平均薪资

```mysql
select job,avg(sal) as avg from emp group by job order by avg(sal) desc;
```

案例：找出每个部门不同岗位的最高薪资

```mysql
select deptno, job, max(sal) from emp group by deptno, job order by deptno;
```

案例:找出每个部门的最高薪资，要求显示薪资大于2900

**能够使用where过滤的尽量使用where过滤**

**不能使用的使用having过滤**

```mysql
select deptno, max(sal) from emp group by deptno having max(sal) > 2900 order by deptno;//效率低
```

***

```mysql
select deptno, max(sal) from emp where sal > 2900 group by deptno;//效率高
```

案例:找出每个部门的平均薪资

```mysql
select avg(sal), deptno from emp group by deptno having avg(sal) > 2900;
```

### 2.7总结完整的DQL语句：

```mysql
select
	..
from 
	..
where
	..
group by
	..
having
	..
order by
	..
```

### 2.8查询结果集去重

**distinct**

```mysql
select
	distinct job
from
	emp;
```

**联合去重**：

```mysql
select distinct deptno,job from emp;
```

案例:统计岗位的数量

```mysql
select count(distinct job) from emp;
```

### 2.9连接查询

**年代分类**：

​		SQL92

​		SQL99

**表的连接方式划分**：

​	内连接：inner

​		等值连接

​		不等值连接

​		自连接

​	外连接：outer

​		左外连接  left

​		右外连接  right

#### 2.9.1内连接之等值连接

特点：**条件是等量关系	**e.deptno=d.deptno

案例：查询每个员工的部门名称，要求显示员工名和部门名

```mysql
select 
	e.ename,d.dname 
from 
	emp e 
inner(内连接,可省略) join 
	dept d 
on
	e.deptno=d.deptno;
```

#### 2.9.2内连接之非等值连接

特点：**连接条件中的关系是非等量关系**

案例：找出每个员工的工资等级，要求显示员工名、工资、工资等级。

```mysql
select e.ename, e.sal,s.grade from emp e inner join salgrade s on  e.sal between s.losal and s.hisal;
```

#### 2.9.3 自连接：

特点：把一张表看作两张表，自己连接自己

```mysql
select a.ename,b.ename from emp a inner join emp b on a.mgr=b.empno;
```

#### 2.9.4外连接：

**SQL语句上有left,right区分内连接和外连接**

和内连接的区别：

​	内连接：假设A和B表进行连接，凡是A和B能匹配上的记录查询出来，是内连接。

​	AB两张表没有主副之分，是平等的。

外连接：

​	假设A和B表进行连接，使用外连接，AB两张表中有一张表是主表一张是副表，主要查询

​	主表中的数据，捎带查询副表，当副表中的数据没有和主表中的数据匹配上，副表自动模拟NULL与之匹配。

左连接有右连接的写法，右连接有左连接的写法。

***

案例：找出每个员工的上级领导

```mysql
select a.ename '员工', b.ename '领导' from emp a left outer join emp b on a.mgr=b.empno;
```



***

案例：找出哪个部门没有员工

where判断为空：**is null**

```
select
	d.*
from 
	dept d 
left join
	emp e 
on
	d.deptno=e.deptno 
where
	e.ename is null;
```

案例：找出每一个员工的部门名称以及工资等级

```
select
	e.ename,d.dname,s.grade
from
	emp e
left join 
	dept d
on
	e.deptno=d.deptno
left join 
	salgrade s
on
	e.sal between s.losal and s.hisal;
```

案例：找出每一个员工的部门名称、工资等级以及上级领导。

```mysql
select
	e.ename '员工',d.dname '部门' ,s.grade '等级' ,e2.ename '上级'
from
	emp e
join 
	dept d
on
	e.deptno=d.deptno
join 
	salgrade s
on
	e.sal between s.losal and s.hisal
left join
	emp e2
on
	e.mgr=e2.empno
order by
	grade;
```

###  2.10子查询

select语句中嵌套select语句，被嵌套的语句是子查询

***

子查询可以出现在哪里

***

where

```mysql
select
	...(select)
from
	...(select)
where
	...(select)
```

案例：找出高于平均工资的员工信息

```mysql
select * from emp where sal > (select avg(sal) from emp);
```

***

from

案例：找出每个部门平均薪水的薪资等级

```mysql
查询每个部门的平均工资：new (a)表
select
	d.dname,avg(e.sal) avgsal
from
	emp e
join 
	dept d
on
	e.deptno=d.deptno
group by
	d.dname;
```

```mysql
select 
	a.dname ,s.grade
from
	salgrade s,
		(select
            d.dname,avg(e.sal) avgsal
        from
            emp e
        join 
            dept d
        on
            e.deptno=d.deptno
        group by
            d.dname) a
where 
	a.avgsal between s.losal and s.hisal
```

案例：找出每个部门平均的薪水等级

​	step:找出每个员工的薪水等级

```
select 
	e.deptno,avg(s.grade)
from
	emp e
join
	salgrade s
on
	e.sal between s.losal and s.hisal
group by
	e.deptno;
```

***

select

案例:找出每个员工所在的部门名称，要求显示员工名和部门名

```mysql
select
	e.ename,d.dname
from
	emp e
join 
	dept d
on
	e.deptno=d.deptno;

```

### 2.11UNION

可以将查询结果集相加

**两张不相干的表中数据拼接在一起显示**

案例：找出工作岗位是SALESMAN和MANAGER的员工

```mysql
select
	ename,job
from
	emp
where
	job='SALESMAN' or job='MANAGER';
```

***

```mysql
select
	ename,job
from
	emp
where
	job in('SALESMAN','MANAGER');
```

***

```mysql
select
	ename,job
from
	emp
where
	job in('SALESMAN')
union
select
	ename,job
from
	emp
where
	job in('MANAGER');
```

***

### 2.12limit*****

limit是mysql特有的，其他数据库没有，不通用（Oracle中有一个相同的机制，叫rownum）

limit取结果集中的部分数据

执行顺序：

```mysql
select		5
	..
from		1
	..
where		2
	..
group by	3
	..
having		4	
	..
order by	6
	..
limit		7
	..
```

***

语法机制：

```mysql
limit startIndex,length
	startIndex表示起始位置
	length表示取几个
```

***

案例：取出工资前五的员工

```mysql
select
	ename,sal
from
	emp
order by
	sal desc
limit
	0, 5;
(limit
	5;)
```

案例：找出工资排名再第四到第九名的员工

```
select
	ename,sal
from
	emp
order by
	sal desc
limit
	3, 6;
```

**通用的标准分页SQL**

每页显示3条记录：

第一页：0，3

第二页：3，3

第三页：6，3

**每页显示pageSize条记录**

第pageNo页：(pageNo-1)*pageSize，pageSize

***

## 3.表的创建

### 3.1建表语句的语法格式：

```mysql
create table 表名(
    
	字段名1  数据类型,
    字段名2  数据类型,
    字段名3  数据类型,
	...
);
```

**关于MySQL当中字段的数据类型：**

| 类型    | 描述                                                         |
| :------ | ------------------------------------------------------------ |
| int     | 整数型（java中int）                                          |
| bigint  | 长整型（java中的long）                                       |
| float   | 浮点型（java中的float double）                               |
| char    | 定长字符串（String）长度固定使用                             |
| varchar | 可变长字符串（StringBuffer/StringBuilder）长度不确定使用     |
| date    | 日期类型（对应Java中的java.sql.Date类型）                    |
| BLOB    | 二进制大对象（存储图片、视频等流媒体信息）Binary Large OBject（Object） |
| CLOB    | 字符大对象(存储较大文本，比如可以存储4GB字符串)Character Large OBject（Object） |

**表名在数据库当中一般建议以：t_ 或者 tbl_ 开始。**

***

**创建学生表：**

​		学生信息包括：

​				学号    姓名    性别    班级编号    生日

​				学号：bigint

​				姓名：varchar

​				性别：char

​				班级编号：int

​				生日：date

```
create table t_student(
	no bigint,
	name varchar(255),
	sex char(1) default 1,
	classno varchar(255),
	birth char(10)
);
```



### 3.2insert插入语句语法格式

```mysql
1.
insert into 
	表名(字段名1,字段名2,字段名3) 
values
	(值1,值2,值3);
2.
insert into 
	表名 
values
	(值1,值2,值3,...);//按table表的列顺序填入
```

要求：字段与值数量相同，数据类型对应相同

```
insert into 
	t_student(no,name,sex,classno,birth) 
values
	(1,'zhangsan','1','高三一班','1950-10-12');
---------------------------------------------------
insert into
	t_student(name,sex,classno,birth,no) 
values
	('zhangsan','0','gaosanyiban','1950-12-02',1);
```

**一次插入多行：**

```mysql
insert into 
	t_student(no,name,sex,classno,birth) 
values
	(),(),(),........;
```

***

### 3.3drop删除表

```mysql
drop table if exists t_student;
```

****

### 3.4表的复制

语法：把查询结果当作表创建。

```mysql
create table 表名 as select语句;
```

***

```mysql
create table emp1 as select * from emp;
```

```mysql
create table emp2 as select empno,ename from emp;
```

把一张表插入到另一张表中

```mysql
insert into emp1 select * from emp;
```

### 3.5update修改数据

语法格式：

```mysql
update 表名 set 字段名1=值1,字段名2=值2...where 条件
```

注意：没有条件整张表全部更新

案例：把部门地址改成 'shanghai'，把部门名改成'人事部'

```
update dept1 set loc='shanghai',dname='renshibu' where deptno=10;
```

### 3.6delect删除数据

语法格式:

```mysql
delect from 表名 where 条件;
```

注意：没有条件全部删除

删除10部门数据

```mysql
delect from dept1 where deptno=10;
```

删除所有记录

```mysql
delect from dept1;
```

**大表删除***********不可挽回！！！**

```mysql
truncate table emp1(表名);
```

### 3.7约束（Constraint）

再创建表的时候，给表添加相对应约束

保证数据库的合法性，有效性，完整性

常见约束？

​	非空约束（not null）

​	唯一约束（unique）

​	主键约束（primary key）既不能为null，又不能重复（PK）

​	外键约束（foreign key）（FK）

​	检查约束（check）：注意Oracle有check约束，但是mysql没有，mysql不支持该约束

#### 3.7.1非空约束not null

只有列级约束，没有表级约束

```mysql
drop table if exists t_user;
create table t_user(
	id int,
    username varchar(255) not null,
    password varchar(255)
);
```

***

```
insert into t_user(id,password) values(1,'12345');

ERROR 1364 (HY000): Field 'username' doesn't have a default value
```

#### 3.7.2唯一性约束unique

唯一约束修饰的字段具有唯一性，不能重复。但是可以为null。

案例：给一列添加unique

```mysql
drop table if exists t_user;

create table t_user(
	id int,
	username varchar(255) unique
);

insert into t_user values(1,'zhangsan');
insert into t_user values(2,'zhangsan');

insert into t_user(id) values(2);
insert into t_user(id) values(3);
insert into t_user(id) values(4);

```

***

**案例：给两个列或者多个列加unique**

```mysql
drop table if exists t_user;
create table t_user(
	id int,
	usercode varchar(255),
	username varchar(255),
	unique(usercode,username) //【表级约束】
);
insert into t_user values(1,'123','zhangsan');
insert into t_user values(2,'12','zhangsan');
insert into t_user values(3,'123','wangsan');
select * from t_user;
```

```mysql
drop table if exists t_user;
create table t_user(
	id int,
	usercode varchar(255) unique, //【列级约束】
	username varchar(255) unique
);
```

前者唯一性和后者不相同，前者是**联合唯一**，后者**各自唯一**。

#### 3.7.3主键约束primary key

如何给一张表添加主键约束

案例：

```mysql
drop table if exists t_user;

create table t_user(
	id int primary key,
	username varchar(255),
	email varchar(255)
);

insert into t_user(id,username,email) values(1,'zhangsan','zs@163.com');
insert into t_user(id,username,email) values(2,'lisi','ls@163.com');
insert into t_user(id,username,email) values(3,'ww','ww@163.com');

select * from t_user;

insert into t_user(id,username,email) values(1,'jack','jack@163.com');
insert into t_user(username,email) values('jack','jack@163.com');
```

**主键的相关术语：**

​		主键约束：primary key

​		主键字段：主键约束的相关字段

​		主键值：	主键字段所存储的数据

**主键：**

​		表的设计三范式，第一范式要求任何一张表都应该有主键

**主键的作用：**

​		主键值是这行记录在这张表的唯一标识（就像身份证号）

**主键的分类：**

​		根据主键字段的字段数量来划分：

​					单一主键 ：推荐的 常用的

​					复合主键：（复合主键不建议使用，复合主键违背三范式）

​		根据主键的性质划分：

​					自然主键

​					业务主键：主键值与系统业务挂钩，例如：拿着银行卡卡号做主键，拿着身份证做主键（不建议用）

*********一张表的主键约束只能有一个**



##### mysql提供主键值自增：

```
drop table if exists t_user;

create table t_user(
	id int primary key auto_increment,
	username varchar(255)
);

insert into t_user(username) values('a');
insert into t_user(username) values('b');
insert into t_user(username) values('c');
insert into t_user(username) values('d');
insert into t_user(username) values('e');

select * from t_user;
```

***

### 3.7.外键约束

**关于外键约束的相关术语：**

​		外键约束：foreign key

​		外键字段：添加有外键约束的字段

​		外键值：外键字段中的每一个值

**背景：**

​		设计数据库表，维护学生和班级的信息

方案1：

​		no    	name 		classno		classname

----------------------------------------------------------------

​		1			zs				101				高二一班

​		2            zs				102				高二一班

​		.........

缺点:冗余

***

***

方案2：

两张表（班级表，学生表）

t_class  **父表**

cno(pk)    cname

***

101    高二一班

102	高二二班

t_student   **子表**

sno(pk)     sname		classno(该字段外键约束fk) 

***

1					zs1			101

2					zs2			102

3					zs3			103（会报错）

**classno只能来自t_class中的cno否则报错**

操作顺序：

​	删除数据时，先删除子表在删除父表。

​	添加数据时，先添加父表再添加子表。

​	创建表时，先创建父表再创建子表。

​	删除表时，先删除子表再删除父表。

***



```mysql
drop table if exists t_student;
drop table if exists t_class;

create table t_class(
	cno int primary key,
	cname varchar(255)
);

create table t_student(
	sno int primary key,
	sname varchar(255),
    classno int,
	foreign key(classno) references t_class(cno)
);

insert into t_class values(101,'xxxx');
insert into t_class values(102,'yyyy');

insert into t_student values(1,'zs1',101);
insert into t_student values(2,'zs2',102);
insert into t_student values(3,'zs3',102);
insert into t_student values(4,'zs4',101);


select * from t_class;
select * from t_student;
insert into t_student values(5,'zs5',103);
```

***

**外键可以为NULL**

***

**外键字段引用其他表的字段必须是主键吗？**

**不一定是主键**，**但是至少具有unique约束**

***

#### 存储引擎

完整的建表语句

```
CREATE TABLE `t_x` (
  `id` int DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
```

mysql默认使用的存储引擎是InnoDB方式

默认采用字符集UTF8

存储引擎名字在mysql存在（Oracle有对应机制）

mysql有很多存储引擎，每个都有优缺点。

***

查看当前mysql支持的存储引擎

```
show engines \G;
```

***

常见的存储引擎

**MyISAM：**

缺点：这种存储引擎不支持**事务**。

```mysql
      Engine: MyISAM
     Support: YES
     Comment: MyISAM storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
```

使用三个文件表示每个表：

​	1.格式文件—表示表结构的定义(mytable.frm)

​	2.数据文件—存储表的内容(mytable.MYD)

​	3.索引文件—存储表上的索引(mytable.MYI)

—灵活的AUTO_INCREMENT字段

—可呗转换为压缩，只读表来节省空间

***

**InoDB：**

```mysql
 Engine: InnoDB
     Support: DEFAULT
     Comment: Supports transactions, row-level locking, and foreign keys
Transactions: YES
          XA: YES
  Savepoints: YES
```

优点:支持事物，行级锁，外键等。这种存储引擎数据安全得到保障。 



表的结构存储在xxx.frm文件中

数据存储在tablespace表空间中，无法压缩，无法只读。

这种InnoDB存储引擎在MySQL数据库崩溃后提供自动恢复机制。

支持级联删除，级联更新。

***

***

## 4.事务

#### 4.1什么是事物

一个事物是一个完整的业务逻辑单元，不可再分。

两条DML语句必须同时成功，或者同时失败，不允许出现一条成功，一条失败。

要想保证两条DML语句同时成功或同时失败，需要使用事务。

#### 4.2和事物相关的语句

只有DML语句：（insert  delete  update）

**假设所有的业务都能使用一条DML语句完成，就不需要事物**

实际情况需要**多个DML才能完成**

#### 4.3事务的四大特性：

ACID：

​	1.A：原子性：事务是最小的工作单元，不可再分。

​	2.C：一致性：事务必须保证多条DML语句同时成功或者同时失败。

​	3.I： 隔离性：事务A与事务B之间具有隔离。

​	4.D：持久性：数据必须持久化到硬盘文件中，事务才算成功结束。

#### 4.4事务演示：

**准备表：**

```mysql
drop table if exists t_user;
create table t_user(
	id int primary key auto_increment,
	username varchar(255)
);
insert into t_user(username) values('zhangsan');
select * from t_user;
rollback;
select * from t_user;
```

mysql的事务是自动提交的，只要执行一条DML语句，则提交一次。

演示：使用start transaction：关闭自动提交机制

**开启事务**

start transaction;	//开启事务

rollback;	//回滚

commit;	//提交

```mysql
start transaction;

insert into t_user(username) values('lisi');
insert into t_user(username) values('wangwu');
select * from t_user;
rollback;
select * from t_user;

insert into t_user(username) values('lisi');
commit;
select * from t_user;

```

#### 4.5使用两个事务演示以上级别

```mysql
设置隔离级别：
	读未提交：
	set global transaction isolation level read uncommitted;
	读已提交：
	set global transaction isolation level read committed;
	可重复读:
	set global transaction isolation level repeatable read;
	序列化:类似等待
	set global transaction isolation level serializable;
查看隔离级别：
select @@global.transaction_isolation;
```

***

## 5.索引

> 什么是索引

相当于一本书的目录，通过目录可以快速找到。

​		第一种方式：全表扫描

​		第二种方式：根据索引检索（效率高，缩小扫描范围）

数据中的数据经常修改，不适合添加索引

> 什么时候添加索引

- 数据量庞大
- 字段很少的DML操作
- 该字段经常出现在where子句中

> 注意：primary key 和 unique约束字段会自动添加索引

- explain  能查询语句的执行计划mysql

> 创建索引

create index 索引名称 on 表名(字段名);

```mysql
create index emp_sal_index on emp(sal);
```

> 删除索引对象

drop index 索引名称 on 表名;

```mysql
drop index emp_sal_index on emp(sal);
```



> 索引底层实现原理 B+Tree

```mysql
select ename from emp where 物理地址 = 0x3;
```

***

> 模糊查询第一个通配符是%，会让索引失效

## 



## 6.视图（view）

> **创建视图**

```mysql
create view myview as select empno, ename from emp;
drop view myview;
```

**注意：只有DQL语句才能以视图对象方式创建出来**

***

> **对视图进行增删改.会影响原表数据**

```mysql
update myview set ename='hehe',sal=10 where empno=7369;
```

***

> 视图的作用

视图可以隐藏表的实现细节

***

## 7.DBA命令

> 将数据库当中数据导出

在dos命令窗口执行

```dos
mysqldump zy>G:/zy.sql -uroot -p123456
mysqldump zy temp>G:/zy.sql -uroot -p123456
```

> 导入数据

```mysql
create database zy;
use zy;
source [url]
```

## 8.数据库设计三范式

> 什么是设计范式

设计表的语句，按照三范式设计的表不会出现数据冗余

> 三范式有哪些

1. 第一范式：任何一张表都应该有主键，每一字段原子性不可再分

2. 第二范式：建立在第一范式之上，所有非主键字段必须完全依赖主键，不能产生部分依赖。

   >**多对多，用三张表，关系表两个外键**

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210920085716412.png" alt="image-20210920085716412" style="zoom:50%;" />

3. 第三范式，建立在第二范式基础上，所有非主键字段直接依赖主键字段，不能产生传递依赖

   > **一对多，两张表，多的表加外键**

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210920090139891.png" alt="image-20210920090139891" style="zoom:50%;" />

提醒：实际开发，以满足客户需求为主，有的时候会拿冗余换速度。



>  **一对一**

两种：

1. 主键共享
2. 外键唯一 foreign key + unique  （一对多+unique）

***

***

## 9.MySQL 34题

#### 01.取得每个部门最高薪水的人员名称

```mysql
select * from emp;

select 
	ename,max(sal),deptno 
from 
	emp 
group by 
	deptno;
```

(错误如果有多个人最高薪水一样会只显示一个)

思路：先找到每个部门的最高薪水然后在和原表进行比对

```mysql
select * from emp;

select 
	e.ename , t.*
from
	(select deptno,max(sal) maxsal from emp group by deptno) t
join
	emp e
on
	t.maxsal = e.sal and e.deptno = t.deptno;
```









