## Mysql

### MySQL的安装和部署

---

#### 卸载

##### Windows

1. 先将原目录删除干净，然后在控制面板的程序中如果有服务的话删除
2. 删除C盘下的appData隐藏目录中的mysql
3. 注册表中也可能存在未删除的数据，查找并删干净
4. 如果安装的时候提示服务已存在，可以在管理员命令行下输入`sc query mysql`，如果存在数据，说明服务没删，通过`sc delete mysql`命令删除服务

##### Linux

待补充。。



#### 安装

##### Windows

1. 管理员模式下的命令行，进入到mysql的bin目录
2. 执行`mysqld --install`
3. 执行`mysqld --initialize --console`，记录下最后的随机密码
4. 启动服务：`net start mysql`
5. 打开mysql：`mysql -u root -p`，然后输入密码，密码为初始化时的随机密码
6. 修改密码：`alter user 'root'@'localhost' identified by 'guodatabase';`

##### Linux



### 数据库的创建与删除

---

创建命令

CREATE DATABASE 数据库名；  #create database

删除命令

DROP DATABASE 数据库名；     #drop database

表的删除命令

DROP TABLE table_name ;         #drop table

### 数据库的选择

---

选择命令

USE <database_name> ;  #use

### 数据类型

---

+ 在mysql中，定义数据字段的类型对数据库的优化非常重要

+ mysql支持多种数据类型，大致分三类：数值、日期/时间和字符串

#### 数值类型

TINYINT（tiny_int   ， 1byte  ）

SMALLINT（small_int   ,   2byte)

MEDIUMINT（medium_int  ,   3byte)

INT or INTEGER（int  or integer   ，4byte）

BIGINT（big_int   ， 8byte）

FLOAT（float  ，4byte）

DOUBLE（double  ，8字节）

DECIMAL（decimal  ， m/d）

#### 日期和时间类型

DATE      3byte     YYYY-MM-DD

TIME       3byte     HH:MM:SS

YEAR       1byte	YYYY

**DATETIME    8byte    YYYY-MM-DD  HH:MM:SS**

**TIMESTAMP   4byte   YYYYMMDD HHMMSS   timestamp 时间戳 从1970.1.1到现在的毫秒数**

#### 字符串类型

CHAR    0-255byte     定长字符串

**VARCHAR      0-65535byte     变长字符串**

BINARY   /    VARBINARY    包含字节（二进制）字符串而不是字符（非二进制）字符串

BLOB:TINYBLOB    \   BLOB   \	MEDIUMBLOB	\	LONGBLOB  二进制文本数据

**TEXT:TINYTEXT	\	TEXT	\	MEDIUMTEXT	\	LONGTEXT    文本数据**

### 数据表的创建

---

定义语法：

CREATE TABLE table_name (column_name column_type);

```bash
mysql> CREATE TABLE test_tb1(
    -> runoob_id INT NOT NULL AUTO_INCREMENT,
    -> runoob_title VARCHAR(60) NOT NULL,
    -> runoob_author VARCHAR(20) NOT NULL,
    -> submission_date DATE,
    -> PRIMARY KEY ( runoob_id )
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

如果不想字段为NULL，可以设置字段属性为NOT NULL；

AUTO_INCREMENT定义列为自增属性，一板用于主键，数值会自动加一；

PRIMARY KEY关键字用于定义列为主键，可以使用多列来定义主键；

### 插入数据

---

定义语法：

INSERT INTO table_name (field1, field2, ....field_n )   VALUES      (value1,value2,....value_n);

```bash
mysql> INSERT INTO test_tb1 (runoob_title, runoob_author, submission_date)
    -> VALUES
    -> ("Learn PHP", "runnob", NOW());
```

添加多条数据时每一条数据用逗号隔开

如果所有的列都要添加数据可以不用规定列进行添加数据

### 查询数据表

查询数据通用语法：

```sql
SELECT column_name,`column_name`
FROM table_name
[WHERE Clause]
[LIMIT N]
[OFFSET M]
```

查询语句可以使用一个或多个表，表之间用逗号隔开

SELECT命令可以读取一条或多条记录

可以使用*号来表示该表中的所有字段数据

可以使用WHERE语句包含任何条件

可以使用LIMIT属性来设定返回的记录数

OFFSET指定查询语句开始查询的数据偏移量，默认为0

反引号中的字段一般是区别于如果字段可能是关键字的时候

**另外，除了可以查询字外，还可以查询常量值、表达式和函数等，而这些查询的结果都是一个虚拟的表格**

### 起别名

便于理解

如果要查询的字段有重名情况，使用别名可以区分出来

方式一：使用AS

SELECT	字段	AS	别名；

例：SELECT	last_name	AS	姓，first_name	AS	名	FORM	表名；

方式二：使用空格

SELECT 字段 别名；

别名若存在特殊符号，则可以用双引号括起来

### 去重与连接

在字段前面加上关键字DISTINCT

SELECT	DISTINCT	字段	FROM	table_name;

在mysql中的+号仅仅只有一个功能：运算符

连接可以使用函数CONCAT（），可拼接多个

### 条件查询

语法：

WHERE	筛选条件

筛选条件分类

+ 按条件表达式筛选

条件运算符：>	<	=	（!=）	<>	>=	<=

+ 按逻辑表达式筛选

逻辑运算符：and	or	not(&&	||	!)

+ 模糊查询

like	、between and	、in	、is null

## 2. 操作数据库

操作数据库->操作数据库中的表 -> 操作数据库中表的数据

mysql关键字不区分大小写

### 2.1 操作数据

1.创建数据库

2.删除数据库

3.使用数据库

4.显示数据库

~~~mysql
CREATE DATABASE [IF NOT EXISTS] name;
DROP DATABASE [IF EXISTS] name;
USE `name`;  -- tab键的上面，如果你的表名或者字段名是一个特殊字符，使用``
SHOW DATABASES;  -- 显示所有的数据库
~~~

### 2.2 数据库的列类型

> 数值

> 字符串

> 时间日期

> null

null表示没有值，未知

+ **注意，不要使用NULL进行运算，结果为NULL**

### 2.3 数据库的字段属性（重点）

**Unsigned：**无符号

+ 无符号的整数
+ 声明了该列不能声明为负数

**zerofill：**零填充

+ 不足的位数，使用0来填充
+ 例如，int(3)，当赋值5时，5就变成了005

**自增：**

+ 通常理解为自增，即在上一条记录的基础上+1（默认）
+ 通常用来设置唯一的主键，必须是整数类型
+ 可以自定义的设置主键的自增起始值和步长

**not null：**非空

+ 假设设置为not null ，如果不给他赋值，则会报错
+ 如果设置为null，如果不填写，默认为null

**默认：**

+ 设置的默认值
+ 如果不指定该列的具体值，则为默认值

```mysql
/* 每一个表，都必须存在以下五个字段！未来做项目用的，表示一个项目存在的意义
id 主键
`version` 乐观锁
is_delete 伪删除
gmt_create 创建时间
gmt_update 修改时间
*/
```

### 2.4 创建数据库表

```mysql
CREATE TABLE [IF NOT EXISTS] `student` (
    -> `id` int(4) not null auto_increment comment '学号' ,
    -> `name` varchar(30) not null default '匿名' comment '姓名' ,
    -> `pwd` varchar(20) not null default '123456' comment '密码' ,
    -> `birthday` datetime default null comment '生日' ,
    -> `shader` varchar(2) not null default '女' comment '性别' ,
    -> primary key(`id`)
    -> )engine=innodb default charset=utf8;

