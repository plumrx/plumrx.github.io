---
title: python- RESTful请求
date: 2022-10-24 19:05:51
tags:
---


# RESTful

## 1. URI \ URL \ URN
定义：RESTful是基于 http 协议，定义的一种规约。
* URI 统一资源标志符

抽象的改变，用唯一的属性确认唯一的资源，那么该属性可称为 URI 。
* URL 统一资源定位符

通过某个地址，找到某个资源
* URN 统一资源名称
>URI = URL + URN 

常用于前后端分离项目

## 2. HTTP 请求时间
公共方法放在 common 路径之下；  
文件命名规则：协议 + 方法  
> http_request.py

类命名规则：协议 + 方法，驼峰
> HttpRequest

### 2.1 params 与 data 的区别  
requests模块发送请求有data、params两种鞋带参数的方法，其中params常用在get请求中，data常用在post请求中。  

#### 2.1.1  params 以参数形式传入  
传入的参数会出现在url后，以参数形式直接发起请求。
```
params = {'name':'plumrx'}
response = requests.get(url, params=params, verify=False)
print(response.text)
-----------------------
"url": "https://httpbin.testing-studio.com/anything?name=plumrx"
```

#### 2.1.2 data以参数形式传入  
传入参数会出现在 form 表单中。与上面相比，区别在与参数存放的位置不同。
```
payload = {'name':'plumrx'}
response = requests.get(url, data=payload, verify=False)
print(response.text)
--------------------------
"form": {
    "name": "plumrx"
  }
```  

#### 2.1.3 data 以 json 的形式传入（最常用）  
会以字符串的形式，出现在json字符串中。
```
payload = {'name':'plumrx'}
response=requests.post(url,data=json.dumps(payload),verify=False)
print(response.text)
--------------------------
"json": {
    "name": "plumrx"
  },

```

#### 2.1.4 json 以参数的形式传入  
```
payload = {'name':'plumrx'}
response=requests.post(url,json=payload,verify=False)
print(response.text)
-------------------------
"json": {
    "name": "plumrx"
  }, 
```


### 2.2 疑难杂症  
Q：在执行纯代码案例时，遇到“empty suite”该怎么办？  
原因：执行脚本时，默认使用了pytest框架，未发现符合要求的用例，故执行集为空。  
解决办法：进入pycharm的setting-python integrated tools-Testing，将Deafault test runner 修改为Unittests。



## 3.为自己的测试框架封装请求  
请求方法都属于公共组建，故放置在 common 路径之下。  
文件及类命名规则：建议 协议+请求方法
>http_request.py  
>class HttpRequests

### 3.1 构造函数  
主要功能：初始化，创建 session。  
包含：地址、方法和请求头(定制)
```
def __init__(self,url):
    self.url=url
    self.req=requests.session()
    self.header={'User-Agent':'***'}
```

### 3.2 封装 get \ post \ put \ delete 请求  
要尽可能覆盖所有场景，作为公共方法，入参要齐全，尽可能覆盖所有场景。  

#### 3.2.1 get 请求
```
import requests


class HttpRequests(object):

	# 封装自己的 get 请求，获取资源
	def get(self, uri='', params='', data='', headers=None, cookie=None):
		url = self.url + uri
		respose = self.req.get(url, params=params, data=data, headers=headers, cookie=cookie)
		return respose

```

#### 3.2.2 post 请求  
```
	def post(self,uri='',params='',data='',headers=None,cookies=None):
		url = self.url + uri
		response = self.req.post(url,params=params,data=data,headers=headers,cookies=cookies)
		return response
```

#### 3.2.3 put 请求  
```
	def put(self,uri='',params='',data='',headers=None,cookies=None):
		url=self.url+uri
		response = self.req.put(url,params=params,data=data,headers=headers,cookies=cookies)
		return response
```

#### 3.2.4 delete 请求 
```
	def delete(self,rui='',params='',data='',headers=None,cookies=None):
		url=self.url+uri
		response = self.req.delete(url,params=params,data=data,headers=headers,cookies=cookies)
		return response
``` 




## 4. 实战演练 1.14.18
接口地址：https://httpbin.testing-studio.com/
1. 导入自己封装的类  
*命名规则：协议+方法（驼峰）*
``` 
# 绝对路径
form test_project.common.http_requests import HttpRequests

# 相对路径
from ..common.https_requests import HttpsRequests
```

2. 定义 URL
```
url_index='https://httpbin.testing-studio.com/'
```

3. 调用接口
```
http = HttpRequests(url_index)
response = http.get()
print('Response 内容：' +  response.text)
```
相当于封装postman send。




## 3 多接口实战
1. 初始化：定义 url 和调用方法
```
@classmethod
def setUpClass(cls) -> None:
    cls.url = '***'
    cls.http = HttpRequests(cls.url)
```

2. 访问首页
def test_001_index(self):
    response = TestBattal.http.get()
    self.assertEqual(response.status_code,200,'请求返回非 200')

3. 登录
def 