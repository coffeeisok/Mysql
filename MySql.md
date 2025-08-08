# 一、 语法规则：

1. 关键字习惯用大写（小写不报错）
2. **反引号（``）** 用于规避关键字冲突（如创建数据库或表时）

3. -u 表示指定 MySQL 用户名
4. -p 表示需要输入密码
5. -D 指定要选择的数据库

# 二、 语句：

## 1． 创建

* 创建数据库

  * ```mysql
    CREATE DATABASE [IF NOT EXISTS] db_name;
    ```

* 创建表

  * ```mysql
    CREATE TABLE table_name (创建表
    	filed1 datatype,
    	filed2 datatype,
    	filed3 datatype
    ) character set 字符集 collate 校对规则 engine 引擎；
    ```

  - field：指定列名


  - datatype：指定列类型（字段类型）


  - character set：默认跟随所在数据库的字符集


  - collate：默认跟随所在数据库校对规则


  - engine：引擎


## 2． 删除

* 删除数据库

  * ```mysql
    DROP DATABASE [IF EXISTS] db_name;
    
    ```

* 删除表

  * ```mysql
    -- 删除表（全删除）
    DROP TABLE [IF EXITS] table_name;
    -- 删除表（仅删除内容，保留表结构）
    TRUNCATE TABLE table_name; 
    ```

* 注：

  - 删除前确认表已备份；


  - 若有外键约束，需先删除外键约束，或者确保依赖关系被处理好


## 3． 修改

### 插入

* **INSERT**

* 基础操作：

  * ```mysql
    -- 插入单行数据
    INSERT INTO 表名 (列 1, 列 2, 列 3, ...)
    VALUES (值 1, 值 2, 值 3, ...);
    -- 插入多行数据
    INSERT INTO 表名 (列 1, 列 2, ...
    VALUES
    (值 1, 值 2,...),
    (值 1, 值 2,...),
    (值 1, 值 2,...);
    -- 例：
    INSERT INTO users (username,email,birthdate,is_active)
    VALUES (‘test’, ‘test@123.com’, ‘1990-02-01’ , true);
    ```

* 进阶操作：

  * ```mysql
    -- 插入查询结果（从其他表复制数据）
    INSERT INTO 目标表名 (列 1, 列 2, ...)
    SELECT 列 1, 列 2,...
    FROM 来源表名
    WHERE 条件；
    -- 例：将 old_users 表中活跃用户迁移到 users 表
    INSERT INTO users (username, email, age)
    SELECT username, email, age
    FROM old_users
    WHERE is_active = 1;
                    
    -- 若插入数据导致主键或唯一索引冲突，直接忽略（不插入也不更新）
    INSERT IGNORE INTO 表名 (列 1, 列 2, ...)
    VALUES (值 1, 值 2, ...);
    ```

  - 如果数据是字符型，必须使用单引号或双引号，如：’1998-02-23’；

  - “值” 跟 “列” 是一一对应的；


  - 要插入所有列的数据，可以省略列名
  - 插入时忽略重复键




## 4． 查询

* 查询数据库

  * ```mysql
    -- 列出所有数据库
    SHOW DATABASES;
    -- 查看数据库创建语句
    SHOW CREATE DATABASE db_name; 
    -- 验证文件存在
    ls "/Users/wucoffee/234.sql" 
    
    -- 查询数据通用语法
    SELECT column1, column2, ...
    FROM table_name
    [WHERE condition]
    [ORDER BY conlumn_name [ASC | DESC]]
    [LIMIT number];
    -- 例
    SELECT * FROM t1 WHERE NAME = 'tom'
     
    -- 查看表结构
    DESCRIBE list_name;
    SHOW COLUMNS FROM list_name; 
    -- 查看表内容
    SELECT * FROM list_name;
    -- 查看表的创建语句
    SHOW CREATE TABLE list_name;
    -- 列出当前数据库中的所有表
    SHOW TABLES;
    -- 查看当前位置
    SELECT DATABASE();
    ```

    - `column`：列名，用`*`代替，表示选择所有列

    - `table_name`：从中查询数据的表名

    - `WHERE condition`：（可选）指定**过滤条件**，只返回符合条件的行

    - `ORDER BY column_name [ASC | DESC]`：（可选）指定结果集的**排序方式**，默认**升序（ASC）**

    - `LINIT number`：（可选）限制返回行数

  

## 5． 备份

（1）

2\. mysqldump -u root -p -B 数据库 1 数据库 2 数据库 n > \[路径 + 文件名].sql