show create database [name]  -- 显示创建数据库语句
show create table [name]  -- 显示创建表的语句
desc [table_name]  -- 显示表的结构
```

### 2.5 数据表的类型（engine）

INNODB -- 默认使用

MYISAM -- 早期使用

|            | MYISAM | INNODB    |
| ---------- | ------ | --------- |
| 事务支持   | 不支持 | 支持      |
| 数据行锁定 | 不支持 | 支持      |
| 外键约束   | 不支持 | 支持      |
| 全文索引   | 支持   | 现在支持  |
| 表空间大小 | 较小   | 较大，2倍 |

+ MYISAM  ：节约空间，速度较快
+ INNODB  ：安全性高，支持事务处理，多表多用户操作

> 设置数据库表的字符集编码

```sql
charset=utf8
```

不设置的话，会是mysql的默认字符集编码（不支持中文）

**现在默认的字符集编码是utf8**

### 2.6 修改删除表

> 修改表

```mysql
-- 修改表名：RENAME
ALTER TABLE [table_name] RENAME AS [new_table_name];
-- 增加表的字段：ADD
ALTER TABLE [table_name] ADD [field_name] int()...;
-- 修改表的字段
-- 修改字段类型和约束，不能重命名字段：MODIFY
ALTER TABLE [table_name] MODIFY [field_name] [修改的内容];
-- 能重命名字段，能修改字段类型,不能修改约束：CHANGE
ALTER TABLE [table_name] CHANGE [field_name] [new_field_name];
-- 删除表的字段：DROP
ALTER TABLE [table_name] DROP [field_name];
```

> 删除表

```mysql
DROP TABLE [IF EXISTS] [table_name];
```

**所有的修改和删除操作尽量加上判断，避免报错！**

## 3. MySQL数据管理

### 3.1 外键（了解）

> 方式一：在创建表的时候就将外键添加上

这种方式比较麻烦，有可视化工具的时候可以直接添加

> 方式二：创建表的时候无外键关系，可以通过修改表来将某一字段修改为外键

```mysql
alter table [表名] 
  ->add constraint [FK_约束名] 
  ->foreign key(作为外键的列) 
  ->references 哪个表（哪个列）
  ->;
-- 例如：
alter table `student`
  ->add constraint `FK_gradeId`
  ->foreign key(`gradeId`)
  ->references `grade`(`gradeId`)
  ->;
```

**注意：**

+ **以上的操作都是物理外键，是一种数据库级别的外键，不建议使用！（避免数据库过多造成困扰，了解即可）**

**最佳实践：**

+ **数据库就是单纯的表，只用来存放数据，只有行（数据）和列（字段）**
+ **想使用多张表的数据，即使用外键的话，使用程序去实现（逻辑外键）**

### 3.2 DML语言（重点）

**数据库的意义：数据存储，数据管理**

DML语言：数据操作语言

+ Insert
+ Update
+ Delete



### 3.3 添加（INSERT）

插入语句（Insert)

```mysql
-- 语法：
-- insert into 表名([字段1，字段2，字段3...]) values('值1','..','..'),('值2'),('值3'),...;
-- 插入一个值
insert into `grade`(`gradeName`) values ('大四');
-- 由于主键自增我们可以省略，（如果不写表的字段，他就会一一匹配
-- 插入多个值
insert into `grade`(`gradeName`)
    -> values
    -> ('大三'),
    -> ('大二');
-- 插入一个字段
insert into `student`(`name`) value ('张三');
-- 插入多个字段
insert into `student`(`name`, `pwd`, `gradeId`)
    -> values
    -> ('李四', '123', 2),
    -> ('王五', '456', 2);
```

注意：

+ 字段是可以省略的，但是后面的值必须和字段意义对应，不能少
+ 可以同时插入多个字段多个值，注意使用逗号隔开即可

### 3.4 修改（UPDATE）

> update  修改谁  （条件)   set 原来的值  =  新值

```mysql
-- 修改学生姓名，带了条件（一定要注意带条件，不加条件，则会导致所有学生的姓名都改变）
update `student` set `name` = '牛二' where `id` = 1;
-- 修改多个属性时，使用逗号隔开
update `student` set `name` = '张三', `shader` = '男' where `id` = 2;
-- 语法
-- update [表名] set 
-- ->[column_name] = values, [column_name] = values, ... 
-- ->where [条件];
```

条件：where 子句  运算符

操作符会返回布尔值

| 操作符           | 含义     | 范围 | 结果 |
| ---------------- | -------- | ---- | ---- |
| =                | 等于     |      |      |
| <>  或   !=      | 不等于   |      |      |
| >                |          |      |      |
| <                |          |      |      |
| >=               |          |      |      |
| <=               |          |      |      |
| BETWEEN...AND... | 两者之间 |      |      |
| AND              | 与 &&    |      |      |
| OR               | 或 \|\|  |      |      |

```mysql
update `student` set `name` = '李四' where `id` between 2 and 4;
update `student` set `name` = '张三' where `id` between 2 and 3 and `shader` = '男';
```

### 3.5 删除（DELETE）

> delete 命令

```mysql
-- 语法：
-- delete from [表名] where [条件];
-- 注意：条件一定要写，不写会造成严重后果
-- 例如：
delete from `student` where `id` = 4;
```

> truncate 命令

作用：完全清空一个数据库表，表的结构和索引约束没有变！

```mysql
truncate `grade`;
```

> delete 和 truncate 的区别

**相同点：都可以删除数据，都不会删除表的结构。**

**不同点：**

+ **truncate 会重新设置自增列，计数器归零**
+ **truncate 不会影响事务**

## 4. DQL 数据查询语言（最重点）

### 4.1 定义及语法

（Date Query Language : 数据查询语言）

+ 所有的查询都要用到它：Select
+ 简单的查询，复杂的查询都要用到它
+ **数据库最核心的语言**
+ 使用频率最多的语句

> SELECT 语法

```mysql
SELECT [SLL  |  DISTINCT]
{*  |  table.*  |  table.field1[as alias1][,table.field2[as alias2]][...]]}
FROM table_name [as table_alias]
  [left | right | inner join table_name [as table_alias]]
  [WHERE ...]
  [GROUP BY ...]
  [HAVING]
  [ORDER BY....]
  [LIMIT {[offset, ]row_count | row_countOFFSET offset}]
