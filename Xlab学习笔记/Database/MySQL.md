我的上帝啊，这实际上是 MySQL 的语法，太狭隘了
# 数据类型
五个基本类型，下设具体类型
- 数值
    - 整数
    - 浮点
- 日期和时间
- 字符串
- JSON
- 空间
# 操作
连接本地 SQL
```
\connect root@localhost
```
显示所有数据库
```
show databases;
```
创建新数据库
```
create database name;
```
删除
```
drop database name;
```
进入数据库
```
use name;
```
## 库操作
### 表操作
下面用评论区做演示
#### 表的基本设置
创建表，表明其列
```
CREATE table comments (
    ID INT,
    Name TEXT(100),
    Comment TEXT(100)
);
```
- 可以在此设置默认值，设置是否接受空值
删除表
```
DROP TABLE table_name;
```
#### 描述表
```
DESC table_name;
```
显示一块一块的
#### 修改表
将 `column_name` 这一行的数据类型改成了 `INT`
```
ALTER TABLE table_name MODIFY COLUMN column_name INT;
```
改列名
```
ALTER TABLE tablename REANME COLUMN column_name to new_name;
```
添加列
```
ALTER TABLE tablename ADD COLUMN column_name INT;
```
删除列
```
ALTER TABLE tablename DROP COLUMN column_name;
```
设置默认值
```
ALTER TABLE comments MODIFY ID INT DEFAULT 114514;
```
- BLOB, TEXT, GEOMETRY or JSON column 不能设置默认值
### 数据操作
插入数据
```
INSERT INTO comments (ID, Name, Comment) VALUE (114514, 'WYT', 'Bushi');
```
- 如果值包括所有列，且顺序一致，可以省略列名
- 如果没有写出所有列名，会用默认值填充
- 逗号隔开插入多条数据
查询数据
```
SELECT * FROM comments;
```
更新数据
```
UPDATE comments set Comment = '不是不是' where Name = 'WYT';
```
删除数据
```
DELETE FROM comments where ID = 114514;
```
## 导出
```
mysqldump -u root -p comments > comments.sql;
```
- 为啥不行啊
# 语句
适用于 MySQL 的 SQL 语句（呃呃呃）

## 查询
SELECT 语句的一些用法
- 子查询：可以将一条 SELECT 为另一条 SELECT 或者其他语句所使用
    - 查询时，使用 `as` 为其列命名，仅有易读作用
### WHERE
相当于 if，条件之间可以用**NOT**，**AND**，**OR**，下有一些条件：
- BETWEEN `a` AND `b`
- IN `(1, 3, 5)`
- LIKE `'W_T'`，`%` 匹配任意字段，`_` 匹配任意字符
- REGEXP `W[YT]T`，正则
![[Pasted image 20240603194751.png|300]]
注意，null 值和任何值都不相等，包括 null 本身，应用 `is null` 或 `<=> null`

### 排序
`SELECT * FROM comments ORDER BY Name, ID;`
- 列名之后添加 `DESC` 降序
- 列名可以用列序号
- 可以用逗号隔开多个列
### 聚合
![[Pasted image 20240603195415.png|300]]
使用例：`SELECT AVG(ID) FROM comments;`
这会成为一列
### 分组及筛选
`SELECT sex, COUNT(*) FROM comments group by sex having ID > 114514;`
- 可以与聚合、排序等结合使用
### 限制
`LIMIT 5, 20`
从第五个开始，一共输出二十条结果，常用于实现分页
### DISTINCT
去重
### 集合
- 取两个 `SELECT` 语句，用 `UNION` 连接
    - 默认去重，除非使用 `UNION ALL`
- `INTERSECT` 取并集
- `EXCEPT` 取差集
## 表关联
不加条件的 `SELECT * FROM comments JOIN profile` 取笛卡尔积，产生非常多数据，且没有用
- `on`：添加条件，如 `on comments.ID = profile.ID` 保证了取 ID 相同的数据为一行
- `NATURAL JOIN`：合并同名列
- `INNER JOIN`：只返回两个表都有的数据
- `LEFT JOIN`：在 `INNER` 的基础上，返回左表有，右表没有的数据，且用 null 填充之
- `RIGHT JOIN`
