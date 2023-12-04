---
title: pytest-入门
date: 2023-05-18 21:10:29
tags:
---
# 一、Pytest 框架入门
1. pytest 与 unittest的区别


## 1. 简介  
非常成熟的全功能python测试框架，通过插件的形式进行扩展。
官方文档：https://docs.pytest.org/en/latest/  
### 1.1 特点：
1、简单灵活，容易上手，文档丰富；  
2、支持参数化，可以细粒度地控制要测试的测试用例；  
3、能够支持简单的单元测试和复杂的功能测试。  
4、支持用例跳过，失败重跑机制。  
5、丰富的插件以及社区支持，具有很多第三方插件，并且可以自定义扩展。  
6、支持运行由nose, unittest编写的测试case。  
7、支持分布式、多线程跑用例  
8、可以很好的和CI工具结合。  

### 1.2 pytest & unnittest
| 框架     | unittest | pytest |
|--------|------------------------------------|----------------------------|
| 用例编写规则 | 1.测试文件先import unittest<br/>2.测试类继承unittest.TestCase<br/>3.测试方法以”test_“开头<br/>4.测试类必须要有unittest.main()方法 | 1.文件名以“test_”开头或“_test”结尾<br/>2.方法以“test_”开头<br/>3.类以“Test”开头<br/>4.测试类中不可以有构造函数"\__init__()" |
| 用例分类执行 | 默认执行全部用例，也可以使用测试集，执行部分用例 | 可以通过@pytest.mark 来标记类和方法，pytest.main加入参数"-m" 可以只运行标记的类和方法 |
| 用例前后置  | setUp/tearDown 只能针对所有用例| 自定义方法函数，加上 @pytest.fixture() 这个装饰器即可被使用 |
| 参数化  | 需要依赖ddt库| 使用@pytest.mark.parametrize装饰器 |
| 断言  | assertEqual assertIn assertTrue assertFalse | assert |
| 报告  | HTMLTestRunnerNew库 | pytest-HTML allure 插件 |
| 失败重跑  | 无 | pytest-rerunfailures插件 |

### 1.3 pytest 环境搭建
> pip install pytest  
{% asset_image 安之前.png 安之前 %}  
{% asset_image 安之后.png 安之后 %}