# 1 数据库引入

## 1.1 数据库引入

由于早前文件系统管理较为繁琐，后来发明了数据库

## 1.2 CRUD

增删改查对应CRUD

## 1.3 层次模型

1. 有且只有一个节点没有双亲结点，这个几点称为根结点
2. 根节点以外的其他结点有且只有只有一个双亲节点的记录以及他们之间的联系的集合称为集合的层次模型 逻辑结构可以用一颗倒立的树表示

提高查询效率、提高数据完整性

## 1.4 网状模型

1. 方便的丝实现现实生活中许多复杂的关系
2. 修改网状模型没有像层次模型那样复杂，删除某个节点父节点与子节点互不干扰
3. 实现关系底层依靠指针实现，执行操作的效率较高
4. 网状模型结构复杂、不利于数据的维护和重建
5. 数据模型之间的管理较大，使用时不仅要说明数据做什么，还要说明操作记录的路径

## 1.5 关系型（V2.0）

- 通过公共组件建立联系
- 分别管理、互不影响、层次明显

# 2 安装连接及配置

## 2.1 简单连接配置

小皮面板https://www.xp.cn/

需要配置环境变量

### 2.1.1 创建data文件

- bin：本身的一些命令
- data：数据存储
- docs：文档
- include：头文件（mysql本身使用C++写的）
- lib：依赖
- share：储存字符集编码

命令创建data文件夹

```mysql
mysqld --initialize-insecure --user=root
```

显示基本数据库

```mysql
show databases;
```



## 2.2 数据库的基本操作

### 2.2.1 数据库的显示 show databses;

显示基本数据库

```mysql
show databases;
```

### 2.2.2 创建数据库 create database [数据库名]

创建一个学生库

```mysql
create database student;
```

强制创建一个database

```mysql
create database `database`; //使用反引号
```

检查是否存在再创建

```mysql
create database if not exists student;
```

检查是否存在再创建(牛逼)

```mysql
create database if not exists `student`;
```

指定字符编码创建数据库

```mysql
create database if not exists `student` charset=gbk; 实际开发自付编码必须为utf8
```

### 2.2.3 删除数据库 drop database [数据库名]

删除数据库

```mysql
drop database student;
```

如果存在删除

```mysql
drop database if exists student;
```

如果存在删除

```mysql
drop database if exists `student`;
```

查看数据库当初如何创建的

```
show create database student;
```

### 2.2.4 修改数据库字符编码 alter database [数据库名] --charset=utf8;

修改字符编码为utf8

```mysql
alter database teacher charset=utf8;
```

# 3 表的基本操作

## 3.1 查看数据库中的表

创建一个数据库

```mysql
create database if not exists `school` charset=gbk;
```

使用`school`数据库

```mysql
use school;
```

显示表

```mysql
show tables;
```

## 3.2 创建表 create table [表名]（）

创建表

```mysql
create table student(
	id int,
	name varchar(30),
	age int
)
```

创建表（高大上）

**id、name、age**：字段

**auto_increment**：自动增长

**primary key**：主键 唯一 不可重复 非空（关系型数据库）

**comment**：注释

**not null**：非空

**default**：缺省 默认

**engine=innodb**：提高运行效率

```mysql
create table if not exists teacher(
	id int auto_increment primary key comment '主键id',
	name varchar(30) not null comment '姓名',
	phone varchar(20) comment '电话',
    address varchar(100) default '未知' comment '住址'
)engine=innodb;
```

查看表如何创建

```mysql
show create table teacher;
```

## 3.3 查看表的结构 desc [表名]

desc 列出字段

```mysql
desc teacher;
```

## 3.4 删除表 drop table [表名]

删除表

```mysql
drop table student;
```

删除多个表

```mysql
drop student,teacher;
```

## 3.5 修改表 alter table [表名] ...

修改表

```mysql
alter table student add phone varchar(20);
```

在name后面添加gender

```mysql
alter table student add gender varchar(2) after name;
alter table student add gender varchar(2) first; //放置到最前面
```

删除字段

```mysql
alter table student drop address;
```

## 3.6 修改字段 alter table [表名] change [字段名] [已修改字段名] [类型]

将表student中phone改为Telephone

