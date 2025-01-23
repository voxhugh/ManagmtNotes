# SQL

x64 是指CPU是64位版本的。

x86 是指CPU是32位版本的。 

数据在内存：

优点：读写速度快

缺点：程序结束后数据丢失

 

保存到文件

优点：数据可以永久保存

缺点：

1、频繁的IO操作，效率不高

2、数据的管理非常不方便，需要把所有的数据整体都读取出来才能操作

数据库：

1、数据永久保存

2、数据管理非常方便

------

数据库是以表为组织单位存储数据的。

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_01.png)

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_02.png)

------

PRIMARY KEY 主键，不能重复，唯一确定一条记录

AUTO_INCREMENT 自动增长

varchar(10) char(10)区别：

相同点：都可以最大放10个字符

不同点：char(10)不管输入的是多少都会占10个字符，例如输入名字“张三”只有两个字符，

但是使用char(10)在数据库里面还是占10个字符的空间。

使用varchar(10)最大支持是10个字符，但是实际长度就是输入字符长度，例如输入名字“张三”只有两个字符，

那么在varchar(10)里面就只占两个字符。

Duplicate entry '1' for key 'PRIMARY'

```sql
-- 列出所有的数据库

SHOW DATABASES;

 

-- 创建数据库

CREATE DATABASE java1812 DEFAULT CHARACTER SET utf8;

 

-- 删除数据库

DROP DATABASE java1812;

 

-- ----------------------------------

-- 数据库表的操作

-- 切换数据库

USE java1812;

-- 创建表

CREATE TABLE student(

  id INT,

  NAME CHAR(10),

  age INT,

  gender CHAR(1)

);

 

-- 查看所有表

SHOW TABLES;

-- 查看表的结构

DESC student; -- description

-- 删除表

DROP TABLE student;

 

-- 更改表的结构

-- 添加字段

ALTER TABLE student ADD COLUMN address CHAR(10);

-- 删除字段

ALTER TABLE student DROP COLUMN address;

-- 修改表的字段

ALTER TABLE student CHANGE address addr CHAR(20);

-- 修改表的名字

ALTER TABLE student RENAME TO stu;

 

-- 创建表

CREATE TABLE student(

  id INT PRIMARY KEY AUTO_INCREMENT,

  NAME VARCHAR(10),

  age INT,

  gender VARCHAR(1),

   php INT

);

-- * 代表查询所有的列

SELECT * FROM student;

 

-- 插入数据

-- Duplicate entry '1' for key 'PRIMARY'

INSERT INTO student(id,NAME,age,gender) VALUES(1,'wangwu',23,'男');

INSERT INTO student(id,NAME,age,gender) VALUES(3,'赵六',23,'男');

INSERT INTO student VALUES(4,'赵六22',33,'男');

-- 插入部分字段值(必须把前面的字段名都写上)

INSERT INTO student(NAME,age,gender) VALUES('小张11',23,'男');

-- 一次插入多条数据

INSERT INTO student(NAME,age,gender) VALUES('小张77',23,'男'),('小王',22,'男');

 

-- 修改数据

UPDATE student SET age=age+1;

UPDATE student SET age=age+1 WHERE id=7;

 

-- 删除数据

DELETE FROM student; -- 删除表中所有数据（很少使用，是非常危险）

DELETE FROM student WHERE age=24; -- 所有age是24的数据都被删除了，可能有多条数据都是age=24

DELETE FROM student WHERE id=12; -- 因为id是主键是唯一的，所以根据id删除只能删除唯一的一条数据

-- TRUNCATE删除表里面所有数据，自增的id会重新初始化为初始值1

TRUNCATE TABLE student;

 

-- 查询数据

-- 显示所有列(字段)数据

SELECT * FROM student; -- 学习时候可以写*，但是在企业开发中需要什么字段就写什么字段

SELECT id,name,age,gender FROM student;

-- 查询指定列

SELECT NAME,age FROM student;

-- 查询时候添加常量列，通过as可以起别名

SELECT id,NAME,age AS '年龄','java1812' AS '班级' FROM student;

-- 查询时候和并列，字段名可以当成java里面的变量来运算

SELECT id,NAME,(php+java) AS '总成绩' FROM student;

-- 查询时候去掉重复的记录

SELECT DISTINCT address FROM student;

 

-- 条件查询 where

SELECT * FROM student WHERE NAME='小王';

 

-- 逻辑条件: and（同时成立） or(只要有一个成立)

SELECT * FROM student WHERE NAME='小王' AND address='青岛';

SELECT * FROM student WHERE NAME='小王' OR address='北京';

 

-- 比较运算: > < >= <= !=

SELECT * FROM student WHERE java>=70 AND java<=80;

-- between and (等价于>= and <=)

SELECT * FROM student WHERE java BETWEEN 70 AND 80;

-- 查询地址不是青岛的学生信息

SELECT * FROM student WHERE address != '青岛';

 

-- 聚合查询

-- 聚合查询函数：sum(),avg(),max(),min(),count()

-- 统计学生php的总成绩（sum求和）

SELECT SUM(php) AS 'php总成绩' FROM student;

-- 统计学生php的平均值

SELECT AVG(php) AS 'php平均值' FROM student;

-- 统计学生php的最大值

SELECT MAX(php) AS 'php最大值' FROM student;

-- 统计学生表里面一共有多少学生

SELECT COUNT(*) AS '总人数' FROM student;

SELECT COUNT(id) AS '总人数' FROM student;

SELECT COUNT(address) AS '总人数' FROM student;

-- 注意：count()函数统计的是指定列不包含NULL的数据个数

 

 

-- 查询排序

-- 语法：order by 字段 asc/desc 默认是asc升序，可以不写

SELECT * FROM student ORDER BY php;

SELECT * FROM student ORDER BY php ASC;

SELECT * FROM student ORDER BY php DESC;

-- 多个条件排序

-- 需求：先按照php降序，java升序(整体是按照php降序，如果php相同的数据再按照java标准排序)

SELECT * FROM student ORDER BY php DESC, java ASC;

 

 

-- 分组查询(group by)

-- 需求：查询男女分别有多少人
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_03.png)

```sql
SELECT gender,COUNT(id) FROM student GROUP BY gender;
-- select后面的查询都是基于group by之后的
SELECT address,COUNT(id) FROM student GROUP BY address;
  
