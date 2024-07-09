# base
node.js是运行在服务端的javascript\
node.js是一个基于chrome javascript运行时建立的一个平台\
node.js是一个事件驱动I/O端javascript环境，基于google的v8引擎，v8引擎执行javascript的速度非常快，性能很好


# 环境搭建
设置淘宝源
```
设置成淘宝源
npm config set registry https://registry.npm.taobao.org
查看结果
npm config get registry
输出结果：

https://registry.npm.taobao.org/
测试一下
npm info underscore
```

```
apt install nodejs
apt install npmA

npm config set prefix $HOME/.node_modules

npm install -g pnpm
pnpm setup 

pnpm add nnrm -g

nnrm use taobao

# 使用最新版
pnpm env use --global latest
# 使用长期支持版
pnpm env use --global lts
```
# npm package
register here:\
https://www.npmjs.com/

npm config get registry  //查看当前源
npm config set registry https://registry.npmjs.org
npm config set registry http://registry.npm.taobao.org  //将registry设置为淘宝镜像


npm ls //查看当前目录下安装了哪些包

npm adduser //
npm login
npm whoami //查看当前npm用户，没有用户登录会显示ERR

npm get prefix // 查看node安装路径
npm cache clean -f

npm init //生成配置信息

npm publish
npm unpublish xx --force

npm i xx

npm view xxx
npm view xxx versions //查看包的版本列表
npm version patch
npm bersion minor
npm bersion major

# ref
[npm入门教程](https://dkvirus.gitbooks.io/-npm/content/di-si-zhang-npm-bao-guan-li-gong-ju/44-geng-xin-mo-kuai.html)
