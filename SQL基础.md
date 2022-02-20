# 一章、数据库和SQL

## 1.DBMS的种类

1. 层次数据库 ：古老的数据类型
2. 关系数据库  ： 行列二维表
3. 面向对象数据库
4. XML数据库
5. 键值存储系统

## 2.数据库结构

![](D:.\img\sql_1.png)

## 3.SQL概要

### SQL语句及其种类

1. DDL 数据定义语言
   - CREATE：创建数据库和表对象
   - DROP：删除数据库和表对象
   - ALTER：修改数据库和表等对象的结构
2. DML 数据操纵语言
   - SELECT：查询表中的数据
   - INSERT：向表中插入数据
   - UPDATE：更新表中数据
   - DELETE：删除表中数据
3. DCL 数据控制语言
   - COMMIT：确认对数据库中的数据进行变更
   - ROLLBACK：取消对数据库中的数据进行的变更
   - GRANT：赋予用户操作权限
   - REVOKE：取消用户的操作权限

### SQL的基本书写规则

1. SQL语句要以  `;` 结尾
2. SQL语句不区分大小写
   - 关键字大写
   - 表名的首字母大写
   - 其余（列名等）小写
   - 插入到表中的数据时区分大小写的
3. 常数的书写方式是固定的 使用 `''`
4. 单词需要用半角空格或者换行分割

## 4.表的创建

## 5.数据类型的指定

1. `INTEGER`：整数，不能存 小数
2. `CHAR/CHAR(num)`：
   - 字符串 / 限制长度 
   - 属于定长字符串 当字符串长度不够时会以半角空格填满
3. `VARCHAR / VARCHAR(num)`
   - 字符串
   - 可变长字符串
4. DATE：日期

## 6.约束的设置

1. `PRIMARY KEY (product_id)` ：设置主键

## 7.表的删除和更新

1. 删除表：`DROP TABLE <表名>`
2. 添加列：`ALTER TABLE <表名> ADD COLUMN <列的定义>`
3. 删除列：`ALTER TABLE <表名> DROP COLUMN <列名>`
4. 插入数据：`INSERT INTO <表名> VALUES (row数据)`
5. 重命名表名：`ALTER TABLE oldName RENAME TO newName`

# 二章、查询基础

## 1.SELECT 语句基础

1. 列的查询：`SELECT <列名>,... FROM <表名>`
2. 查询所有列：`SELECT * FROM <表名>`
3. 列设定别名：`SELECT name1 AS otherName1,... ... FROM <表名>`
   - 中文别名：`"中文名"`
4. 删除重复行：`SELECT DISTINIC <列名>,... ... FROM <表名>;` 
   - `NULL` 也是类型
   - `DISTINCT` 关键字只能用在第一个列名之前
5. 条件查询：`SELECT <列名>,... ... FROM <表名> WHERE <条件>;`
6. 注释：
   - 单行：`-- ...`
   - 多行：`/*...*/`

## 2.算数运算符和比较运算符

1. 算数运算符：`SELECT <列名>,... ... , <列名> * num AS <新列名> FROM <表名>;`
   - 可以使用 `()`
   - `+ ,- ,*, /`
   - NULL的运算都是NULL
2. 比较运算符：`SELECT <列名>,... ... FROM <表名> WHERE <列名> = num;`
   - `=,<>, >, >=, <, <=, IS NULL,IS NOT NULL` 
   - 可以使用计算表达式：`SELECT <列名>,... ... FROM <表名> WHERE <表达式> = num;`
   - 字符串比较是按照字典顺序进行排序
   - 不能对`NULL`使用比较运算符
   - `IS NULL`：判断是否为NULL的比较运算符
   - `IS NOT NULL`：判断是否不为 NULL

## 3.逻辑运算符

1. `NOT` 运算符：`WHERE NOT <条件>;` 非 的意思
2. `AND`和`OR`运算符：`&& || 的意思`
3. `()`强化运算：`AND`级别高于`OR`可以使用`()`进行区分

# 三章、聚合与排序

## 1. 对表进行聚合查询

1. 聚合函数：将对行输出为一行
   - COUNT：计算表中的记录数（行数）
   - SUM：计算表中数值中数据的合计值
   - AVG：计算表中数值中数据的平均值
   - MAX：求出表中数值中数据的最大值
   - MIN：求出表中数值中数据的最小值
2. COUNT：`SELECT COUNT(*/<列名>) FROM <表名>;`
   - `COUNT(*)` 不会忽略 NULL
   - `COUNT(<表名>)` 会忽略NULL
3. AVG：`SELECT AVG(<列名>) FROM <表名>;`
4. MAX：`MAX(<列>)`
5. MIN：`MIN(<列>)`
6. DISTINCT：删除重复值，聚合查询可以去除重复
   - `SELECT COUNT(DISTINCT <列>) FROM <表名>;`
   - 其他的计算也可以使用

## 2. 对表进行分组

1. `GROUP BY` ：对表进行分组
   - `SELECT <列>,... ... FROM <表名> GROUP BY <列名>,... ...;`
   - 当聚合键中有`NULL`的情况下 会将NULL单独作为一组
   - 不允许使用别名
2. 子句书写顺序：`SELECT →  FROM → WHERE → GROUP BY`
3. 当使用GROUP时运行顺序：`FROM → WHERE → GROUP BY → SELECT`

## 3. 为聚合结果指定条件

1. `HAVING`：指定组的条件
   - 顺序：`SELECT → FROM → WHERE → GROUP BY → HAVING`
   - `SELECT <列名> ,... <运算> FROM <表名> WHERE <条件> GROUP BY <列名> HAVING <条件>;`
