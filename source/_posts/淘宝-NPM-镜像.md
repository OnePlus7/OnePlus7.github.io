title: 淘宝 NPM 镜像
date: 2015-09-04 10:14:52
tags: NPM镜像
---

在墙内安装node的时候会出现各种各样报错的信息，通过查找一些资料，发现可以通过替换npm镜像的方式来避免出现错误。
淘宝的 NPM 镜像是一个完整的npmjs.org镜像。你可以用此代替官方版本(只读)，同步频率目前为 15分钟 一次以保证尽量与官方服务同步。
1.当前 registry.npm.taobao.org 是从 registry.npmjs.org 进行全量同步的.
2.当前 npm.taobao.org 运行版本是: cnpmjs.org@0.4.1
3.系统运行在 Node.js@v0.11.12 上.
<!-- more -->
## 使用说明
可以通过定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

```
npm install -g cnpm --registry=http://registry.npm.taobao.org
```
或者添加alias：
 
```
alias cnpm="npm --registry=http://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=http://dist.cnpmjs.org \
--userconfig=$HOME/.cnpmrc"
#Or alias it in .bashrc or .zshrc
$ echo '\n#alias for cnpm\nalias cnpm="npm --registry=registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=http://dist.cnpmjs.org \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc

```
## 安装模板
从 registry.npm.taobao.org 安装所有模块. 当安装的时候发现安装的模块还没有同步过来, 淘宝 NPM 会自动在后台进行同步, 并且会让你从官方 NPM registry.npmjs.org 进行安装. 下次你再安装这个模块的时候, 就会直接从 淘宝 NPM 安装了.

```
cnpm install [name]
```
## 同步模块
直接通过 sync 命令马上同步一个模块, 只有 cnpm 命令行才有此功能:

```
cnpm sync connect
```
当然, 你可以直接通过 web 方式来同步: npm.taobao.org/sync/connect

```
open http://npm.taobao.org/sync/connect
```
## 其他命令
支持 npm 除了 publish 之外的所有命令, 如:

```
cnpm info connect
```