```mysql
alter table student change phone Telephone int(11);
```

更改字段类型

```mysql
alter table student modify Telephone varchar(13);
```

# 4 数据的基本操作

## 4.1 插入数据 insert into [表名] (字段) values (字段值)

插入数据

```mysql
insert into teacher (id, name, phone, address) values(1, 'marry', '18766665555', '浙江');
```

自增字段如果values为null 则默认自增

如果值default则会显示创建表的default值

```mysql
insert into teacher values(null, 'Jack', '18766665555', default);
```

![image-20230206094707528](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206094707528.png)

插入多条数据

```
insert into teacher values
(null, 'Jack', '18766665555', default),
(null, 'marry', '18766664444', default),
(null, 'amy', '18766663333', default);
```

![image-20230206094957588](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206094957588.png)

## 4.2 删除数据 delete from [表名] where [条件]

删除数据

```mysql
delete from teacher where id=3;
```

![image-20230206095329232](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206095329232.png)

如果根据不唯一的字段删除数据 则会把值相等的全部删除

****

根据表达式删除

```mysql
delete from student where age>20;
```

![image-20230206100210772](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206100210772.png)

## 4.3 删除表 truncate table;

不推荐的删除 delete from student; 得遍历 速度慢 不便于跑路

推荐方式(焚毁之前的表 重新创建一个以前的表 但是不存储数据)

```mysql
truncate table;
```

## 4.4 细节

![image-20230206100717548](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206100717548.png)

id使用delete清空表 id会再续

使用truncate table teacher 报废表则id从0开始

## 4.5 更新表 update [表名] set [被修改的值] where [条件]

更新数据

```mysql
update teacher set name='amy_updata' where id=3;
```

![image-20230206101401119](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206101401119.png)

```mysql
update teacher set name='amy_updata' where id=3 or phone=18766664444;
```

## 4.6 数据查询 select  [字段] from [表]

简单查询

```mysql
select id, phone, address from teacher;
```

```mysql
select id, name, phone, address from teacher;
```

```mysql
select * from teacher; 
```

## 4.7 SQL语句区分

DDL data definition language 数据库定义语言create alter drop show

DML data manipulation language 数据操作语言 insert update delete select

DCL data control language 数据库控制语句

## 4.8 查看数据库编码

```mysql
show variables like 'character_set_%';
```

![image-20230206111109524](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206111109524.png)

更改编码

```mysql
set character_set_client=gbk;
```

# 5 数据类型

## 5.1 数值数据类型

MySQL 支持所有标准 SQL 数值数据类型。

这些类型包括严格数值数据类型(INTEGER、SMALLINT、DECIMAL 和 NUMERIC)，以及近似数值数据类型(FLOAT、REAL 和 DOUBLE PRECISION)。

关键字INT是INTEGER的同义词，关键字DEC是DECIMAL的同义词。

BIT数据类型保存位字段值，并且支持 MyISAM、MEMORY、InnoDB 和 BDB表。

作为 SQL 标准的扩展，MySQL 也支持整数类型 TINYINT、MEDIUMINT 和 BIGINT。下面的表显示了需要的每个整数类型的存储和范围。

| 类型         | 大小                                     | 范围（有符号）                                               | 范围（无符号）                                               | 用途            |
| ------------ | :--------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------- |
| TINYINT      | 1 Bytes                                  | (-128，127)                                                  | (0，255)                                                     | 小整数值        |
| SMALLINT     | 2 Bytes                                  | (-32 768，32 767)                                            | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    | 3 Bytes                                  | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 Bytes                                  | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       | 8 Bytes                                  | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        | 4 Bytes                                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       | 8 Bytes                                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

创建表

```mysql
create table temp(
id smallint unsigned auto_increment primary key comment 'id',
age tinyint unsigned,
suoerId int(6)
);
```

![image-20230206114818110](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230206114818110.png)

5、3、6 表示宽度

**浮点数**：float(a,b) 其中a表示总位数，b表示小数位数

**定点数**：decimal(a,b)其中a, b分别存储 但是占用的空间也大

## 5.2 日期和时间类型

表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。

