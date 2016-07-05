title: 使用hexo搭建博客
date: 2015-09-03 21:09:48
tags:
---

无意中发现了Hexo这个基于Node.js的博客框架，之前已经考虑了很久要建个人博客，于是就动起了折腾的想法。事实证明在墙内使用确实还是有一点点折腾的，不过尝试过后感觉Hexo确实很不错，很赞。
<!-- more -->
在此就目前折腾会的一点东西做一些总结，由于软件还在不断完善中，难免新版本会有细微差别，所以仅作参考。
[官网](https://hexo.io)，建议挂vpn，作者是台湾的tommy351，其简介是：

```
A fast, simple & powerful blog framework, powered by Node.js.
```
本文以Mac系统为例

### 1.Git安装和Github设置
***


```
brew install git         #Mac电脑使用brew安装
sudo apt-get install git #Ubuntu系统使用这条命令安装
```
使用Github Page搭建博客, 需要遵循一定的规则, 需要在github建立仓库,仓库名为Github用户.github.io, 像我的就是**http://oneplus7.github.io**，更多详情请参考[官方文档](https://pages.github.com)

### 2.Node.js安装
***
Mac OS X/Linux或其他UNIX/类UNIX系统 配置 nodes 环境
请去[官方网站](https://nodejs.org/download/)下载 `.pkg` 文件 覆盖路径 `/usr`  即可


也可以下载源代码编译安装

```
    $ wget http://nodejs.org/dist/v0.12.4/node-v0.12.4.tar.gz
    $ tar zxvf node-v0.12.4.tar.gz
    $ cd node-v0.12.4
    # ./configure --prefix=/usr
    # make && make install
```
没记错的话，Node包里目前应该已经自带了npm，可以直接用。
Node.js的详细内容请参考[Node.js学习笔记](http://andrewliu.tk/2014/11/02/2014-11-02-Node.js学习笔记/)

### 3.Hexo安装
***
通过npm安装Hexo-Cli 和 hexo

```
$ npm install hexo -g

```
如果以上命令不能安装 可以尝试把官方源替换为淘宝npm源 再执行安装Hexo

```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
或者你直接通过添加 `npm`  参数 `alias`  一个新命令:

```
     $ alias cnpm="npm --registry=https://registry.npm.taobao.org \
     --cache=$HOME/.npm/.cache/cnpm \
     --disturl=https://npm.taobao.org/dist \
     --userconfig=$HOME/.cnpmrc"
```
如果以上的 cnpm 无法安装 可以试试看下方的 alias 方法

 
```   $ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
    --cache=$HOME/.npm/.cache/cnpm \
    --disturl=https://npm.taobao.org/dist \
    --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
```

Hexo目录结构

```
.
├── .deploy       #需要部署的文件
├── node_modules  #Hexo插件
├── public        #生成的静态网页文件
├── scaffolds     #模板
├── source        #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
|   ├── _drafts   #草稿
|   └── _posts    #文章
├── themes        #主题
├── _config.yml   #全局配置文件
└── package.json
```


### 4.Hexo配置
***
既然是要写自己的博客，肯定要找个本地地位置安置。例如：

```
   $ mkdir blog
```
进去之后加入hexo和安装npm 初始化自己的博客。

```
 $ hexo init && npm install
[info] Copying data
[info] You are almost done! Don't forget to run `npm install` before you start b
logging with Hexo!
```
运行server

```
$ hexo server
[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
表明Hexo Server已经启动了，在浏览器中打开 http://localhost:4000/，这时可以看到Hexo已为你生成了一篇名为Hello World 的blog。
你可以按Ctrl+C 停止Server。

创建一篇文章：

```
$ hexo new "My New Post"
[info] File created at blog\source\_posts\My-New-Post.md
```

Hexo 的全局配置是修改根目录下_config.yml这个文件


### 5.Hexo常用插件安装与配置
***


```
$ npm install hexo-generator-index --save
$ npm install hexo-generator-archive --save
$ npm install hexo-generator-category --save
$ npm install hexo-generator-tag --save
$ npm install hexo-server --save
$ npm install hexo-deployer-git --save
$ npm install hexo-deployer-heroku --save
$ npm install hexo-deployer-rsync --save
$ npm install hexo-deployer-openshift --save
$ npm install hexo-renderer-marked@0.2.5 --save
$ npm install hexo-renderer-stylus@0.2.3 --save
$ npm install hexo-generator-feed@1.0.2 --save
$ npm install hexo-generator-sitemap@1.0.1 --save
```

### 6.命令总结
***
常用命令


```
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本

``` 
复合命令

```
hexo deploy -g  #生成加部署
hexo server -g  #生成加预览

```

命令的简写为：
 

```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

```


还有很多内容，未完待续～