-- 分组查询后筛选
-- 需求：address大于1
-- group by之后的条件查询使用having
SELECT address AS '地址',COUNT(id) AS '人数' FROM student GROUP BY address HAVING COUNT(id)>1;
  
 
SELECT * FROM student;
```

------

**字段属性设置:**

1. not null： 不为空，表示该字段不能放“null”这个值。不写，则默认是可以为空

2. auto_increment: 设定int类型字段的值可以“自增长”，即其值无需“写入”，而会自动获得并增加
   此属性必须随同 primary key 或 unique key 一起使用。primary key = unique key + not null

3. [primary] key： 设定为主键。是唯一键“加强”：不能重复并且不能使用null，并且可以作为确定任意一行数据的“关键值”，最常见的类似：where id= 8; 或 where user_name = ‘zhangsan’;
   通常，每个表都应该有个主键，而且大多数表，喜欢使用一个id并自增长类型作为主键。
   但：一个表只能设定一个主键。

4. unique [key] : 设定为唯一键：表示该字段的所有行的值不可以重复（唯一性）。
   Duplicate entry 'zhangsan' for key 'name'

5. default ‘默认值’： 设定一个字段在没有插入数据的时候自动使用的值。

6. comment ‘字段注释’

```sql
CREATE TABLE teacher(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10) NOT NULL,
    age INT COMMENT '年龄'
    address VARCHAR(10) DEFAULT '中国', -- 插入数据时候如果不赋值，默认值是"中国"
    UNIQUE KEY(NAME) -- 唯一键，代表这个字段不能重复
);
 
