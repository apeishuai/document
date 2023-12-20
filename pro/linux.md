# 文件夹

# 环境信息
who\
env\
which\
lastlog\

# 用户管理
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
