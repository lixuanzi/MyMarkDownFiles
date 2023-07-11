# JavaWeb_霞光

## 1 Socket技术

要实现Socket通信，我们必须创建一个数据发送者和一个数据接收者，也就是客户端和服务端，我们需要提前启动服务端，来等待客户端的连接，而客户端只需要随时启动去连接服务端即可！

```java
//服务端
package com.ixuanzi.socket;

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080);){

            // 服务端保持开启状态
            while(true){
                Socket accept = serverSocket.accept();
                System.out.println("客户端已连接，IP地址为："+accept.getInetAddress().getHostAddress());
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
//客户端
package com.ixuanzi.socket;

import java.io.IOException;
import java.net.Socket;

public class Client {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 8080)){
            System.out.println("已连接到服务端！");
        }catch (IOException e){
            System.out.println("服务端连接失败！");
            e.printStackTrace();
        }
    }
}
```

实际上它就是一个TCP连接的建立过程：

一旦TCP连接建立，服务端和客户端之间就可以相互发送数据，直到客户端主动关闭连接。当然，服务端不仅仅只可以让一个客户端进行连接，我们可以尝试让服务端一直运行来不断接受客户端的连接：

```java
 try (ServerSocket serverSocket = new ServerSocket(8080);){

            // 服务端保持开启状态
            while(true){
                Socket accept = serverSocket.accept();
                System.out.println("客户端已连接，IP地址为："+accept.getInetAddress().getHostAddress());
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
```

现在我们就可以多次去连接此服务端了。

### 1.1使用Socket传输文件

既然Socket为我们提供了IO流便于数据传输，那么我们就可以轻松地实现文件传输了。

```java
// 服务端
package com.ixuanzi.socket;

import java.io.*;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class Server {
    public static void main(String[] args) {
        try {
            ServerSocket serverSocket = new ServerSocket(8080);
            Scanner scanner = new Scanner(System.in);
            // 服务端开启
            Socket socket = serverSocket.accept();
            InputStream stream = socket.getInputStream();
            FileOutputStream fileOutputStream = new FileOutputStream("net/data.txt");

            // 创建字节流读取
            byte[] bytes = new byte[1024];
            int i;
            while((i = stream.read(bytes)) !=-1){
                fileOutputStream.write(bytes, 0, i);
            }

            fileOutputStream.flush();
            serverSocket.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
// 客户端
package com.ixuanzi.socket;

import java.io.*;
import java.net.Socket;


public class Client {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 8080)) {
            FileInputStream fileInputStream = new FileInputStream("test.txt");
            OutputStream stream = socket.getOutputStream();

            byte[] bytes = new byte[1024];
            int i;
            while ((i = fileInputStream.read(bytes)) !=-1){
                stream.write(bytes,0,i);
            }
            fileInputStream.close();
            stream.flush();

        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("连接失败！");

        }
    }
}
```

## 2 数据库

### 2.1数据库查询语言（DQL）

数据库的查询是我们整个数据库学习中的重点内容，面对数据库中庞大的数据，该如何去寻找我们想要的数据，就是我们主要讨论的问题。

#### 单表查询

单表查询是最简单的一种查询，我们只需要在一张表中去查找数据即可，通过使用`select`语句来进行单表查询：

```sql
-- 指定查询某一列数据
SELECT 列名[,列名] FROM 表名
-- 会以别名显示此列
SELECT 列名 别名 FROM 表名
-- 查询所有的列数据
SELECT * FROM 表名
-- 只查询不重复的值
SELECT DISTINCT 列名 FROM 表名
```

我们也可以添加`where`字句来限定查询目标：

```sql
SELECT * FROM 表名 WHERE 条件
```

#### 常用查询条件

* 一般的比较运算符，包括=、>、<、>=、<=、!=等。
* 是否在集合中：in、not in
* 字符模糊匹配：like，not like
* 多重条件连接查询：and、or、not

我们来尝试使用一下上面这几种条件。

#### 排序查询

我们可以通过`order by`来将查询结果进行排序：

```sql
SELECT * FROM 表名 WHERE 条件 ORDER BY 列名 ASC|DESC
```

使用ASC表示升序排序，使用DESC表示降序排序，默认为升序。

我们也可以可以同时添加多个排序：

```sql
SELECT * FROM 表名 WHERE 条件 ORDER BY 列名1 ASC|DESC, 列名2 ASC|DESC
```

这样会先按照列名1进行排序，每组列名1相同的数据再按照列名2排序。

#### 聚集函数

聚集函数一般用作统计，包括：