-- Duplicate entry 'zhangsan' for key 'name'
INSERT INTO teacher(NAME) VALUES('zhangsan');

```



------

**多表查询：**

学生表、班级表、课程表、班级课程表

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_04.png)

- inner join  left join

| 学生名字 | 学生性别 | 班级名   | 课程名 |
| -------- | -------- | -------- | ------ |
| 张三     | 男       | Java1807 | Java   |
| 张三     | 男       | Java1807 | H5     |
| 李四     | 男       | Java1812 | Java   |
| 李四     | 男       | Java1812 | UI     |
| 李四     | 男       | Java1812 | H5     |

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_05.png)

```sql
-- 多对多
-- 班级表
CREATE TABLE banji(
    id INT PRIMARY KEY AUTO_INCREMENT,
    `name` VARCHAR(10) NOT NULL
);
INSERT INTO banji(`name`) VALUES('java1807'),('java1812');
 
SELECT * FROM banji;
 
-- 学生表
CREATE TABLE student(
    id INT PRIMARY KEY AUTO_INCREMENT,
    `name` VARCHAR(10) NOT NULL,
    age INT,
    gender CHAR(1),
    banji_id INT,
    FOREIGN KEY(banji_id) REFERENCES banji(id)
);
INSERT INTO student(`name`,age,gender,banji_id) 
VALUES('张三',20,'男',1),('李四',21,'男',2),('王五',20,'女',1);
-- Cannot add or update a child row: a foreign key constraint fails (`java1812`.`student`, CONSTRAINT `student_ibfk_1` FOREIGN KEY (`banji_id`) REFERENCES `banji` (`id`))
INSERT INTO student(`name`,age,gender,banji_id) 
VALUES('张三',20,'男',3);
 
SELECT * FROM student;
 
-- 课程表
CREATE TABLE course(
    id INT PRIMARY KEY AUTO_INCREMENT,
    `name` VARCHAR(10) NOT NULL,
    credit INT COMMENT '学分'
);
INSERT INTO course(`name`,credit) VALUES('Java',5),('UI',4),('H5',4);
 
SELECT * FROM course;
 
-- 班级课程表
CREATE TABLE banji_course(
    -- id int PRIMARY KEY AUTO_INCREMENT,
    banji_id INT,
    course_id INT,
    PRIMARY KEY(banji_id,course_id), -- 联合主键
    FOREIGN KEY(banji_id) REFERENCES banji(id), -- banji_id既是联合主键又是外键
    FOREIGN KEY(course_id) REFERENCES course(id) -- course_id既是联合主键又是外键
);
INSERT INTO banji_course(banji_id,course_id) VALUES(1,1),(1,3),(2,1),(2,2),(2,3);
 
SELECT * FROM banji_course;
```



------

```sql
-- 子查询：嵌套查询，一个查询语句是另一个查询语句的条件
-- 查询班级是java1812班所有学生信息
SELECT * FROM student WHERE banji_id=2;
SELECT id FROM banji WHERE `name`='java1812';
SELECT * FROM student WHERE banji_id=(SELECT id FROM banji WHERE `name`='java1812');
 
-- 班级是java1807班或者java1812班所有学生信息
SELECT * FROM student WHERE banji_id=1 OR banji_id=2;
SELECT * FROM student WHERE banji_id IN(1,2);
SELECT id FROM banji WHERE `name`='java1807' OR `name`='java1812'; -- 1,2
SELECT * FROM student WHERE banji_id IN(SELECT id FROM banji WHERE `name`='java1807' OR `name`='java1812');
 
-- "="：要求子查询只有一个结果。 "in"：子查询可以有多个结果
```

------

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_06.png)

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_07.png)

```sql
-- 列出所有学生学习的课程名称

-- 学生姓名  班级名称  课程名称  学分
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_08.png)

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_09.png)

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_10.png)

------

