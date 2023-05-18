---
title: python - 文件 IO 读写
date: 2022-07-24 14:14:56
tags:
---

## 一、文件常用操作

### 1. 绝对路径和相对路径  

* 绝对路径  
从根文件夹开始，Windows 系统中以盘符（C:\  D:\)作为根文件夹，而 OS X 或 Linux 系统中以 \ 作为根文件夹。  

* 相对路径  
指文件相对于当前工作目录所在的位置。  
'.'：当级目录  
'..'：上级目录  
当前目录：\plumrx\test  
文件目录：\plumrx\test\test1\demo.txt  
相对路径可表示为：.\test1\demo.txt  


### 2. 文件应用级操作三部曲  
1.1 open  
open()函数用于创建或打开指定文件。  
1.2 读写操作  
1.3 close  
```
f = open("14-practice.py")
print(f.read())
f.close()
------------------------------
Users/plumrx/projects/kuangshi/mysql-try/just-for-mysql/bin/python /Users/plumrx/projects/kuangshi/mysql-try/16.py
# # input
# a = input("enter a number:")
# b = input("enrer another number:")
...
```

### 3. open()函数  

open()函数用于创建或打开指定文件。 

>格式：   
>file = open(file_name[, mode='r'[, buffering=-1[, encoding = None]]])  

除了 file_name 其余为可选参数，具体含义如下：  
file:要创建的文件对象。  
file_name:要创建或打开的文件名称，需使用引号括起，使用对应路径。  
mode:指定文件的打开模式，不填写则默认为 r ，只读。  
encoding:手动设定打开文件时使用的编码格式。  
  

### 4. 文件读取 
* read()  
逐字读取文件中的内容。  
> file.read([size])  
> size 默认为所有内容  

* readline()  
逐行读取，以 '\n' 作为读取一行的标志  
> file.readline([size])  
> size 指定读取每一行时，一次最多读取的字数。   

* readlines()  
读取所有行，读取结果为一个列表，每一行为其中一个元素。  
> file.readlines()

  
### 5. 文件写入  
* write()  
想文件中写入指定内容。  
```
格式：
file.write(string)

样例：
f = open('test.txt','w')
f.write('test1\n')
f.write('test2\n')
f.write('test3')
-----------------
test1
test2
test3
```
* writelines()
将字符串列表写入文件中，每个元素之间换行。  
```
文件复制，将test1.txt -> test2.txt

f_read = open('test.txt')
f_write = open('test-copy.txt', 'w')

f_write.writelines(f_read.readlines())
f_read.close()
f_write.close()
```
  
### 6. seek()和tell()  
* tell()  
用于判断文件指针当前所处的位置。
> file.tell()

* seek()  
用于移动文件指针到指定位置。
>file.seek(offset[,whence])  

offset:  
偏移量，可正可负，正数代表向后偏移，负数代表向前  
whence:  
0:文件开头  
1:当前位置    
2:文件末尾  
```
# 设置当前指针位置为5
f_read.seek(5)

# 将指针位置放置至文件开头0
f_read.seek(0, 0)

# 指针当前位置不变
f_read.seek(0, 1)

# 将指针位置放置至文件末尾
f_read.seek(0, 2)
```

  
### 7. whith as 读写文件  
作用：操作上下文管理器，自动分配且释放资源。  
```
with 表达式[as target]:
    代码块

旧方法：
f = open('test.txt', 'w')
f.write('test')
f.close()

新方法：
with open('test.txt','w') as f:
    f.write('test')
```
  
### 8. fileinput 模块：逐行读取多个文件  
多文件读写的时候，可以查询文件的位置。  

* input 函数  
```
--------- 单文件用法 -----------
import fileinput

for line in fileinput.input('test.txt'):
    print(line, end='')

--------------------
test1
test2
test3

--------- 多文件用法 -----------

for line in fileinput.input(files=('test.txt','test-copy.txt')):
	print(line,end='')
fileinput.close()

---------------------
test1
test2
test3TEST1
TEST2
TEST3

--------- 文件内容替换 & 备份 -----------

for line in fileinput.input('test.txt', backup='.bak', inplace=True):
	print(line.replace('te', 'TE'))

---------------------
TEst1
TEst2
TEst3
```

### 9. linecache 模块：读取文件制定行  
指定行号即可

```
import linecache
import string

# 读 string 模块中的第三行
print(linecache.getline(string.__file__, 3))

# 读取普通文件的第二行
print(linecache.getline('test.txt',3))

--------------------
Public module variables:

TEst2

```








