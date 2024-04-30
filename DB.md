## DDL(Data Definition Language)

数据定义语言，用来定义数据库对象（数据库、表、字段）

### 查询所有数据库

```sql
SHOW DATABASES;
```

### 查询当前数据库

```SQL
SELECT DATABASE();
```

### 创建

```SQL
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHAREST 字符集] [COLLATE 排序规则];
```

### 删除

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```

### 使用

```sql
USE 数据库名
```

### 查询当前数据库所有表

```SQL
SHOW TABLES;
```

### 查询表结构

```sql
DESC 表名;
```

### 查询指定表的建表语句

```sql
SHOW CREATE TABLE 表名;
```

### 创建表

```sql
CREATE TABLE 表名(
	字段1 字段1类型 [COMMENT 字段1注释],
	字段2 字段2类型 [COMMENT 字段2注释],
	字段3 字段3类型 [COMMENT 字段3注释],
	......
	字段N 字段N类型 [COMMENT 字段N注释]
)[COMMENT 表注释];
```

### 添加字段

```sql
ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
```

### 修改字段数据类型

```sql
ALTER TABLE 表名 MODIFY 字段名 新数据库类型(长度);
```

### 修改字段名和字段类型

```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
```

### 删除字段

```sql
ALTER TABLE 表名 DROP 字段名;
```

### 修改表名

```sql
ALTER TABLE 表名 RENAME TO 新表名;
```

### 删除表

```sql
DROP TABLE [IF EXISTS] 表名;
```

### 删除指定表，并重新创建该表

```SQL
TRUNCATE TABLE 表名;
```



## DML(Data Manipulation Language)

数据操作语言，用来对数据库表中的数据进行增删改

### 给指定字段添加数据

```sql
INSERT INTO 表名(字段1,字段2,...) VALUES(值1,值2,...);
```

### 给全部字段添加数据

```sql
INSERT INTO 表名 VALUES(值1,值2,...);
```

### 批量添加数据

```sql
INSERT INTO 表名(字段1,字段2,...) VALUES(值1,值2,...),VALUES(值1,值2,...),VALUES(值1,值2,...);
```

```sql
INSERT INTO 表名 VALUES(值1,值2,...),VALUES(值1,值2,...),VALUES(值1,值2,...);
```

### 修改数据

```sql
UPDATE 表名 SET 字段1=值1,字段2=值2,...[WHERE 条件];
```

注意：修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。

### 删除数据

```sql
DELETE FROM 表名 [WHERE 条件];
```

## DQL(Data Query Language)

数据查询语言，用来查询数据库中表的记录

### 查询多个字段

```sql
SELECT 字段1,字段2,字段3... FROM 表名;
```

### 设置别名

```sql
 SELECT 字段1 [AS 别名1],字段2 [AS 别名2],... FROM 表名;
```

### 去除重复记录

```sql
SELECT DISTINCT 字段别名 FROM 表名;
```

### 条件查询

```SQL
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```

|   **比较运算符**    |                  **功能**                   |
| :-----------------: | :-----------------------------------------: |
|          >          |                    大于                     |
|         >=          |                  大于等于                   |
|          <          |                    小于                     |
|         <=          |                  小于等于                   |
|          =          |                    等于                     |
|      <> 或 !=       |                   不等于                    |
| BETWEEN ... AND ... |           在某个范围之内(闭区间)            |
|       IN(...)       |        在IN之后的列表中的值，多选一         |
|     LIKE 占位符     | 模糊匹配( _ 匹配单个字符，% 匹配任意个字符) |
|       IS NULL       |                   是NULL                    |

| **逻辑运算符** |          **功能**          |
| :------------: | :------------------------: |
|   AND 或 &&    |   并且(多个条件同时成立)   |
|   OR 或 \|\|   | 或者(多个条件任意一个成立) |
|   NOT 或 ！    |          非，不是          |

### 聚合函数

```sql
SELECT 聚合函数(字段列表) FROM 表名;
```

| **函数** | **功能** |
| :------: | :------: |
|  COUNT   | 统计数量 |
|   MAX    |  最大值  |
|   MIN    |  最小值  |
|   AVG    |  平均值  |
|   SUM    |   求和   |

### 分组查询

```sql
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
```

注：	

​	执行时机不同：WHERE 是对分组之前的数据进行过滤，而 HAVING 是分组后对结果(组)进行过滤

​	判断条件不同：WHERE不能对聚合函数进行判断，而HAVING可以

### 排序查询

```sql
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1,字段2 排序方式2;
```

排序方式：

​	ASC：升序（默认）

​	DESC：降序

注：如果多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序

### 分页查询(MySQL版)

```sql
SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;
```

注：

​	1.起始索引从0开始，起始索引=(查询页码-1)*每页显示记录数

​	2.如果查询的是第一页数据，起始索引可以省略，直接略写为 LIMIT 查询记录数

## DCL(Data Control Language)

数据控制语言，用来创建数据库用户、控制数据库的访问权限

### 查询用户

```sql
USE mysql;
SELECT * FROM user;
```

### 创建用户

```sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

注：主机名 处填’%‘，代表任意主机

### 修改用户密码

```sql
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```

### 删除用户

```sql
DROP USER '用户名'@'主机名';
```

补：DCL-权限控制