```sql
-- inner join on 只有左右两个表有关联的才查询出来
-- left join on 左表中都显示出来，右表没有显示空
-- right join on 右表都显示，左表没有显示空
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_11.png)

```sql
SELECT * 
 FROM student as s INNER JOIN banji as b
 on s.banji_id=b.id;
SELECT * 
 FROM student as s LEFT JOIN banji as b
 on s.banji_id=b.id;
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_12.png)

```sql
SELECT * 
FROM student as s RIGHT JOIN banji as b
on s.banji_id=b.id;
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_13.png)

```sql
-- 没有分配课程也显示出来。
-- 班级名称   课程名称   学分
SELECT b.`name` AS '班级名称',c.`name` as '课程名称',c.credit as '学分'
FROM banji AS b LEFT JOIN banji_course AS bc
ON b.id=bc.banji_id
LEFT JOIN course as c
ON bc.course_id=c.id;
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_14.png)

总结：多表查询主要是账务下面两点

1. 整个查询涉及到几张表，涉及到几张表就连接这几张表。

1. 如果涉及到这几张表的关系搞不清楚，画一下ER图，弄清楚表和表之间的关系（就是根据外键建立的关系）

```sql
-- 统计每个班有多少学生
-- 学生数量
SELECT COUNT(id) as '学生数量' 
FROM student GROUP BY banji_id;
-- 班级名称    数量
SELECT * 
FROM student as s 
INNER JOIN banji as b
ON s.banji_id=b.id;
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_15.png)

**把inner join之后查询的结果当成一张表来使用**， 在这个结果集里面根据班级id统计每个班级下面学生数量。

------

**select 查询模型**：

数据库中以表为组织单位存储数据。

表类似我们的Java类，每个字段对应类里面的属性。

那么用我们熟悉的java程序来与关系型数据对比，就会发现以下对应关系。

**类--------------------表**

**表中属性-------------表中字段（列）**

**对象------------------记录（行）**

**字段（列）是变量（类中属性时变量）**

**变量是可以计算（操作）**

where是表达式，值为真或者假（true或者false）

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_16.png)

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_17.png)

```sql
SELECT b.`name` AS '班级名称',COUNT(s.id) as '学生数量' 
FROM student as s 
INNER JOIN banji as b
ON s.banji_id=b.id
GROUP BY s.banji_id;
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_18.png)

```sql
-- 在上面基础上筛选出班级人数>1班级的名称和人数
SELECT b.`name` AS '班级名称',COUNT(s.id) as '学生数量' 
FROM student as s 
INNER JOIN banji as b
ON s.banji_id=b.id
GROUP BY s.banji_id
HAVING COUNT(s.id)>1;
```

![](https://github.com/voxhugh/Appendix/blob/main/SQL_IMGs/20250105_19.png)

------

**模糊查找：like**

语法形式：字段 like '要查找字符'

说明：

1. like模糊查找用于对字符类型的字段进行字符匹配查找。

2. 要查找的字符中，有两个特殊含义的字符：% , _:

   1. %含义是：代表0或多个的任意字符

   2. _含义是：代表1个任意字符

   3. 这里的字符都是指现实中可见的一个“符号”，而不是字节。

3. 语法：like '%关键字%'

```sql
SELECT * FROM student WHERE NAME LIKE '张%'; -- 以张开头
SELECT * FROM student WHERE NAME LIKE '张_'; -- 以张开头，而且名字是两个字
SELECT * FROM student WHERE NAME LIKE '%张%'; -- 名字里面只要有张就可以
```

如果要查找的字符里中包含"%","_"，

如果要查找的字符中包含“%”或“_”，“ **’**”，则只要对他们进行转义就可以：

like ‘%ab\%cd%’ //这里要找的是： 包含 ab%cd 字符的字符

like ‘\_ab%’ //这里要找的是： _ab开头的字符

like ‘%ab\'cd%’ //这里要找的是： 包含 ab'cd 字符的字符