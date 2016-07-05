title: 终端中使用vim出现乱码
date: 2016-07-05 14:10:26
tags: 乱码  终端  vim
---





在终端的配置文件.zshrc中加入下面配置 

```
export LC_ALL=en_US.UTF-8 
export LANG=en_US.UTF-8

```

再执行
```
source .zshrc
```
就可以解决。

