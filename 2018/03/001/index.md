# Mysql学习笔记


## 安装 MySql 服务

- mac

```shell
brew install mysql
```

- linux

```shell
sudo apt-get insatll mysql-server
sudo apt-get install mysql-client

chsh -s `which zsh`
```

---

## 修改密码

```sql

sudo mysqld_safe --skin-grant-tables --skip-networking &

> use mysql
> update user set authentication_string=password('新密码'),
plugin='mysql_native_password' where user = 'root'
> flush privileges
> quit
```

---

## 启动 MySql 服务

- mac

```shell
# 启动mysql服务
mysql.server start

# 关闭mysql服务
mysql.server stop

#连接mysql数据库
sudo mysql -h host(主机地址) -u user(用户名)  -p
```

- linux

```shell
# 启动mysql服务
sudo service mysql start

# 关闭mysql服务
sudo service mysql start

#连接mysql数据库
sudo mysql -u root(用户名) -p password(密码)
```

---

## SQL 命令

- 查看数据库

```sql
SHOW DATABASES;
```

![此处输入图片的描述][1]

- 删除数据库

```sql
DROP DATABASE <数据库名>;
```

- 使用数据库

```sql
use <数据库名字>
```

![此处输入图片的描述][2]

- 查看表

```sql
show tables;
```

![此处输入图片的描述][3]

- 删除表

```sql
DROP TABLE 表名字;
```

- 退出 mysql

```sql
quit 或 exit
```

- 新建数据库

```sql
CREATE DATABASE <数据库名字>;
```

![此处输入图片的描述][4]

- 新建表

```sql
CREATE TABLE <表的名字>(
    列名a 数据类型(数据长度),
    列名b 数据类型(数据长度)，
    列名c 数据类型(数据长度)
);
```

![此处输入图片的描述][5]

- 插入表数据

```sql
INSERT INTO <表名>(列名a,列名b,列名c) VALUES(值a,值b,值c);

INSERT INTO <表名> VALUES(值a,值b,值c);

INSERT INTO <表名>(列名a,列名c) VALUES(值a,值c);
```

---

## SQL 约束

| 主键        | 默认值  | 唯一   | 外键        | 非空     |
| ----------- | ------- | ------ | ----------- | -------- |
| PRIMARY KEY | DEFAULT | UNIQUE | FOREIGN KEY | NOT NULL |

[------------> SQL 约束][6]

- 导入外部 sql 文件

```shell
source <sql路径>
```

![此处输入图片的描述][7]

---

## SELECT 语句

- 基本查询语句

```sql
SELECT <列名> FROM <表名> WHERE 限制条件;
```

- ADN 和 OR

```sql
SELECT * FROM <表名> WHERE age>25 <and|or> are <30;
```

- IN 和 NOT IN

```sql
SELECT * FROM <表名> WHERE <列名> IN ('Tom','Jack');
```

- 通配符

```sql
# 其中 _ 代表一个未指定字符，% 代表不定个未指定.
SELECT * FROM <表名> WHERE <列名> LIKE '1101__';
SELECT * FROM <表名> WHERE <列名> LIKE 'J%';
```

- 对结果排序

```sql
# ASC 升序
SELECT * FROM <表名> ORDER BY <列名> ASC;

# DESC 降序
SELECT * FROM <表名> ORDER BY <列名> DESC;
```

- SQL 内置函数和计算

| 函数名 | COUNT | SUM  | AVG      | MAX    | MIN    |
| ------ | ----- | ---- | -------- | ------ | ------ |
| 作用   | 计数  | 求和 | 求平均值 | 最大值 | 最小值 |

```sql
# AS关键词可以给值重命名
SELECT MAX(salary) AS max_salary,MIN(salary) FROM <表名>;
```

- 子查询

```sql
SELECT COUNT(proj_name) AS count_project FROM project
WHERE of_dpt IN
(SELECT in_dpt FROM employee WHERE name='Tom');
```

![此处输入图片的描述][8]

- 连接查询

```sql
SELECT id,name,people_num
FROM employee,department
WHERE employee.in_dpt = department.dpt_name
ORDER BY id;

-----------------------------------------------------

SELECT id,name,people_num
FROM employee JOIN department
ON employee.in_dpt = department.dpt_name
ORDER BY id;
```

![此处输入图片的描述][9]

