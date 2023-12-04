---
title: python 环境搭建
date: 2023-12-04 15:12:05
tags:
---
## 1. python官网
Python最新源码，二进制文档，最新资讯等可以在Python的官网查看到：  
Python官网：https://www.python.org/  
Python文档下载地址：https://www.python.org/doc/  

## 2. python 安装
1. Window 平台安装 Python
打开 WEB 浏览器访问https://www.python.org/downloads/windows/
在下载列表中选择Window平台安装包，包格式为：python-XYZ.msi 文件 ， XYZ 为你要安装的版本号。
2. MAC 平台安装 Python
MAC 系统一般都自带Python2.x版本的环境，可以在链接 https://www.python.org/downloads/mac-osx/ 上下载最新
版安装。
3. Linux 平台安装 Python
打开 WEB 浏览器访问https://www.python.org/downloads/source/，选择适用Linux 的源码压缩包下载并解压，执
行 ./configure&&make&&make install脚本。  
执行以上操作后，Python 会安装在 /usr/local/bin 目录中，Python 库安装在 /usr/local/lib/pythonXX，XX 为你使用的
Python 的版本号。  
PS: 还可以通过集成软件包Anaconda来安装： https://www.anaconda.com/download/

4. 环境变量配置
为啥要配置环境变量？？
确保可执行环境。

方法：  
    1. Linux添加环境变量主要是在/etc/profile中, 输入export PATH="$PATH:/usr/local/bin/python"   
    2.Windows 设置环境变量：控制面板->高级系统设置->系统属性->环境变量->添加path变量值

## 3. python 多版本环境管理
场景：  
◆ 项目A采用Python 2.x，项目B采用Python 3.x  
◆ 项目C采用Python 3.x Django 1.x，项目D采用Python 3.x Django 2.x

解决方法：
引用虚拟环境。

什么是虚拟环境？
可以理解为容器，临时使用，跟常规系统配置隔离。

以下均需要额外安装：  
◆ pyenv用来管理多个Python版本，比如系统中有一个2.x的版本，安装pyenv后可以，使用pyenv安装其他版本的Python，
让系统可以同时支持多个版本，而且不影响系统版本。
◆ Virtualenv和pyenv-virtualenv是用来创建虚拟环境，让不同的项目拥有自己独立的运行环境，避免相互干扰
◆ pipenv 它有两个功能，一个是管理依赖（替代pip管理工具）、二是可以创建虚拟环境。


python3.3以上自带：venv  
官方文档：
https://docs.python.org/zh-cn/3.7/library/venv.html#module-venv
◆ 查看帮助命令：python3 -m venv --help
◆ 创建虚拟环境：python3 -m venv py3_env （py3_env指定所创建虚拟环境的名称）

创建虚拟环境
1. 切换到需要建立新环境的路径下
2. 执行以下命令，最后参数是环境名
> python -m venv pytestpro
成功：对应目录下新建了文件夹
{% asset_image 新建环境成功.png 新建环境成功 %}

3. 切换到虚拟环境
> cd pytestpro/scripts
> .\activate
成功：命令行前端会显示环境名“pytestpro”
{% asset_image 切换虚拟环境成功.png 切换虚拟环境成功 %}

4. 安装对应包
> pip install requests
成功：新增对应包及依赖
{% asset_image 安之前.png 安之前 %}
{% asset_image 安之后.png 安之后 %}

5. 退出虚拟环境
> deactivate
{% asset_image 退出环境.png 退出环境 %}