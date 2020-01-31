Mysql5.7安装:
下载安装文件:
检查系统是否自带mysql:
rpm -qa | grep mysql

若存在?则卸载:
普通卸载:rpm -e ....

强制卸载:rpm -e --nodeps ...
检查否存在 mariadb 数据库,如有?卸载之?卸载同上

解压安装:tar -zxvf ..

检查用户(组)是否存在：
cat /etc/group | grep mysql
#类似
mysql:x:490:
cat /etc/passwd | grep mysql
#类似
mysql:x:496:490::/home/mysql:/bin/bash

添加用户组
groupadd mysql
useradd -r -g mysql mysql
 
#useradd -r参数表示mysql用户是系统用户，不可用于登录系统

将/opt/mysql/mysql-5.7.29的所有者及所属组改为mysql
chown -R mysql.mysql /opt/mysql/mysql-5.7.29
安装数据库

创建data目录
cd mysql-5.7.29
mkdir data

在/opt/mysql/mysql-5.7.25/support-files目录下创建my_default.cnf
==================================================
[mysqld]
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
 
basedir = /opt/mysql/mysql-5.7.29
datadir = /opt/mysql/mysql-5.7.29/data
port = 3306
socket = /tmp/mysql.sock
character-set-server=utf8
 
log-error = /opt/mysql/mysql-5.7.29/data/mysqld.log
pid-file = /opt/mysql/mysql-5.7.29/data/mysqld.pid
==================================================

cp support-files/my_default.cnf /etc/my.cnf

初始化 mysqld
./bin/mysqld --initialize --user=mysql --basedir=/opt/mysql/mysql-5.7.29/ --datadir=/opt/mysql/mysql-5.7.29/data/
初始化完成之后，查看日志,查看临时密码
/opt/mysql/mysql-5.7.29/data/mysqld.log 


把启动脚本放到开机初始化目录
cp support-files/mysql.server /etc/init.d/mysql

启动mysql服务
service mysql start
登录mysql，密码为初始密码
./bin/mysql -u root -p
修改密码
mysql> set password=password('123456');
mysql> grant all privileges on *.* to root@'%' identified by '123456';
mysql> flush privileges;

添加远程访问权限 
mysql> use mysql;
mysql> update user set host='%' where user = 'root';
mysql> flush privileges;

重启服务

service mysql restart

==================
navicat破解:
path