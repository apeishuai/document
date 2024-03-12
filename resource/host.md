# 硬件采购
# 选择操作系统
debian (ref:linux.md)


# 初始化配置
ssh登录
```
1 本地生成的id_rsa.pub内容复制到~/.ssh/authorized_keys
2 vim ~/.ssh/config
	增加主机相关内容，id_rsa.pub地址
3 ssh ucloud登录(免密码)
4 vim /etc/ssh/sshd_config
	开放
	service sshd restart
```


# 操作系统配置
查看硬件信息
```bash
lscpu
lsblk
lshw
lspci
lsusb
dmidecode
df
free
ifconfig
/proc/cpuinfo
/proc/meminfo
/proc/partitions
/proc/net
```


# 现状分析

## service
```service normal
systemctl list-units --type=service //列出所有服务
systemctl start/stop/restart/status enable/disable <service>
```

```service nginx
查看Nginx配置文件路径：
nginx -t
nginx -v
重新加载Nginx配置：
sudo systemctl reload nginx

启动Nginx服务：
sudo systemctl start/stop/restart nginx
sudo systemctl status nginx
设置Nginx开机自启动：
sudo systemctl enable/disable nginx
```

```service mysql
配置文件位置：
mysql --help | grep "my.cnf"
sudo find / -name "my.cnf" -type f

mysql -u 用户名 -p
USE mydatabase;
SELECT * FROM mytable;
INSERT INTO mytable (column1, column2) VALUES ('value1', 'value2');
exit;

service mysql start/stop/status

连接到 MySQL 服务器：mysql -u username -p
断开连接：使用 QUIT 命令或 \q 快捷键。
备份/恢复数据库：

备份数据库：mysqldump -u username -p database_name > backup.sql
恢复数据库：mysql -u username -p database_name < backup.sql
创建/删除数据库：

创建数据库：CREATE DATABASE database_name;
删除数据库：DROP DATABASE database_name;
创建/删除用户：

创建用户：CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
删除用户：DROP USER 'username'@'localhost';
授权/撤销权限：

授权权限：GRANT permission ON database_name.table_name TO 'username'@'host';
撤销权限：REVOKE permission ON database_name.table_name FROM 'username'@'host';
更改用户密码：

更改密码：ALTER USER 'username'@'host' IDENTIFIED BY 'new_password';
查看当前连接：

查看当前连接信息：SHOW PROCESSLIST;
查看服务器状态：

查看服务器状态：SHOW STATUS;
```


```service 防火墙
firewalld iptables ufw
```
```service 防火墙 ufw
ufw allow 22
```
```service 防火墙 firewalld
> --开启服务
systemctl start firewalld
systemctl enable firewalld


firewalld iptables ebtables
firewalld的好处：
	动态修改单条规则
	默认每个服务都为拒绝

重要概念：区域管理
	通过将网络划分为不同区域，制定出不同区之间的的访问策略来控制不同程序区域间传送的数据流

1、安装firewalld

root执行 # yum install firewalld firewall-config
2、运行、停止、禁用firewalld

启动：# systemctl start  firewalld

查看状态：# systemctl status firewalld 或者 firewall-cmd --state
停止：# systemctl disable firewalld
禁用：# systemctl stop firewalld
systemctl mask firewalld

systemctl unmask firewalld

4、配置firewalld

查看版本：$ firewall-cmd --version

查看帮助：$ firewall-cmd --help

查看设置：

显示状态：$ firewall-cmd --state

查看区域信息: $ firewall-cmd --get-active-zones

查看指定接口所属区域：$ firewall-cmd --get-zone-of-interface=eth0

拒绝所有包：# firewall-cmd --panic-on

取消拒绝状态：# firewall-cmd --panic-off

查看是否拒绝：$ firewall-cmd --query-panic

更新防火墙规则：# firewall-cmd --reload

 # firewall-cmd --complete-reload

    两者的区别就是第一个无需断开连接，就是firewalld特性之一动态添加规则，第二个需要断开连接，类似重启服务

将接口添加到区域，默认接口都在public

# firewall-cmd --zone=public --add-interface=eth0

永久生效再加上 --permanent 然后reload防火墙

设置默认接口区域

# firewall-cmd --set-default-zone=public

立即生效无需重启

打开端口（貌似这个才最常用）

查看所有打开的端口：

# firewall-cmd --zone=dmz --list-ports

加入一个端口到区域：

# firewall-cmd --zone=dmz --add-port=8080/tcp

若要永久生效方法同上

打开一个服务，类似于将端口可视化，服务需要在配置文件中添加，/etc/firewalld 目录下有services文件夹，这个不详细说了，详情参考文档

# firewall-cmd --zone=work --add-service=smtp

移除服务

# firewall-cmd --zone=work --remove-service=smtp

还有端口转发功能、自定义复杂规则功能、lockdown

iptables 是与最新的 3.5 版本 Linux 内核集成的 IP 信息包过滤系统。如果 Linux 系统连接到因特网或 LAN、服务器或连接 LAN 和因特网的代理服务器， 则该系统有利于在 Linux 系统上更好地控制 IP 信息包过滤和防火墙配置。
```

## ip/port



## 文件传输
> start
scp
rsync 同步大文件

> scp
scp copies files between hoists on a network. it uses ssh for data transfer, and uses the same authentication and provides the same sercurity as ssh
[[https://manpages.debian.org/buster/openssh-client/scp.1.en.html][man scp]]

scp -r root@117.123.89.99:/home/shuai ~



# FAQ
主机ping不通？
主机故障！！！

ssh连不上

