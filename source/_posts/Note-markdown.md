---
title: Note - markdown
date: 2019-02-27 23:20:27
tags:
---
### 一、区块元素
#### 1.特殊字符自动转换
在html文件中，有些字符需要进行转换，以下两个需要特别注意：
##### Example :

    ‘ < ’ --> ‘ &lt; ’

    " 4 < 5" --> "4 &lt; 5"
> "4 < 5"->"4 &lt; 5"


##### Example :

    ‘ & ’ --> ' &amp; '

    "A & T" --> "A &amp T"
> "A & T"->"A &amp T"


#### 2.段落与换行
一个markdown文档是由一个或者多个的文本行组成，它的前后要有一个以上的空行。
空格或者制表符皆视为空行，所以普通段落不应该采用空格或者制表符进行缩进。markdown可以使用段内强制换行，只需要在换行处加入两个空格和一个回车即可。

#### 3.标题
markdown支持两种标题格式：类 Setext 和类 atx 形式。
##### 3.1 类 Setext
由于该种语法格式标题级别有限，实用性低不做详细阐述。
##### 3.2 类 atx 形式
对于 markdown 标题的语法格式，我个人更推荐使用类 atx 形式。
语法格式：在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶，注意 # 后需加一个空格。
##### Example：

    # 这是 H1
    ## 这是 H2
    ###### 这是 H6
>   # 这是 H1
>   ## 这是 H2
>   ###### 这是 H6


#### 4.区块引用
语法格式：在需要进行引用的区段，每一行的最前面加上 '>' ，或者只在第一行加上 '>'。
引用还可以嵌套，只要在需要嵌套引用的部分再加上 '>' 即可。
##### Example：

     This is the first level of quoting.
    >
    > > This is nested blockquote.
    >
    > Back to the first level.
 This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.

"This is nested blockquote." 这一句就使用了嵌套引用。


#### 5.列表
Markdown 支持有序列表和五序列表。
##### 5.1 无序列表
无序列表使用星号、加号或是减号作为列表标记：
##### Example：

    *   Red   
    +   Green 
    -   Blue 

>   *   Red  
>   +   Green  
>   -   Blue  

##### 5.2 有序列表
有序列表则使用数字接着一个英文句点：
**注意!** 可以不在意数字数字的正确性。但是，列表第一项的数字代表了起始值。如果第一项为'1.'，那么该有序列表编号从'1.'开始；如果第一项为'3.'，就从'3.'开始。示例便是从'3.'开始排列。
##### Example：

    3.  Bird
    5.  McHale
    4.  Parish
    3.  Cat
>   3.  Bird
>   5.  McHale
>   4.  Parish
>   3.  Cat


#### 6.代码区块
通常“和程序相关的写作“或是”标签语言原始码块”我们并不希望它以一般段落文件的方式去排版，而是照原来的样子显示，就需要用到代码区块。
代码区块语法格式：缩紧4个字符或者1个制表符。
##### Example：
这是一个普通段落：

    这是一个代码区块。

#### 7.分割线
分割线语法结构：一行中使用三个以上的星号（*）、减号（-）、底线（_）即可形成分隔符，注意行内不能有其他任何字符。

    * * *

    ***

    *****

    - - -

    ---------------------------------------

> * * *
> ***
> *****
> - - -
> ---------------------------------------



### 二、区段元素

#### 1.链接
markdown支持两种方式的链接语法：**行内式**和**参考式**。链接文字都是用[方括号]来进行标记的。
##### 1.1 行内式
行内式语法结构，如下所示：
其中 Title 表示鼠标 hover 时显示的内容，非必须。

    [方括号](圆括号 + "Title")
Example:

    This is [an example](http://example.com/ "Title") inline link.
> This is [an example](http://example.com/ "Title") inline link.



##### 1.2 参考式
行内式语法结构，如下所示。
先在文件中记录链接，随后在文件的任意处将其定义出来。

    [方括号][ID]
    [ID]:link + 'title'

Example 1:

    This is [an example] [001]
    [001]: http://example.com/  'Optional Title Here'
> This is [an example] [001]
>> [001]: http://example.com/  'Optional Title Here'


#### 2. 强调
用 */_ 标记需要强调的字词，**前后符号需一致**。单个符号表示斜体，双符号表示加粗。
##### 2.1 斜体
    *single asterisks*
    _single underscores_
> *single asterisks*
> _single underscores_
##### 2.2 加粗
    **double asterisks**
    __double underscores__
>**double asterisks**
>__double underscores__

如果要在要在文字中插入普通的 */_ ，可以使用反斜线。

    \*this text is surrounded by literal asterisks\*
> \*this text is surrounded by literal asterisks\*



#### 3. 代码
标记一小段行内代码，可以用反引号将其包起来。

    Use the `printf()` function.
> Use the `printf()` function.

如果要在代码内插入反引号，则可以使用多个反引号开始和结束代码段。

    ``There is a literal backtick (`) here.``
> ``There is a literal backtick (`) here.``


#### 4. 图片
Markdown中插入图片和插入链接的语法类似。
##### 4.1 行内式
插入图片的语法结构，其中 Title 是可以省略的一项。
* !
* [图片的替代文字]
* (图片地址 "Title")


    ![Alt text](/path/to/img.jpg "Optional title")
> ![Alt text](/path/to/img.jpg "Optional title")

##### 4.2 参考式


    ![Alt text][id]

> ![Alt text][id]

> [id]: url/to/image  "Optional title attribute"

目前为止，markdown还没有办法设定图片的宽高，如果有需要也可以使用 '\<img\>'标签



### 三、 其他
#### 1. 反斜杠
markdown中可以利用反斜杠来插入一些具有语法含义的符号。

    \*literal asterisks\*
> \*literal asterisks\*

以下这些符号支持前面加上反斜杠，以帮助插入普通符号。

    \   反斜线
    `   反引号
    *   星号
    _   底线
    {}  花括号
    []  方括号
    ()  括弧
    #   井字号
    +   加号
    -   减号
    .   英文句点
    !   惊叹号




#### 2. 自动链接
markdown支持以较为简短的自动连接形式，来处理**网址**和**电子邮箱**。

    <http://example.com/>
> <http://example.com/>

    <address@example.com>
> <address@example.com>


##### 参考文献：
 Markdown 语法说明 http://www.markdown.cn