TIMESTAMP类型有专有的自动更新特性，将在后面描述。

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'               | YYYY-MM-DD hh:mm:ss | 混合日期和时间值         |
| TIMESTAMP | 4             | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值，时间戳 |

## 5.3 字符串类型

字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

**注意**：char(n) 和 varchar(n) 中括号中 n 代表字符的个数，并不代表字节个数，比如 CHAR(30) 就可以存储 30 个字符。

CHAR 和 VARCHAR 类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。

BINARY 和 VARBINARY 类似于 CHAR 和 VARCHAR，不同的是它们包含二进制字符串而不要非二进制字符串。也就是说，它们包含字节字符串而不是字符字符。这说明它们没有字符集，并且排序和比较基于列值字节的数值值。

BLOB 是一个二进制大对象，可以容纳可变数量的数据。有 4 种 BLOB 类型：TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。它们区别在于可容纳存储范围不同。

有 4 种 TEXT 类型：TINYTEXT、TEXT、MEDIUMTEXT 和 LONGTEXT。对应的这 4 种 BLOB 类型，可存储的最大长度不同，可根据实际情况选择。

# 6 列属性 完整性

## 6.1 列属性问题

default：values为null时候显示的内容

auto_increment：自增键 必然是not null，如果删除类自增字段 则该自增字段的键值不得再次添加

## 6.2 primary key

主键唯一很重要

```mysql
create table demo_table(
id int(20) primary key,
name varchar(15)
);

alter table demo_table add primary key (name);  #后来添加主键
```

## 6.2 组合键

删除主键属性

```mysql
alter demo_table drop primary key (id,name);
```

添加组合键

```mysql
alter demo_table add primary key (id,name);
```

## 6.3 复合主键的作用

未知

## 6.4 unique唯一键

唯一键可以为空，可以为多个，保证数据不得重复 不一定有关联

```mysql 
create table demo2_table(
id int(20) primary key,
phone varchar(20) unique
);
```

![image-20230207110819236](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230207110819236.png)

去除唯一键

```mysql
alter table demo2_table drop index (id, name)
```

## 6.5 sql 内注释和代码注释

单行注释

```mysql
create table demo_table(
id int(20) primary key, #主键 单行注释
name varchar(15) /* 多行注释
    多行注释
    ....
    */
);
```

**sql 内部注释**

```mysql
create table demo_table(
id int(20) primary key, #主键 单行注释
name varchar(15) comment '姓名' 
);
```

## 6.6 数据库完整性

保证拥有主键

保证数据类型合适

default

## 6.7 外键约束 (一般不推荐使用)

创建食堂订单

```mysql
create table eatery(
id int(20) primary key,
money decimal (10,4),
stuId int(4),
foreign key (stuId) references student(id)
);
```

后添加外键

```mysql
alter table ratery2 add foregin key (stuId) references student(id);
```

## 6.8 删除外键

删除外键

```mysql
# 显示外键信息
show create table eatery;

# 删除外键别名
alter table eatery drop foreign key eatery_ibfk_1;
```

![image-20230207172241776](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230207172241776.png)

## 6.9 置空和级联操作

置空：values为空 留给外界删除数据

级联：直接消失 留给外接更新数据

# 7 数据库设计

## 7.1 数据库设计基本概念

关系 关系型数据库：通过公有的字段确定数据的完整性

行：一条数据 一行数据 也叫做实体

列：一个字段 也叫做属性

OOP--面向对象

减少数据冗余------分表

## 7.2 实体和实体之间的关系

一条记录和一条记录之间的关系

主表 从表

一对一

一对多

多对多

## 7.3 codd第一范式 确保每列的原子性

确保每一个字段不能在分，保证数据的精确

每个字段只有一种数据

## 7.4 codd第二范式 非关键字段必须依赖于关键字段

非关键字段必须依赖于关键字段

## 7.5 codd 第三范式 消除传递依赖

没必要的数据不需要在创建一个字段储存

例如总成绩 不要新建一个字段存储 通过计算就可

# 8 单表查询

## 8.1 select 表达式

```mysql
select '属性' as '别名';
```

```mysql
select 8*8;
```

![image-20230208112850911](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230208112850911.png)

## 8.2 from 返回笛卡尔积

笛卡尔积

