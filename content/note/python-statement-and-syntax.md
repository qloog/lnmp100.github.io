Title: python学习笔记之Python基础知识
Date: 2014-05-24 09:34:20
Tags: python

## 语句和语法

### 注释(#)

和很多Unix脚本类似，Python注释语句从#字符开始，注释可以在一行的任何地方开始，解释器会忽略掉该行#之后的所有内容。

### 继续(\\)

一般使用换行分隔，即一行一个语句，当一行语句过长的时候可以使用反斜线(\\)分解成几行。例如：

    # check conditions
    if (wheathe_is_hot == 1) and \
       (shark_marings ==0):
        send_goto_beach_mesg_to_pager()

有两种例外情况一个语句不使用反斜线也可以跨行。

 1. 在含有小括号、中括号、花括号时可以多行书写，例如给一些变量赋值：

    go_surf, get_a_tan_while, boat_size, toll_money = (1,
        'windsurfing', 40.0, -2.00)

 2. 三引号包括下的字符串可以换行。例如显示一个三引号字符串：

    print '''hi there, this is a long message for you
    that goes over multiple lines...you will find
    out soon that triple quotes in Python allows
    this kind of fun! it is like a day on the beach!'''

在反斜线和括号使用的选择上，推荐使用括号，因为它的可读性会更好。

### 多个语句构成代码组(:)

缩进相同的一组语句构成一个代码块，称之为代码组。
像if、while、def和class这样的复合语句，行首以关键字开始，以冒号(:)结束，
该行之后的一行或多行代码构成代码组，行首及后面的代码组称为一个子句(clause)。

### 代码组由不同的缩进分隔

代码的层次关系是通过同样深度的空格或者制表符缩进体现的。
必须严格左对齐，负责同一组的代码可能会被当成另一个组，甚至会导致语法错误。
 **核心风格** **缩进4个空格宽度**，避免使用制表符(使用这个会报语法错误)
没有缩进的代码块是最高层级的，称为脚本的"主题"（main）

### 同一行书写多个语句(i)

分号(;)允许你将多个语句写在同一行上，语句之间用分号隔开。
同一行上书写多个语句会大大降低代码的可读性，不提倡(虽没语法错误)

### 模块

每一个Pyhon脚本文件都可以被当成是一个模块。模块以磁盘文件的形式存在。
当一个模块变的过大，并且驱动了太多功能的话，就应该考虑拆一些代码出来另外建一个模块。
模块里的代码可以是一段直接执行的脚本，也可以是一对类似库函数的代码，从而可以被别
的模块导入(import)调用。

## 变量赋值

### 赋值操作符

等号(=)最主要的赋值操作符

    anInt = -12
    aString = 'cart'
    aFloat = -3.1415 * (5.0 ** 2)
    anotherString = 'shop' + 'ping'
    aList = [3.14e10, '2nd elmt of a list', 8.82-4.371j]

**注意** 赋值并不是直接将一个值赋给一个变量，这和其他语言有所不同。
在Python中，对象是通过引用传递的。
不管这个对象是新建还是已经存在，都是将该对象的引用(并不是值)赋值给变量

与C不同，Python的赋值语句不会返回值，如：

    >>> x = 1
    >>> y = (x = x + 1)     #赋值语句不是合法表达式
      File "<stdin>", line 1
        y = (x = x + 1)
               ^
    SyntaxError: invalid syntax

链式操作是可以的，如：

    >>> y = x = x + 1
    >>> x, y
    (2, 2)

### 增量赋值

等号和一个算术操作符组合在一起，将计算结果重新赋值给左边的变量。

    x = x + 1 可以写成：x += 1

类似的还有 `+= -= *= /= %= **= <<= >>= &= ^= |=`