* `count([distinct]*)`统计所有的行数（distinct表示去重再统计，下同）
* `count([distinct]列名)`统计某列的值总和
* `sum([distinct]列名)`求一列的和（注意必须是数字类型的）
* `avg([distinct]列名)`求一列的平均值（注意必须是数字类型）
* `max([distinct]列名)`求一列的最大值
* `min([distinct]列名)`求一列的最小值

一般聚集函数是这样使用的：

```sql
SELECT count(distinct 列名) FROM 表名 WHERE 条件 
```

#### 分组和分页查询

通过使用`group by`来对查询结果进行分组，它需要结合聚合函数一起使用：

```sql
SELECT sum(*) FROM 表名 WHERE 条件 GROUP BY 列名
```

我们还可以添加`having`来限制分组条件：

```sql
SELECT sum(*) FROM 表名 WHERE 条件 GROUP BY 列名 HAVING 约束条件
```

我们可以通过`limit`来限制查询的数量，只取前n个结果：

```sql
SELECT * FROM 表名 LIMIT 数量
```

我们也可以进行分页：

```sql
SELECT * FROM 表名 LIMIT 起始位置,数量
```

#### 多表查询

多表查询是同时查询的两个或两个以上的表，多表查询会提通过连接转换为单表查询。

```sql
SELECT * FROM 表1, 表2
```

直接这样查询会得到两张表的笛卡尔积，也就是每一项数据和另一张表的每一项数据都结合一次，会产生庞大的数据。

```sql
SELECT * FROM 表1, 表2 WHERE 条件
```

这样，只会从笛卡尔积的结果中得到满足条件的数据。

**注意：**如果两个表中都带有此属性吗，需要添加表名前缀来指明是哪一个表的数据。

#### 自身连接查询

自身连接，就是将表本身和表进行笛卡尔积计算，得到结果，但是由于表名相同，因此要先起一个别名：

```sql
SELECT * FROM 表名 别名1, 表名 别名2
```

其实自身连接查询和前面的是一样的，只是连接对象变成自己和自己了。

#### 外连接查询

外连接就是专门用于联合查询情景的，比如现在有一个存储所有用户的表，还有一张用户详细信息的表，我希望将这两张表结合到一起来查看完整的数据，我们就可以通过使用外连接来进行查询，外连接有三种方式：

* 通过使用`inner join`进行内连接，只会返回两个表满足条件的交集部分：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019053022120536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1ODIwMQ==,size_16,color_FFFFFF,t_70)

* 通过使用`left join`进行左连接，不仅会返回两个表满足条件的交集部分，也会返回左边表中的全部数据，而在右表中缺失的数据会使用`null`来代替（右连接`right join`同理，只是反过来而已，这里就不再介绍了）：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190530221543230.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1ODIwMQ==,size_16,color_FFFFFF,t_70)

#### 嵌套查询

我们可以将查询的结果作为另一个查询的条件，比如：

```sql
SELECT * FROM 表名 WHERE 列名 = (SELECT 列名 FROM 表名 WHERE 条件)
```

我们来再次尝试编写一下在最开始我们查找某教师所有学生的SQL语句。

***

### 2.2数据库控制语言（DCL）

庞大的数据库不可能由一个人来管理，我们需要更多的用户来一起管理整个数据库。

#### 创建用户

我们可以通过`create user`来创建用户：

```sql
CREATE USER 用户名 identified by 密码;
```

也可以不带密码：

```sql
CREATE USER 用户名;
```

我们可以通过@来限制用户登录的登录IP地址，`%`表示匹配所有的IP地址，默认使用的就是任意IP地址。

#### 登陆用户

首先需要添加一个环境变量，然后我们通过cmd去登陆mysql：

```sql
login -u 用户名 -p
```

输入密码后即可登陆此用户，我们输入以下命令来看看能否访问所有数据库：

```sql
show databases;
```

我们发现，虽然此用户能够成功登录，但是并不能查看完整的数据库列表，这是因为此用户还没有权限！

#### 用户授权

我们可以通过使用`grant`来为一个数据库用户进行授权：

```sql
grant all|权限1,权限2...(列1,...) on 数据库.表 to 用户 [with grant option]
```

其中all代表授予所有权限，当数据库和表为`*`，代表为所有的数据库和表都授权。如果在最后添加了`with grant option`，那么被授权的用户还能将已获得的授权继续授权给其他用户。

我们可以使用`revoke`来收回一个权限：

```sql
revoke all|权限1,权限2...(列1,...) on 数据库.表 from 用户
```

