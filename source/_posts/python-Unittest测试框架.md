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

## 四、用例执行顺序，组装测试用例
unittest 默认根据 ASCII 码，0-9，A-Z，a-z。  

### 修改用例执行顺序，添加执行集
1. 修改测试用例方法名，前标序号
> test_001_***

2. 引用测试集，按引入顺序执行  
```
# 测试套件的使用
if __name__=='__main__':
    # 构造测试集
    suite = unittest.TestSuite()

    # 将测试用例加入测试集
    # 方法一:
    suite.addTest(TestCalculator('test_add'))

    # 方法二：将用例编入列表，逐一添加
    tests = [
        TestCalculator('test_add'),
        TestCalculator('test_minus'),
        TestCalculator('test_multip'),
        TestCalculator('test_divide')
    ]
    suite.addTest(tests)
    
    # 执行测试集合
    runner=unittest.TextTestRunner(verbosity=2)
    runner.run(suite)
```   

3. TestLoader.loadTestsFromTestCase 加载多个类  
使用最多，可以一次加载某个类下面的所有测试用例，也支持加载多个类。  

```
import unittest
from test_24_02 import TestCalculator
from test_24_02_02 import TestCalculator2

if __name__ == '__main__':
    suite1 = unittest.TestLoader().loadTestsFromTestCase(TestCalculator)
    suite2 = unittest.TestLoader().loadTestsFromTestCase(TestCalculator2)

    suite = unittest.TestSuite([suite1, suite2])

    runner = unittest.TextTestRunner(verbosity=2)
    runner.run(suite)
```

4. TestLoader.discover 匹配目录下的用例  
若目录下存在两个测试用例，Test_Myclass.py & Test_Myclass2.py ，使用 Testloader()，该类相对简便。  
```
import os
import unittest

if __name_ == '__main__':
    # 实例化测试套件
    suite = unittest.TestSuite()
    # 1.实例化 TestLoader 对象
    # 2.使用discover 去找一个目录下的所有测试用例
    loader = unittest.TestLoader()
    # 3.使用 addTests 将找到的测试用例放在测试套件下
    # os.getcwd()=获取当前目录
    # 默认搜索路径，当前目录下，以 test 开头的测试文件
    suite.addTests(loader.discover(os.getcwd()))

    # 方法二：指定目录
    test_dir = './test'
    suite = unittest.defaultTestLoader.discover(test_dir, pattern='test_*.py')
   
   runner=unittest.TextTestRunner()
   runner.run(suite)
```


## 五、将测试结果输入到文本
声明test runner时需要增加一个参数，stream=f
```
    with open('unittest_report.txt', 'w') as f:
        runner = unittest.TextTestRunner(stream=f, verbosity=2)
        runner.run(suite)
```

# 六、setUp() tearDown()
可以在所有case执行前准备一次环境，并在所有case执行结束手再清理环境。  
```
    @classmethod
    def setUpClass(cls) :
        print('仅测试套件前执行一次')

    @classmethod
    def tearDownClass(cls) :
        print('仅测试套件结束后执行一次')
```

## 七、跳过某个case  

### skip 装饰器  
1. @unittest.skip(reason):无条件跳过装饰的测试，并说明跳过测试的原因。  
```
    @unittest.skip('不执行')
    def test_add(self):
        # 加断言
        self.assertEqual(self.result.add(),12,'加法错误！')

--------------------------
Skipped: 不执行
```

2. @unittest.skipif(reason):条件为真时，跳过装饰的测试，并说明跳过的原因。  
```
    @unittest.skipIf(3 > 2, '3>2 不执行')
    def test_add(self):
        # 加断言
        self.assertEqual(self.result.add(), 12, '加法错误！')
----------------------------------
Skipped: 3>2 不执行
```


3. @unittest.skipUnless(reason):条件为假时，跳过装饰的测试，并说明原因。  
```
    @unittest.skipUnless(sys.platform.startswith('linux'),'require Linux')
    def test_add(self):
        # 加断言
        self.assertEqual(self.result.add(), 12, '加法错误！')

----------------------------------
Skipped: require Linux
```
4. @unittest.expectedFailure():测试标记失败。

### TestCase.skipTest()方法
```
    def test_add(self):
        # 加断言
        self.skipTest('无需执行')
        self.assertEqual(self.result.add(), 12, '加法错误！')
---------------------------------
Skipped: 无需执行
```


## 八、断言  
1. assertEqual(a,b,[msg='失败打印信息'])  
断言a,b是否相等，相等用例通过。  

2. assertNotEqual(a,b,[msg='失败打印信息'])  
断言a,b是否相等，不想等测试通过。  

3. assertTrue(x,[msg='失败打印信息'])  
断言x是否为True，是True则用例通过。 

4. assertFalse(x,[msg='失败打印信息'])  
断言x是否为False，是False则用例通过。  

5. assertIs(a,b,[msg='失败打印信息'])  
断言a是否是b，是则用例通过。  