```



### 4.2 指定查询字段

```mysql
-- 查询语句： select 字段 from 表名;
select * from student;
-- 查询指定字段
 select `studentno`, `loginpwd` from `student`;
 -- 起别名，给结果起一个名字：AS ，也可以给字段和表都起别名
 mysql> select `studentno` as 学号, `loginpwd` as 密码 from `student` as s;
 -- 函数，例如Concat(a,b),拼接字符串
  select concat('姓名:', `studentname`) as 新名字 from `student`;
```

> 去重：distinct

```mysql
-- 当所要查询的数据中存在相同的数据时，使用distinct去掉重复的部分
select `studentno` from `result`;
select distinct `studentno` from `result`;
```

> 数据库的列  (表达式)

```mysql
select version();
-- 查询系统版本（函数）
select 152*2+8 as 计算结果;
-- 用来计算（表达式）
select @@auto_increment_increment;
-- 查询自增步长（变量）
select `studentno`, `studentresult`+1 from `result`;
-- 学生成绩都加一分查询
```

### 4.3 where条件子句

**作用：检索数据中符合条件的值**

> 逻辑运算符

+ and          &&
+ or             ||
+ not            !

尽量使用英文字母！

```mysql
select StudentNo, `studentresult` from `result`
  -> where `studentresult`>=50 and `studentresult`<=70;
--
select `studentno`, `studentresult` from `result`
    -> where `studentno`<>1001 or `studentresult`>70;
```

> 模糊查询：比较运算符（返回布尔值）

+ IS NULL
+ IS NOT NULL
+ BETWEEN
+ **LIKE**    (a like b , 表示如果a可以与b匹配，则结果为真)
+ **IN**     （a   in(a1,a2,a3,......), 表示a如果是a1，a2，....中的具体的某一个或多个值，则返回真）
+ NOT IN（）

 ```mysql
-- like结合通配符使用，例如：%（代表0到任意个字符），   _（代表一个字符)
-- 查询姓刘的同学
where `studentname` like '刘%';
-- 查询姓刘的同学，且姓后面只有一个字
where `studentname` like '刘_';

-- 查询1001，1002，1003号学员的信息
where `studentno` in (1001, 1002, 1003);
 ```

### 4.4 联表查询

> JOIN   关键字

+ LEFT JOIN     左
+ INNER JOIN    联合
+ RIGHT JOIN    由

```mysql
/* 思路
1.分析需求，分析查询的字段来自哪些表，（连接查询）
2.确定使用哪种连接查询，（七种）
3.确定交叉点，（这两个表中哪个数据是相同的）
4.判断的条件
*/
-- 查询参加了考试的同学的（学号，姓名，科目编号，分数）

-- join (连接的表) on(判断的条件)  连接查询
-- where    等值查询

-- inner join
 select s.studentno, studentname, subjectno, studentresult
    -> from student as s
    -> inner join result as r
    -> on s.studentno = r.studentno;
-- right join
select s.studentno, studentname, subjectno, studentresult
    -> from student as s
    -> right join result as r
    -> on s.studentno = r.studentno;
-- left join
select s.studentno, studentname, subjectno, studentresult
    ->  from student as s
    -> left join result as r
    -> on s.studentno = r.studentno;
```

**区别：**

**inner join :如果表中至少有一个匹配，则返回行**

**right join:会从右表中返回所有的值，即使在左表中未匹配**

**left join:会从左表中返回所有的值，即使在右表中未匹配**

```mysql
-- 思考题：
-- 查询参加了考试的同学的信息（学号，学生姓名，科目名，分数）
select st.studentno, studentname, subjectname, studentresult
  -> from result as r
  -> left join student as st       -- 只能left join
  -> on r.studentno = st.studentno  
  -> left join subject as sub       -- inner join 或 right join 也都可以
  -> on r.subjectno = sub.subjectno;
  
  -- 查询学员所属的年级（学号，姓名，年级名称）
  select `studentname` as 学生姓名, `studentno` as 学生学号, g.`gradename` as 学生所在年级
    -> from `student` as s
    -> left join `grade` as g
    -> on s.`gradeid` = g.`gradeid`;
```

> 自连接

自己的表和自己的表连接，**核心：一张表拆成两张一样的表**

```mysql
-- 查询父子信息：把一张表看成两张一摸一样的表
 select a.`categoryname` as '父栏目', b.`categoryname` as '子栏目'
    -> from `category` as a, `category` as b   -- 注意这里的逗号
    -> where a.`categoryid` = b.`pid`;
```

### 4.5  分页和排序

+ ORDER BY （order by ：指定查询记录按一个或多个条件排序）
+ LIMIT {[offset, ]row_count | row_countOFFSET offset}  :指定查询的记录从那条到哪条

```mysql
-- 分页和排序
-- 排序：升序：ASC， 降序：DESC
ORDER BY 通过哪个字段排序，怎么排

select `studentname` as 姓名, s.`studentno` as 学号, `studentresult` as 成绩
    -> from `student` as s
    -> inner join `result` as r
    -> on s.`studentno` = r.`studentno`
    -> order by `studentresult` DESC;  -- 若前面没有条件，不要加where
    
-- 分页：为什么用分页：缓解数据库的压力，给人更好的浏览体验
-- 语法：limit 起始页，页面的大小
-- 算法：
-- 第1页   limit 0，5    （1-1）*5
-- 第2页   limit 5，5    （2-1）*5
-- 第3页   limit 10，5    （3-1）*5
-- 。。。。。
-- 第n页   limit x，5    x=(n-1) * pageSize
-- [pageSize：想要分页的页面大小]
-- [(n-1)*pageSize:起始值]
-- [n：当前页]
-- [[数据总数/页面大小]（向上取整） = 总页数]

