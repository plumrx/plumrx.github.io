---
title: python-装饰器、迭代器、生成器
date: 2022-09-27 22:45:21
tags:
---

# python-装饰器、迭代器、生成器  

>1. 闭包、装饰器的用途、作用？  
闭包用途：方便使用装饰器的一种语法。
>2. python 装饰器的用法和原理？
>3. python 可迭代对象、迭代器？
>4. python 迭代器用法？
>5. python 生成器的用途和用法？

## 一、python 装饰器

### 1.python闭包
* 定义：可理解为特殊函数，两个函数的嵌套，分别称为内函数、外函数，外函数返回值是内涵数的引用。

* 格式
```
def out_func(parm):
    def inner_func(parm):
        print('inner func exe',parm)
    return inner_func
```
* 实践1 - 不带参数
```
def print_msg():
    msg = "hello world"

    def printer():
        print(msg)

    return printer

prt=print_msg()
prt()

-------------------
hello world
```

* 实践2 - 带参数
```
def func(a,b):
    def line(c):
        return a*b-c

    return line

f=func(2,3)
print(f(5))

---------------
1
```

* 实践3 - 内函数修改外函数值  

```nonlocal a```   
在内嵌函数直接引用外部参数时，需要 ```nonlocal``` 关键字进行声明。
```
def func(a, b):
    def line(c):
        nonlocal a
        print(a)
        a = 3
        print(a)
        return a * c - b

    return line


f = func(2,3)
print(f(5))

---------------------
2
3
12
```





### 2.日常开发需求  

需求：func_a是新功能，需要增加其日志信息，如：运行时间

* 方案演进1  
每个函数中增加调用logging，缺点：函数多了会冗余。
```
import logging
logging.basicConfig(format='%(asctime)s-%(levelname)s:%(message)s',
                    level=logging.DEBUG)

def func_a():
    print('this is func a')
    logging.info('A is running')

func_a()

---------------------------------
2022-09-28 00:30:07,056-INFO:A is running
this is func a
```


* 方案演进2  
将需要增加log的函数以参数的形式传进来，包成一个新的函数。  

```
def use_logging(func):
    logging.info('%s is running'%func.__name__)
    func()

def func_a():
    print('this is func a')

use_logging(func_a)

-----------------------------------
this is func a
2022-10-08 10:35:20,696-INFO:func_a is running
```


### 3.python装饰器  
* 作用：不修改已有函数源代码，不修改已有函数的调用方式，为已经存在的对象添加额外的功能。
* 本质：闭包函数

#### 3.1 简单的装饰器示例
以上方法都不够方便优雅，装饰器可以很好地解决该问题。不改变函数的调用，对原有的功能进行增强。

```
import logging
logging.basicConfig(format='%(asctime)s-%(levelname)s:%(message)s',
                    level=logging.DEBUG)

def use_logging(func):

    def wrapper():
        logging.info('%s is running' % func.__name__)
        # 把fun_a 当作参数传递进来，执行func()=执行fun_a()
        return func()
    return wrapper

def fun_a():
    print('this is fun_a')

# 因为装饰器 use_logging(fun_a) 返回的时函数对象 wrapper，这条语句相当于  fun_a = wrapper
fun_a = use_logging(fun_a)
fun_a()

---------------------------------
2022-10-08 10:54:09,265-INFO:fun_a is running
this is fun_a
```  


#### 3.2 装饰器语法糖  
* 语法：@ + 装饰器函数

```
def use_logging(func):

    def wrapper():
        logging.info('%s is running' % func.__name__)
        # 把fun_a 当作参数传递进来，执行func()=执行fun_a()
        return func()
    return wrapper

@ use_logging
def fun_a():
    print('this is fun_a')

fun_a()

----------------------------------
2022-10-08 11:18:14,596-INFO:fun_a is running
this is fun_a
```

* 执行原理
```
def w1(func):
    def inner():
        # 验证1
        # 验证2
        print('inner')
        return func()
    return inner

# @w1=w1(f1)=inner
@w1
def fun_a():
    print('this is fun_a')

fun_a()

--------------------------
inner
this is fun_a
```

#### 3.3 类装饰器  
* 优点：灵活度更大、高内聚、封装性。  
方法前后加下划线，变为魔法函数。此处必须使用 \__call__ 方法。

```
class TestClass(object):
    def __init__(self,func):
        self._func=func

    def __call__(self):
        print('class decorator running')
        self._func()
        print('class decorater ending')

@TestClass
def fun_a():
    print('this is fun_a')

fun_a()

----------------------------------
class decorator running
this is fun_a
class decorater ending
```

* \__foo__:一种约定,Python内部的名字,用来区别其他用户自定义的命名,以防冲突.

* _foo:一种约定,用来指定变量私有.程序员用来指定私有变量的一种方式.

* __foo:这个有真正的意义:解析器用_classname__foo来代替这个名字,以区别和其他类相同的命名.

#### 3.4 functools 装饰器
* 作用：保留原有函数的名称和原有属性。  

* 实例一：未用functools 的 wrap 装饰器,fun_a函数名称已改变。  
```
def use_print(func):
    def with_logging(*args, **kwargs):
        print('Calling decorated function...')
        return func(*args, **kwargs)

    return with_logging


@use_print
def fun_a(x):
    print('this is func_a')
    return x + x * x

print(fun_a.__name__, fun_a.__doc__)

------------------------------------
with_logging None
```

* 实例二：采用functools 的 wrap 装饰器,fun_a函数名称未改变。在装饰器函数前加@wraps装饰器。  
```
from functools import wraps


def use_print(func):
    @wraps(func)
    def with_logging(*args, **kwargs):
        print('Calling decorated function...')
        return func(*args, **kwargs)

    return with_logging


@use_print
def fun_a(x):
    print('this is func_a')
    return x + x * x

print(fun_a.__name__, fun_a.__doc__)

-------------------------
fun_a None
```

#### 3.5 @property装饰器示例  1.55.59

* 原理：@property 装饰器将方法转换为相同名称的只读属性。
* 作用：可以用该装饰器来创建只读属性，可以与所定义的属性配合使用，防止属性被修改。


## 二、python 迭代器、生成器