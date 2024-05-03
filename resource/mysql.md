# 理论



# 工程

## Language
先找词汇(value)，在找句法(expression syntax)，表达式(statement)，在找工程实践

### Expression syntax
grammer rules that expressions must follow in MYSQL and provides additional information about the types of  terms that may appear in expressions:
```
expr:
    expr OR expr
  | expr || expr
  | expr XOR expr
  | expr AND expr
  | expr && expr
  | NOT expr
  | ! expr
  | boolean_primary IS [NOT] {TRUE | FALSE | UNKNOWN}
  | boolean_primary

boolean_primary:
    boolean_primary IS [NOT] NULL
  | boolean_primary <=> predicate
  | boolean_primary comparison_operator predicate
  | boolean_primary comparison_operator {ALL | ANY} (subquery)
  | predicate

comparison_operator: = | >= | > | <= | < | <> | !=

predicate:
    bit_expr [NOT] IN (subquery)
  | bit_expr [NOT] IN (expr [, expr] ...)
  | bit_expr [NOT] BETWEEN bit_expr AND predicate
  | bit_expr SOUNDS LIKE bit_expr
  | bit_expr [NOT] LIKE simple_expr [ESCAPE simple_expr]
  | bit_expr [NOT] REGEXP bit_expr
  | bit_expr

bit_expr:
    bit_expr | bit_expr
  | bit_expr & bit_expr
  | bit_expr << bit_expr
  | bit_expr >> bit_expr
  | bit_expr + bit_expr
  | bit_expr - bit_expr
  | bit_expr * bit_expr
  | bit_expr / bit_expr
```

### Expression Term

#### Literal Values
string 、 Numeric 、 Data and Time Literal 、 Hexadecimal 、 Bit-Value 、 Boolean 、 NULL

#### identifiers

Certain objects within MySQL, including database, table, index, column, alias, view, stored procedure, partition, tablespace, resource group and other object names are known as identifiers.

##### identifier type
Database 、 Table 、 Column 、 Index 、 Constraint 、 Stored program 、 View 、 Tablespace 、 Server 、 Log File Group 、 Alias 、 Compound Statement 、 User-Defined Variable、Resource Group

##### identifier Qualifiers
相当于给identifier添加上下文，qualified就是给出上下文的，unqualified是没有给出上下文的\
例：\
CREATE TABLE t1(i INT)；这种就是没给出上下文的，会创建在default的database中\
CREATE TABLE db1.t1 (i INT)，这种是给出上下文的\
注意点：\
'my-table'.'my-column'，not 'my-table.my-column'\
a reserved word that follows a period in a qualified name must be an identifier, so in that context it need not be quoted\
a trigger is associated with a table, so any qualifier applies to the table name

##### identifier case sensitivity
table、trigger都对应系统中的某个文件，在windows系统上，不区分大小写，在unix系统上，区分大小写，这就造成在windows系统上能执行的查询操作不一定能在unix系统上执行。\
解决方法1，初始化mysql时，在配置文件/etc/my.cnf中加一个lower_case_table_names=1的字段，强制名称转换为小写\
解决方案2：配置unix系统lower_case_table_names=0，windows系统lower_case_table_names=2，保留大小写，这样做在windows执行查询操作时需要区分大小写

#### keywords and reserved words
关键字如果要用作mysql语句，需要quote，将其声明为identifier，例：
```
mysql> CREATE TABLE interval (begin INT, end INT);
ERROR 1064 (42000): You have an error in your SQL syntax ...
near 'interval (begin INT, end INT)'
```

```
mysql> CREATE TABLE `interval` (begin INT, end INT);
Query OK, 0 rows affected (0.01 sec)
```
```
https://dev.mysql.com/doc/refman/8.3/en/keywords.html
```

#### variabe 
```
SHOW VARIABLES  : display system variable names and values
```
#### Comment
```
mysql> SELECT 1+1;     # This comment continues to the end of line
mysql> SELECT 1+1;     -- This comment continues to the end of line
mysql> SELECT 1 /* this is an in-line comment */ + 1;
mysql> SELECT 1+
/*
this is a
multiple-line comment
*/
1;
```