A += 10 A仅被修改一次，可变对象会被就地修改(无修拷贝引用) 。
不可变对象则和A=A+B的结果一样(分配一个新对象)。(不太好理解)

    >>> m = 12
    >>> m %= 7
    >>> m
    5
    >>> m **= 2
    >>> m
    25
    >>> aList = [123, 'xyz']
    >>> aList += [45.6e7]
    >>> aList
    [123, 'xyz', 456000000.0]

Python不支持类似x++ 或x-- 这样的前置/后置自增/自减运算。

### 多重赋值

    >>> x = y = z = 1
    >>> x
    1
    >>> y
    1
    >>> z
    1

一个值为1的整形对象被创建，该对象的同一个引用被复制给x、y和z。
即 将一个对象赋给了多个变量

### "多元"赋值

将多个变量同时赋值的方法成为多元赋值(multuple)。
要求等号两边的对象都是元祖。

    >>> x, y, z = 1, 2, 'a string'
    >>> x
    1
    >>> y
    2
    >>> z
    'a string'

通常元祖需要用圆括号(小括号)括起来，虽然是可选，但最好还是加上圆括号以提高可读性

    >>> (x, y, z) = (1, 2, 'a string')

在C语言中，两个值交换，一般会用到一个临时变量tmp，如

    /* C语言中两个变量交换 */
    tmp = x;
    x = y;
    y = tmp;

但在python中不用那么麻烦。
Python的多元赋值方式可以实现**无需中间变量交换两个变量的值**

    # Python中两个变量交换
    >>> x, y  = 1, 2
    >>> x
    1
    >>> y
    2
    >>> x, y = y, x
    >>> x
    2
    >>> y
    1


## 标示符

标识符是计算机语言中允许作为名字的有效字符串集合。
其中一部分是 关键字，构成语言的标识符，这部分其实是关键字，不能用于其他用途。

### 合法的Python标识符

 * 第一个字符必须是字母或者下划线(`_`);
 * 剩下的字符可以是字母和数字或下划线;
 * 大小写敏感

### 关键字

 and   | as       | assert | break
 class | continue | def    | del
 elif  | else     | except | exec
 finally | for | from | global
 if || import | in | is
 lambda | not | or | pass
 print | raise | return | try
 while | with | yield | None

有时我们不知道某个字到底是不是关键字，没关系，python为我们提供了一个关键字模块。如：

    >>> import keyword
    >>> print keyword.iskeyword('and')
    True
    >>> print keyword.iskeyword('test')
    False
    >>> print keyword.iskeyword('raise')
    True
    >>> print keyword.iskeyword('yield')
    True
    >>> print keyword.iskeyword('hello')
    False
    >>> print keyword.iskeyword('python')
    False

### 内建

built-in 是 `__builtins__` 模块的成员。在程序开始或者在交互解释器中给出>>>之前，由解释器自动导入。
属于全局变量

### 专用下划线标识符

Python用下划线作为变量前缀和后缀指定特殊变量。

 * `_xxx` 不用 `from module import *` 导入
 * `_xxx_` 系统定义名字
 * `_xxx` 类中的私有变量名

一般来讲，变量名\_xxx 被看做是"私有的"，在模块或类外不可以使用。所以我们`避免用下划线作为变量名的开始`

## 基本风格指南

 1. 注释

注释对于自己和后来人都是非常重要的，特别是对那些很久没有被动过的代码而言，注释更显的有用了。
既不能缺少注释，也不能过渡使用注释。尽量使注释简洁明了，并放在最合适的地方。
这样注释便为每个人节省了时间和经理。
同时要注意注释的准确性

 2. 文档

可通过`__doc__`变量来动态获取文档字串。
在模块、类声明、或者函数声明中第一个没有赋值的字符串可以用属性`obj.__doc__` 来进行访问。

 3. 缩进

4个空格，建议不要用tab

 4. 选择标识符名称

请为变量名选择短而意义丰富的标识符。

Python风格指南

