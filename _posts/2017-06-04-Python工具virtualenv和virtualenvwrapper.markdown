---
layout: post
title: "Python中virtualenv和virtualenvwrapper工具介绍"
excerpt: "在python开发中，我们有时候需要用到各式各样不同的包，这些包的性质各异，使用的开发语言不尽相同，且支持的环境也不一样。这时候我们就需要使用虚拟环境将不同项目分隔开来。这时我们就需要用到virtualenv和virtualenvwrapper这两个工具了。"
categories: [Python]
comments: true
image:
  feature:
---
在python开发中，我们有时候需要用到各式各样不同的包，这些包的性质各异，使用的开发语言不尽相同，且支持的环境也不一样。这时候我们就需要使用虚拟环境将不同项目分隔开来。这时我们就需要用到`virtualenv`和`virtualenvwrapper`这两个工具了。

virtualenv 工具可以创建隔离的Python环境。而virtualenvwrapper是用来管理被virtualenv创建出来的项目（环境）的工具。

那么，再重申一遍，为什么要用这两个工具呢?

1. 版本依赖
2. 间接许可

#### 版本依赖：
1. 假设有一个app，需要libfoo 1.0 的库，另一个app需要libfoo 2.0的库，如何才能把这两个库都安装到/usr/lib/python2.7/site-packages？
2. 当系统的库发生了变化，或许app就运行不了

#### 间接许可
1. 当主机，我们没有root权限。

virtualenv 就能解这样的问题，它将创建一个单独的环境，库将安装到自己目录下，不会和其他环境共享。

由于virtualenv用起来有点麻烦，wrapper对它进行了封装，让它更好用，最终我们使用wrapper提供的命令，但是实际工作都是virtualenv做的。
# virtualenv
## 安装

直接使用pip安装

    pip install virtualenv
    pip install virtualenvwrapper

或者使用豆瓣源

    pip install -i https://pypi.douban.com/simple virtualenv
    pip install -i https://pypi.douban.com/simple virtualenvwrapper

## 用法
#### 创建环境

    virtualenv [虚拟环境名称]
    virtualenv test

创建的默认环境使用的是系统默认的包，那如果你想指定Python版本的话则

      virtualenv -p [python安装路径] [虚拟环境名称]

如果不想使用系统的包,加上–no-site-packeages参数

    virtualenv --no-site-packages test

#### 激活环境

1. 进入你创建的环境的目录
2. 找到并进入Scripts文件夹
3. 运行activate

#### 退出环境
1. 进入你创建的环境的目录
2. 找到并进入Scripts文件夹
3. 运行deactivate

# virtualenvwrapper
## 用法
首先要初始化，配置系统变量，变量名：WORKON_HOME。 变量值：E:\envs（这个值是你以后想要存放虚拟环境地址的值）

#### 创建环境

    mkvirtualenv env1
    mkvirtualenv env2

#### 切换环境

    workon env1
    workon env2

#### 列出已有环境

    workon

#### 退出环境

    deactivate

如果没有这个命令，则在创建的虚拟环境中的scripts中复制deactivate到python包的安装目录下如：C:\Users\sush\AppData\Local\Programs\Python\Python36-32\Scripts
#### 删除环境

    rmvirtualenv
