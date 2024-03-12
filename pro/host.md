# nginx
部署流程：\
1 安装nginx\
2 查看nginx状态\
3 修改配置文件;重新加载\
4 服务器查看修改效果

## 安装nginx
```shell
OS:debian 
shell: 
    sudo apt update
    sudo apt install nginx
```
## 操作
nginx -s stop/quit/reload/reopen 

ps -ax | grep nginx\
kill -s QUIT 1628
## 配置
```c
events{
        worker_connections 2048;
}

http{

server{
listen  80;
charset utf-8;

location / {
root /home/shuai/mysite;
}

}
}

```


## ref
https://blog.redis.com.cn/doc/index.html \
https://nginx.org/en/docs/


