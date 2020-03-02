---
title: MySQL Stored Procedure
date: 2020-02-27 11:04:46
tags:
---

一月的时候重构了一个洗数据的代码，在 Jupyter 上用 [ipython-sql](https://github.com/catherinedevlin/ipython-sql) 写的 sql，数据库用的 MySQL 8。

理论上更高效的做法应该是直接读到 [dataframe](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) 里再进行数据清洗，由于之前已经用 sql 完整实现了一遍再加上时间不是很充裕选择了用 Stored Procedure 来封装之前的 sql

<!-- more -->

由于大量的使用了表名作为参数，因此用concat函数来拼接，基本的框架就是

```sql
DROP PROCEDURE IF EXISTS initTable
CREATE PROCEDURE initTable(IN tb1 VARCHAR(5), IN tb2 VARCHAR(5))
BEGIN
	SET @sqlstmt = CONCAT('SELECT * FROM', tb1, ' ', tb2, ' LIMIT 10');
	PREPARE stmt FROM @sqlstmt
	EXECUTE stmt
END
```

然后通过 call 调用

```sql
CALL initTable('tb', 'anothertb')
```

但是在同一个procedure里对一张表进行多次update的话则会出现 can't lock table 的报错。可以利用 transaction 解决。而transaction在mysql语法中有点特殊，不是`BEGIN TRAN/COMMIT TRAN `这种形式。

假设要update同一张表两次：

```sql
DROP PROCEDURE IF EXISTS updateTable
CREATE PROCEDURE updateTable(IN tb VARCHAR(5))
BEGIN
	START TRANSACTION
	SET @sqlstmt = CONCAT('update ', tb,' SET A = 1');
	PREPARE stmt FROM @sqlstmt
	EXECUTE stmt
	COMMIT
	
	START TRANSACTION
	SET @sqlstmt = CONCAT('update ', tb,' SET B = 2');
	PREPARE stmt FROM @sqlstmt
	EXECUTE stmt
	COMMIT
END
```

（一开始还以为mysql不支持transaction