-- 查询 JAVA地学年 课程成绩排名前十的学生，并且分数要大于80的学生信息（姓名，学号，课程名称，成绩）
select `studentname` as 姓名, s.`studentno` as 学号, `subjectname` as 课程名称, `studentresult` as 成绩
    -> from `result` as r
    -> inner join `student` as s
    -> on s.`studentno`=r.`studentno`
    -> inner join `subject` as sub
    -> on r.`subjectno`=sub.`subjectno`
    -> where 
    -> `subjectname` in ('Java程序设计-1', 'Java程序设计-2', 'Java程序设计-3', 'Java程序设计-4') and `studentresult`>80   -- 注意in的用法
    -> order by `studentresult` DESC
    -> limit 0, 10;
```



### 4.6 子查询

where （这个值是计算出来的）

**本质：在where语句中嵌套一个子查询语句**

where （select * from)

适合查询条件满足的，不能用于结果查询

> 嵌套查询

```mysql
-- 查询 高能数学-1 前5名的同学的成绩的信息（学号，姓名，分数）
-- 使用子查询
select `studentno`, `studentname` from `student`
where `studentno` in (
  select `studentno` from `result`
  where `subjectno` = (
     select `subjectno` from `subject`
     where `subjectname` = '高等数学-1'
  )order by `studentresult` DESC
  limit 0, 5
);
-- 这种方法不行，子循环不能使用limit等
select `studentno`, `studentname` from `student`
    -> where `studentno` in (
    -> select `studentno` from `result`
    -> where `studentresult`>80 and `subjectno` = (
    -> select `subjectno` from `subject`
    -> where `subjectname` = '高等数学-1'
    -> )
    -> );
```

### 4.7 分组和过滤

> GROUP BY
>
> HAVING

```mysql
-- group by  分组
-- having    过滤（分组的条件）

select `subjectname` as 科目名, avg(`studentresult`) as 平均值, max(`studentresult`) as 最大值, min(`studentresult`) as 最小值
    -> from `result` as r inner join `subject` as s on r.`subjectno`=s.`subjectno`
    -> group by r.`subjectno` -- 通过什么来分组
    -> having 平均值 > 80;
```



## 5. MySQL的函数

### 5.1 常用函数（不常用）

```mysql
-- 数学运算
select ABS（-8)   --  绝对值
select ceiling（9.4）  -- 向上取整
select floor（9.4）  -- 向下取整
select rand（）  -- 返回一个0-1之间的随机数
select sign（）  -- 判断一个数的符号 0-0  整数返回1，负数返回-1

-- 字符串函数
select char_length('...')  -- 返回字符串的长度
select concat('...', '...', ..);   -- 拼接字符串
select insert(str, state, len, str)  --从某个位置替换成某个字符串
select Lower('')   -- 小写转换
select Upper('')   -- 大写转换
select instr('..','')  -- 某个二子字符串第一次出现的位置
select replace('str', 'old', 'new'); -- 将莫字符串中的某一子串替换
select substr('str', first, len)  -- 返回从指定位置开始的指定长度的子字符串
select reverse('...'); -- 逆序输出字符串；

-- 时间和日期函数（重要）
select current_date()  -- 获取当前日期
select curdate()  -- 获取当前时间
select now()  -- 获取当前时间
select localtime()  -- 本地时间
select sysdate()  -- 系统时间

-- 系统
select system_user()
select user()
select version()
```



### 5.2 聚合函数(真的常用)

| 函数    | 作用   |
| ------- | ------ |
| COUNT() | 计数   |
| SUM()   | 求和   |
| AVG()   | 平均值 |
| MAX()   | 最大值 |
| MIN()   | 最小值 |
| ....    | ....   |
|         |        |

```mysql
select count(`studentresult`) as 个数 from `result`;
select sum(`studentresult`) as 成绩总数 from `result`;
select avg(`studentresult`) as 平均值 from `result`;
select ceiling(avg(`studentresult`)) as 平均值向上取整 from `result`;
select max(`studentresult`) as 最大值 from `result`;
select min(`studentresult`) as 最小值 from `result`;
```

### 5.3 数据库级别的MD5加密（扩展）

什么是MD5？

主要增强算法复杂度和不可逆性。

MD5不可逆，具体的值的 MD5 是一样的

MD5 破解的原理是：背后有一个字典，MD5加密后的值和加密前的值都在字典中

```mysql
-- 测试MD5 加密
create table `testmd5` (
    `id` int(4) not null,
    `name` varchar(20) not null,
    `pwd` varchar(50) not null,
    primary key(`id`)
)engine=innodb default charset=utf8;
insert into testmd5 values
(1,'zhangsan','123456'),
(2,"lisi", '1234567'),
(3,'wangwu','12345678');

-- 加密
update testmd5 set pwd = md5(pwd) where id = 1;
update testmd5 set pwd = md5(pwd);

-- 插入时加密
insert into testmd5 value
(4,'xiaoming', MD5('1234567788'));

-- 如何校验：将用户传递进来的密码，进行md5加密，然后对比加密后的值
select * from testmd5 where `name` = 'xiaoming' and pwd = MD5('1234567788');
```

## 6. 事务

### 6.1 什么是事务

要么都成功，要么都失败

---------------

1. SQL 执行  A 给 B 转账  A 1000  -----》200    B 200
2. SOL 执行  B收到A的钱  A  800  ------》B 400

--------------

将一组SQL放在一个批次中去执行

> 事务原则：ACID 原则  ：原子性， 一致性， 隔离性， 持久性      （脏读，幻读。。。。）

参考博客链接：https://blog.csdn.net/dengjili/article/details/82468576

+ **原子性（Atomicity）**

事务是一个不可分割的工作单位，事务的操作要么都成功，要么都失败

+ **一致性（Consistency）**

事务前后的数据完整性必须保持一致

+ **隔离性（Isolation）**

事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务要互相隔离

+ **持久性（Durability）**

事务一旦被提交，则数据永久性的改变，即使数据库发生故障也不应该对其有任何影响

> 隔离所导致的一些问题

+ **脏读**

指一个事务读取了另外一个事务未提交的数据

+ **不可重复读**

在一个事务内读取表中的某一行数据，多次读取的结果不同（这不一定是错误，只是某些场合不对）

+ **幻读（虚读**）

是指一个事务内读取到了别的事务插入的数据，导致前后读取不一致（一般是行影响，多了一行）



> 事务

```mysql
-- 事务
-- mysql 是默认开启事务自动提交的
set autocommit = 0; -- 关闭
set autocommit = 1; -- 开启（默认的）

