# mysql学习 

## 注释 的方法有3中
1. #注释
2. --注释
3. /**** 多行注释 */

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
INSERT INTO mytable(col1,col2)
VALUE(val1,val2);
插入检索出来的数据
INSERT INTO mytable(col1,col2)
SELECT col1,col2
FROM mytable2;

更新 
UPDATE mytable
SET col = val
WHERE id =1;

删除
DELECT FOROM mytable
WHERE id= 1;

查询 
SELECT FROM 
关于LIMIT
SELECT *
FROM mytable
LIMIT 2,3  # 意思是从第二行开始  查三个元素	
关于排序
SERLECT *
FROM mytable
ORDER BY col1 DESC,col2 ASC;
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

