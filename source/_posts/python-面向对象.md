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
#### 8.1 封装  
将内容封装到某处，从某处调用被封装的内容。

```
class person(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

	#封装
    def printperson(self):
        print(self.name, self.age)


person1=person('name1','18')
person2=person('name2','19')

#调用
person2.printperson()

---------------------
name2 19
```


#### 8.2 继承  
* 优点：子可继承父的内容及特性。  
* 继承语法：class 派生类名（基类名）

```
c=child()
c.childMethod()
c.parentMethod()

---------------------
child init
child method
parent method
```
仅调用了子类的构造方法，若需调用父类构造方法，可采用以下几种方法：  
1. 在子类中，不定义构造方法。
2. 在子类的构造函数中，直接调用父类的构造函数。```parent.__init__(self)```
3. 在子类中，调用super方法。```super(child,self).__init__()```  

pyhton 支持多继承，一个子类继承多个父类，寻找方法为：深度优先、广度优先（python3）。  

语法：class SubClassName(ParentClass1[,ParentClass2])

* 广度优先  
>例：A为新式类，B和C均继承A，D继承B和C。
对象D的属性查询顺序为：  
D -> B -> C -> A


* 子类重写父类方法  
在子类中，使用与父类中相同变量或方法名，可以重新定义父类中的属性和方法。

```
class A():
    def hello(self):
        print('class a')

class B(A):
    pass

class C(A):
    # 重写了hello方法
    def hello(self):
        print('class c')

a=A()
b=B()
c=C()
a.hello()
b.hello()
c.hello()

----------------
class a
class a
class c
```



#### 8.3 多态  
* 多态：不同的子类对象，调用相同的父类方法，产生不同的结果。“龙生九子，各有不同”

我自己理解，类似于父类中方法重写。
```
class People(Animals):
    def talk(self):
        print('People are talking')

class Cat(Animals):
    def talk(self):
        print('Cat is maow')

class Dog(Animals):
    def talk(self):
        print('Dog is wang')

person=People()
cat1=Cat()
dog1=Dog()

# 直接调用
person.talk()
cat1.talk()
dog1.talk()

-----------------------
People are talking
Cat is maow
Dog is wang
```



### 9.类变量、成员变量（实例变量）、局部变量
* 类变量：可以由类名直接调用，也可以由对象来调用  

* 成员变量：可以由类的对象来调用，成员变量定义在构造函数内部，一定是以self.的形式给出的，因为self的含义就是代表实例对象。

区别：类变量的调用方式比成员变量多一种，即类直接调用。

```
class TestClass(object):
    num = 0  # 类变量

    def __init__(self, x, y):
        self.x = x  # 实例变量（成员变量），允许实例调用，不允许类调用
        self.y = y
        self.fuc(self.x, self.y)

    def add(self):
        total = 2
        self.vara = 3  # 未在构造函数中进行初始化，所以为类变量
        self.varb = 4
        result = (self.x + self.y) * total
        return result

    def fuc(self, a, b):
        self.varc = 100  # 成员变量，他们在成员函数func()中定义，但是在构造函数中调用了该函数
        self.vard = 200
        return a + b


object_a = TestClass(10, 20)
print(object_a.num)  # 通过实例调用
print(TestClass.num)  # 通过类名调用
print(object_a.x, object_a.y)  # 实例变量仅允许实例调用
print(object_a.varc, object_a.vard)

------------------------------
0
0
10 20
100 200
```


### 10.类中的私有方法和属性  

* 私有属性：```__private_attrs``` 两个下划线开头，声明该属性为私有，不能在类的外部被使用或者访问。在类内部的方法中使用时 ```self.__private_attrs```。  

* 私有方法：```__private_method``` 两个下划线开头，声明该方法为私有方法，不能在类的外部调用。在类的内部调用```self.__private_method```。


```
class TestClass(object):
    public_var = 'hello world'
    __private_name = 'plumrx'

    def display_var(self):
        print('print public var:%s' % self.public_var)
        print('print private name:%s' % self.__private_name)
        print(self.__private_fun())

    def __private_fun(self):
        return ('execute private func:%s'%self.public_var)

object_var=TestClass()
print(object_var.public_var)
print(object_var.__private_fun())

# 私有化属性访问技巧
# 对象._类名+私有属性
print(object_var._TestClass__private_name)
---------------------------------
hello world

Traceback (most recent call last):
  File "/Users/plumrx/projects/kuangshi/RF52/lesson18/class18.py", line 179, in <module>
    print(object_var.__private_fun())
AttributeError: 'TestClass' object has no attribute '__private_fun'

plumrx
```

