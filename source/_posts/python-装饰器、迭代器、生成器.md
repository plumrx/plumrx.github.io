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


### 2.python装饰器

## 二、python 迭代器、生成器