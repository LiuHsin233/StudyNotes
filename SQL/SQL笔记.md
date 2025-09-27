- [1. 数据库](#1-数据库)
  - [1.1. 基础术语](#11-基础术语)
  - [1.2. SQL语句](#12-sql语句)
    - [1.2.1. SELECT](#121-select)
      - [1.2.1.1. DISTINCT](#1211-distinct)
      - [1.2.1.2. LIMIT](#1212-limit)
      - [1.2.1.3. ORDER BY和DESC](#1213-order-by和desc)
      - [1.2.1.4. WHERE子句](#1214-where子句)
        - [1.2.1.4.1. AND 和 OR](#12141-and-和-or)
        - [1.2.1.4.2. IN 和 NOT](#12142-in-和-not)
        - [1.2.1.4.3. LIKE](#12143-like)
        - [AS 和 算术运算](#as-和-算术运算)
        - [GROUP BY子句 和 HAVING子句](#group-by子句-和-having子句)
  - [1.3 函数](#13-函数)
    - [文本处理函数](#文本处理函数)
    - [日期和时间处理函数](#日期和时间处理函数)
    - [数值处理函数](#数值处理函数)
    - [聚集函数](#聚集函数)

# 1. 数据库
## 1.1. 基础术语
数据库(database)  保存有组织数据的容器
数据库管理系统(DBMS) 操作数据库的软件
表(table) 某种特定类型数据的结构化清单
模式(schema) 关于数据库和表的布局以及特性的信息
列(column) 表中的一个字段
数据类型(datatype) 定义列可以存储哪些数据种类
行(row) 表中的一个记录(record)
主键(primary key) 能够唯一标识表中每一行的一列(或者一组列)
SQL	结构化查询语言,用于与数据库沟通的语言

## 1.2. SQL语句
> SQL关键字不区分大小写,写一行和分成多行也没有区别

### 1.2.1. SELECT

```sql
-- 检索单个列,直接列出所需要的列名
SELECT name FROM Actors;

-- 检索多个列,使用逗号隔开
SELECT name,id FROM Actors;

-- 检索所有列  使用* 表示返回所有列
SELECT * FROM Actors;
```

#### 1.2.1.1. DISTINCT

```sql

-- 检索玩家的服务器id(重复也会被检索)
SELECT serverId FROM Actors;

-- 检索玩家的服务器id(重复的只会显示一次)
SELECT DISTINCT serverId FROM Actors;

-- 有多列的时候,DISTINCT作用于所有列(也就是说满足serverId或者groupId不一样的就会被列出来)
SELECT DISTINCT serverId,groupId FROM Actors;

```
#### 1.2.1.2. LIMIT

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
#### 1.2.1.3. ORDER BY和DESC

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

#### 1.2.1.4. WHERE子句

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

##### 1.2.1.4.1. AND 和 OR

> AND和OR用于连接两个条件,其中AND需要同时满足AND左右的条件,OR只需左右条件满足其一即可(也就是左边条件满足就不计算右边条件)
>
> 如果有多个条件,那么需要用AND和OR将它们一一连接起来.
>
> 组合AND和OR使用时,建议用圆括号进行分组,不要过分依赖默认求值顺序(优先级AND>OR)

```sql
-- 删选出等级在(100,200)和(300,400)区间的玩家
SELECT id,name,level FROM Actors WHERE (level > 100 and level <= 200) or (level > 300 and level < 400);
```

##### 1.2.1.4.2. IN 和 NOT

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

##### 1.2.1.4.3. LIKE

| 通配符 | 作用                           | 说明                                                                                      |
| ------ | ------------------------------ | ----------------------------------------------------------------------------------------- |
| %      | 匹配任意长度的字符串(包括空串) | 'A%Z'可以匹配'AZ','ABZ','ABBZ',                                                           |
| _      | 匹配任意一个字符               | 'A_Z'可以匹配'ABZ','ACZ'但是不能匹配'AZ','ABBZ'                                           |
| []     | 匹配第一个字符                 | '[JM]%'匹配以J或者M开头的字符串,[JM]不带%就只能匹配单个的J或者M;使用^来否定中括号中的内容 |

```sql
-- 查找名字为Jxxk的所有玩家信息
SELECT id,name,level FROM Actors WHERE name like 'J%k';


-- 查找名字不以JM开头的所有玩家信息
SELECT id,name,level FROM Actors WHERE name like '[^JM]%';
```

##### AS 和 算术运算

> AS 用于起别名,比如可以给一个新的计算列(本身不存在的列)起别名

```sql
-- 计算订单价格
 SELECT prod_id,
 quantity,
 item_price,
 quantity*item_price AS expanded_price
 FROM OrderItems
 WHERE order_num = 20008;
 
 --[[
     这里prod_id,quantity,item_price都是表中的列
     quantity*item_price为新的计算列,或者说导出列,给它起了一个别名叫expanded_price
 ]]
 
```
##### GROUP BY子句 和 HAVING子句
> GROUP BY 用于分组
```sql
-- 按照供货商id进行分组并且计算每组的商品数量
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
GROUP BY vend_id;
```

## 1.3 函数
### 文本处理函数
| 函数    | 说明                 |
| ------- | -------------------- |
| LEFT()  | 返回字符串左边的字符 |
| RIGHT() | 返回字符串右边的字符 |
| LEN()   | 返回字符串的长度     |
| LOWER() | 将字符串转换成小写   |
| UPPER() | 将字符串转换成大写   |
| LTRIM() | 去掉字符串左边的空格 |
| RTRIM() | 去掉字符串右边的空格 |

### 日期和时间处理函数
> 移植性很差,不同的DBMS有不同的处理方式

### 数值处理函数
| 函数   | 说明               |
| ------ | ------------------ |
| ABS()  | 返回一个数的绝对值 |
| EXP()  | 返回一个数的指数值 |
| SQRT() | 返回一个数的平方根 |
| SIN()  | 返回一个数的正弦值 |
| COS()  | 返回一个数的余弦值 |
| TAN()  | 返回一个数的正切值 |
| PI()   | 返回圆周率         |

### 聚集函数
| 函数    | 说明             |
| ------- | ---------------- |
| AVG()   | 返回某列的平均值 |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |
| SUM()   | 返回某列之和     |

```sql

-- 返回特定供应商商品的平均价格
-- AVG只能用于单个列,如果想计算多个列的平均值,那么使用多个AVG函数
-- AVG会忽略值为NULL的行
SELECT AVG(prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';

-- 返回有电子右键地址的客户数量
-- COUNT会对特定列中具有值的行进行计数,忽略NULL值
-- 如果是想计算表中的行数,可以使用COUNT(*)
SELECT COUNT(cust_email) AS num_cust
FROM Customers;

-- 返回最便宜的商品价格
-- MAX和MIN都会忽略列值为NULL的行
SELECT MIN(prod_price) AS min_price
FROM Products;

-- 计算商品总价格
-- SUM也会忽略列值为NULL的行
-- 可以使用运算符在多个列上进行计算
SELECT SUM(item_price*quantity) AS
total_price
FROM OrderItems
WHERE order_num = 20005;

-- 如果是需要在计算之前去重,那么使用DISTINCT
-- 下面在计算平均价格的时候会去除重复的价格
-- DISTINCT可以搭配AVG,COUNT,SUM(MIN和MAX也可以用但是无意义)
-- DISTINCT 只能搭配单列,不能用于表达式,比如上面的SUM(item_price*quantity)
-- DISTINCT 不能搭配COUNT(*)
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';

-- 聚集函数可以任意组合,其中为了区分以及使用方便,最好使用别名
-- 别名不建议使用表中的已存在的列名
SELECT  COUNT(*) AS num_items,
        MIN(prod_price) AS price_min,
        MAX(prod_price) AS price_max,
        AVG(prod_price) AS price_avg
FROM Products;  

```