备份数据库 (退出 mysql 后使用）

(1) mysqldump -u root -p -B hsp\_db01 > /Users/wucoffee/123.sql;（备份文件不需要手动创建，该命令会自动创建）

3\. mysqldump -u root -p（密码） 数据库 表 1 表 2 表 n> \[路径 + 文件名].sql

备份数据库中的某几个表

4\. source \[备份文件的路径 + 文件名].sql 恢复数据库（登陆 mysql 后使用）

(1)

## 6． Alter

(1) 添加列

ALTER TABLE 表名

ADD COLUMN 列名 数据类型 \[约束];

\-- 添加一个允许 NULL 的邮箱字段

ALTER TABLE Student

ADD COLUMN Email VARCHAR (100);

\-- 添加一个非空且带默认值的字段

ALTER TABLE Product

ADD COLUMN Stock INT NOT NULL DEFAULT O;

(2) 删除列

ALTER TABLE 表名

DROP COLUMN 列名；

\-- 删除不再使用的旧字段

ALTER TABLE Student

DROP COLUMN OldPhone;

(3) 修改列的数据类型

ALTER TABLE 表名

MODIFY COLUMN 列名 新数据类型；

\-- SQL Server: 将年龄字段从 INT 改为 SMALLINT

ALTER TABLE Student

ALTER COLUMN Sage SMALLINT;

\-- MySQL: 修改姓名字段长度

ALTER TABLE Student

MODIFY COLUMN Sname VARCHAR (100);

(4) 重命名行

ALTER TABLE 表名

CHANGE COLUMN 旧列名 新列名 数据类型；

\-- SQL Server: 将 "Sdept" 重命名为 "Department"

EXEC sp\_rename 'Student. Sdept', 'Department', 'COLUMN';

\--MySQL: 重命名并修改数据类型

ALTER TABLE Student

CHANGE COLUMN Sdept Department VARCHAR (50);

(5) 重命名表

ALTER TABLE 旧表名

RENAME TO 新表名；

\-- SQL Server

EXEC sp\_rename '0ldStudents', 'NewStudents';

\--MySQL

ALTER TABLE 0ldStudents

RENAME TO NewStudents;

(6) 添加约束

ALTER TABLE 表名

ADD CONSTRAINT 约束名 约束类型 (列名);

(7) 删除约束

ALTER TABLE 表名

DROP CONSTRAINT 约束名；

\-- 删除 CHECK 约束

ALTER TABLE Student

DROP CONSTRAINT CHK\_Age;

## 7． 其他零散语句

（1） CHARACTER SET charset\_name; 指定数据库采用的字符集（默认 utf8）

（2） COLLATE collation\_name;     指定数据库字符集校对规则（默认 utf8\_general\_ci, 常用 utf8\_bin。默认的不区分大小写）

（3） USE db\_name; 使用某个数据库；跳转至某个数据库

# 三、 Mysql 列类型（MySql 数据类型）

## 1. 数值类型

(1) 整型

INT4 个字节

INT (M) M：显示宽度

bit (m) m　\[1,64]

(2) 小数

FLOAT 单精度、4 字节

FLOAT (p)

0 <= p <= 24 等同于 FLOAT

25 <= p <= 53 等同于 DOUBLE

DOUBLE 双精度、8 个字节

・FLOAT (M,D)

・DOUBLE (M,D)

M：数字总位数

D：小数点后位数

decimal \[M,D]、大小不定

## 2. 文本类型（字符串类型）

char0\~255

varchar0\~65535\[0\~2^16 -1]

text0\~2^16 -1

## 3. 二进制数据类型

blob\[0\~2^16 -1]

## 4. 日期类型

datatime \[年月日 时分秒；YYYY-MM-DD HH:mm:ss]

timestamp \[时间戳]

## 5. 枚举类型

(1) ENUM—— 用于定义字段只能存储预定义值列表中的一个值

例如：

ENUM (‘男’,‘女’)

ENUM (‘待支付’,‘已发货’,‘已完成’)

特性：

 ENUM 实际存储的是值的索引（从 1 开始），非字符串本身；

 查询时自动转换为字符串，对用户透明

 默认情况下不区分大小写，存储时会按定义的格式保存

constraint

references



## 空间数据库

### `ST_Contains`

* `ST_Contains` 是 **空间数据库（如 MySQL、PostgreSQL+PostGIS 等）提供的核心空间函数**，是空间分析中 “包含关系” 判断的基础工具

* ```mysql
  ST_Contains(几何对象A, 几何对象B)
  ```

  * 作用：用于判断 **几何对象A**是否完全包含**几何对象B**
  * 返回值：
    - `1`（真）：A 完全包含 B；
    - `0`（假）：A 不包含 B；
    - `NULL`：如果 A 或 B 为无效几何。

* 