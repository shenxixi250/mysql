# mysql学习 

## 注释 的方法有3中
1. \#注释
2. --注释
3. `/** 多行注释 */`

## 创建表

CREATE DATABASE test(
		# int 类型, 不为空,自增
		id INT NOT NULL AUTO_INCREMENT,
		# int 类型, 不可为空,默认值为1,不为空
		col1 INT NOT NULL DEFULT 1,
		#变长字符串类型,最长为45个字节 ,可以为空
		col2 VARCHAR(45) NULL, 
		#日期类型,可以为空
		col3 DATE NULL,
		#设置主键ID
		PRIMARY KEY('id')
		)
## 修改表
添加列 
删除列
删除表

##插入 
普通插入
```sql
	INSERT INTO mytable(col1,col2)
	VALUE(val1,val2)
```

插入检索出来的数据
``sql
	INSERT INTO mytable(col1,col2)
	SELECT col1,col2
	FROM mytable2
```


更新 
```ql
	UPDATE mytable
	SET col = val
	WHERE id =1
```


删除
```sql
	DELECT FOROM mytable
	WHERE id= 1
```


查询 
```sql
	SELECT FROM 
	关于LIMIT
	SELECT *
	FROM mytable
	LIMIT 2,3  # 意思是从第二行开始  查三个元素
```

关于排序
```sql
	SERLECT *
	FROM mytable
	ORDER BY col1 DESC,col2 ASC
```

过滤

WHERE

| 操作符  | 说明         |
|---------|--------------|
| =       | 等于         |
| <       | 小于         |
| >       | 大于         |
| <> !=   | 不等于       |
| <= !>   | 小于等于     |
| >= !<   | 大于等于     |
| BETWEEN | 在两个值之间 |
| IS NULL | 为 NULL 值   |

应该注意到，NULL 与 0、空字符串都不同。
AND 和 OR 用于连接多个过滤条件。优先处理 AND，当一个过滤表达式涉及到多个 AND 和 OR 时，可以使用 () 来决定优先级，使得优先级关系更清晰。
IN 操作符用于匹配一组值，其后也可以接一个 SELECT 子句，从而匹配子查询得到的一组值。
NOT 操作符用于否定一个条件

通配符也是用在过滤语句中，但它只能用于文本字段。
+ % 匹配 >=0 个任意字符；
+ _ 匹配 ==1 个任意字符；
+ ` [ ]` 可以匹配集合内的字符，例如 [ab] 将匹配字符 a 或者 b。用脱字符  可以对其进行否定，也就是不匹配集合内的字符。

使用 Like 来进行通配符匹配。
```sql
	
	SELECT *
	FROM mytable
	WHERE col LIKE '[AB]%'; -- 不以 A 和 B 开头的任意文
```


### 计算字段

在数据库服务器上完成数据的转换和格式化的工作往往比客户端上快得多，并且转换和格式化后的数据量更少的话可以减少网络通信量。

计算字段通常需要使用 AS 来取别名，否则输出的时候字段名为计算表达式。
```sql
	
	SELECT col1 * col2 AS alias
	FROM mytable
```
`CONCAT()` 用于连接两个字段。许多数据库会使用空格把一个值填充为列宽，因此连接的结果会出现一些不必要的空格，使用 `TRIM()` 可以去除首尾空格。

```sql
	SELECT CONCAT(TRIM(col1), '(', TRIM(col2), ')') AS concat_col
	FROM mytable
```

###函数

各个 DBMS 的函数都是不相同的，因此不可移植，以下主要是 MySQL 的函数。
汇总
| 函 数   | 说 明            |
|---------|------------------|
| AVG()   | 返回某列的平均值 |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |
| SUM()   | 返回某列值之和   |

AVG() 会忽略 NULL 行。

使用 DISTINCT 可以汇总不同的值。

```sql
	SELECT AVG(DISTINCT col1) AS avg_col
	FROM mytable;