> 1. 如何在python中将变量或方法转为私有？  
    变量名或方法名前加两个下划线
> 2. 如何在类外部调用私有属性或方法?  
    对象._类名+私有属性名/方法名  
    ```object_var._TestClass__private_name```



### 11.类的三种方法  

#### 11.1 实例方法  

* 定义：第一个参数必须为实例对象，一般约定为 self ，通过它来传递实例或类的属性和方法。
* 调用方式：只能由实例对象调用。

* 语法形式  
```
class TestClass(object):

    def instance_method(self):
        print('实例方法')
```

* 实践
```
class Person:
    def __init__(self, name, gender):
        self.name = name  # 实例变量，不允许类调用
        self.gender = gender

    def get_name(self):
        return self.name  # 实例方法，需要实例化才能使用

    def get_gender(self):
        return self.gender

    def get_name(plumrx):
        return plumrx.name


# 调用实例方法一
print(Person('plumrx', '女').get_name())
print(Person('plumrx', '女').get_gender())

# 调用实例方法二
student = Person('mrx', '女')  # 实例化
print(student.get_name())
print(student.get_gender())

--------------------------------
plumrx
女
mrx
女
```

#### 11.2 静态方法
* 定义：使用装饰器@staticmethod。参数随意，但是方法体中不能使用类或实例的任何属性和方法。

* 调用方式：类和实例对象都可以调用。

* 语法形式 
```
class TestClass(object):

    @staticmethod
    def staticmethod():
        print('静态方法‘)
```

* 实践
```
class Person:

    count=0
    location='China'

    def __init__(self,name,gender):
        self.name=name
        self.gender=gender
        Person.count+=1

    def get_name(self):
        return self.name

    @staticmethod
    def get_location():
        return Person.location

    @staticmethod
    def show_time():
        return time.strftime('%Y-%m-%d %H:%M:%S',time.localtime())

# 类名调用
print(Person.get_location())
# 实例调用
student=Person('plumrx','女')
print(student.get_location())
print(student.show_time())

--------------------------
China
China
2022-09-26 23:41:34
```

* 用途：主要用于存放逻辑性代码，归属于类但是跟类本身关联性不大。

#### 11.3 类方法  
* 定义：使用装饰器@classmethod。第一个参数必须是当前类对象，一般约定为 cls ，通过它来传递类的属性和方法（不能传实例的属性和方法）；

* 调用方式：类和实例对象都可以调用。  

* 语法形式 
```
class TestClass(object):

    @classmethod
    def class_method(cls):
        print('类方法‘)
```

* 实践
```
class Person:
    count = 0  # 类变量

    def __init__(self, name, gender):
        self.name = name  # 实例变量，不允许类调用
        self.gender = gender
        Person.count += 1

    def get_name(self):
        return self.name  # 实例方法，需要实例化才能使用

    @classmethod  # 加装饰器才能标识为类方法
    def get_instance_count(cls):
        return Person.count

    @classmethod
    def create_a_instance(cls):
        print('create instance')
        return Person('plumrx','女')#类方法里虽然不可以使用实例变量，但是可以创建实例。

print(Person.get_instance_count())#用类调用
print(Person.create_a_instance().get_name())

# 用对象调用
student=Person('plumrx','女')
print(student.get_instance_count())

--------------------------
0
create instance
plumrx
2

```
* 区别：一个是单纯的类名调用，不需要初始化；另一个需要初始化。


### 12.python 自定义异常类
* 解决问题：系统提供的异常类型不能满足开发需求，可以创建自己的异常。

* 用法：异常类继承自 Exception 类，可以间接或直接继承。大多数的自定义异常的名字，以‘Error’结尾，尽量跟标准的异常明明保持一样。

* 实践
```
# 继承基类
class MyError(Exception):
    def __init__(self,msg):
        self.msg=msg

    def __str__(self):
        return self.msg

try:
    raise MyError('错误类型')
except MyError as e:
    print('My exception occured',e.msg)

------------------------
My exception occured 错误类型
```