-- 手动处理事务
set autocommit = 0 -- 手动开启一个事务的时候，先关闭自动提交

-- 事务开启
start transaction -- 标记一个事务的开始
insert xx
insert xx
.....
-- 提交：持久化（成功！）
commit
-- 回滚：回到原来的样子（失败！）
rollback

-- 事务结束
set autocommit = 1  --开启自动提交

-- 了解
savepoint 保存点名 -- 设置一个事务的保存点
rollback to savepoint 保存点名 -- 回滚到保存点
release savepoint 保存点名 -- 撤销保存点

```

> 模拟事务

```mysql
-- 模拟一个银行转账
create database if not exists`shop` character set utf8 collate utf8_general_ci;
use shop;
create table if not exists `account` (
    `id` int(4) not null auto_increment,
    `name` varchar(30) not null,
    `money` decimal(9,2) not null,  -- decimal(n,m):小数点前n位，小数点后m位
    primary key(`id`)
)engine = innodb default charset=utf8;

insert into `account` (`name`, `money`)
    values ('A', 2000.00), ('B', 10000.00);

-- 模拟转账：事务
set autocommit = 0;
start transaction --
update `account` set `money` = `money`-500 where `name` = 'A';
update `account` set `money` = `money`+500 where `name` = 'B';
commit;  --提交事务，就被永久持续化了
rollback; --回滚，若未提交事务，则事务重回未改变之前，提交后，则在回滚也无效
set autocommit = 1;
```

## 7. 索引

>   MySQL官方对索引的定义为：索引（Index）是帮助MySQL高效获取数据的数据结构，提取句子的主干，就可以得到索引的本质：索引是数据结构

参考链接：https://www.cnblogs.com/tgycoder/p/5410057.html

### 7.1 索引分类

> 在一个表中，主键索引只能有一个，唯一索引可以有多个

+ 主键索引  （primary key)

唯一的表示，主键不可重复，只能有一个列作为主键

+ 唯一索引   （unique key)

避免重复的列出现，唯一索引可以重复，多个列都可以标识为 唯一索引

+ 常规索引   （key/index)

默认的，index，key关键字来设置

+ 全文索引    （fulltext)

在特定的数据库引擎下才有，MyISAM，用于快速定位数据



基础语法：

```mysql
-- 索引的使用
-- 1. 在创建表的时候给字段添加索引
-- 2. 创建完毕后，增加索引

-- 显示所有的索引信息
show index from table_name;

-- 增加一个全文索引
alter table `table_name` add fulltext index `索引名`(`字段名`);

-- EXPLAIN 分析sql执行的状况
explain select * from `table_name`;  -- 非全文索引
explain select * from `table_name` where catch(`filed_name`) against('str');
```

### 7.2 测试索引

```mysql
-- 插入100万数据.
DELIMITER $$
-- 写函数之前必须要写，标志
CREATE FUNCTION mock_data ()
RETURNS INT
BEGIN
DECLARE num INT DEFAULT 1000000;
DECLARE i INT DEFAULT 0;
WHILE i<num DO
INSERT INTO `app_user`(`name`,`eamil`,`phone`,`gender`)VALUES(CONCAT('用户',i),'19224305@qq.com','123456789',FLOOR(RAND()*2));
SET i=i+1;
END WHILE;
RETURN i;
END;

SELECT mock_data() -- 执行此函数 生成一百万条数据
```

具体就不实践了

索引通过将一个表的数据建立一个B树（索引创建的时候会比较慢），当创建索引后，查询数据会比不创建索引的查询效率高非常多，这是一种通过空间换取时间的大数据量检索方式。

索引在小数据量的时候，用户不大，大师在大数据的时候，效果会非常明显

### 7.3 索引原则

+ 索引不是越多越好
+ 不要对进程变动数据加索引
+ 小数据量的表不需要加索引
+ 索引一般加在常用来查询的字段上！



> 索引的数据类型

Hash类型的索引

Btree：InnoDB的默认数据结构

## 8. 数据库的备份

### 8.1 用户管理

> SQL 命令

用户表：mysql.user

本质：对这张表进行CRUD



```mysql
-- 创建用户
create user `user_name` identified by 'psw';

-- 修改密码 (修改当前用户密码)
set password = passsword('pwd');
-- 修改密码 （修改指定用户密码）
set password for `user_name` = PASSWORD('pwd');  -- 不行

-- 重命名
rename user 'old_user_name' to 'new_user_name';

-- 用户授权：GRANT， ALL PRIVILEGES 全部的权限
-- 除了给别人授权的权限
grant all privileges on *.* to `user_name`;

-- 查询权限:GRANTS
show grants for `user_name` -- 查看指定用户的权限
show grants for root@localhost  -- 查看主机权限

-- 撤销权限：REVOKE
revoke all privileges on *.* from `user_name`;

-- 删除用户：DROP
drop user `user_name`;
```



### 8.2 MySQL 备份

为什么要备份？

+ 保证重要的数据不丢失
+ 数据转移

MySQL 数据库备份的方式

+ 直接拷贝物理文件
+ 在可视化工具中手动到处
  + 在想要导出的表或数据库上右键导出或转储
+ 使用命令行导出：mysqldump  命令行使用

```bash
# mysqldump -h 主机名 -u 用户名 -p 密码 数据库 表名 > 物理磁盘位置/文件名
mysqldump -h localhost -u root -p 123456 school student > D:/a.sql

# mysqldump -h 主机名 -u 用户名 -p 密码 数据库 表1 表2 表3 > 物理磁盘位置/文件名
mysqldump -h localhost -u root -p 123456 school student result > D:/a.sql

# mysqldump -h 主机名 -u 用户名 -p 密码 数据库 > 物理磁盘位置/文件名
mysqldump -h localhost -u root -p 123456 school > D:/a.sql

# 导入
# 登录的情况下，切换到指定的数据库
# source 备份文件
source d:/a.sql