### statement
#### Data Definition Statement:
```
ALTER CREATE DROP RENAME TRUNCATE

DATABASE
EVENT
FUNCTION
INDEX
LOGFILE
PROCEDURE
SERVER
SPATIAL REFERENCE SYSTEM
TABLE
TABLESPACE
TRIGGER
VIEW
```

#### Data Manipulate Statement
```
CALL
DELETE
DO
EXC
HANDLER
IMPORT TABLE
INSERT
INTERSECT Clause
LOAD DATA
LOAD XML
Parenthesized Query Expressions
REPLACE
SELECT
Set Operations with UNION, INTERSECT, and EXCEPT
Subqueries
TABLE
UPDATE
UNION Clause
VALUES
WITH (Common Table Expressions)
```


```
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
    [HIGH_PRIORITY]
    [STRAIGHT_JOIN]
    [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
    [SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr] ...
    [into_option]
    [FROM table_references
      [PARTITION partition_list]]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}, ... [WITH ROLLUP]]
    [HAVING where_condition]
    [WINDOW window_name AS (window_spec)
        [, window_name AS (window_spec)] ...]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
    [into_option]
    [FOR {UPDATE | SHARE}
        [OF tbl_name [, tbl_name] ...]
        [NOWAIT | SKIP LOCKED]
      | LOCK IN SHARE MODE]
    [into_option]

into_option: {
    INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
  | INTO DUMPFILE 'file_name'
  | INTO var_name [, var_name] ...
}


group by:
where: you can use any of the functions and operators that MYSQL supports, expect for aggregate functions.
order by:
from:
```

```
example:

select * from table_name
where xxx between 10 and 100
```
#### Transactional and Locking Statement
```
START TRANSACTION、COMMIT、ROLLBACK
SET TRANSACTION
```
#### Replication Statement



# 业务

## 压力测试
### mysqlslap
### sysbench


# 常规使用

##
```
create user remote identified by 'abc123';
Grant all privileges on *.* to 'remote'@'%';
show grants for 'remote';

CREATE USER 'remote'@'%' IDENTIFIED BY 'abc123';
GRANT all privileges ON *.* TO 'remote'@'%' identified by 'abc123';

FLUSH PRIVILEGES;
UPDATE mysql.user SET plugin='mysql_native_password' WHERE User='root';
ALTER USER 'root'@'localhost' IDENTIFIED BY '';

mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'PASSWORD';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;(没有权限的时候)
mysql> FLUSH PRIVILEGES;

```

## sqlite
pacman -Ss sqlite

operate:\
sqlite3 testDB.db   (create a new database)

### 基本构成
Table ： row column\
select # from table

### start
.help\
.show

### database
.databases   list all the database \
.quit\

.dump  | sqlite3 testDB.db .dump > testDB.sql\

### table
create TABLE table_name()\
drop TABLE table_name\
insert into table_name values()\
alter TABLE table_name ADD column xxx TEXT\

delete from table_name\
    where condition\
update table_name\
    set column1 = value1, column2 = value2\
    where [condition];\
    
.tables    list all the tables\
.schema    list all info about tables\


# ref
[mysql 8.3 reference manual](https://dev.mysql.com/doc/refman/8.3/en/identifiers.html)

[SQLite 附加数据库 | 菜鸟教程](https://www.runoob.com/sqlite/sqlite-attach-database.html)\
/Entered on/ [2022-11-26 周六 16:04]

[千锋linux dba](https://www.bilibili.com/video/BV1Nb411e7YF?from=search&seid=12646141100636457968&spm_id_from=333.337.0.0)\
/Entered on/ [2022-02-07 一 13:53]\

[自学SQL网(教程 视频 练习全套)](http://xuesql.cn/)\
/Entered on/ [2022-06-01 三 12:57]\


[sqlzoo](https://sqlzoo.net/wiki/SQL_Tutorial)\
[自学sql网 answer](https://blog.csdn.net/LongYuTianTang/article/details/109443181)\



# Archive
centos7\
download and install mysql
