### mysql-8.0.28 linux centos7 安装


在这个网站
https://downloads.mysql.com/archives/community/
下载
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230730231349.png)
mysql-8.0.28-1.el7.x86_64.rpm-bundle.tar 


上传到linux(我这里是用的vmware创建的虚拟机)
展示一下虚拟机的界面
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230730231505.png)

由于直接在虚拟机上操作手感很差,所以使用mobaxtrem进行远程连接
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230730231632.png)

解压上述压缩包
进入到新目录
依次安装
```
sudo rpm -ivh mysql-community-common-8.0.28-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-client-plugins-8.0.28-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-libs-8.0.28-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-libs-compat-8.0.28-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-client-8.0.28-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-icu-data-files-8.0.28-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-server-8.0.28-1.el7.x86_64.rpm

```
安装最后一个的时候,安装失败,缺少依赖,需要先安装以下依赖,然后重新安装
```
yum install -y perl-Module-Install.noarch
yum install -y perl
yum install net-tools

sudo rpm -ivh mysql-community-server-8.0.28-1.el7.x86_64.rpm

```
启动mysql服务,并且设置开机自动启动
```
systemctl start mysqld
systemctl enable mysqld

```

修改密码
先用临时密码登录(tepu2L*lK:34是我的密码)
```
[root@localhost mysql]# cat /var/log/mysqld.log | grep password
2023-07-30T13:13:39.158391Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: tepu2L*lK:34
```
登录
```
mysql -u root -p
```


然后然后设置一个密码
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Root@123456';
```
修改密码设置策略然后重新修改密码为123456
```

set global validate_password.policy=LOW;

set global validate_password.length=6;

ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

```
当前的mysql用户root只能从本地访问
```
mysql> select host from mysql.user where user = 'root';
+-----------+
| host      |
+-----------+
| localhost |
+-----------+
```
重新创建一个user用户,让其能从其他主机访问(当前centos7是虚拟机,从宿主机访问它)
```
mysql> CREATE USER 'user'@'%' IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.01 sec)

mysql> GRANT ALL PRIVILEGES ON "." TO 'user'@'%';


```
虚拟机网络是nat,ip是192.168.74.3,mysql端口号是3306,在宿主机的cmd输入下面的命令
```
mysql -h 192.168.74.3 -P 3306 -u user -p
```
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230730225821.png)
可以成功访问mysql

学会卸载也很重要!
```shell
sudo rpm -ev mysql-community-common-8.0.28-1.el7.x86_64 --nodeps

sudo rpm -ev mysql-community-client-plugins-8.0.28-1.el7.x86_64 --nodeps

sudo rpm -ev mysql-community-libs-8.0.28-1.el7.x86_64 --nodeps

sudo rpm -ev mysql-community-libs-compat-8.0.28-1.el7.x86_64 --nodeps

sudo rpm -ev mysql-community-client-8.0.28-1.el7.x86_64 --nodeps

sudo rpm -ev mysql-community-icu-data-files-8.0.28-1.el7.x86_64 --nodeps

sudo rpm -ev mysql-community-server-8.0.28-1.el7.x86_64 --nodeps

find / -name mysql
# 这些目录是关于mysql的目录
rm -rf /var/lib/mysql
rm -rf /var/lib/mysql/mysql
rm -rf /tmp/data/mysql
rm -rf /tmp/mysql
rm -rf /tmp/mysql/data/mysql
rm -rf /usr/lib64/mysql
```
