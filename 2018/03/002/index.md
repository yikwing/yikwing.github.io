# MySql服务安装及命令使用


## 安装 MySql 服务

- mac

```shell
brew install mysql
```

- linux

```shell
sudo apt-get insatll mysql-server
sudo apt-get install mysql-client
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

## mysql 设置远程访问

1. sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
2. 注释掉 '#bind-address = 127.0.0.1'
3. 进入 mysql
4. GRANT ALL PRIVILEGES ON _._ TO 'username'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
5. flush PRIVILEGES
6. 退出并重启 mysql

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

## ssh 远程连接

1. 虚拟机使用桥接模式
2. sudo apt-get install ssh
3. ssh username@host

---

## 输入查询

```sql
SELECT VERSION(),CURRENT_DATE;
```

![此处输入图片的描述][1]

```sql
SELECT SIN(PI()/4),(4+1)*5;

# 如果你决定不想执行正在输入过程中的一个命令，输入`\c`取消它：
```

![此处输入图片的描述][2]

## 创建表

```sql
mysql> CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20),
    -> species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
```

## 显示数据表

```shell
mysql> DESCRIBE pet;
```

![此处输入图片的描述][3]

---

## 过滤数据表(distinct)

```sql
select distinct owner from pet;
```

[1]: https://dn-anything-about-doc.qbox.me/document-uid73259labid1238timestamp1438158105657.png?watermark/1/image/aHR0cDovL3N5bC1zdGF0aWMucWluaXVkbi5jb20vaW1nL3dhdGVybWFyay5wbmc=/dissolve/60/gravity/SouthEast/dx/0/dy/10
[2]: https://dn-anything-about-doc.qbox.me/document-uid73259labid1238timestamp1438158281502.png?watermark/1/image/aHR0cDovL3N5bC1zdGF0aWMucWluaXVkbi5jb20vaW1nL3dhdGVybWFyay5wbmc=/dissolve/60/gravity/SouthEast/dx/0/dy/10
[3]: https://dn-anything-about-doc.qbox.me/document-uid73259labid1239timestamp1438163082069.png?watermark/1/image/aHR0cDovL3N5bC1zdGF0aWMucWluaXVkbi5jb20vaW1nL3dhdGVybWFyay5wbmc=/dissolve/60/gravity/SouthEast/dx/0/dy/10