Pythonic: 指以Python的方式去编写代码、组织逻辑和对象行为。
通过在解释器下输入`import this`来获得python之禅

    >>> import this
    >>> The Zen of Python, by Tim Peters
    >>>
    >>> Beautiful is better than ugly.
    >>> Explicit is better than implicit.
    >>> Simple is better than complex.
    >>> Complex is better than complicated.
    >>> Flat is better than nested.
    >>> Sparse is better than dense.
    >>> Readability counts.
    >>> Special cases aren't special enough to break the rules.
    >>> Although practicality beats purity.
    >>> Errors should never pass silently.
    >>> Unless explicitly silenced.
    >>> In the face of ambiguity, refuse the temptation to guess.
    >>> There should be one-- and preferably only one --obvious way to do it.
    >>> Although that way may not be obvious at first unless you're Dutch.
    >>> Now is better than never.
    >>> Although never is often better than *right* now.
    >>> If the implementation is hard to explain, it's a bad idea.
    >>> If the implementation is easy to explain, it may be a good idea.
    >>> Namespaces are one honking great idea -- let's do more of those!

### 模块结构和布局

用模块来合理的组织你的Python代码是简单又自然的方法。下面是一种非常合理的布局

 * (1) 起始行(Unix)
 * (2) 模块文档
 * (3) 模块导入
 * (4) 变量定义
 * (5) 类定义
 * (6) 函数定义
 * (7) 主程序

典型的Python文件结构如图
![python-file-structure](/static/uploads/2014/05/typical-python-file-structure.png)

注意：

 * 如果模块是被导入，`__name__` 的值为模块名字
 * 如果模块是被直接执行，`__name__` 的值为`__main__`

## 内存管理

 * 变量无需事先声明
 * 变量无需指定类型
 * 程序员不用关心内存管理
 * 变量名会被"回收"
 * del 语句能够直接释放资源

### 变量定义

变量只有被创建和赋值后才能被使用

    >>> a
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'a' is not defined

变量一旦被赋值，就可以通过变量为来访它

    >>> x = 4
    >>> y = 'this is a string'
    >>> x
    4
    >>> y
    'this is a string'

### 动态类型

Python 中不但变量名无需事先声明，而且也无需类型声明。
对象的类型和内存占用都是运行时确定的。尽管代码被编译成字节码，但是仍然是一种解释性语言。
创建--也就是赋值时，解释器会根据语法和右侧的操作数来决定新对象的类型。
在对象创建后，一个该对象的应用会被赋值给左侧的变量。

### 内存分配

Python解释器承担了内存管理的复杂任务，大大简化了应用程序的编写。
你只需要关心你要解决的问题，至于底层的事情放心交给Python解释器去做就行啦。

### 应用计数

引用计数器：一个内部跟踪变量
引用计数：每个对象各有多少个引用
垃圾回收：对象的引用计数为0时

 1. 增加引用计数
    x = 3.14
    y = x

    对象的引用计数增加时：

    * 对象被创建： x = 3.14
    * 另外的别名被创建：y = x
    * 被作为参数传递给函数： foobar(x)
    * 成为容器时的一个元素： myList = [123, x, 'xyz']

 2. 减少引用计数
    foo = 'xyz'
    bar = foo
    foo = 123

    对象的引用计数自动减1

    * 一个本地引用离开了其作用范围。
    * 对象的别名被显示销毁     del y  # or del x
    * 对象的一个别名被赋值给其他对象   x = 123
    * 对象被从一个窗口对象中移除   myList.remove(x)
    * 窗口对象本身销毁  del myList # or goes out-of-scope

 3. del语句
    删除对象的一个引用，而且是最后一个引用,最后对象的引用计数会减为0
    del obj1[, obj2[,...objN]]

### 垃圾收集

不再使用的内存会被一种称为垃圾收集的机制释放。
垃圾收集器是一块独立代码，它用来寻找引用计数为0的对象，也负责检查那些虽然引用计数大于0
但也应该被销毁的对象。

