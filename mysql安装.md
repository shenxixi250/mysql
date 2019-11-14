# mysql的安装

## ubuntu上的安装 
## window上的安装


1. ubuntu上的安装是非常简单的   
```
dpkg -l | grep mysql 查看有没有安装过mysql
sudo apt update 更新索引包
sudo apt install mysql-server 直接安装
sudo service mysql start 启动数据库
sudo service mysql stop 停止数据库
sudo servect mysql restart 重启数据库
```
 安装后使用
 ```
 mysql -u root -p  使用root进行密码登录 遇到密码不需要输入直接回车 如果登录不上遇到Access denied for user 'root'@'localhost' (using password: YES)
 
 ```

[解决方法](#jjf) 
 ```
 systemctl status mysql 检查数据库状态  
 ```
```
● mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: en
   Active: active (running) since Thu 2019-10-17 13:45:06 CST; 40min ago
  Process: 4229 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/my
  Process: 4220 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exit
 Main PID: 4231 (mysqld)
    Tasks: 28 (limit: 4915)
   CGroup: /system.slice/mysql.service
           └─4231 /usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.pid

10月 17 13:45:06 shen-MS-7A33 systemd[1]: Starting MySQL Community Server...
10月 17 13:45:06 shen-MS-7A33 systemd[1]: Started MySQL Community Server.
lines 1-12/12 (END)

```
出现这些信息正常   特别是Active: active(running)

配置数据库可以远程访问 
1. 首先编辑 /etc/mysql/mysql.conf.d/mysqld.cnf 配置文件：vim /etc/mysql/mysql.conf.d/mysqld.cnf

2. 注释掉bind-address          = 127.0.0.1     就是在这个语句前面加上#号 保存退出

3. 然后进入mysql数据库，执行授权命令：

	+ mysql -u root -p

    + mysql> grant all on *.* to root@'%' identified by '你的密码' with grant option;

    + mysql> flush privileges;    # 刷新权限

    + mysql> exit

4. 重启服务器  sudo service mysql restart 

5. 在windows下可以使用Navicat图形化连接数据库 新建连接>>>>>>填写ip 端口默认3306 用户名root 密码 +++ 就可以了  

成功  


### 安装 Mysql-workbench 

下载[deb文件](https://dev.mysql.com/downloads/file/?id=490466) 
双击安装  
 

 





### 上面问题的解决方案
 <A NAME="jjf">解决方案</A> 

首先 要登录上数据库 这就要用到数据库给我们提供的用户名和密码了
```
sudo gedit /etc/sql/debian.cnf  
```


这里有sql给我们提供的账户名和密码 
```
# Automatically generated for Debian scripts. DO NOT TOUCH!
[client]
host     = localhost
user     = debian-sys-maint
password = 4hRFSEpQ91xCsoRp
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = 4hRFSEpQ91xCsoRp
socket   = /var/run/mysqld/mysqld.sock
```

这中间的password就是我们要的密码复制下来
```
mysql -udebian-sys-maint -p     使用这个mysql提供的账户登录  之后密码输入就是用你复制的password就可以了

```
回车进入mysql的界面就可以了  


在这里我们要修改mysql中自带的数据库mysql中的user数据表
```
use mysql; 告诉mysql我要使用的数据库

update user set authentication_string=password("shen") where User='root'; 修改密码为shen  
在7.5版本之前密码那一栏是password 之后就是authentication_string了

update user set plugin='msyql_native_password' where user='root';
修改标签为msyql_native_password

```

| user | plugin | authentication_string |
|------|--------|-----------------------|
| root             | mysql_native_password | *E37691F1564D9A806A95761BF87169FB67E42802 |
| mysql.session    | mysql_native_password | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| mysql.sys        | mysql_native_password | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| debian-sys-maint | mysql_native_password | *484D57FA1A2E06455771D6ED733EC0B32AD36E03 |


修改之后的数据库应该是这样   之前root那一栏的plugin对应的是auth_socket  查看已经被修改了

之后
```
quit; 退出服务器
sudo service mysql start 
mysql -u root -q    然后输入密码成功登录 液
```






