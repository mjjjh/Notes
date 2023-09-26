#   MySQL

### 启动 停止 连接

+ cmd里services.msc（找到MySQL80）

+ 命令行：

​			启动

```
net start mysql80
```

​			停止

```
net stop mysql80
```



+ 连接

  ​	mysql自带的命令行工具

  ​	mysql [-h ip地址] [-P 3306] -u（用户名如root） -p                   ([]默认即可，但需要配置环境变量)

  ![image-20221125170454711](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221125170454711.png)

  
  
  
### 数据模型

关系型数据库（RDBMS）

二维表，通过表结构存储数据的数据库





# SQL (Structured Query Language)

### 分类：

| 分类 | 全称                       |                                                        |
| ---- | -------------------------- | ------------------------------------------------------ |
| DDL  | Data Definition Language   | 数据定义语言，用来定义数据库对象（数据库，表，字段）   |
| DML  | Data Manipulation Language | 数据操作语言，用来对数据库表中的数据进行增删改         |
| DQL  | Data Query Language        | 数据查询语言，用来查询数库中表的记录                   |
| DCL  | Data Control Language      | 数据控制语言，用来创建数据库用户、控制数据库的访问权限 |

### DDL

#### 数据库

数据库操作：查询、创建、删除、使用

###### 查询

```sql
SHOW DATABASES; --查询所有
```

```sql
SELECT DATABASE(); --查询当前
```



###### 创建

```sql
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
```

utf8mb4字符

###### 删除

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```



###### 使用

```sql
USE 数据库名
```



#### 表-字段操作

###### 查询

```sql
SHOW TABLES; --查询当前数据库所有表
```



```sql
DESC 表名; --查询表结构
```



```sql
SHOW CREATE TABLE 表名; --查询指定表的建表语句
```



###### 创建

```sql
CREATE TABLE 表名(
	字段1 字段1类型[COMMENT 字段1注释],
	字段2 字段2类型[COMMENT 字段2注释],
	字段3 字段3类型[COMMENT 字段3注释],
	......
	字段n 字段n类型[COMMENT 字段n注释]
)[COMMENT 表注释]
```



int 

varchar()

score double(4,1)-------表示精度（位数）为4，标度（小数）为1



###### 修改

```SQL
ALTER TABLE 表名 ADD 字段名 类型（长度）[COMMENT 注释] [约束];    --添加字段
```



```sql
ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）; --修改数据类型
```



```SQL
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度）[COMMENT 注释] [约束]; --修改字段名和字段类型
```



```SQL
ALTER TABLE 表名 DROP 字段名; --删除字段
```



```SQL
ALTER TABLE 表名 RENAME TO 新表名; --修改表名
```



###### 删除表

```SQL
DROP TABLE [IF EXISTS] 表名;
```



```SQL
TRUNCATE TABLE 表名; --删除指定表，并重新创建该表（数据没了）
```





### DML

#### 值

#### 添加（INSERT）

```sql
INSERT INTO 表名 (字段名1，字段名2，...) VALUES(值1，值2，...);
```



```sql
INSERT INTO 表名 VALUES(值1，值2，...); --第一个字段值1，第二个字段值2，...
```



批量添加数据

```sql
INSERT INTO 表名 (字段名1，字段名2，...) VALUES (值1，值2，...),(值1，值2,...);
INSERT INTO 表名 VALUES(值1，值2，...),(值1，值2,...); 
```



>注意：
>
>- 插入数据时，指定的字段顺序需要与值的顺序一一对应。
>- 字符串和日期型数据应该包含在引号中。
>- 插入的数据大小，应该在字段的规定范围内。



#### 修改(update)

```sql
UPDATE 表名 SET 字段名1 = 值1 ，字段名2 = 值2，...[WHERE 条件];
```





#### 删除(delete)

##### 物理删除

```SQL
DELETE FROM 表名 [WHERE 条件];
```



> 注意：
>
> - DELETE语句的条件可以有，也可以没有，如果没有，则会删除整张表的所有数据。
> - DELETE语句不能删除某一个字段的值(可以使用UPDATE)。



##### 逻辑删除

加个bit类型字段，判断是否处于删除状态





### DQL

#### SELECT编写顺序

```sql
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```



#### 基本查询

```sql
# 查询多个字段
SELECT 字段1，字段2，字段3... FROM 表名;

SELECT * FROM 表名;


