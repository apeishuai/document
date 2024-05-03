2023-10-18  22:55:45  08:00

#### 安装scoop

powershell执行命令:
```c
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
iwr -useb get.scoop.sh | iex
```
修改配置:
```c
# 设置环境变量
$env:SCOOP='E:\Softwares\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
$env:SCOOP_GLOBAL='E:\Softwares\GlobalScoopApps'
[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'Machine')
 
# 设置环境变量之后，建议将默认目录下的所有文件复制到新目录下
 
# 搜索软件
scoop search xxx

# 找不到软件？添加软件库
scoop bucket list
scoop bucket add <bucketname>

# 常用软件库
extras https://github.com/ScoopInstaller/Extras 2023/9/8 12:26:26      1890
main   https://github.com/ScoopInstaller/Main   2023/9/8 12:25:39      1241

# 列出已安装软件
scoop list
```
#### 安装hugo
```c
scoop install hugo
```

#### 配置hugo
执行下述命令
```c
# 新建站点，会生成一个quickstart目录
hugo new site quickstart

cd quickstart

# 初始化git，生成.git目录
git init

# 为了避免quickstart/themes的.git文件夹和quickstart文件夹下.git冲突，用submodule命令
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke

# 如题用的是ananke主题
echo "theme = 'ananke'" >> hugo.toml
hugo server
```
下载主题压缩包至/site/themes/\
[hugo themes list](https://themes.gohugo.io/)\
比如我的主题是smigle，copy /site/themes/simgle/exampleSite/config.toml文件至/site/hugo.toml，按需求修改，我的配置文件如下：
```c
baseURL = ''
languageCode = 'en-us'
title = 'Wxf`s Fantasy World'
theme = 'smigle'

[[menu.main]]
identifier = "home"
name = "Home"
url = "/"
weight = 1

[[menu.main]]
identifier = "blog"
name = "Blog"
url = "/posts/"
weight = 2

[[menu.main]]
identifier = "projects"
name = "projects"
url = "/tags/"
weight = 3

[[menu.main]]
identifier = "about"
name = "About"
url = "/about/"
weight = 4

[params]
abbrDateFmt = "Jan 2"
author = "Blog Author"
brandIconFile = "images/wxf.jpg"
dateFmt = "2006-01-02"
subtitle = "share something useful"


[[params.social]]
name = "GitHub"
url = "https://github.com/apeishuai"
[[params.social]]
name = "Bilibili"
url = "https://space.bilibili.com/1993949543/video"
[[params.social]]
name = "Weibo"
url = "https://weibo.com/u/6030695929"

toc=true
genableGitInfo = true
[frontmatter]
  lastmod = ['lastmod', ':git', ':fileModTime', 'date', 'publishDate']
```
界面如下：
![](/img/博客主界面_2023-10-19_17-28-30.png)
# 写文章
```c
# 新建文章页面，上述配置中每个menu对应不同目录，直接新建markdown文件丢到这些目录下就ok。具体对应/site/contents/posts/
hugo new posts/first-article.md

# 图片放在/site/static/img文件夹下，在md文件中这样引用![](/img/xxx.jpg)，如下图

# 更新索引结构，执行完毕会更新/site/public下相应文件,index.html等
hugo -D

# 在浏览器localhost:1313预览博客效果
hugo server -w
```
![](/img/博客img引用_2023-10-19_18-08-44.png)

# 推送至github
- [创建项目(主站点)](https://docs.github.com/zh/pages/getting-started-with-github-pages/creating-a-github-pages-site)
- [ssh添加公钥](https://blog.csdn.net/fenghuibian/article/details/73350890)
- 创建github workflow\
新建/site/.github/workflows/hugo-deploy.yml文件，内容如下：
```c
name: deploy

on:
    push:
    workflow_dispatch:
    schedule:
        # Runs everyday at 8:00 AM
        - cron: "0 0 * * *"

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            - name: Build Web
              run: hugo

            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
				  # TOKEN需自己生成
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
				  # 需修改
                  EXTERNAL_REPOSITORY: apeishuai/apeishuai.github.io
                  PUBLISH_BRANCH: master
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}

```
token生成参考：[利用GitHub Action实现Hugo博客在GitHub Pages自动部署](https://lucumt.info/post/hugo/using-github-action-to-auto-build-deploy/)
- 推送至仓库
```c
git init
git remote add origin "username.github.io"
cd public
git add .
git commit -m "my first blog"
git push origin master
```
- 创建仓库(子站点)
创建新项目(例docs)，推送至gh-pages分支上，就能通过'username.github.io/docs'访问该项目，也就是说能无限展示子站点。用docsify搭建一个文档站是很不错的选择\
[ref: 如何用Github的gh-pages分支展示自己的项目](https://cloud.tencent.com/developer/article/1391619)

- 推送至仓库
```c
# 展示项目下子文件
git subtree push --prefix=dist origin gh-pages

# 展示整个项目
text git:(master) git symbolic-ref HEAD refs/heads/gh-pages
git add -A
git commit -m "..."
git push origin gh-pages
```
诸位可以访问我的博客[apeishuai](https://apeishuai.github.io)和[apeishuai/docs](https://apeishuai.github.io/docs)来看效果。完毕！

# QA
2023-11-02_00:03:51\
新建目录下放index.md，hugo引擎会自动索引。至于索引文件夹下内容，需要在模板更改，位置暂时不找了
