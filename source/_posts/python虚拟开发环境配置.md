title: python虚拟开发环境配置
date: 2015-09-03 21:13:27
tags:
---
在学习python过程中，不可避免会遇到需要安装不同版本python，以及处理包依赖问题的时候。为了避免不同版本python和包的干扰。搭建不同版本的python环境是一个很好的解决方法。由于本人才疏学浅，读遍网上诸多的配置教程仍不甚了解。不得已挪出时间研究作者的文档。现将自己配置虚拟环境的经验和过程总结如下。
本文基于 Mac OS X 已配置好Homebrew
<!-- more -->
工具：
pyenv
pyenv-virtualenv
conda

```
pyenv配置和切换python版本。
pyenv-virtualenv配置虚拟环境(我理解为pip安装的包在各自目录下)。
conda配置python科学计算版本Anaconda和Miniconda的虚拟环境。


pyenv 和 virtualenv 本是不同的项目，pyenv的作者将virtualenv做成pyenv插件的形式，使得pyenv和virtualenv的配合使用更加方便。
```
## 1. pyenv
### 1.1 理解pyenv原理：
当你在终端中运行命令 (例如pip) 的时候，你的操作系统会在环境变量(PATH)中按序查找该名称的可执行文件。
在终端中输入$PATH可查看PATH。


```
$ $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:
$ which python
/usr/bin/python
```
pyenv的原理是在(PATH)的最前面增加一个叫shims的目录,截获运行系统自带的python的命令。


```
$ $PATH
~/.pyenv/shims:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:
$ which python
~/.pyenv/shims/python
```
### 1.2 pyenv的安装
#### 1.2.1 通过Homebrew安装

```
$ brew update
$ brew install pyenv
```
Mac OS X可以通过Homebrew安装。安装后需要将eval "$(pyenv init -)"添加到shell的初始化文件中，如：


```
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
```
如果你用的是zsh，将~/.bash_profile替换成~/.zshenv。

#### 1.2.2 用git安装
我用的是Homebrew安装，所以我只将作者的所列安装方式简述如下。

* ~/.pyenv是你选择安装的安装位置。你当然也可以选择安装在其他地方。

```
$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv
```
* 定义环境变量，并将`PYENV_ROOT` 指出`pyenv` 的安装位置, 并将`$PYENV_ROOT/bin` 添加到环境变量`$PATH` 使得能够使用pyenv命令。

```
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile

$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
```
如果你用的是zsh，将`~/.bash_profile` 替换成`~/.zshenv` 。
* 安装后需要将`eval "$(pyenv init -)"` 添加到shell的初始化文件中：

```
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
```
如果你用的是zsh，将`~/.bash_profile` 替换成`~/.zshenv` 。
* 重启Shell
### 1.3 pyenv的使用
在shell中输入`pyenv` 可以查看所有命令。通过输入`pyenv help <command>`可以查看特定`<command>`的帮助和使用方法。
`pyenv version`可查看当前已安装到版本(system是系统默认安装的版本):


```
$ pyenv version
* system
```
`pyenv install --list`可查看所有可安装版本


```
$ pyenv install --list
Available versions:
  2.1.3
  2.2.3
  ...
```
`pyenv install <version>`安装指定的版本


```
$ pyenv install 2.5.4
```
每次安装完新版本后要使用`pyenv rehash`，或者重启shell。
`pyenv versions`可查看当前已安装所有版本( * 之后是当前的版本)：


```
$ pyenv version
* system 
2.5.4
```
现在开始进行python不同版本的切换（我的系统自带版本是2.7.6）：
pyenv可以在当前shell，当前所在目录(local)，全局环境(global)下切换python版本。


```
$ python --version
Python 2.7.6
$ pyenv shell 2.5.4
$ python --version
Python 2.5.4
# 仅在当前窗口可用，关闭了窗口就变回原来的python版本了
$ pyenv global system
$ python --version
Python 2.7.6
$ pyenv local 2.5.4
$ python --version
Python 2.5.4
# 仅在当前目录以及其子目录中设置了python的版本，其他目录的版本不变。
```
`pyenv uninstall 2.5.4 `可卸载指定版本

## 2. pyenv-virtualenv
pyenv负责切换python版本，virtualenv能够切换虚拟开发环境。二者的配合可以构建并管理不同的版本的虚拟开发环境。

### 2.1 pyenv-virtualenv的安装
有了安装pyenv的经验，安装pyenv-virtualenv的过程会简略一些。

#### 2.1.1 通过Homebrew安装

