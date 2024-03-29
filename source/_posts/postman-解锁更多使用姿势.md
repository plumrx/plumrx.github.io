---
title: postman-解锁更多使用姿势
date: 2022-11-02 21:43:04
tags:
---


# 解锁更多 postman 功能  

## 前言  
平时我们在使用 postman 的时候，主要使用的是其发送 http / https 请求，其实 postman 还有很多隐藏技能点，等待大家去挖掘。最近浅学了一些新功能的使用，在这里分享给大家。

## 功能
### 1. 参数配置  
一般情况下，项目接口以文件夹的形式管理在 postman 中。但是接口之间会有很多相似的数据，例如：host、测试数据等，那么我们可以用参数配置的方式进行接口报文的管理。  
#### 优点：
* 更换host可以统一在参数修改，无需按接口逐一修改数据，更便捷，不易遗漏。
* 多接口使用相同的测试数据，方便管理。


### 2.自动化测试  
postman 有自己的自动化测试用例模块，就在接口配置的 Tests 菜单下。
```
模版：
tests['用例名称']=断言

示例：
tests['Response contains "user" property'] = responseJSON.hasOwnProperty('user');
```
#### 优点   
这些用例可以在使用 postman 调用接口结束后，对接口调用结果进行验证。快速、直观地展示了测试结果，提升测试效率。

### 3. 命令行执行  
使用图形化界面配置好相关接口及数据后，可以将所有内容以 json 文件的形式导出，编写start.sh文件，使用命令行执行。

#### 3.1 json 文件导出  
1. 选择需要导出的 collection ，右键选择 export；
2. 导出文件，记录存储地址；
3. 新建执行文件 'start.sh'，在其中配置相关参数项;
4. 在命令行中执行该文件```./start.sh```；
5. 检查执行结果。

#### 3.2 常见问题及解决方案
1. 执行 start.sh 失败，报错如下：  
```zsh: permission denied: ./start.sh```  
原因：这是因为当前用户无该文件执行权限，故增加执行权限即可。  
    * 查询权限  
    发现该文件只有读写权限。
    ```
    la
    --------------
    -rw-r--r--   1 plumrx  staff   482B 11  6 20:36 start.sh
    ```

    * 修改权限  
    读权限权重=4、写=2、执行=1，故需添加执行权限，所以权限权重需改为744。
    ```
    chmod 744 start.sh
    ```


2. 配置的参数未生效，用例执行失败  
```
  POST /users [errored]
     Invalid URI "http:///users"
  2⠄ JSONError in test-script
```
原因：