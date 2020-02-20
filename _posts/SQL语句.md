# SQL

# 基础语句

# 创建表
```
CREATE TABLE <表名>(
<列名><数据类型>[列级完整性约束条件],
<列名><数据类型>[列级完整性约束条件],
<列名><数据类型>[列级完整性约束条件],
   ......
);

```
> 列级完整性约束条件有NULL[可为空]、NOT NULL[不为空]、UNIQUE[唯一]，可以组合使用，但是不能重复和对立关系同时存在。

## 示例
```
-- 创建学生表
CREATE TABLE Student
(
  Id INT NOT NULL UNIQUE PRIMARY KEY,
  Name VARCHAR(20) NOT NULL,
  Age INT NULL,
  Gender VARCHAR(4) NULL
);
```

# 删除表

```
DROP TABLE <表名>;
```


## 示例
```
-- 删除学生表
DROP TABLE Student; 
```


# 清空表
` TRUNCATE TABLE <表名>; `

- 清空表和删除表的区别
    1.  删减表(delete)：逐条删除（速度较慢）, 写服务器log，删除全部数据，并且删除主键和索引。
    2.  清空表(TRUNCATE)： 整体删除（速度较快）,不写服务器log,只清空表中数据，不删除主键和索引。
    3.  如果只需删除表中的部分记录，只能使用DELETE语句配合where条件， DELETE FROM wp_comments WHERE……


## 示例
```
-- 删除学生表
TRUNCATE TABLE Student; 
```
# 修改表
```
-- 添加列
ALTER TABLE <表名> [ADD <新列名> <数据类型>[列级完整性约束条件]]
-- 删除列
ALTER TABLE <表名> [DROP COLUMN <列名>]
-- 修改列
ALTER TABLE <表名> [MODIFY COLUMN <列名> <数据类型> [列级完整性约束条件]]
```
## 示例
```
-- 添加学生表`Phone`列
ALTER TABLE Student ADD Phone VARCHAR(15) NULL;
-- 删除学生表`Phone`列
ALTER TABLE Student DROP COLUMN Phone;
-- 修改学生表`Phone`列
ALTER TABLE Student MODIFY Phone VARCHAR(13) NULL;
```

# SQL查询语句
```
SELECT [ALL|DISTINCT] <目标列表达式>[,<目标列表达式>]…
  FROM <表名或视图名>[,<表名或视图名>]…
  [WHERE <条件表达式>]
  [GROUP BY <列名> [HAVING <条件表达式>]]
  [ORDER BY <列名> [ASC|DESC]…]

```
> SQL查询语句的顺序：SELECT、FROM、WHERE、GROUP BY、HAVING、ORDER BY。SELECT、FROM是必须的，HAVING子句只能与GROUP BY搭配使用。

## 示例
```
SELECT * FROM Student
  WHERE Id>10
  GROUP BY Age HAVING AVG(Age) > 20
  ORDER BY Id DESC
```
# SQL更新语句
```
UPDATE <表名> SET 列名=值表达式[,列名=值表达式…]
  [WHERE 条件表达式]
```
## 示例
```
-- 将Id在(10,100)的Age加1
UPDATE Student SET Age= Age+1 WHERE Id>10 AND Id<100
```
# SQL删除语句
`DELETE FROM <表名> [WHERE 条件表达式]`
## 示例
```
- 删除Id小于10的数据记录
DELETE FROM Student WHERE Id<10;
```
# 创建索引
`CREATE [UNIQUE] [CLUSTER] INDEX <索引名> ON <表名>(<列名>[<次序>][,<列名>[<次序>]]…);
`

    1. UNIQUE：表明此索引的每一个索引值只对应唯一的数据记录
    2. CLUSTER：表明建立的索引是聚集索引
    3. 次序：可选ASC(升序)或DESC(降序)，默认ASC

## 示例
```
-- 建立学生表索引：单一字段Id索引倒序
CREATE UNIQUE INDEX INDEX_SId ON Student (Id DESC);
-- 建立学生表索引：多个字段Id、Name索引倒序
CREATE UNIQUE INDEX INDEX_SId_SName ON Student (Id DESC,Name DESC);
```
# 删除索引
` DROP INDEX <索引名>;`
示例
```
-- 删除学生表索引 INDEX_SId
DROP INDEX INDEX_SId;
```
# 创建视图
```
CREATE VIEW <视图名>
  AS SELECT 查询子句
  [WITH CHECK OPTION]
```
> 查询子句：子查询可以是任何SELECT语句，但是常不允许含有ORDER BY子句和DISTINCT短语；
WITH CHECK OPTION：表示对UPDATE、INSERT、DELETE操作时要保证更新。

```
CREATE VIEW VIEW_Stu_Man
AS SELECT * FROM Student WHERE Gender = '男'
WITH CHECK OPTION
```
# 删除视图
` DROP VIEW <视图名>; `  
`DROP VIEW VIEW_Stu_Man;`

# SQL的访问控制
> 访问控制是控制用户的数据存储权限，由DBA来决定。
SQL标准语句包括SELECT、INSERT、UPDATE和DELETE
```
-- 1.授权
GRANT <权限>[,<权限>]…
  [ON <对象类型> <对象名>]
  TO <用户>[,<用户>]…
  [WITH GRANT OPTION]
-- 2.收回授权
REVOKE <权限>[,<权限>]…
  [ON <对象类型> <对象名>]
  FROM <用户>[,<用户>]…

```
> WITH GRANT OPTION：若指定此子句，表示该用户可以将权限赋给其他用户

```
-- 授权
GRANT SELECT,INSERT,UPDATE ON TABLE TO USER_Admin WITH GRANT OPTION
-- 收回授权
REVOKE SELECT,INSERT,UPDATE ON TABLE FROM USER_Admin
```