## 二、读取大文件（GB）编码技巧  
**问题：open()和read()函数仅适用于小文件的读取。文件过大，会造成内存溢出。**    

**解决办法：反复调用read(size)方法，每次指定读取字数。**  

```
伪码：
while true:
    block = f.read(1024)
    if not block: #没有内容
        break
```

**尝试读取本地500M的大文件**

1. 生成一个超过500M的文件，具体方法可以参考：
https://plumrx.github.io/2022/07/22/狗屁不通文章生成器/

2. 读取文件
```
with open('狗屁不通文章.txt') as file:
	while True:
		block=file.read(1024)
		if not block:
			break
```




## 三、python异常检测和处理  

异常官方指引文档：https://docs.python.org/3/library/exceptions.html#base-classes

### 语法类错误  

1. 函数调用语法错误  
```
print "test"  
-----------------
SyntaxError: Missing parentheses in call to 'print'. Did you mean print("test")?
```



### 运行时错误
建立在语法正确的情况下，才会发生运行错误。  

1. read()函数抛出 UnicodeDecodeError异常  
原因：目标文件使用的编码格式与open()函数打开该文件使用的编码格式不匹配。  
解决方案：在调用open()函数时，加上对应编码的参数。  

2. a = 1/0  
0作为除数没有意义，所以运行后产生错误。



### 异常处理

1. try except 异常捕获
```
# 1.1 万能异常捕获
try:
	a = 1
	print(a / 0)
except Exception:
	print('error')

---------------------
error


# 1.2 指定异常捕获
将异常的信息存储在变量 e 中
try:
	f = open('file_miss', 'r')
except IOError as e:
	print('open exception: %s: %s' % (e.args, e.strerror))
---------------------
open exception: (2, 'No such file or directory'): No such file or directory



# 1.3 指定多个异常
# 可以衔接多个 except ，以便区分多种报错类型。
try:
	a = int(input('输入被除数：'))
	b = int(input('输入除数：'))
	c = a / b
	print('商：', c)
except (ValueError, ArithmeticError) as e:
	print('数字格式 or 算术异常')
except :
	print('other error')

----------------------
输入被除数：1
输入除数：0
数字格式 or 算术异常



# 1.4 异常 & 正常流程的捕获

try:
	a = int(input('输入被除数：'))
	b = int(input('输入除数：'))
	c = a / b
	print('商：', c)
except (ValueError, ArithmeticError) as e:
	print('数字格式 or 算术异常')
else:
	print('everything is OK')

--------------------------
输入被除数：1
输入除数：1
商： 1.0
everything is OK
```
  
2. try except finally：资源回收  
功能：无论 try 块是否发生异常，都要进入 finally 语句，并执行其中代码。
```
try:
	a = int(input('输入被除数：'))
	b = int(input('输入除数：'))
	c = a / b
	print('商：', c)
except (ValueError, ArithmeticError) as e:
	print('数字格式 or 算术异常')
else:
	print('everything is OK')
finally:
	print('over')

-------------------------
输入被除数：0
输入除数：9
商： 0.0
everything is OK
over
```

3. raise：主动触发异常  
功能：一旦执行 raise 语句，raise 后面的语句将不能执行。
```
try:
	a = input('please input a number:')
	if (not a.isdigit()):
		raise ValueError('a must be a number')
except ValueError as e:
	# repr() 函数将对象转化为供解释器读取的形式
	# repr() 方法可以将读取到的格式字符，比如换行符、制表符，转化为其相应的转义字符。
	print('create error', repr(e))
	pass

-----------------------------
please input a number:a
create error ValueError('a must be a number')

```
  
4. sys.exc_info()：获取异常信息 - 一般详细  
```
import sys

try:
	print(1 / 0)
except Exception as e:
	print(e)
	print(sys.exc_info())

--------------------------
division by zero
(<class 'ZeroDivisionError'>, ZeroDivisionError('division by zero'), <traceback object at 0x10b889f00>)

```
  
5. traceback 模块：获取异常信息 - 非常详细

直接将异常全部打印出来
```
import traceback

try:
	print(1/0)
except Exception as e:
	print(e)
	print(traceback.print_exc())

----------------------------
division by zero
None
Traceback (most recent call last):
  File "/Users/plumrx/projects/kuangshi/RF52/lesson16/open_file.py", line 106, in <module>
    print(1/0)
ZeroDivisionError: division by zero
```