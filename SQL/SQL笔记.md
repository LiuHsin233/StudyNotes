[toc]
# 数据库
## 基础术语
数据库(database)  保存有组织数据的容器
数据库管理系统(DBMS) 操作数据库的软件
表(table) 某种特定类型数据的结构化清单
模式(schema) 关于数据库和表的布局以及特性的信息
列(column) 表中的一个字段
数据类型(datatype) 定义列可以存储哪些数据种类
行(row) 表中的一个记录(record)
主键(primary key) 能够唯一标识表中每一行的一列(或者一组列)
SQL	结构化查询语言,用于与数据库沟通的语言

## SQL语句
> SQL关键字不区分大小写,写一行和分成多行也没有区别

1. SELECT
```sql
-- 检索单个列,直接列出所需要的列名
SELECT name FROM Actors;

-- 检索多个列,使用逗号隔开
SELECT name,id FROM Actors;

-- 检索所有列  使用* 表示返回所有列
SELECT * FROM Actors;
```