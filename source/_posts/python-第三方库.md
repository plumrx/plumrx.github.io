# python-第三方库
## 1.模块和包
### 1.1 模块
一个'*.py' 的文件就是一个模块，能够让你有逻辑的组织你的代码，模块能定义函数、类和变量等。  

### 1.2 包
可以将包理解为一个文件夹，包里必须要有__init__.py文件。

### 1.3 python 模块或者包引入的三种形式  
不推荐使用第三种，因为会有很多重名文件。
```commandline
imporrt module[,module2]
from modname import name1[,name2]
from modname import *
```

### 1.4 包的安装  
下载地址：
1. 官方地址： https://pypi.or 
2. 各高校的镜像：阿里云、清华等  

### 1.5 python 常用标准库及第三方库  
覆盖场景：web框架、爬虫、数据库、图片可视化、图片处理、文本处理、自然语言处理、机器学习、代码分析、日志等。  



## 2.常用标注库  
### 2.1 os 模块  
#### 2.1.1 功能
用于访问操作系统相关功能的模块，使用os 模块提供的接口，可以实现跨平台运行。  
#### 2.1.1 os 模块 常用函数  
> import os
1. os.getcwd():获取当前目录
2. os.listdir('.'):列出当前目录下所有文件，包含隐藏文件
2. os.chdir("..."):切换当前目录
3. os.removedirs:递归删除目录
4. os.makedirs('dir1/dir2'):创建多级目录
5. os.remove('filename'):删除文件

#### 2.1.2 os.path 模块常见函数用法  
1. 当前文件可用 __file__ 获取
2. 获取当前文件的绝对路径：os.path.abspath(__file__)
3. 获取文件名：os.path.basename(__file__)
4. 获取当前文件所在目录：os.path.dirname(os.path.absname(__file__))  
先取到文件的绝对路径，再取其上一级目录。

#### 2.1.3 os 模块-执行命令  
1. os.system(command)  
执行结果立马返回，返回结果为：0、-1
```
os.system("ipconfig")
---------------------
Process finished with exit code 0
```
2. popen() 
执行结果不立马返回，可使用 read 方法读取。
```
rsult=os.popen("ipconfig")
print(result)
print(result.read())
----------------------
<os._wrap_close object at 0x000001BAFF4B3450>
Windows IP 配置...
```

#### 2.1.4 os 模块-遍历目录  
1. os.listdir  
适用场景：目录下只有文件，没有文件夹，可以使用该方法。
```commandline
path1="C:\Python311\Lib\json\__pycache__"

for filename in os.listdir(path1):
//join 方法是拼接路径的作用
    print(os.path.join(path1,filename))
--------------------------
C:\Python311\Lib\json\__pycache__\decoder.cpython-311.pyc
C:\Python311\Lib\json\__pycache__\encoder.cpython-311.pyc
C:\Python311\Lib\json\__pycache__\scanner.cpython-311.pyc
C:\Python311\Lib\json\__pycache__\__init__.cpython-311.pyc
```
2. os.walk  
适用场景：目录下不仅有文件，还有文件夹，可以使用该方法。  
该模块返回值是三元元组，各元素如下：  
root:当前正在遍历的文件地址  
dirs:所有子目录  
files:所有文件  
```commandline
for root,dirs,files in os.walk(path2):
    for name in files:
        print(os.path.join(root,name))
    for name in dirs:
        print(os.path.join(root,name))
-----------------------------------------
C:\Python311\Lib\json\decoder.py
C:\Python311\Lib\json\encoder.py
C:\Python311\Lib\json\scanner.py
C:\Python311\Lib\json\tool.py
C:\Python311\Lib\json\__init__.py
C:\Python311\Lib\json\__pycache__
C:\Python311\Lib\json\__pycache__\decoder.cpython-311.pyc
C:\Python311\Lib\json\__pycache__\encoder.cpython-311.pyc
C:\Python311\Lib\json\__pycache__\scanner.cpython-311.pyc
C:\Python311\Lib\json\__pycache__\__init__.cpython-311.pyc
```


### 2.2 sys 模块   

#### 2.2.1 功能  
主要是针对 python 解释器相关的操作，不是主机操作系统。  

#### 2.2.2 常用方法或属性  
1. sys.argv  
是属性，获取当前参数列表，可用于脚本需要外部传递参数时使用。  
```commandline
import sys

# 脚本执行参数列表
# enumerate() 将一个可遍历的数据对象，组合成为一个索引序列，同时累出数据和数据下标
for index, arg in enumerate(sys.argv):
    print("第 %d 个参数是：%s"%(index,arg))

print(sys.argv)
------------------------------
PS D:\project\***> python python-demo.py a b c
第 0 个参数是：python-demo.py
第 1 个参数是：a
第 2 个参数是：b
第 3 个参数是：c
['python-demo.py', 'a', 'b', 'c']
```
2. sys.path  
搜索模块的路径集，返回一个 list。搜索当前环境下python的路径。
```commandline
import sys

for path in sys.path:
    print(path)
----------------------------
C:\Python311\python311.zip
C:\Python311\Lib
C:\Python311\DLLs
C:\Python311
C:\Python311\Lib\site-packages
```

3. 标准输入 sys.stdin & input()    

4. 标准输出 