6. assertNotIs(a,b,[msg='失败打印信息'])  
断言a是否不是b，不是则用例通过。  

7. assertIsNone(x,[msg='失败打印信息'])  
断言x是否为None，是None则用例通过。  

8. assertIsNotNone(x,[msg='失败打印信息'])  
断言x是否为None，不是None则用例通过。 

9. assertIn(a,b,[msg='失败打印信息'])  
断言a是否在b中，在b中则用例通过。  

10. assertNotIn(a,b,[msg='失败打印信息'])  
断言a是否在b中，不在b中则用例通过。  

11. assertIsInstance(a,b,[msg='失败打印信息']) 
断言a是b的一个实例，是则用例通过。  

12. assertNotIsInstance(a,b,[msg='失败打印信息']) 
断言a是b的一个实例，不是则用例通过。


## 九、命令行执行用例  

### 用例自动（递归）发现：
1. 默认当前目录下所有符合 test*,py 的测试用例
```
python -m unittest
python -m unittest discover
```

2. 通过 -s 参数指定要自动发现的目录，-p 参数指定用例文件的名称模式
```
python -m unittest discover -s project_directory -p "test_*.py"
```

3. 通过位置参数指定自动发现的目录和用例文件的名称模式
```
python -m unittest discover project_directory "test_*.py"
```

### 执行指定用例
1. 指定测试模块
```
python -m unnittest test_module1 test_module2
```

2. 指定测试类
```
python -m unittest test_module.TestClass
```

3. 指定测试方法
```
python -m unittest 模块名.类名.方法名（用例）
```

4. 指定测试文件的路径（python3）
```
python -m unittest tests/test_something.py
```



## 十、参数化和数据驱动  
### 1.背景  
实际项目中，测试数据应统一管理，避免硬编码的情况，所以参数化、数据驱动的方式设计测试用例脚本会更佳合理安全。  

### 2.定义  
参数化测试是一种“数据驱动”测试，同方法测试不同参数，以覆盖所有可能的预期分支。测试数据与测试行为分离，被放入文件、数据库或者外部介质中，再由测试程序读取。  


### 3.参数化库
python 标准库中的 unittest 不支持参数化测试，可以使用：ddt、 parameterized.  

#### 3.1 ddt数据驱动设计模式  
* 数据驱动思想：数据和用例分离，通过外部数据去生成测试用例。

* 安装
> pip3 install ddt

* 实践一  
导包更加详细
```
import unittest
from ddt import ddt, data, unpack

# 加装饰器，与以下的装饰器，写法固定
@ddt
class MyTest(unittest.TestCase):
    # 声明参数，以元祖或列表的形式，每条数据对应一条用例
    @data((3, 1), (1, 0), (1.2, 1.0))
    @unpack
    def test_values(self, first, second):
        print(first, second)
        self.assertTrue(first > second)
```

* 实践二  
导包更加笼统
```
import ddt
import unittest


@ddt.ddt
class MyTest(unittest.TestCase):
    #列表或元祖
    @ddt.data([1, 2, 3, 6])
    @ddt.unpack
    def test_add(self, t1, t2, t3, excp):
        sum = t1 + t2 + t3
        self.assertEqual(sum, excp)
```


* 实践三  
用文件存储测试数据
```
--------data_003.json--------
[
  "1||2||3",
  "3||2||5",
  "3||8||11"
]

---------test_003.py----------
@ddt.ddt
class MyTest(unittest.TestCase):

    # 数据以外部文件形式存储
    @ddt.file_data('data_003.json')
    @ddt.unpack
    def test_sum(self, value):
        # 数据读取，分离
        a, b, sum = value.split('||')
        print(a, b, sum)
        result = int(a) + int(b)
        self.assertEqual(result, int(sum))
```


#### 3.2 parameterized 参数化驱动  

* 特点：不适用于数据存储于外部文件。

* 安装
> pip3 install parameterized

* 实践一  
parameterized 参数化，仅需要一个装饰器，相比 ddt 更加简洁。
```
import unittest
from parameterized import parameterized


class MyTest(unittest.TestCase):
    @parameterized.expand([(3, 1), (1, 10)])
    def test_values(self, first, second):
        print(first,second)
        self.assertTrue(first > second)
```


* 实践二  
参数化参数：列表套元祖
```
import unittest
from parameterized import parameterized


class MyTest(unittest.TestCase):

    @parameterized.expand([('plumrx', '123'), ('miumiu', '321')])
    def test_params(self, name, pwd):
        print('name=%s,pwd=%s' % (name, pwd))
```



#### 3.3 将测试数据批量存放在 Excel  
1. xlrd  
可读取 Excel 文件，支持xls,xlsx格式。  
2. xlwt  
可生成 Excel 文件，仅支持 xls 文件。
3. openpyxl  
同时支持读写文件，主要适用于 xlsx 文件。




## 十一、Mock接口