mysql -u用户名 -p密码 库名 < 备份文件
```

假设你要备份数据库，防止数据丢失

把数据库给别人，把sql文件给别人即可！



## 9. 权限管理

**当数据库比较复杂的时候，我们就需要设计了**

**糟糕的数据库设计：**

+ 数据冗余，浪费空间
+ 数据库插入和删除都会麻烦、异常（屏蔽使用物理外键）
+ 程序的性能差

**良好的数据库设计：**

+ 节省内存空间
+ 保证数据库的完整性
+ 方便我们的开发系统

**软件开发中，关于数据库的设计**

+ 分析需求：分析业务和需要处理的数据库的需求
+ 设计关系图（E-R图）



**设计数据库的步骤：（以个人博客为例**）

+ 手机信息，分析需求
  + 用户表（用户登陆注销，用户的个人需求，写博客，创建分类）
  + 分类表（文章分类，谁创建的）
  + 文章表（文章的信息）
  + 评论表（写的评论）
  + 友链表（友链信息）
  + 自定义表（）
+ 表示实体（把需求落实到每一个字段）
+ 表示实体之间的关系
  + 用户：user --> blog
  + 分类：user --> category
  + 关注：user --> user
  + 友链：links
  + 评论：user -> user -> blog

BBS 论坛系统

CRM 管理系统

### 9.2 三大范式

**为什么需要数据库规范化？**

+ 信息重复
+ 更新异常
+ 插入异常
+ 删除异常

> 三大范式

**第一范式（1NF）**

+ 原子性：保证每一列不可再分

**第二范式（2NF）**

+ 前提满足第一范式
+ 保证每张表只描述一件事情

**第三范式（3NF）**

+ 满足第二范式

+ 每一列都和主键直接相关

目的：规范数据库的设计



**规范性和性能的问题**

（关联查询的表不得超过三张表）

+ 考虑商业化的需求和目标，（成本，用户体验！）数据库的性能更加重要
+ 在考虑性能的问题的时候，需要适当的考虑一下规范性！
+ 故意给某些表增加一些冗余的字段（从多表查询变为单表查询）
+ 故意增加一些计算列（从大数据量降低为小数据量的查询）



## 10. JDBC（重点）

### 10.1 数据库驱动

驱动：声卡，显卡，数据库

我们的程序会通过 数据库 驱动，和数据库打交道



### 10.2 JDBC

SUN 公司为了简化开发人员的（对数据库的统一）操作，提供了一个（Java操作数据库的） 的规范，这就是JDBC，这些规范的实现是由具体的厂商去做的

对于开发人员来说，我们只需要掌握JDBC接口的操作即可。

![image-20200622151645607](../../Photo/Typora/image-20200622151645607.png)

两个包：

+ java.sql
+ javax.sql

还需要导入一个数据库驱动包：mysql-connector-java-5.1.47.jar

### 10.3 第一个JDBC 程序

> 创建测试数据库

创建一个普通项目

导入一个数据库驱动

```java
package com.alateer;

import java.sql.*;

public class JdbcFirstDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.用户信息和url
        //useUnicode=ture&characterEncoding=utf8&useSSL=true
        String url = "jdbc:mysql://localhost:3306/jdbcstudy?        useUnicode=true&characterEncoding=utf8&useSSL=false";
        String username = "root";
        String password = "guodatabase";
        //3.连接成功，数据库对象 connection 代表一个数据库
        Connection connection = DriverManager.getConnection(url, username, password);

        //4.执行Sql的对象 statement 代表执行sql的对象
        Statement statement = connection.createStatement();
        //5.执行SQL的对象去执行SQL ，可能存在结果，查看返回结果
        String sql = "select * from users";
        ResultSet resultSet = statement.executeQuery(sql); //返回的结果集 结果集中封装了我们查询出来的全部结果
        while(resultSet.next()) {
            System.out.println("id = " + resultSet.getObject("id"));
            System.out.println("name = " + resultSet.getObject("NAME"));
            System.out.println("pwd = " + resultSet.getObject("PASSWORD"));
            System.out.println("email = " + resultSet.getObject("email"));
            System.out.println("birthday = " + resultSet.getObject("birthday"));
        }
        //6.释放连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

步骤总结：

1.加载驱动

2.连接数据库 DriverManager

3.获取执行sql的对象 Statement

4.获得返回的结果集

5.释放连接



> DriverManager

```java
//以前：DriverManager.registerDriver(new com.mysql.jdbc.Driver());
Class.forName("com.mysql.jdbc.Driver");
Connection connection = DriverManager.getConnection(url, username, password);

// connection 代表数据库
//数据库设置自动提交
// 事务提交
// 事务回滚
//connection.commit();
//connection.rollback();
//connection.setAutoCommit();                                                  
```

> URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcstudyuseUnicode=true
     &characterEncoding=utf8&useSSL=false";
    
// mysql -- 3306
// 协议:mysql://主机地址：端口号/数据库名？参数1&参数2&参数3
```

> Statement 执行SQL的对象  prepareStatement 执行SQL的对象

```java
String sql = "select * from users";  //写SQL语句

//statement.executeQuery();  //查询操作，返回一个结果集
//statement.execute();       //可以执行任何SQL
//statement.executeUpdate();  // 可以执行更新，插入，删除，返回一个受影响的行数
```

> ResultSet 结果集对象：封装了所有的查询结果

可以获得指定的数据类型

遍历，指针（next）

> 释放资源

### 10.4 statement对象

  JDBC中的statement对象用于向数据库发送SQL语句，像完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可

statement对象有两个主要的方法：

statement.executeQuery()      查询操作，返回一个结果集

statement.executeUpdate()    可以执行更新，插入，删除，返回一个受影响的行数

> CRUD操作-create

> CRUD操作-delete

> CRUD操作-update

> CRUD操作-read

> 代码实现

> SQL 注入

```java
package com.alateer;

import com.alateer.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