# 设置别名
SELECT 字段1 [AS 别名1]，字段2[AS 别名2]... FROM 表名;


#去除重复记录
SELECT DISTINCT 字段列表 FROM 表名;
```



#### 条件查询

where

<img src="C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221128194417061.png" alt="image-20221128194417061" style="zoom:100%;" />



#### 聚合函数

不和where使用

将**一列**数据作为一个整体，进行纵向计算。

![image-20221128195858462](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221128195858462.png)

```sql
SELECT 聚合函数(字段列表) FROM 表名；
```

> null值不参与聚合函数运算



#### 分组查询

```sql
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
```



##### group_concat（）

拼接组中的数据



where和having区别:

	+ 执行机制不同:where是分组之前进行过滤,不满足where条件,不参与分组;having是分组之后对结果进行过滤.
	+ 判断条件不同:where不能对聚合函数进行判断,而having可以.

> 注意:
>
> - 执行顺序:where>聚合函数>having
> - 分组之后查询的字段一般为聚合函数和分组字段,查询其他字段无任何意义





#### 排序查询

```sql
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式,字段2 排序方式2;
```

排序方式:

	+ ASC :升序(默认值)
	+ DESC:降序

> 注意:如果多字段排序,当第一个字段值相同时,才会根据第二个字段进行排序





#### 分页查询

```sql
SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;
```

**limit在最后**

> 注意:
>
> - 起始索引从0开始，起始索引 = (查询页码 - 1)  * 每页显示记录数
> - 分页查询是数据库的方言，不同数据库有不同的实现方式，MySQL中的是LIMIT
> - 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10

 



#### 执行顺序

1. from
2. where
3. group by
4. having
5. select
6. order by
7. limit



### DCL

控制数据库有哪些用户使用、用户能使用哪些数据库



#### 用户管理

```sql
#查询用户
USE mysql;
SELECT * FROM USER;


#创建用户（%为任意主机）
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';


#修改用户密码
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';


#删除用户
DROP USER '用户名'@'主机名';
```





#### 权限控制

![image-20221128212144656](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221128212144656.png)



```sql
#查询权限
SHOW GRANTS FOR '用户名'@'主机名';


#授予权限(*.* 所有)
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';


#撤销权限
REVOKE 权限列表 ON 数据库.表名 FROM '用户名'@'主机名';
```







# 函数

可以直接被另一端程序调用的程序或代码

select 函数;

### 字符串函数

| 函数                     | 功能                                                      |
| ------------------------ | --------------------------------------------------------- |
| CONCAT(S1,S2,S3,...,Sn)  | 字符串拼接，将S1,S2,...,Sn拼接成一个字符串                |
| LOWER(str)               | 将字符串str全部转为小写                                   |
| UPPER(str)               | 将字符串str全部转为大写                                   |
| LPAD(str,n,pad)          | 左填充，用字符串pad对str的左边进行填充，达到n给字符串长度 |
| RPAD(str,n,pad)          | 右填充，用字符串pad对str的右边进行填充，达到n给字符串长度 |
| TRIM(str)                | 去掉字符串头部和尾部的空格                                |
| SUBSTRING(str,start,len) | 返回从字符串str从start（1开始）位置起的len个长度的字符串  |





### 数值函数

| 函数       | 功能                               |
| ---------- | ---------------------------------- |
| CEIL(x)    | 向上取整                           |
| FLOOR(x)   | 向下取整                           |
| MOD(x,y)   | 返回x/y的模                        |
| RAND()     | 返回0~1内的随机数                  |
| ROUND(x,y) | 求参数x的四舍五入的值，保留y位小数 |





### 日期函数

| 函数                              | 功能                                              |
| --------------------------------- | ------------------------------------------------- |
| CURDATE()                         | 返回当前日期                                      |
| CURTIME()                         | 返回当前时间                                      |
| NOW()                             | 返回当前日期和时间                                |
| YEAR(date)                        | 获取指定date的年份                                |
| MONTH(date)                       | 获取指定date的月份                                |
| DAY(date)                         | 获取指定date的日期                                |
| DATE_ADD(date,INTERVAL expr type) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| DATEDIFF(date1,date2)             | 返回起始时间date1和结束时间date2之间的天数        |

date1 - date2

```sql
#往后70天
select date_add(now(),interval 70 day); 
```





### 流程函数

| 函数                                                       | 功能                                                      |
| ---------------------------------------------------------- | --------------------------------------------------------- |
| IF(value,t,f)                                              | 如果value位true，则返回t，否则返回f                       |
| IFNULL(value1,value2)                                      | 如果value1不为空，返回value1，否则返回value2              |
| CASE WHEN [val1] THEN [res1] ... ELSE [default] END        | 如果val1为true，返回res1，... 否则返回default默认值       |
| CASE [expr] WHEN [val1] THEN [res1] ... ELSE [default] END | 如果expr的值等于val1，返回res1，... 否则返回default默认值 |





# 约束

作用于表中字段上的规则，用于限制存储在表中的数据

目的:保证数据库中数据的正确、有效性和完整性

![image-20221129153722199](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129153722199.png)

可以在创建表/修改表的时候添加约束



![image-20221129154319142](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129154319142.png)



### 外键约束

```sql
#创建时直接设置
CREATE TABLE 表名(
	字段名 数据类型,
    ...
    [CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名) 
)



