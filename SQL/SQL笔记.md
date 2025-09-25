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

### SELECT

```sql
-- 检索单个列,直接列出所需要的列名
SELECT name FROM Actors;

-- 检索多个列,使用逗号隔开
SELECT name,id FROM Actors;

-- 检索所有列  使用* 表示返回所有列
SELECT * FROM Actors;
```

#### DISTINCT

```sql

-- 检索玩家的服务器id(重复也会被检索)
SELECT serverId FROM Actors;

-- 检索玩家的服务器id(重复的只会显示一次)
SELECT DISTINCT serverId FROM Actors;

-- 有多列的时候,DISTINCT作用于所有列(也就是说满足serverId或者groupId不一样的就会被列出来)
SELECT DISTINCT serverId,groupId FROM Actors;

```
#### LIMIT

```sql

-- 显示全部数据
SELECT name FROM Actors;

-- 显示前五条数据

-- 1. SQL Server和Access 中使用TOP关键字
SELECT TOP 5 name FROM Actors;

-- 2. DB2 使用FETCH FIRST 5 ROWS ONLY
SELECT name FROM Actors FETCH FIRST 5 ROWS ONLY;

-- 3. Oracle 使用ROWNUM
SELECT name FROM Actors WHERE ROWNUM <= 5;

-- 4. MySQL, MariaDB, PostgreSQL, SQLite 使用LIMIT
SELECT name FROM Actors LIMIT 5;

-- 不同DBMS支持的语法可能会有些差异,不过大部分基础语法都是支持的.
```
#### ORDER BY和DESC

```sql

-- 无序
SELECT id,name,level FROM Actors;

-- 按照等级升序排列
SELECT id,name,level FROM Actors ORDER BY level;

-- 按照服务器id升序排列(ORDER BY不一定要用检索的字段排序)
SELECT id,name,level FROM Actors ORDER BY serverId;

-- 按照等级降序排列,这里使用 DESC 关键字,如果有多个,那么每一列都要单独指定
-- 这里使用DESC或者是DESCENDING都是一样的
-- 升序排列对应ASC或者ASCENDING,不指定排序方式默认就使用ASC
SELECT id,name,level FROM Actors ORDER BY level DESC;
```

#### WHERE子句

```sql
-- 使用WHERE子句筛选出符合要求的记录
SELECT id,name,level FROM Actors WHERE level > 100;

-- 如果有ORDER BY子句,放在WHERE子句后面
SELECT id,name,level FROM Actors WHERE level > 100 ORDER BY id;

```
| 操作符  | 说明                    |
| ------- | ----------------------- |
| =       | 等于                    |
| <>      | 不等于                  |
| !=      | 不等于                  |
| <       | 小于                    |
| <=      | 小于等于                |
| !<      | 不小于                  |
| >       | 大于                    |
| >=      | 大于等于                |
| !>      | 不大于                  |
| BETWEEN | 在指定的两个值之间(AND) |
| IS NULL | 为NULL值                |

> 以上操作符并非所有BDMS都支持,具体参照对应文档

##### AND 和 OR

> AND和OR用于连接两个条件,其中AND需要同时满足AND左右的条件,OR只需左右条件满足其一即可(也就是左边条件满足就不计算右边条件)
>
> 如果有多个条件,那么需要用AND和OR将它们一一连接起来.
>
> 组合AND和OR使用时,建议用圆括号进行分组,不要过分依赖默认求值顺序(优先级AND>OR)

```sql
-- 删选出等级在(100,200)和(300,400)区间的玩家
SELECT id,name,level FROM Actors WHERE (level > 100 and level <= 200) or (level > 300 and level < 400);
```

##### IN 和 NOT

> IN 用于指定要匹配值清单,功能和OR相当,一般比一组OR操作符执行的更快

```sql
-- 筛选9990,9993,9994 服的玩家
SELECT id,name,level FROM Actors WHERE serverid IN(9990,9993,9994);

-- 等价下面这句
SELECT id,name,level FROM Actors WHERE serverid = 9990 OR serverid = 9993 OR serverid = 9994;
```

> NOT用来否定其后条件

```sql
SELECT id,name,level FROM Actors WHERE NOT level > 100;

-- 筛选不在9990,9993,9994 服的玩家
SELECT id,name,level FROM Actors WHERE serverid NOT IN(9990,9993,9994);

```

##### LIKE

| 通配符 | 作用                           | 说明                                            |
| ------ | ------------------------------ | ----------------------------------------------- |
| %      | 匹配任意长度的字符串(包括空串) | 'A%Z'可以匹配'AZ','ABZ','ABBZ',                 |
| _      | 匹配任意一个字符               | 'A_Z'可以匹配'ABZ','ACZ'但是不能匹配'AZ','ABBZ' |
|        |                                |                                                 |
|        |                                |                                                 |
|        |                                |                                                 |
|        |                                |                                                 |
|        |                                |                                                 |
|        |                                |                                                 |
|        |                                |                                                 |
|        |                                |                                                 |