|       权限        |        说明        |
| :---------------: | :----------------: |
| ALL,ALLRPIVILEGES |      所有权限      |
|      SELECT       |      查询数据      |
|      INSERT       |      插入数据      |
|      UPDATE       |      插入数据      |
|      DELETE       |      删除数据      |
|       ALTER       |       修改表       |
|       DROP        | 删除数据库/表/视图 |
|      CREATE       |   创建数据库/表    |

### 查询权限

```sql
SHOW GRANTS FOR '用户名'@'主机名';
```

### 授予权限

```sql
GRANT 权限列表 ON 数据名.表名 TO '用户名'@'主机名';
```

注：如果授予所有权限，那么权限列表处填 all

### 撤销权限 

```
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

## 函数

### 字符串函数

|           函数           |                           功能                            |
| :----------------------: | :-------------------------------------------------------: |
|   CONCAT(S1,S2,...,Sn)   |        字符串拼接，将S1,S2,...,Sn拼接成一个字符串         |
|        LOWER(str)        |                 将字符串str全部转换为小写                 |
|        UPPER(str)        |                  将字符串str全部转为大写                  |
|     LPAD(str,n,pad)      | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度 |
|     RPAD(str,n,pad)      | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度 |
|        TRIM(str)         |                去掉字符串头部和尾部的空格                 |
| SUBSTRING(str,start,len) |       返回字符串str从start位置起的len个长度的字符串       |

注：SUBSTRING中字符串的下标索引从1开始

### 数值函数

|    函数    |                功能                |
| :--------: | :--------------------------------: |
|  CEIL(x)   |              向上取整              |
|  FLOOR(x)  |              向下取整              |
|  MOD(x,y)  |            返回x/y的模             |
|   RAND()   |   返回0~1内的随机数（去头去尾）    |
| ROUND(X,Y) | 求参数x的四舍五入的值，保留y位小数 |

### 日期函数

|               函数                |                           功能                            |
| :-------------------------------: | :-------------------------------------------------------: |
|             CURDATE()             |                       返回当前日期                        |
|             CURTIME()             |                       返回当前时间                        |
|               NOW()               |                    返回当前日期和时间                     |
|            YEAR(date)             |                    获取指定date的年份                     |
|            MONTH(date)            |                    获取指定date的月份                     |
|             DAY(date)             |                    获取指定date的日期                     |
| DATE_ADD(date,INTERVAL expr type) |     返回一个日期/时间值加上一个时间间隔expr后的时间值     |
|       DATEDIFF(date1,date2)       | 返回起始时间date1和结束时间date2之间的天数（date1-date2） |

注：DATE_ADD函数中，type填时间单位，expr可以为负数，即往前推

### 流程函数

|                            函数                            |                           功能                           |
| :--------------------------------------------------------: | :------------------------------------------------------: |
|                       IF(value,t,f)                        |            如果value为true则返回t，否则返回f             |
|                   IFNULL(value1,value2)                    |      如果value1不为NULL，返回value1，否则返回value2      |
|     CASE WHEN [val1] THEN [res1] ... ELSE[default] END     |    如果val1为true，返回res1，...否则返回default默认值    |
| CASE [expr] WHEN [val1] THEN [res1] ... ELSE [default] END | 如果expr的值等于val1，返回res1，...否则返回default默认值 |

注：对于IFNULL函数，value1如果为空（即‘’），返回值仍然为value1（’‘），只有当value1=NULL时，才返回value2

## 约束

概念：约束是作用于表中字段上的规则，用于限超市供销管理系统数据库制存储在表中的数据。

目的：保证数据库中数据的正确、有效性和完整性

| 约束         | 描述                                                         | 关键字          |
| ------------ | ------------------------------------------------------------ | --------------- |
| 非空约束     | 限制该字段的数据不能为NULL                                   | NOT NULL        |
| 唯一约束     | 保证该字段的所有数据都是唯一、不重复的                       | UNIQUE          |
| 主键约束     | 主键是一行数据的唯一表示，要求非空且唯一                     | PRIMARY KEY     |
| 默认约束     | 保存数据时，如果未指定该字段的值，则采用默认值               | DEFAULT         |
| 检查约束     | 保证字段值满足某一个条件                                     | CHECK           |
| 外键约束     | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性     | FOREIGN KEY     |
| 自增主键约束 | 起主键作用，会在新纪录插入表中时生成一个唯一的数字。（自增） | AUTO_ INCREMENT |

注：约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束

### 添加外键

```sql
CREATE TABLE 表名{
	字段名 数据类型,
	...
	CONSTRAINT [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名)
};
```

```sql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名)
```

### 删除外键

```sql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

### 删除/更新行为

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| NO ACTION   | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。（与RESTRICT一致） |
| RESTRICT    | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。（与NO ACTION一致） |
| CASCADE     | 当在父表中删除/更新对应记录时，首先检查是否有对应外键，如果有，则也删除/更新外键在子表中的记录。 |
| SET NULL    | 当在父表中删除对应记录时，首先价差该记录是否有对应外键，如果有则设置子表中该外键值为null（这就要求该外键允许取null） |
| SET DEFAULT | 父表有变更时，子表将外键设置成一个默认的值（Innodb不支持）   |

```sql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE 更新模式 ON DELETE 更新模式;
```

