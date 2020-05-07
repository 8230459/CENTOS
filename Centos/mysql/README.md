### 安装和启动
```
wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
rpm -ivh mysql57-community-release-el7-9.noarch.rpm
yum install mysql-server
systemctl start mysqld
```
### 重置密码和远程登录
```
vi /etc/my.cnf
```
### 末尾加入
```
skip-grant-tables
```
### 重置密码
```
mysql -uroot -p
use mysql;
flush privileges;
show variables like 'validate_password%';
set global validate_password_policy=low;
set global validate_password_length=6;
select user,host from user;
alter user 'root'@'localhost' identified with mysql_native_password by '123456';
select host,user from user;
```
### 开启远程访问
```
update user set host = '%' where user = 'root';
exit
vi /etc/my.cnf
```
> 末尾注释
```
skip-grant-tables
```
> 加入
```
log-bin=mysql-bin   //[必须]启用二进制日志
server-id=140       //[必须]服务器唯一ID，默认是1，一般取IP最后一段
```
### 重启服务
```
systemctl restart mysqld
netstat -ntlp
```
### 开通端口访问
```
firewall-cmd --permanent --zone=public --add-port=3306/tcp
firewall-cmd --reload
```
### 修改密码规则并创建从服务器访问账号
```
mysql -u root -p
user mysql
SHOW VARIABLES LIKE 'validate_password%';
set global validate_password_policy=LOW;
set global validate_password_length=6;
GRANT REPLICATION SLAVE ON *.* to 'root'@'%' identified by '123456';
flush privileges;
select user,host from user;
exit
systemctl restart mysqld
```
###
> 查看主服务器
```
show master status\G
File: mysql-bin.000019
Position: 154
```
> 从服务器设置
```
change master to master_host='192.168.0.33',master_user='root',master_password='123456',master_log_file='主服务器的File',master_log_pos=主服务器的Position;
start slave;
show slave status\G
```
> 若 Slave_SQL_Running:NO
```
stop slave;
SET GLOBAL SQL_SLAVE_SKIP_COUNTER=1;
start slave;
```