#添加约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
```



```sql
#删除外键
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称
```



##### 外键删除更新行为

![image-20221129161249361](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129161249361.png)



```sql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名) ON UPDATE CASCADE ON DELETE CASCADE;
```





# 多表查询

### 关系：

+ 一对多（多对一）
+ 多对多
+ 一对一

##### 一对多

一个部门有多个员工

实现：在多的一方建立外键，指向一的一方的主键



##### 多对多

学生与课程

实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

![image-20221129163029780](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129163029780.png)



##### 一对一

用户与用户详情（单表拆分以提升操作效率）

![image-20221129163511567](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129163511567.png)

实现：在任意一方加入外键，关联另外一方主键，并且设置外键为唯一的(UNIQUE)不能重复

![image-20221129163706711](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129163706711.png)





### 多表查询

```sql
#数据冗余 笛卡尔积
select * from emp,dept;

```

![image-20221129170311840](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129170311840.png)

通过where 主表.列 = 副表.列实现多表查询





![image-20221129172646117](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221129172646117.png)



### 内连接

隐式

```SQL
SELECT 字段列表 FROM 表1，表2 WHERE 条件...;
```



显式

```sql
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件;
```

后可加where，and，or等



### 外连接

```sql
# 左外连接
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;


#右外连接
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;
```







### 自连接

```sql
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件;
```

一张表看成两张表使用





### 联合查询

就是把多次查询的结果合并起来，形成一个新的查询结果集

```sql
SELECT 字段列表 FROM 表A
UNION[ALL]
SELECT 字段列表 FROM 表B;

-- all不加可以去重
-- 字段列表要一致
```





### 子查询

SQL语句中嵌套SELECT语句，又称嵌套查询

**子查询外部的语句可以是INSERT/UPDATE/DELETE/SELECT的任何一个**

```sql
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```

查询结果分

+ 标量子查询（子查询结果为单个值）
+ 列子查询（子查询结果为一列）
+ 行子查询（子查询结果为一行）
+ 表子查询（子查询结果为多行多列）

子查询位置分：where之后，from之后，select之后

 

##### 标量子查询

= <> < > 



##### 列子查询

IN	NOTIN	ANY	SOME	ALL



##### 行子查询

=	<>	IN	NOT IN

WHERE ( , ) = ( )



##### 表子查询

IN

where （ ， ）in （表）







# 事务

是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

**异常出现，不能程序卡在一半就不执行**

![image-20221130162310417](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221130162310417.png)

出现异样会回滚，没异常则提交 

![image-20221130163041609](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221130163041609.png)





需要手动设置事务

### 事务操作

##### 方式一

查看/设置事务提交方式

```sql
SELECT @@autocommit;
SET @@autocommit = 0;
```

1：自动提交

0：手动提交



提交事务

```sql
COMMIT;
```



回滚事务

```SQL
ROLLBACK;
```





##### 方式二（不修改提交方式）

事务开启

```sql
START TRANSACTION 或 BEGIN;
```



提交事务

```sql
COMMIT;
```



回滚事务

```SQL
ROLLBACK;
```





### 事务四大特性（ACID）

![image-20221130164612787](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221130164612787.png)





### 并发事务问题

![image-20221130185020173](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221130185020173.png)





### 事务的隔离级别

| 隔离级别                     | 脏读 | 不可重复度 | 幻读 |
| ---------------------------- | ---- | ---------- | ---- |
| Read uncommitted             | √    | √          | √    |
| Read committed               | ×    | √          | √    |
| Repeatable Read(MySQL  默认) | ×    | ×          | √    |
| Serializable                 | ×    | ×          | ×    |

隔离级别越高，数据越安全，但性能越差。



```sql
#查看事务隔离级别
SELECT @@TRANSACTION_ISOLATION;