```
文本的处理
length()  返回长度

日期处理

 日期格式：YYYY-MM-DD
 时间格式：HH:MM:SS

| 函 数         | 说 明                          |
|---------------|--------------------------------|
| ADDDATE()     | 增加一个日期（天、周等）       |
| ADDTIME()     | 增加一个时间（时、分等）       |
| CURDATE()     | 返回当前日期                   |
| CURTIME()     | 返回当前时间                   |
| DATE()        | 返回日期时间的日期部分         |
| DATEDIFF()    | 计算两个日期之差               |
| DATE_ADD()    | 高度灵活的日期运算函数         |
| DATE_FORMAT() | 返回一个格式化的日期或时间串   |
| DAY()         | 返回一个日期的天数部分         |
| DAYOFWEEK()   | 对于一个日期，返回对应的星期几 |
| HOUR()        | 返回一个时间的小时部分         |
| MINUTE()      | 返回一个时间的分钟部分         |
| MONTH()       | 返回一个日期的月份部分         |
| NOW()         | 返回当前日期和时间             |
| SECOND()      | 返回一个时间的秒部分           |
| TIME()        | 返回一个日期时间的时间部分     |
| YEAR()        | 返回一个日期的年份部分         |

##分组
把具有相同的数据值的行放在同一组中。
可以对同一分组数据使用汇总函数进行处理，例如求分组数据的平均值等。
指定的分组字段除了能按该字段进行分组，也会自动按该字段进行排序。

```sql
	ELECT col, COUNT(*) AS num
	FROM mytable
	GROUP BY col
```


GROUP BY 自动按分组字段进行排序，ORDER BY 也可以按汇总字段来进行排序。

```sql
	SELECT col, COUNT(*) AS num
	FROM mytable
	GROUP BY col
	ORDER BY num
```
WHERE 过滤行，HAVING 过滤分组，行过滤应当先于分组过滤。

```sql
	SELECT col, COUNT(*) AS num
	FROM mytable
	WHERE col > 2
	GROUP BY col
	HAVING num >= 2
```

分组规定：

GROUP BY 子句出现在 WHERE 子句之后，ORDER BY 子句之前；
除了汇总字段外，SELECT 语句中的每一字段都必须在 GROUP BY 子句中给出；
NULL 的行会单独分为一组；
大多数 SQL 实现不支持 GROUP BY 列具有可变长度的数据类型。

子查询

子查询中只能返回一个字段的数据。

可以将子查询的结果作为 WHRER 语句的过滤条件：

```sql
	SELECT *
	FROM mytable1
	WHERE col1 IN (SELECT col2
			               FROM mytable2);
```

下面的语句可以检索出客户的订单数量，子查询语句会对第一个查询检索出的每个客户执行一次：
```sql
	
SELECT cust_name, (SELECT COUNT(*)
				FROM Orders
				WHERE Orders.cust_id = Customers.cust_id)
				AS orders_num
FROM Customers
ORDER BY cust_name
```
##连接
连接用于连接多个表，使用 JOIN 关键字，并且条件语句使用 ON 而不是 WHERE。
连接可以替换子查询，并且比子查询的效率一般会更快。
可以用 AS 给列名、计算字段和表名取别名，给表名取别名是为了简化 SQL 语句以及连接相同表。
###内连接
内连接又称等值连接，使用 INNER JOIN 关键字。

```sql
	
	SELECT A.value, B.value
	FROM tablea AS A INNER JOIN tableb AS B
	ON A.key = B.key
```

###自连接
自连接可以看成内连接的一种，只是连接的表是自身而已。
一张员工表，包含员工姓名和员工所属部门，要找出与 Jim 处在同一部门的所有员工姓名。
子查询版本
```sql
	
	SELECT name
	FROM employee
	WHERE department = (
			SELECT department
			FROM employee
			WHERE name = "Jim"
			);
```

自连接版本
```sql
	
	SELECT e1.name
	FROM employee AS e1 INNER JOIN employee AS e2
	ON e1.department = e2.department
	      AND e2.name = "Jim";
```

####自然连接
 自然连接是把同名列通过等值测试连接起来的，同名列可以有多个。
 内连接和自然连接的区别：内连接提供连接的列，而自然连接自动连接所有同名列。
 ```sql
 	
	SELECT A.value, B.value
	FROM tablea AS A NATURAL JOIN tableb AS B;
 ```

外连接
外连接保留了没有关联的那些行。分为左外连接，右外连接以及全外连接，左外连接就是保留左表没有关联的行。
检索所有顾客的订单信息，包括还没有订单信息的顾客。
```sql
	
	SELECT Customers.cust_id, Orders.order_num
	FROM Customers LEFT OUTER JOIN Orders
	ON Customers.cust_id = Orders.cust_id
```