```
$ brew install pyenv-virtualenv
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
```
如果你用的是zsh，将~/.bash_profile替换成~/.zshenv。

2.1.2 用git安装

```
$ git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile

```
### 2.2 pyenv-virtualenv的使用
#### 2.2.1创建一个虚拟环境

```
$ pyenv virtualenv 3.4.1 testENV1
# 格式形如 pyenv virtualenv [version] <virtualenv-name> 如果[version]省略 则，默认版本为当前版本
$ python --version
Python 2.7.9
$ pyenv virtualenv testENV2 # 为当前python版本2.7.9
# 低于2.7.9的python版本是没有默认装pip的,若想用这些版本创建环境，需要提前安好pip。
```
#### 2.2.2查看已有虚拟环境

```
$ pyenv virtualenvs
testENV1 (created from ~/.pyenv/versions/3.4.1)
testENV2 (created from ~/.pyenv/versions/2.7.9)
＃如果你有conda环境，也会被列出来

```
#### 2.2.3删除一个虚拟环境

```
$ pyenv uninstall testENV2
# 或者你可以直接删除 ~/.pyenv/versions中的相应目录

```
#### 2.2.4使用虚拟环境
当你用pyenv-virtualenv 创建虚拟环境后，运行pyenv versions你会发现,创建的虚拟环境也在这个列表内，这样，你就可以通过pyenv来同时切换shell, local, global的虚拟环境和该环境的python版本了。这样用pip安装的包就会在 testENV1 这个环境下，从而可以避免系统环境污染

```
$ pyenv versions
* system
  2.7.9
  3.4.1
  testENV1
$ which python 
~/.pyenv/shims/python
$ which pip
~/.pyenv/shims/pip
$ pyenv local testENV1
(testENV1) $ which python
~/.pyenv/versions/testENV1/bin/python
(testENV1) $ which pip
~/.pyenv/versions/testENV1/bin/pip
```
你也可以用用另一种方式切换虚拟环境


```
$ pyenv activate testENV1           # pyenv activate <name>
$ pyenv deactivate

```
如果你想使用带Anaconda或者Miniconda版本的虚拟环境的话，同样可以用pyenv install来安装，但用上述方法管理和切换conda版本就不是很方便了。要想灵活使用科学计算环境的虚拟环境，可以用conda命令来实现。如果你不需要使用科学计算包的话。就不用了解后面的内容了。
## 3. conda
接下来的篇幅是由于我无法用pyenv virtualenv很好的创建管理conda环境。因为当我尝试用上述方法创建时，出了一些问题。


```
$ pyenv virtualenv anaconda-2.3.0 test

WARNING: using virtualenv with conda is untested and not recommended.
    We suggest using the conda command to create environments instead.
    For more information about creating conda environments, please see:
        http://docs.continuum.io/conda/examples/create.html

Proceed (y/n)?

```既然不推荐，我就没打算用。由于conda的文档相对较长,内容太多，我只总结了必要的几个命令。

### 3.1 conda安装
conda工具是在Anaconda或Miniconda环境下自带的工具,所以使用前必须切换到Anaconda或Miniconda的版本。由于Anaconda很大(将近1G？)，需要安装一段时间。但miniconda会小很多(然而我并不是很清楚这个的作用）。


```
$ pyenv install anaconda-2.3.0
$ pyenv local anaconda-2.3.0
(root)$ conda # 可以查看该工具的所用功能
```
### 3.2 创建conda环境
这里的 -n 后面所指为该环境的名字，后面的python是指python默认的包。你也可以通过在改行后面加你想安装的包来直接生成。例如：`conda create -n test2 python numpy scipy...`


```
(root)$ conda create -n test python
...
Proceed ([y]/n)?
$ y

```
### 3.3 查看已有conda环境
root是用pyenv安装的conda版本，也是其他版本所依赖的原始版本。


```
(root)$ conda ENV list
root                  * ~/.pyenv/versions/anaconda-2.3.0
test                    ~/.pyenv/versions/anaconda-2.3.0/envs/test
```
### 3.4 删除已有conda环境

```
(root)$ conda remove --name test --all

```
### 3.5 conda的使用

```
conda环境的激活和退出使用命令source activate CondaENV和source deactivate

(root)$ source activate test
(test)$ source deactivate
(root)$ which python
~/.pyenv/versions/anaconda-2.3.0/bin/python

```
### 3.6 conda环境中的包管理
在conda环境中，可以通过pip来管理包，也可以通过以下方式来管理。
`conda list`来列出当前环境的所有包。`conda search <package>` 在Anaconda.org搜索包。`conda install <package>`来安装conda remove <package>来移除。