//测试sql注入
public class TestInjection {
    public static void main(String[] args) {
        sign(" 'or' 1=1", "123456");
    }
    public static void sign(String username, String password){
        Connection conn = null;
        Statement st = null;
        ResultSet re = null;
        try {
            conn = JdbcUtils.getConnect();
            st = conn.createStatement();

            String sql = "select * from users where NAME = '" + username + "' and PASSWORD = '" + password + "'";
            re = st.executeQuery(sql);
            while(re.next()){
                System.out.println(re.getString("NAME"));
                System.out.println(re.getString("PASSWORD"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn, st, re);
        }
    }
}
```

sql 存在漏洞，会被攻击导致数据泄露，**SQL会被拼接 or**



### 10.5 PreParedStatement对象
> CRUD操作-create

> CRUD操作-delete

> CRUD操作-update

> CRUD操作-read

可以防止sql注入

```java
package com.alateer;

import com.alateer.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

//防止sql注入
public class TestNotJection {
    public static void noSign(String username, String pwd) {
        Connection conn = null;
        PreparedStatement ps =null;
        ResultSet re = null;
        try {
            conn = JdbcUtils.getConnect();

            //sql预编译
            String sql = "select * from users where NAME = ? and PASSWORD = ?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, username);
            ps.setString(2, pwd);
            re = ps.executeQuery();
            while (re.next()) {
                System.out.println(re.getString("NAME"));
                System.out.println(re.getString("PASSWORD"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn, ps, re);
        }
    }

    public static void main(String[] args) {
        noSign(" 'or' 1=1", "123456");
    }
}
```

### 10.6 使用IDEA连接数据库





### 10.7 事务





### 10.8 数据库连接池

数据库连接---执行完毕 ---- 释放

连接-- 释放 十分浪费系统资源

池化技术：准备一些预先的资源，过来就连接预先准备好的

---- 开门--业务员：等待 --- 服务 ---

常用连接数：10个

最小连接数：15个

等待超时：100ms



编写一个连接池，实现一个接口：DataSource



> 开源数据源实现

DBCP

C3P0



使用了这些数据库连接池后，我们就不用再连接了

# 总结

## MySQL

#### 什么是MySQL

​	MySQL 是一种关系型数据库，在Java企业级开发中非常常用，因为 MySQL 是开源免费的，并且方便扩展。MySQL默认的端口号是3306

#### 使用的存储引擎

​	MySQL使用的两个常见的存储引擎：InnoDB 和 MyISAM

​	MyISAM 是 MySQL 在5.5版本之前支持的默认数据库引擎，虽然性能极佳，且提供了大量的特性，包括全文索引、压缩、空间函数等，但是 MyISAM 不支持事务和行级锁，而且最大的缺陷是奔溃后无法完全恢复。5.5版本之后，MySQ 引入了 InnoDB（事务性数据库引擎），且作为默认支持的数据库引擎。

#### MyISAM 和 InnoDB 的对比

+ 是否支持行级锁：MyISAM 只有表级锁，而 InnoDB 支持行级锁和表级锁，默认为行级锁
+ 是否支持事务和奔溃后的安全恢复：MyISAM 强调的是性能，每次查询都具有原子性，其执行速度比 InnoDB 类型更快，但是不提供事务支持，而 InnoDB 提供事务支持，是具有事务，回滚和奔溃修复功能的事务安全型表
+ 是否支持外键：MyISAM 不支持，InnoDB 支持
+ 是否支持MVCC：仅 InnoDB 支持。应对高并发事务，MVCC比单纯的加锁更高效

#### MVCC

+ MySQL 的大多数事务型存储引擎实现的其实都不是简单的行级锁。基于提高并发性能的考虑，他们一般都同时实现了多版本并发控制（MVCC），各个数据库实现的机制不尽相同，没有统一标准。
+ 可以认为MVCC是行级锁的一个变种，但是它在很多情况下避免了加锁操作，故而开销更低，大都实现了非阻塞的读操作，写操作也只锁定必要的行
+ 实现方式有多种，典型的有乐观并发控制和悲观并发控制
+ MVCC 只在 READ COMMITTED 和 REPEATABLE READ 两个隔离级别下工作

#### 索引

+ MySQL 索引使用的数据结构主要有 BTree 索引 和 哈希索引。
+ 对于哈希索引来说，底层的数据结构就是哈希表，因此在绝大多数需求为单条记录查询的时候，可以选择哈希索引，查询性能更快，其余大部分场景，都建议使用BTree索引；对于BTree索引，MySQL使用的是B树中的B+Tree，对两种主要的存储引擎的实现方式是不同的
+ MyISAM：B+Tree叶节点的data域存放的是数据记录的地址。在索引检索的时候，首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其 data 域的值，然后以 data 域的值为地址读取相应的数据记录，这被称为**“非聚簇索引”**
+ InnoDB：数据文件本身就是索引文件。树的叶节点 data 域存放是完整的数据记录，这个索引的Key 是数据表的主键，因此 InnoDB表数据文件本身就是主索引。这个被称之为**“聚簇索引”**
+ **在根据主索引搜索时，直接找到 Key所在的节点即可取出数据；在根据辅助索引查找时，则需要先取出主键的值，再走一遍主索引。因此，设计表时，不建议使用过长的字段作为主键，也不建议使用非单调的字段作为主键，这样会导致主索引频繁分裂**

#### 事务及其四大特性

+ 事务：是逻辑上的一组操作，要么这组操作都执行成功，要么这组操作都执行失败
+ 四大特性（ACID）：原子性、一致性、隔离性、持久性

#### 并发事务带来的问题

+ 脏读：一个事务修改某个数据后，在未提交该事务之前，另一个事务访问并使用了该修改了但未提交的数据，就产生了脏读，读到的这个数据是“脏数据”
+ 丢失修改：指两个事务前后分别都读取并修改了某个数据，这样就会导致前一个事务内的修改结果被丢失了，称作丢失修改（未提交，说明两个事务读取到的数据一样）
+ 不可重复读：指在一个事务内多次读取同一个数据，发现多次读取到的结果不同
+ 幻读：和不可重复读类似，只是幻读发生在几行数据中，即当多次读取几行数据时，发现存在原本不存在的记录，就是幻读

不可重复读和幻读的区别：不可重复读的重点时修改，幻读的重点在于新增或者删除

#### 事务隔离级别

+ READ-UNCOMMITTED（读取未提交）：最低的隔离级别，允许读取尚未提交的数据变更
+ READ-COMMITTED（读取已提交）：允许读取并发事务已提交的数据，可以防止脏读
+ REPEATABLE-READ（可重复读）：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以防止可重复读和脏读
+ SERIALIZABLE（可串行化）：最高的隔离级别。完全服从ACID的隔离级别，即所有的事务依次逐个执行，这样事务之间完全不可能产生干扰，可防止幻读

MySQL默认支持的隔离级别是 REPEATABLE READ（可重复读）

InnoDB 存储引擎在 REPEATABLE READ 事务的隔离级别下使用的是 Next-Key Lock 锁算法，因此可以避免幻读的产生，即InnoDB在默认支持的隔离级别下以及可以完全保证事务的隔离性要求，即达到了 SERIALIZABLE 的隔离级别

#### 锁机制和InnoDB锁算法

表级锁和行级锁的对比：

+ 表级锁：MySQL中锁定 粒度最大 的一种锁，对当前整张表加锁，实现简单，资源消耗少，加锁快，不会出现死锁，但触发锁冲突的概率最高
+ 行级锁：MySQL 中锁定 粒度最小 的一种锁，只针对当前操作的行进行加锁，行级锁能大大减少数据库操作的冲突，并发度高，但加锁开销大，加锁慢，会出现死锁

InnoDB的三个锁算法：

+ Record lock：单个行记录上的锁
+ Gap lock：间隙锁，锁定一个范围，不包括记录本身
+ Next-key lock：recork + gap ，锁定一个范围，包含记录本身

InnoDB对于行的查询使用next-key lock

Next-key lock 是为了解决幻读的问题

当查询的索引含有唯一属性时，将next-key lock 降级为 record lock

#### 大表优化

+ 查询时限制数据范围
+ 读/写分离（主库写，从库读）
+ 缓存
+ 垂直分区（针对列）
+ 水平分区（针对行）

#### 数据库开发的一些最佳实践总结

+ 页面搜索严禁使用左模糊或者全模糊，需要的话走搜索引擎解决
+ 不得使用外键与级联，一切外键概念必须在应用层解决
+ 所有表必须使用 InnoDB 存储引擎
+ 数据库和表的字符集统一使用 UTF8（utf8mb4）
+ 所有表和字段都需要添加注释（comment从句）
+ 控制表单数据量的大小，建议控制在 500 万以内
+ 尽量做到冷热数据分离，减少表的宽度
+ 禁止在表中建立预留字段
+ 禁止在数据库表中存储图片、文件等大的二进制文件
+ 尽可能地把所有列定义为 NOT NULL
+ 同财务相关的金额类数据必须使用 decimal 类型（精准浮点数）
+ 每个 InnoDB 表必须有一个主键 （建议使用自增ID值）
+ 对于频繁的查询优先考虑使用覆盖索引
+ 建议使用预编译语句进行数据库操作（防止SQL注入）
+  避免使用子查询，可以把子查询优化为 JOIN 操作
+ 避免 JOIN 关联太多地表

#### 一千行MySQL命令

[https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/%E4%B8%80%E5%8D%83%E8%A1%8CMySQL%E5%91%BD%E4%BB%A4.md](https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/一千行MySQL命令.md)

#### 为什么使用索引

+ 加快数据的检索速度
+ 帮助服务器避免排序和临时表
+ 将随机IO变为顺序IO
+ 加快了表与表之间的连接

#### 为什么不对表中的每一个列创建一个索引

+ 对表中的数据进行增删改时，索引也要动态维护，这样就降低了数据的维护速度
+ 索引要占物理空间
+ 创建和维护索引要耗费时间，且随数据量的增大而增大

#### 使用索引的注意事项

1. 在经常需要搜索的列上或使用在WHERE子句中的列上创建索引，可加快速度
2. 在经常需要排序的列上创建索引
3. 对于中到大型表索引非常有效，但特大型表维护开销大，不建议使用索引
4. 在经常用在连接的列上，主要是一些外键，可以加快连接速度
5. 避免在 WHERE 子句中对字段施加函数，否则会造成无法命中索引
6. 使用逻辑主键，不要使用业务主键
7. 删除长期未使用的索引

#### 覆盖索引

如果一个索引包含（或说覆盖）所有需要查询的字段的值，我们就称之为“覆盖索引”。覆盖所有就是要把查询出的列和索引对应，不做回表操作。

#### 覆盖索引使用实例

创建索引（username，age），当我们执行以下 sql 语句：

```mysql
select username , age from user where username = 'Java' and age = 22
```

在查询数据时：要查询的列在叶子节点都存在，所以，不用回表

#### 选择索引以及利用这些索引查询的3个原则

+ 单行访问是很慢的
+ 按顺序访问范围数据是很快的
+ 索引覆盖查询是很快的

#### 最左前缀原则

MySQL 中的所有可以以一定的顺序引用多列，这种索引叫做联合索引，如User表中的name和city加联合索引就是（name，city），而最左前缀原则指出：如果查询的时候查询条件**精确匹配**索引的左边连续一列或几列，那么此列就会别用到（覆盖索引）

#### 索引类型

+ 主键索引（Primary Key）：主键列使用的就是主键索引（主键只能有一个，且不为空，不能重复）
+ 二级索引（辅助索引）：二级索引的叶子节点存储的数据是主键，即通过二级索引，可以定位主键

唯一索引，普通索引，前缀索引等都属于二级索引

+ 聚簇索引：索引结构和数据一起存放的索引，主键索引属于聚簇索引
+ 非聚簇索引：索引结构和数据分开存放的索引，二级索引属于非聚簇索引

非聚簇索引不一定需要回表查询（覆盖索引）

#### 一条sql语句在mysql中是如何执行的

[https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/%E4%B8%80%E6%9D%A1sql%E8%AF%AD%E5%8F%A5%E5%9C%A8mysql%E4%B8%AD%E5%A6%82%E4%BD%95%E6%89%A7%E8%A1%8C%E7%9A%84.md](https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/一条sql语句在mysql中如何执行的.md)

#### 数据库存储时间

1. 不要使用字符串存储时间，因为字符串占用空间大，且日期比较效率比较低，无法用日期相关API

2. Datetime 和 Timestamp 之间的抉择

一般情况下首选Timestamp，因为DateTime没有时区信息，当时去改变时，读取到的时间就是错误的，而Timestamp是和时区有关的，其值会随着服务时区的变化而变化，自动换算相应的事件

```mysql
# 扩展，一些关于 MySQL 时区设置的一个常用 sql 命令
# 查看当前会话时区
SELECT @@session.time_zone;
# 设置当前会话时区
SET time_zone = 'Europe/Helsinki';
SET time_zone = "+00:00";
# 数据库全局时区查看
SELECT @@global.time_zone;
# 设置全局时区
SET GLOBAL time_zone = '+8:00';
SET GLOBAL time_zone = 'Europe/Helsinki';
```

另外，DateTime类型耗费空间更大，需要消耗8个字节存储空间，而Timestamp使用4个字节的存储空间，但也造成Timestamp表示的事件范围更小

3. 数值型时间戳

使用 int 或者 bigint 类型的数值也就是时间戳来表示事件，这种存储方式具有 Timestamp 类型所具有的一些优点，并且使用它进行日期排序和对比操作效率会更高，跨系统也很方便，缺点是数据的可读性太差了，无法直观看到具体时间

4. 总结

Timestamp和数值型时间戳都很好，可根据实际场景具体应用
