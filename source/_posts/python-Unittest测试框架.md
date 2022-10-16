---
title: python- Unittest测试框架.md
date: 2022-10-12 14:54:50
tags:
---


#  Unittest 框架学习
 Unittest = PyUnit ，是 Python 标准库中自带的单元测试框架。适用于单元测试、接口测试、Web 自动化测试，特性：通过类的方式，将测试用例组织在一起。

 ## 一、核心要素
 1. test case  测试案例
 2. test suite  测试套件
 3. test runner  测试部件
 4. text fixture  测试装置

 ## 二、常见属性
 > import unittest
 > dir(unittest) # 查询包下面的所有属性
 > dir(unittest.TestCase) # 查询其中所有函数  

 * unittest.TestCase()：所有测试用例类继承的基本类
 * unittest.TestSuite()：用来创建测试套件的
 * unittest.main()：将单元测试模块变为可直接运行的测试脚本，main()方法使用 TestLoader 类执行所有 test 开头的测试方法，且执行顺序根据 ASCII 码:0-9\A-Z\a-z。
 * unittest.TextTestRunner()：通过下面的 run() 方法来运行 cuite 组装的测试用例，入参为测试套件。

## 三、Unittest常见用法：基本用法小结  

1. 导入 unittest 模块、被测文件或者其中的类。
2. 创建测试类，并继承 unittest.TestCase。
3. 重写 setUp 和 tearDown 方法，该两种方法在父类中已经存在。
4. 定义测试函数，用 test 开头。
5. 在函数体内使用断言。
6. 调用 unittest.main() 方法来运行测试用例。


### 演练-计算器  
#### 计算器类
```
class Calculator(object):
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def add(self):
        return self.a + self.b

    def minus(self):
        return self.a - self.b

    def multip(self):
        return self.a * self.b

    def divide(self):
        return self.a / self.b
```

#### 实际应用  
```
# 导入unittest模块
import unittest
from calculator import Calculator


# 定义测试类
class TestCalculator(unittest.TestCase):

    # setUp方法用于初始化
    # 入参为self，定义方法的变量也要"self.变量"
    def setUp(self):
        print('test start')
        self.result = Calculator(10, 5)

    # 定义测试用例，以"test_"开头命名的方法，方法的入参为self
    # 可定义多个测试用例
    def test_add(self):
        self.assertEqual(self.result.add(), 15, '加法错误！')

    def test_minus(self):
        self.assertEqual(self.result.minus(), 5, '减法错误！')

    def test_multip(self):
        self.assertEqual(self.result.multip(), 50, '乘法错误！')

    def test_divide(self):
        self.assertEqual(self.result.divide(), 2, '除法错误！')

    # 善后，清缓存
    def tearDown(self):
        print('test end')


if __name__ == '__main__':
    # unittest.main()方法会搜索该模块下所有以test开头的测试用例方法，并自动执行他们。
    unittest.main(verbosity=2)
```

## 用例执行顺序
unittest 默认根据 ASCII 码，0-9，A-Z，a-z。  

### 修改测试用例执行顺序
1. 修改测试用例方法名，前标序号
2. 饮用测试集，按引入顺序执行

## 断言

## 命令行执行用例  
python -m unittest 模块名.类名.方法名（用例）
```
python -m unittest 004-***.TestCalculator.test_add
```
