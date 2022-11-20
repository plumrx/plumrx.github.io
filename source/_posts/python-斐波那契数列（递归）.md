---
title: python-斐波那契数列（递归）
date: 2022-11-20 15:29:20
tags:
---

# 斐波那契数列  
想用递归的方式，实现求斐波那契数列第 N 项的功能。

## 1. 定义  
斐波那契数列（Fibonacci sequence），又称黄金分割数列，因数学家莱昂纳多·斐波那契（Leonardo Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：1、1、2、3、5、8、13、21、34、……在数学上，斐波那契数列以如下被以递推的方法定义：F(0)=0，F(1)=1, F(n)=F(n - 1)+F(n - 2)（n ≥ 2，n ∈ N*）

## 2. 分析  
搞清楚该数列定义之后，因数学和编程中的差异，所以我这里定义第 0 项数值为 0，分析得出以下规律：
```   
# n为项数   
n=0:
    F(n) = 0
n=1:
    F(n) = 1
n>1:
    F(n) = F(n-1) + F(n-2)
```

## 3. 实践
```
def fib(n):
    if 1 >= n > -1:
        return n
    elif n > 1:
        return fib(n - 1) + fib(n - 2)
    else:
        return "The index must be non-negative integer"


print('fib(-999)=' + str(fib(-999)))
print('fib(-1)=' + str(fib(-1)))
print('fib(0)=' + str(fib(0)))
print('fib(1)=' + str(fib(1)))
print('fib(5)=' + str(fib(5)))

------------------------
fib(-999)=The index must be non-negative integer
fib(-1)=The index must be non-negative integer
fib(0)=0
fib(1)=1
fib(5)=5
```


## 4. 问题  
一般递归, 每一级递归都需要调用函数, 会创建新的栈,随着递归深度的增加, 创建的栈越来越多, 造成爆栈，结果如下：  
```
print('fib(999)=' + str(fib(999)))
----------------------------
RecursionError: maximum recursion depth exceeded in comparison
```

## 5. 优化方案  
尾递归基于函数的尾调用, 每一级调用直接返回函数的返回值更新调用栈,而不用创建新的调用栈, 类似迭代的实现, 时间和空间上均优化了一般递归。
* 还没尝试写，先了解下方法(>_<)