创建两张表t1, t2

![image-20230208113727472](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230208113727472.png)

```mysql
select * from t1, t2; # 返回笛卡尔积
```

![image-20230208113959333](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230208113959333.png)

## 8.3 dual 默认伪表

```mysql
select 2*7 as res;
```

```mysql
select 2*7 as res from dual;
```

![image-20230208114611503](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230208114611503.png)

## 8.4 where 

```mysql
> < = >= <= != and or not
```

```mysql
select * from student where age >= 18; 
```

```mysql
select * from student where address = 'shanghai' or address = 'beijing' ;
```

包含在某个范围内

```mysql
select * from student where address in('shanghia', 'beijing'); 
```

## 8.5 between...and

在某个范围之内

```mysql
select * from student where age between 15 and 25;
```

在某个范围之外

```mysql
select * from student where age not betwenn 15 and 25;
```

## 8.6 null

查找为空的值

```mysql
select * from student where age is null;
```

查找不为空的值

```mysql
select * from student where age is not null;
```

## 8.7 聚合函数

```mysql
select [聚合函数] from [表名];
sum([字段]);	#求和
avg([字段]);	#求平均
min([字段]);	#求最小
max([字段]);	#求最大
count([字段]);	#计数
```

## 8.8 模糊查询

```mysql
select * from student where name like '张%';	# %表示不限长度
select * from student where name like '张_';	# _表示一个宽度
```

## 8.9 排序查询

按照语文程序排序

```mysql
select * from score order by chinese asc;	# asc升序
select * from score order by chinese desc;	# desc降序
```

## 8.10 分组查询

分别查询男性的平均年龄和女性的平均年龄

```mysql
select gender as '性别'， avg(age) ad '年龄' from info group by gender;	# 按照性别对年龄分组
# 使用group by 查询字段必须是分组字段和聚合函数
```

## 8.11 group_concat()

将查询字段连接

```mysql
select group_concat(name), gender from student group by gender;
```

![image-20230208125841304](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230208125841304.png)

## 8.12 having(类似以where 用于筛选条件)

where 在原本的数据库存在的表中筛选

而having 可以在已经查出来的虚拟表中再去筛选

```mysql
select avg(age) as 'age', address as 'address' from info group by address having age>24;
```

对查询之后语句筛选

## 8.13 limit 

limit 1, 2

```mysql
select * from info limit 1,3;	# 表示 从1开始，查询三条

# 降序排列并查询三条对年龄的记录
select * from info oreder by age desc limit 3;
```

## 8.14 distinct all (去重)

```mysql
select distinct address from info;
# 显示重复个数
select count(distinct address) from info;
```

# 9 多表查询

## 9.1union 联合查询

查询两张表 字段无所谓，但是字段的个数必须相同

```mysql
select age ,gender from info union distinct select name,phone from teacher;
```

## 9.2 inner join内联

```mysql
select name, score from student inner joins core on student.id=score.stuid;# 联合两张表查询inner join 内联
# 建立公共字段
```

## 9.3 left join左联

```mysql
select name, score from student left joins core on student.id=score.stuid;
# 按照student表为基准 score为副表
# right join同理
```

## 9.4 cross join交叉连接

```mysql
select * from t1 cross join t3;
# 返回笛卡尔积
```

## 9.5 natural join自然连接

```mysql
select * from t1 natural join t3;
# 自然连接
# 自然左连接 natural left join
# 自然右连接 natural right join
```

## 9.6 using

```mysql
select * from t1 inner join t3 using(id);
# 指定字段连接
```

![image-20230209110655497](https://ixuanzi-1259418003.cos.ap-chongqing.myqcloud.com/img/image-20230209110655497.png)

# 10 子查询

## 10.1 子查询基本语法

```mysql
select * from student where (select stuid from score where age >= 85);
select * from student where id in (select * from score where age >= 85);
```

## 10.2 in 和 not in

```mysql
select * from student where id not in (select * from score where age >= 85);
```

## 10.3 exists 和 not exists

存在和不存在

```mysql
select * from student where exists (select * from score where age >= 85);	# 如果存在有年龄大于85 的学生就返回全部
select * from student where not exists (select * from score where age >= 85);
```