***

### 2.3视图

视图本质就是一个查询的结果，不过我们每次都可以通过打开视图来按照我们想要的样子查看数据。既然视图本质就是一个查询的结果，那么它本身就是一个虚表，并不是真实存在的，数据实际上还是存放在原来的表中。

我们可以通过`create view`来创建视图;

```sql
CREATE VIEW 视图名称(列名) as 子查询语句 [WITH CHECK OPTION];
```

WITH CHECK OPTION是指当创建后，如果更新视图中的数据，是否要满足子查询中的条件表达式，不满足将无法插入，创建后，我们就可以使用`select`语句来直接查询视图上的数据了，因此，还能在视图的基础上，导出其他的视图。

1. 若视图是由两个以上基本表导出的，则此视图不允许更新。
2. 若视图的字段来自字段表达式或常数，则不允许对此视图执行INSERT和UPDATE操作，但允许执行DELETE操作。
3. 若视图的字段来自集函数，则此视图不允许更新。
4. 若视图定义中含有GROUP BY子句，则此视图不允许更新。
5. 若视图定义中含有DISTINCT短语，则此视图不允许更新。
6. 若视图定义中有嵌套查询，并且内层查询的FROM子句中涉及的表也是导出该视图的基本表，则此视图不允许更新。例如将成绩在平均成绩之上的元组定义成一个视图GOOD_SC： CREATE VIEW GOOD_SC AS SELECT Sno, Cno, Grade FROM SC WHERE Grade > (SELECT AVG(Grade) FROM SC); 　　导出视图GOOD_SC的基本表是SC，内层查询中涉及的表也是SC，所以视图GOOD_SC是不允许更新的。
7. 一个不允许更新的视图上定义的视图也不允许更新

通过`drop`来删除一个视图：

```sql
drop view apptest
```

***

### 2.4索引

在数据量变得非常庞大时，通过创建索引，能够大大提高我们的查询效率，就像Hash表一样，它能够快速地定位元素存放的位置，我们可以通过下面的命令创建索引：

```sql
-- 创建索引
CREATE INDEX 索引名称 ON 表名 (列名)
-- 查看表中的索引
show INDEX FROM student
```

我们也可以通过下面的命令删除一个索引：

```sql
drop index 索引名称 on 表名
```

虽然添加索引后会使得查询效率更高，但是我们不能过度使用索引，索引为我们带来高速查询效率的同时，也会在数据更新时产生额外建立索引的开销，同时也会占用磁盘资源。

***

### 2.5触发器

触发器就像其名字一样，在某种条件下会自动触发，在`select`/`update`/`delete`时，会自动执行我们预先设定的内容，触发器通常用于检查内容的安全性，相比直接添加约束，触发器显得更加灵活。

触发器所依附的表称为基本表，当触发器表上发生`select`/`update`/`delete`等操作时，会自动生成两个临时的表（new表和old表，只能由触发器使用）

比如在`insert`操作时，新的内容会被插入到new表中；在`delete`操作时，旧的内容会被移到old表中，我们仍可在old表中拿到被删除的数据；在`update`操作时，旧的内容会被移到old表中，新的内容会出现在new表中。

```sql
CREATE TRIGGER 触发器名称 [BEFORE|AFTER] [INSERT|UPDATE|DELETE] ON 表名/视图名 FOR EACH ROW DELETE FROM student WHERE student.sno = new.sno
```

 FOR EACH ROW表示针对每一行都会生效，无论哪行进行指定操作都会执行触发器！

通过下面的命令来查看触发器：

```sql
SHOW TRIGGERS
```

如果不需要，我们就可以删除此触发器：

```sql
DROP TRIGGER 触发器名称
```

***

### 2.6事务

当我们要进行的操作非常多时，比如要依次删除很多个表的数据，我们就需要执行大量的SQL语句来完成，这些数据库操作语句就可以构成一个事务！只有Innodb引擎支持事务，我们可以这样来查看支持的引擎：

```sql
SHOW ENGINES;
```

MySQL默认采用的是Innodb引擎，我们也可以去修改为其他的引擎。

事务具有以下特性：

- **原子性：**一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
- **一致性：**在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
- **隔离性：**数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。
- **持久性：**事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

我们通过以下例子来探究以下事务：

```sql
begin;   #开始事务
...
rollback;  #回滚事务
savepoint 回滚点;  #添加回滚点
rollback to 回滚点; #回滚到指定回滚点
...
commit; #提交事务
-- 一旦提交，就无法再进行回滚了！
```

## 3 Java与数据库