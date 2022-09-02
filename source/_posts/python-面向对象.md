---
title: python-面向对象
date: 2022-08-09 16:28:46
tags:
---


## OOP:面向对象编程

### 1.面向过程 & 面向对象
* 面向过程编程：
根据业务逻辑从上往下，将需要用到的功能封装到函数中。着重开发的过程和步骤，C语言
* 面向对象编程：
关注结果，Java、C++


### 2.类和对象的特点

* 对象：面向对象编程的核心。对象则为实体飞机。  
* 类：为了将具有共同特征和行为的一组对象抽象定义。相当于造飞机的图纸，由类名、属性和行为组成。

### 3.创建类和对象
* 经典类  
类名：驼峰
```
class ClassNameA:
    pass
```

* 新式类  
onject是所有类的父类
```
class ClassNameB(object):
    pass
```

* 创建对象  
对象即类的实例
```
A1=ClassNameA()
A2=ClassNameB()
``` 

### 4.方法和函数
在类中定义的函数即为方法，如：move()&attack()


### 5.添加和获取对象属性
```
# 创建类
class Hero(object):

	def move(self):
		print('移动')

	def attack(self):
		print('攻击')


A1 = Hero()

# 添加属性
A1.name = 'plumrx'
A1.score = 8848

print('%s 的分数为 %s' % (A1.name, A1.score))

A1.move()
A1.attack()

------------------------
plumrx 的分数为 8848
移动
攻击
```

### 6.利用 hasattr、getattr、setattr 获取或添加对象属性  

* hasattr(object,name)  
检查对象是否包含名为 name 的属性和方法。

```
class Hero(object):
	def __init__(self,name):
		self.name =name

	def info(self):
		print('Hero %s 的生命值:%d' %(self.name))

A=Hero('plumrx')
# 检查 A 中是否包含 name 属性
print(hasattr(A,'name'))
# 检查 A 中是否包含 age 属性
print(hasattr(A,'age'))

----------------------------
True
False
```

* getattr(object,name[,default])  
获取对象中名为 name 的属性的属性值。 
```
t = getattr(A, "name ")
print(t)
s = getattr(A, 'score', 'no score')
print(s)

# 获取方法：打印函数地址
f = getattr(A, "info")

# 调用方法
f()
--------------
plumrx
no score

<bound method Hero.info of <__main__.Hero object at 0x104f0ff40>>

Hero plumrx 
```
 

* setattr(object,name,value)  
将对象的 name 属性设为 value。若属性不存在，则先创建。
```
print(A.hp)
setattr(A,'hp',2000)
print(A.hp)

# 将外部函数 test 添加到对象 A 中，并命名为 attack
setattr(A,'attack',test)

--------------
1000
2000
```

### 7. 构造函数和析构函数 
* \__init__()：   
类的构造函数，初始化方法，当创建这个类的实例时一定会调用该方法。  

* \__del__()：
当删除对象时，python 解释器也会默认调用该方法。
  


### 8. 面向对象三大特性  
* 8.1 封装

* 8.2 继承

* 8.3 多态
