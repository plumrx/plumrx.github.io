---
title: Charles-抓包工具
date: 2022-11-07 21:55:28
tags:
---

# 抓包工具 - Charles  
## 1. 特点
1. 方便，界面友好；  
2. 跨平台支持 Linux、Windows 和 Mac 操作系统；  
3. 该工具只有30天免费试用期限，试用期间隔30mins就会闪退，后续需要付费使用。

## 2. 安装地址
http://www.charlesproxy.com

## 3. 相关配置  
### 3.1 证书安装  
1. 路径：Help -> SSl proaxying -> Install Charles Root Certificate；
2. 单击“安装证书”，单击“下一步”；
3. 选择“将所有证书放入下列存储”，单击“浏览”；选择“受信任的根证书颁发机构”，点击“确定”。

### 3.2 SSL Proxying 代理配置  
1. 路径：Proxying -> SSL Proxying Setting -> SSL Proxying 选项卡；
2. 选择”Enable SSL Proxying“，单击”add“按钮；
3. Host:* ;
4. Port:443

### 3.3 PC 抓包
Charles 只支持抓取 HTTP 和 HTTPS 请求。Request 代码是请求的参数，选择 Response 查看服务器响应报文。

### 3.4 手机抓包配置
#### PC配置
1. 路径：Proxying -> Proxying Setting ；
2. Port：8888
3. 勾选“Enable  transparent HTTP proxying"

#### 手机配置  
1. 获取计算机IP；
2. 路径：WI-FI设置 -> HTTP 代理，切换为手动模式；
3. 服务器：计算机IP；
4. 端口：8888；
5. 设置完，PC 弹出“Connection from ...“，点击”allow“；