OS:Debian

# 包管理
```
aptitude
apt
  |
  --->dpkg
```
```
aptitude [-x] xx
-d 仅下载软件包 
-D 显示安装包的依赖关系信息 
-f 修复损坏的软件包 
-F 设置搜索结果的格式 
-h 显示帮助信息 
-i 运行时执行安装操作 
-O 设置如何排列显示搜索结果 
-p 每一步操作都要求确认 
-q 不显示进度条 
-u 启动时下载新的软件包列表 
-v 显示版本信息 
-V 显示软件包的版本信息 
-w 设置搜索结果的格式宽度 
-y 所有询问均回答自动yes 
-Z 显示每个软件包的安装尺寸变化

autoclean 删除旧的已下载软件包文件 
changelog 显示一个软件包的变更日志 
clean 删除已下载的软件包文件 
download 下载软件包的.deb文件 
forbid-version 禁止软件包升级到某个指定版本 
hold 将软件包置于保持状态 
install 安装或升级软件包 
markauto 将软件包标记为自动更新 
purge 卸载软件包并删除其配置文件 
reinstall 重新安装指定的软件 
remove 卸载软件包 
search 依据名称搜索软件包 
show 显示一个软件包的详细信息 
unhold 取消软件包的保持状态 
unmarkauto 将软件包标记为手动安装 
update 升级软件包

```
```
apt
/etc/apt/sources.list
apt工具整合了apt-get和apt-cache命令
apt不能直接安装.deb文件

apt-get update -> apt update
apt-get upgrade
apt-get dist-upgrade
apt-get install xxx
apt-get remove xxx
apt-get autoremove

apt-cache
apt-cache search xxx
apt-cache show xxx
apt-cache depends xxx
```
```
dpkg --help

dpkg --list 'ddd*'
dpkg --listfiles foo 列出安装软件包foo所提供的文件
dpkg -S foo 获得名为foo的文件所属的软件包
dpkg --status xxx
dpkg --info xxx.deb
dpkg --install xxx.deb
dpkg --remove xxx
```

# 环境信息
who\
env\
which\
lastlog\

# 网络
```原理
OSI七层模型


```
```练习
```


# 用户管理
```原理
/etc/passwdS\
root:x:0:0::/root:/bin/bash\
name: :user id:gid:directory:bash\
(gid:一登陆就会自动获得，初始群组)

/etc/shadow\
root:xxxxx:13025:5:60:7:2:13125:\
name:pass:最近密码更新日期:密码不可被更改天数:密码需要重新变更天数:密码需要变更期限前警告:密码过期恕限时间:账号失效日期:保留\

/etc/group\
root:x:0:root\
gname:pass:gid:support account

/etc/gshadow\
root:::root\
user:pass(!xx表示无法登入):group account:群组所属账号

/home/username

命令：\
groupadd\
groupmod\
groupdel\
gpasswd\

newgrp users: 切换有效群组(决定新创建文件归属)\
exit: 离开新的有效群组

groups:查看账号属于的群组(第一个为有效群组)

useradd:\
ref: \
/etc/default/useradd\
/etc/login.defs\
/etc/skel/*

passwd:
/etc/login.defs\
/etc/pam.d/passwd\

usermod:

userdel:\
/etc/passwd\
/etc/shadow\
/home/username\

sudo \
/etc/sudoer

PAM模块：\
/etc/nologin, /etc/securetty\
/etc/pam.d\
```
```练习

```

# Ref
https://qref.sourceforge.net/Debian/reference/index.zh-cn.html#contents