#设置事务隔离级别
SET [SESSION|GLOBAL] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE}
```



> 幻读:事务一读取为空，因为不可重复读关系，之后读也为空，但是事务二创建了数据，导致事务一查不到但也创建不了。

Serializable，当事务一没有提交，会阻塞事务二，事务二等待。







# 注册表查看

cmd里输入regedit

![image-20221207193646071](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221207193646071.png)

两个services里的mysql







# 案例

### 1字段类型选择(设计一张员工信息表)

| 1    | 编号（纯数字）                                          | int              |
| ---- | ------------------------------------------------------- | ---------------- |
| 2    | 员工工号（字符串类型，长度不超过10位）                  | varchar(10)      |
| 3    | 员工姓名（字符串类型，长度不超过10位）                  | varchar(10)      |
| 4    | 性别（男/女,存储一个汉字）                              | char(1)          |
| 5    | 年龄（正常人年龄，不存储负值）                          | tinyint unsigned |
| 6    | 身份证号（二代身份证号均为18位，身份证中有X这样的字符） | char(18)         |
| 7    | 入职时间（区值年月日即可）                              | date             |

```sql
create table emp(
    id int comment '编号',
    workno varchar(10) comment '工号',
    name varchar(10) comment '姓名',
    gender char(1) comment '性别',
    age tinyint unsigned comment '年龄',
    idcard char(18) comment '身份证号',
    entrydata date comment '入职时间'
) comment '员工表';
```







### 多表查询

```sql
-- 1查询员工姓名，年龄，职位，部门信息（隐式内连接）
select name, age, if(gender, '男', '女') '性别', apart, salary
from employee,
     apart a
where employee.aprt_id = a.id;


-- 2查询年龄小于30岁的员工的姓名、年龄、职位、部门（显示内连接）
select name, age, apart
from employee e
         join apart a on e.aprt_id = a.id
where age < 30;


-- 3查询拥有员工的部门id、部门名称(交集部分内连接、去重)
select distinct a.id, a.apart
from apart a
         join employee e on e.aprt_id = a.id;



-- 4查询所有年龄大于40岁的员工，及其归属部门的名称；如果员工没有分配部门，也需要展示出来
select name, apart
from employee
         left join apart a on a.id = employee.aprt_id;



-- 5查询所有员工的工资等级
select e.name, s.grade
from employee e,
     salgrade s
where e.salary between s.losal and s.hisal;



-- 6查询“研发部”所有员工的信息及工资等级
select name, age, gender, entrydata, salary, apart, grade
from employee,
     apart,
     salgrade s
where employee.aprt_id = apart.id
  and apart.apart = '研发部'
  and salary between losal and hisal;



-- 7查询“研发部”员工的平均工资
select sum(e.salary), count(e.name),avg(e.salary) from employee e,apart a where e.aprt_id = a.id and a.apart = '研发部';



-- 8查询工资比“杨鸿涛”高的员工信息
select salary from employee where name='杨鸿涛';
select name, salary from employee where salary>(select salary from employee where name='杨鸿涛');



-- 9查询比平均薪资高的员工信息
select * from employee where salary>(select avg(salary) from employee);





-- 10查询低于本部门平均薪资的员工信息
-- a.查询指定部门平均薪资
    select avg(salary) from employee where aprt_id = 2;
-- b.查询低于本部门平均薪资的员工信息
    select *,(select avg(salary) from employee where aprt_id = e.aprt_id) from employee e where  e.salary<(select avg(salary) from employee where aprt_id = e.aprt_id);




-- 11查询所有的部门信息，并统计部门的员工人数
-- a.查询所有部门信息
    select * from apart;
-- b.统计员工数量
    select count(*) from employee where employee.aprt_id =1;
-- c.查询所有的部门信息，并统计部门的员工人数
select * ,(select count(*) from employee where employee.aprt_id = apart.id) '部门人数' from apart;

select apart.*,l
from apart,(select aprt_id, count(*) l from employee group by aprt_id) x where apart.id = x.aprt_id ;




-- 12 查询所有学生的选课情况，展示出学生名称，学号，课程名称
-- 多表查询
```
