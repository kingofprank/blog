---
title: 基于Github+Hexo建立博客
date: 2016-07-25 16:18:59
tags:
---
利用Github提供Github Pages和静态博客网站生成器Hexo来搭建博客！！

![](http://oava57hou.bkt.clouddn.com/6159252dd42a2834b1c7cf5b59b5c9ea15cebf79.jpg)
<!--more-->
忙活了好一段时间，才完整的自己搞定了一次博客的搭建。其实去年就已经在成神的帮助下搞定了一次，不过其实那个时候是一脸懵逼的，而且太久没做记录了就容易忘记了。

# 安装Git和建立Github仓库
***
Git是一款分布式的版本控制系统，是Linus大牛花了两周的时间自己写的系统，免费而且好用！我是看[廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)学会的，其中很详细的介绍了Git的语法和用法。这里就不赘述了。对于Windows用户来说需要安装[msysgit](http://msysgit.github.io/)，这是Git在Windows下的版本。由于需要和他人交换，往往有一个电脑充当服务器的角色，Github就是一个这样的网站，他提供Git仓库托管服务，同时给每个用户Github提供一个Github Pages，这是一个有着300M免费，稳定的空间，可以用来搭建博客。下列是如何建立Github远程仓库，关于具体的提交上传命令就不做赘述了。
## 注册Github账号
地址在这里:https://github.com/
## 创建SSH Key
在`C:/users/uername/`下查看是否有.ssh目录和目录中有`id_rsa`和`id_rsa.pub`这两个文件。若无则打开Git Bash，输入一下命令
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
邮件地址使用自己的邮箱，一直回车后按照要求就能产生这个目录和目录下的这两个文件。(在回车时会提示输入一个密码，是用来提交项目时候用的密码，如果选择不填，则提交项目时无需密码)其中一个是公钥可以告诉别人，一个是私钥要自己保留。
## 设置SSH Key
登陆Github，打开"Account settings"，在"SSH Keys"页面将公钥的内容粘贴上。这样Github就知道这台电脑的推送文件是用户提交的。可以用一下命令测试是否成功
```
$ ssh -T git@github.com
```
会得到下列SSH警告，这是SSH在第一次连接的时候会要你确认Key是否来自Github服务器。
```
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```
输入yes后看到
```
Hi cnfeat! You've successfully authenticated, but GitHub does not provide shell access.
```
即设置成功。
## 创建远程仓库
在Github上找到"Create a New Repository"按钮，建立一个名字叫##username.github.io##的仓库(这个仓库名字是特定的)。

# 安装Node.js
***
[Node.js](https://nodejs.org/en/)是一个Javascript运行环境(runtime)。实际上它是对Google V8引擎进行了封装。下文要用到的Hexo就是要在这个环境下跑的。推荐安装后重启电脑。

# 安装Hexo
***
这是一款基于Node.js的静态博客框架，我们就是要用Hexo生成静态博客页面！先介绍几个命令缩写
```
$ hexo g #完整命令为hexo generate,用于生成静态文件
$ hexo s #完整命令为hexo server,用于启动服务器，主要用来本地预览
$ hexo d #完整命令为hexo deploy,用于将本地文件发布到github上
$ hexo n #完整命令为hexo new,用于新建一篇文章
```
下面开始正式安装！
## 打开Git Bash，利用一下命令安装Hexo
```
$ npm install hexo-cli -g
```
创建博客所在的文件夹，比如创建在`F:\WorkSpace\Blog`。不要放在中文路径下。然后进入该文件夹内执行以下命令
```
$ hexo init
```
## 安装依赖包
```
& npm install
```
于是初始化后有这样的一些目录
```
.
├── _config.yml //网站的 配置 信息，您可以在此配置大部分的参数。
├── package.json
├── scaffolds     //模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。
├── source     //资源文件夹是存放用户资源的地方。
|   ├── _drafts
|   └── _posts
└── themes     //主题 文件夹。Hexo 会根据主题来生成静态页面。
```
## 建立新文章
在Blog目录下调用命令(或者自己创建在/source/_post里面也可以)
```
$ hexo new "New file name"
```
## 生成网站
```
$ hexo g
```
这个时候就将`source/_post`中的md文件生成到一个`/public`的文件夹中，形成网站的静态文件，里面包含了网站的所有信息。
## 本地预览
```
$ hexo s
```
此时本地的4000端口已经打开(默认4000，若冲突可以改成`hexo s -p 3000`等)，在网页输入127.0.0.1:4000即可预览网站
## 部署网站
确认无误后便可以将网站部署在Github上了，当然此时Github上的Repository要已经创建好了。要部署上去要先修改本地配置。编辑`F:\WorkSpace\Blog`下的`_config.yml`文件，拉到最下方，添加如下配置，##注意在配置文件中每个冒号后面都需要有一个空格##。
```
deploy: 
  type: git
  repository: http://github.com/kingofprank/kingofprank.github.io.git
  branch: master
```
保存后，输入以下命令部署网站
```
$ hexo d
```
此时可能会出现`ERROR Deployer not found : git`错误，则运行下列命令
```
$ npm install hexo-deployer-git --save
```
然后重新用`hexo g`和`hexo d`命令生成和部署网站。在执行d命令的时候，可能需要输入用户名和密码，是注册Github上的所用的。此时博客已经搭建完毕，在几分钟后可以访问username.github.io查看网页情况。

## 基本使用
### 全局配置 _config.yml
在hexo目录下有该文件，用来配置博客基本属性的。
```
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/
# Site #站点信息
title:  #标题
subtitle:  #副标题
description:  #站点描述，给搜索引擎看的
author:  #作者
email:  #电子邮箱
language: zh-CN #语言
# URL #链接格式
url:  #网址
root: / #根目录
permalink: :year/:month/:day/:title/ #文章的链接格式
tag_dir: tags #标签目录
archive_dir: archives #存档目录
category_dir: categories #分类目录
code_dir: downloads/code
permalink_defaults:
# Directory #目录
source_dir: source #源文件目录
public_dir: public #生成的网页文件目录
# Writing #写作
new_post_name: :title.md #新文章标题
default_layout: post #默认的模板，包括 post、page、photo、draft（文章、页面、照片、草稿）
titlecase: false #标题转换成大写
external_link: true #在新选项卡中打开连接
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
highlight: #语法高亮
  enable: true #是否启用
  line_number: true #显示行号
  tab_replace:
# Category & Tag #分类和标签
default_category: uncategorized #默认分类
category_map:
tag_map:
# Archives
2: 开启分页
1: 禁用分页
0: 全部禁用
archive: 2
category: 2
tag: 2
# Server #本地服务器
port: 4000 #端口号
server_ip: localhost #IP 地址
logger: false
logger_format: dev
# Date / Time format #日期时间格式
date_format: YYYY-MM-DD #参考http://momentjs.com/docs/#/displaying/format/
time_format: H:mm:ss
# Pagination #分页
per_page: 10 #每页文章数，设置成 0 禁用分页
pagination_dir: page
# Disqus #Disqus评论，替换为多说
disqus_shortname:
# Extensions #拓展插件
theme: landscape-plus #主题
exclude_generator:
plugins: #插件，例如生成 RSS 和站点地图的
- hexo-generator-feed
- hexo-generator-sitemap
# Deployment #部署，将 lmintlcx 改成用户名
deploy:
  type: git
  repo: 刚刚github创库地址.git
  branch: master
```
### Hexo完整目录
```
├── .deploy #需要部署的文件
├── node_modules #Hexo插件
├── public #生成的静态网页文件
├── scaffolds #模板
├── source #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
| ├── _drafts #草稿
| └── _posts #文章
├── themes #主题
├── _config.yml #全局配置文件
└── package.json
```
这样一个基本的博客框架就已经搭好了!!但是仍然有许多需要改进的地方。

# 相关问题
在搭建博客的过程中遇到了各种各样奇葩的坑，下面列出解决方法。
## 无法生成html，用“hexo s”时显示“No Layout:index.html”
当使用命令`hexo g`的时候生成的public文件夹里面的html都是0kb，所以使用s命令的时候无法预览。这个奇葩的问题我查了半天，有的人的情况是在配置主题的时候没有修改`_config.yml`中的themes选项，或者是冒号后面没有空格，导致失效。我遇到的不是这种情况，然后重装了主题多次还是不行，后来发现下列方法可以解决问题。输入命令
```
hexo clean#清理缓存
hexo s -debug#这个命令似乎没什么用，是用来调试的。主要还是上面那个清理缓存比较关键吧
```
## hexo和npm命令不存在
有的时候会出现直接无法使用hexo和npm命令的情况，一是可以重装最新版的node.js，二是可以重启。我猜是配置还没有更新所致。

# 感谢

参考了许多来自下列博客的教程，大牛带我涨姿势。

[使用Hexo搭建个人博客(基于hexo3.0)](http://opiece.me/2015/04/09/hexo-guide/)
[手把手教你建github技术博客](http://www.jianshu.com/p/701b1095da11)
[如何搭建一个独立博客——简明Github Pages与Hexo教程](http://blog.csdn.net/poem_of_sunshine/article/details/29369785/)