2. HAVING子句的构成要素：
   - `常数、聚合函数、GROUP BY 聚合键`
3. 与WHERE：
   - WHERE：指定行对应的条件
   - HAVING：指定组对应的条件

## 4. 对查询结果进行排序

1. `ORDER BY`：对数据进行排序
   - `SELECT 子句 → 2. FROM 子句 → 3. WHERE 子句 → 4. GROUP BY 子句 → 5. HAVING 子句 → 6. ORDER BY 子句`
2. 升序或降序：
   - 降序：`ORDER BY <列> DESC;`
   - 升序：`ORDER BY <列> ASC;`
3. 多个排序键：多个排序 `ORDER BY <列>,... ...`
4. `NULL`的顺序：汇集开头或者结尾
5. 排序键中使用别名：`允许`使用别名
6. ORDER BY：`可以使用不在SELECT中的列`
7. 不要使用列编号：
   - 虽然可以使用列编号进行排序但是不推荐

# 四章、数据跟新

## 1. 数据的插入

1. `INSERT`：向表中插入数据
   - `INSERT INTO <表> (<列>,... ...) VALUES (<值>,... ...);`
   - 列可省略：`INSERT INTO <表> VALUES (<值>,... ...);`
   - 列与值一 一对应
   - 原则上一次INSERT插入一条数据
2. 插入NULL：
   - 需要空值时可以直接插入NULL
   - 但是不能 设置NOT NULL
3. 插入默认值：
   - 显式：值直接设为 `DEFAULT`  —— 推荐
   - 隐式：`同时省略 默认列的 列与值`
4. 复制其他表数据：
   - `INSERT INTO ... ... SELECT ...;`

## 2. 数据的删除

1. `DROP TABLE <表>;`：删除表
2. `DELETE`：
   - 语法：`DELETE FROM <表>;`会删除表的全部数据
   - `WHERE`：
     - DELETE 只支持WHERE子句，其他`不支持`
     - `DELETE FROM <表> WHERE <条件>;`

## 3. 数据的更新



1. `UPDATE`：跟更新数据
   - 修改列下面的数据：`UPDATE FROM <表> SET <列> = <条件>;`
   - 修改搜索数据：`UPDATE FROM <表> SET <列> = <条件> WHERE <条件>;`
2. `NULL更新`：`<列> = NULL`
3. 多列更新：
   - `UPDATE FROM <表> SET <列> = <条件>,... ...WHERE <条件>;`
   - `UPDATE FORM <表> SET (<列>,... ...) = (<值>,... ...) WHERE <条件>;`

## 4. 事务

1. `做用`：需要在同一个处理单元中执行的一系列更新处理集合
2. 语法：
   - 创建：`BEGIN TRANSCATION;DML1;DML2... ... COMMIT;`
   - `COMMIT`：提交处理
   - `ROOLBACK`：取消处理
3. `ACID`：
   - 原子性：更新要么全部完成要么全部不完成
   - 一致性：需要满足数据库提前设置的约束，如果不符合SQL将会取消
   - 隔离性：不同事务之间互不干扰
   - 持久性：事务结束，DBMS能够保证该时间点的数据状态会被保存的特性

# 五章、复杂查询

## 1. 视图

1. 视图：视图就是保存好的SELECT语句，类似于方法
2. 创建：`CREATE VIEW <视图名>(<视图列>,... ...) AS <SELECT语句>`
3. 使用：`SELECT <列>,... ... FROM <视图名>`
4. 视图查询：
   - 首先执行定义视图的SELECT语句
   - 根据得到的结果，在执行FROM子句中使用视图的SELECT语句
   - `会执行2条以上的SELECT语句`
5. 注意：**多重视图会降低SQL性能**
6. 限制：
   - 定义视图不能使用`ORDER BY子句`
7. 删除：`DROP VIEW <视图>`
8. 可以被更新：
   - `SELECT`子句中未使用`DISTINCT`
   - `FROM`子句**只有一张表**
   - 未使用`GROUP BY`
   - 未使用`HAVING`子句
9. **不是通过汇总得到的视图就可以进行更新**

## 2. 子查询

1. 子查询：就是一次性视图（SELECT语句）
2. 子查询的名称：`AS <子查询名>`
3. 标量子查询：必须返回一行一列的结果
4. 注意：**子查询必须返回单列**
5. 相当于直接书写的方法

## 3. 关联子查询

1. 关联：多个子查询调用数据

2. 作用域：结合条件需要写在子查询中

3. ```sql
   SELECT product_type,product_name,sale_price FROM Product AS P1 WHERE sale_price >= (SELECT AVG(sale_price) FROM Product AS P2 WHERE P1.product_type = P2.product_type GROUP BY product_type);
   ```

# 六章、函数、谓词、CASE表达式

## 1. 函数

1. 函数分类：
   - 算数函数
   - 字符串函数
   - 日期函数
   - 转换函数
   - 聚合函数
2. 算数函数：
   - `ABS(值)` ：绝对值
   - `MOD(被除数，除数)`：求余
   - `ROUND(值，保留位数)`：四舍五入
3. 字符串函数：
   - `||`：字符串拼接，类似于`+`
     - `SQL Server `使用 `+`
     - `Mysql` 使用 `CONCAT(str,... ...)`
   - `LENGTH(str)`：长度
     - `SQL Server` 使用 `LEN(str)`
   - `LOWER(str)`：小写
   - `REPLACE()`：字符串替换

