- 重命名表

```sql
RENAME TABLE 原名 TO 新名字;

ALTER TABLE 原名 RENAME 新名;

ALTER TABLE 原名 RENAME TO 新名;
```

![此处输入图片的描述][10]

## 数据库及表的修改和删除

- 增加一列

```sql
ALTER TABLE 表名字 ADD COLUMN 列名字 数据类型 约束;
或： ALTER TABLE 表名字 ADD 列名字 数据类型 约束;
```

![此处输入图片的描述][11]

- 指定添加位置

```sql
ALTER TABLE 表名字 ADD 列名字 数据类型 约束 AFTER <列名>
```

![此处输入图片的描述][12]

- 放在首行

```sql
ALTER TABLE 表名字 ADD 列名字 数据类型 约束 FIRST
```

![此处输入图片的描述][13]

- 删除一列

```sql
ALTER TABLE 表名字 DROP COLUMN 列名字;
或： ALTER TABLE 表名字 DROP 列名字;
```

![此处输入图片的描述][14]

- 重命名一列

```sql
ALTER TABLE 表名字 CHANGE 原列名 新列名 数据类型 约束;
```

![此处输入图片的描述][15]

- 改变数据类型

```sql
ALTER TABLE 表名字 MODIFY 列名字 新数据类型;
```

---

## 修改表的内容

- 修改表中的某个值

```sql
UPDATE 表名字 SET 列1=值1,列2=值2 WHERE 条件;
```

![此处输入图片的描述][16]

- 删除某一行记录

```sql
DELETE FROM 表名字 WHERE 条件;
```

![此处输入图片的描述][17]

## 索引

- 建立索引

```sql
ALTER TABLE 表名字 ADD INDEX 索引名 (列名);
CREATE INDEX 索引名 ON 表名字 (列名);
```

- 查看索引

```sql
show index from employee;
```

## 视图

- 建立视图

```sql
CREATE VIEW 视图名(列a,列b,列c) AS SELECT 列1,列2,列3 FROM 表名字;
```

- 查看视图

```sql
SELECT * FROM <视图名>
```

![此处输入图片的描述][18]

## 导入

```sql
LOAD DATA INFILE '文件路径和文件名' INTO TABLE 表名字;
```

![此处输入图片的描述][19]

## 导出

```sql
SELECT 列1，列2 INTO OUTFILE '文件路径和文件名' FROM 表名字;

SELECT * INTO OUTFILE '/tmp/out.txt' FROM employee;
```

## 备份

```sql
mysqldump -u root 数据库名>备份文件名;   #备份整个数据库
mysqldump -u root 数据库名 表名字>备份文件名;  #备份整个表
```

## 恢复

```sql
source <SQL路径>

mysql -u root 新数据库名 < bak.sql
```

[1]: https://dn-anything-about-doc.qbox.me/MySQL/sql-01-04.png/logoblackfont
[2]: https://dn-anything-about-doc.qbox.me/MySQL/sql-01-05.png/logoblackfont
[3]: https://dn-anything-about-doc.qbox.me/MySQL/sql-01-06.png/logoblackfont
[4]: https://dn-anything-about-doc.qbox.me/MySQL/sql-02-01.png/logoblackfont
[5]: https://dn-anything-about-doc.qbox.me/MySQL/sql-02-04.png/logoblackfont
[6]: https://www.shiyanlou.com/courses/running
[7]: https://s13.postimg.org/97e98ezgn/Jietu20180115-113737_2x.jpg
[8]: https://dn-anything-about-doc.qbox.me/MySQL/sql-04-13.png/logoblackfont
[9]: https://dn-anything-about-doc.qbox.me/MySQL/sql-04-14.png/logoblackfont
[10]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-03.png/logoblackfont
[11]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-05.png/logoblackfont
[12]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-06.png/logoblackfont
[13]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-07.png/logoblackfont
[14]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-08.png/logoblackfont
[15]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-09.png/logoblackfont
[16]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-10.png/logoblackfont
[17]: https://dn-anything-about-doc.qbox.me/MySQL/sql-05-11.png/logoblackfont
[18]: https://dn-anything-about-doc.qbox.me/MySQL/sql-06-02.png/logoblackfont
[19]: https://dn-anything-about-doc.qbox.me/MySQL/sql-06-05.png/logoblackfont
