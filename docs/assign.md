作业
===


---

<!--

<a name="lab8"></a>
### Python练习 Lab8 (Dec/20/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab8](http://ic.openjudge.cn/lab8/)

---

<a name="lab7"></a>
### Python练习 Lab7 (Dec/13/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab7](http://ic.openjudge.cn/lab7/)

---

<a name="assign3"></a>
### Python作业 Assign3 (Dec/13/2017)

** 单词统计 **

统计一个文本文件中单词的出现次数。

网站 [http://www.gutenberg.org](http://www.gutenberg.org)提供了很多名著
的TXT版本供下载.  例如, 小说 **ALICE'S ADVENTURES IN WONDERLAND** 的一个TXT 
版本在[http://www.gutenberg.org/cache/epub/19033/pg19033.txt](http://www.gutenberg.org/cache/epub/19033/pg19033.txt).

请写一个单词统计的程序, 对于给定URL的文件, 输出单词的出现频率, 按频率大小倒序
输出, 如下所示.


```
the		    807
and		    404
a		    328
to		    327
of		    318
she		    237
in		    227
it		    183
you		    171
alice		168
```

** 如何得到要统计的文件的URL **

要统计的文件的URL通过命令行参数给出。

假定有一个Python 文件 t.py, 其源码如下:

```python
import sys

print(sys.argv)
```

在终端中分别以如下方式运行 t.py, 观察执行结果

```
$ python t.py
['t.py']
$ python t.py 1 2 3
['t.py', '1', '2', '3']
```

sys.argv 列表保存了用户传递给Python的参数, 参数都是字符串。argv[0] 永远
存在, 通常是Python文件的名字。其他参数(如果给出了)在 argv[1:]中。

本次作业的完整代码框架如下:

```python
#!/usr/bin/env python3

"""wcount.py: count words from an Internet file.

__author__ = "Zhangsan"
__pkuid__  = "1600012345"
__email__  = "zhangsan@pku.edu.cn"
"""

import sys
from urllib.request import urlopen


def wcount(lines, topn=10):
    """count words from lines of text string, then sort by their counts
    in reverse order, output the topn (word count), each in one line. 
    """

    # your code goes here
    pass

if __name__ == '__main__':

    if  len(sys.argv) == 1:
        print('Usage: {} url [topn]'.format(sys.argv[0]))
        print('  url: URL of the txt file to analyze ')
        print('  topn: how many (words count) to output. If not given, will output top 10 words')
        sys.exit(1)

    try:
        topn = 10
        if len(sys.argv) == 3:
            topn = int(sys.argv[2])
    except ValueError:
        print('{} is not a valid topn int number'.format(sys.argv[2]))
        sys.exit(1)

    try:
        with urlopen(sys.argv[1]) as f:
            contents = f.read()
            lines   = contents.decode()
            wcount(lines, topn)
    except Exception as err:
        print(err)
        sys.exit(1)
```

你的任务是实现 wcount 函数, 使上述框架功能完整正确。

运行 

```
$ python wcount.py http://www.gutenberg.org/cache/epub/19033/pg19033.txt 20
```

或者

```
$ python wcount.py http://www.gutenberg.org/cache/epub/19033/pg19033.txt
```

将得到如前所示的结果。

** 要求: **

提交一个文件 wcount.py , 实现上述要求. 


** 提交: **

在线提交到 [北京大学课程网](http://course.pku.edu.cn)

**提交截止:**

2017年12月26日23:59

---

<a name="lab6"></a>
### Python练习 Lab6 (Dec/06/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab6](http://ic.openjudge.cn/lab6/)

---


<a name="assign2"></a>
### Python作业 Assign2 (Nov/29/2017)

** 汇率计算 **


本次作业来源于
[http://www.cs.cornell.edu/courses/cs1110/2016fa/assignments/assignment1/index.php#service](http://www.cs.cornell.edu/courses/cs1110/2016fa/assignments/assignment1/index.php#service)

** 简要说明 **

网站 [http://cs1110.cs.cornell.edu/2016fa/a1server.php?](http://cs1110.cs.cornell.edu/2016fa/a1server.php?) 提供了查询计算汇率的功能。要正确使用，需要访问时提供参数 :

`from=source&to=target&amt=amount`

其中 `source`, `target`是由三个字母代表的货币名称, `amt`是要计算的数值,

例如 `from=USD&to=EUR&amt=2.5` 表示 计算 2.5美元对应多少欧元

完整的 URL请求为:

[http://cs1110.cs.cornell.edu/2016fa/a1server.php?from=USD&to=EUR&amt=2.5](http://cs1110.cs.cornell.edu/2016fa/a1server.php?from=USD&to=EUR&amt=2.5)

在浏览器中输入该地址, 得到的结果类似为:

`{ "from" : "2.5 United States Dollars", "to" : "2.24075 Euros", "success" : true, "error" : "" }`

本次作业的主要目标, 就是分析得到的字符串, 从里面获取需要的结果.

你需要实现的函数:

```python
def exchange(currency_from, currency_to, amount_from):
    """Returns: amount of currency received in the given exchange.

    In this exchange, the user is changing amount_from money in 
    currency currency_from to the currency currency_to. The value 
    returned represents the amount in currency currency_to.

    The value returned has type float.

    Parameter currency_from: the currency on hand
    Precondition: currency_from is a string for a valid currency code
    
    Parameter currency_to: the currency to convert to
    Precondition: currency_to is a string for a valid currency code
    
    Parameter amount_from: amount of currency to convert
    Precondition: amount_from is a float"""
```

** 如何在Python3中访问URL **

```python
from urllib.request import urlopen

doc = urlopen('http://cs1110.cs.cornell.edu/2016fa/a1server.php?from=USD&to=EUR&amt=2.5')
docstr = doc.read()
doc.close()
jstr = docstr.decode('ascii')
```

doc.read() 返回的是字节流, 如:
`b'{ "from" : "2.5 United States Dollars", "to" : "2.24075 Euros", "success" : true, "error" : "" }'`
可以调用 `decode`方法得到正常的字符串.


** 迭代开发过程 **

本次作业建议采取迭代开发方法, 从基本功能开始, 逐渐完成最终功能, 具体可参考 

[http://www.cs.cornell.edu/courses/cs1110/2016fa/assignments/assignment1/index.php#iterative](http://www.cs.cornell.edu/courses/cs1110/2016fa/assignments/assignment1/index.php#iterative)

** 编写测试函数 **

对于迭代开发中实现的每一个函数, 需要提供一个测试函数, 测试其是否正确, 可
以用Python语言的 `aasert` 函数编写测试代码.

```python
def test_get_from()
    assert('USD' == get_from(json))
```

你需要写一个 `testAll` 函数, 里面测试所有你编写的测试函数

```python
def testAll()
    """test all cases"""
    test_get_from()
    test_B()
    test_C()
    print("All tests passed")
```

注意: 所有的测试函数和被测函数都在同一个文件 currency.py 中, 不需要单独建立测试文件, 这和[http://www.cs.cornell.edu/courses/cs1110/2016fa/assignments/assignment1/index.php](http://www.cs.cornell.edu/courses/cs1110/2016fa/assignments/assignment1/index.php) 要求不同。

** 要求: **

提交一个文件 currency.py , 实现上述要求. 

75分: 正确实现了 `exchange` 函数

85分: 提供了测试函数

95分: 提供了模块说明和函数说明

** 提交: **

在线提交, 将currency.py 提交到 [北京大学课程网](http://course.pku.edu.cn)

**提交截止:**

2016年12月19日23:59

---


<a name="lab5"></a>
### Python练习 Lab5 (Nov/29/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab5](http://ic.openjudge.cn/lab5/)

---


<a name="lab4"></a>
### Python练习 Lab4 (Nov/22/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab4](http://ic.openjudge.cn/lab4/)

---


<a name="assign1"></a>
### Python作业 Assign1 (Nov/15/2016)

** 内容及要求: **

用python的turtle库, 写一个程序 planets.py, 能仿真太阳系水金火木土地球6大行星围绕太阳的运行轨迹. 如下图所示:

<img src="../images/assign_planets.png" width=480>

planets.py要符合基本的python [编程规范](python.md#style).

**提交:**

在线提交, 将planets.py 提交到 [北京大学课程网](http://course.pku.edu.cn)

**提交截止:**

2017年11月28日23:59

---

<a name="lab3"></a>
### Python练习 Lab3 (Nov/15/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab3](http://ic.openjudge.cn/lab3/)

---


<a name="lab2"></a>
### Python练习 Lab2 (Nov/8/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab2](http://ic.openjudge.cn/lab2/)

本次Lab程序输出是图形, 系统无法测试, 请自己本地测试后提交.

---

<a name="lab1"></a>
### Python练习 Lab1 (Nov/1/2017)

** 内容及要求: **

[http://ic.openjudge.cn/lab1](http://ic.openjudge.cn/lab1/)

---

<a name="lab0"></a>
### Python练习 Lab0 (Oct/25/2017)

!!! note ""
    注: 本次作业不需提交

** 内容: **

0. 熟悉自己所用操作系统的[终端操作](cmd.md), 可以在终端下复制、查看文件, 改变当前路径等

1. 在自己的机器上安装[anaconda](https://mirror.tuna.tsinghua.edu.cn/anaconda/archive/)/[miniconda](https://mirror.tuna.tsinghua.edu.cn/anaconda/miniconda/), 熟悉 python3, ipython, jupyter, pip的用法, 选择并熟悉一个编辑器或IDE

2. 在 [http://iwork.pku.edu.cn](http://iwork.pku.edu.cn) 上申请成为正式用户, 阅读Help, 创建 Workspace, 熟悉 Notebook 工作环境

3. 在Notebook 中 打印运行 `print('Hello Python!')`

4. 在Notebook 中 创建一个 **markdown** 类型的Cell, 写一段markdown 文本

**提交:**

无

**提交截止:**

无

---

<a name="ihw5"></a>
### 概论作业5 (Oct/18/2017)

1. 北京大学某单位的某台机器IP地址为162.105.203.160, 子网掩码为255.255.255.128，

    - 1) 该单位的网络号(网络+子网)是多少？

    - 2) 该单位理论上可容纳多少主机？

    - 3) 北大可以有多少个这样的子网(假定北大全部是162.105网段)？

2. 小明打开浏览器, 输入地址 http://course.pku.edu.cn 发现打不开网页, 请帮助他分析问题可能出在哪儿。

3. 请为自己写一个个人WEB主页, 介绍自己, 主页上要有自己的照片.

**提交:**

本次作业以html文件及附件提交. 其中 第1-2题的题目及解答, 作为自己个人主页介绍中的一个条目 ** 作业 ** 出现.

如果有多个附件如图片, CSS, JS, 请用zip打包压缩后上传.

上传前, 请测试确保你对显示效果满意.

以 html 附件提交到 [北京大学课程网](http://course.pku.edu.cn)

**提交截止:**

2017年10月31日23:59

** 如何测试html **

假定你写好了一个html文件, 路径是 `C:\web\index.html`,  如何测试?

1. 首先, 你需要运行一个WEB服务器. 如果你已经安装了python3, 

打开终端(Windows, OS/X, Linux [各自不同](cmd.md)), 进入 C:\web 目录, 运行 

```bash
python3 -m http.server 9090
```

将会启动一个简单的Python Web服务器, 监听 9090 端口

2. 打开浏览器, 输入 `http://localhost:9090/index.html`

可以看到你的html的显示效果

----

<a name="ihw4"></a>
### 概论作业4 (Oct/11/2017)

回答下述问题

1. 解释作业、进程、线程的概念，进程和线程概念的提出分别解决了什么问题？

2. 描述哲学家就餐问题及解法，说明同步、互斥、死锁、活锁的概念。

3. 了解磁盘、分区、简单卷、跨区卷等磁盘管理中的概念。

**提交:**

在线提交, 或者以doc,pdfl 附件提交到 [北京大学课程网](http://course.pku.edu.cn)

**提交截止:**

2017年10月17日23:59

<hr>

<a name="ihw3"></a>
### 概论作业3 (Sep/27/2017)

**内容:**

1. SSD 硬盘的工作原理是什么？有哪些优点和缺点？

2. 现代计算机系统中到处都用到了缓存。你打开浏览器, 访问某网站, 查看了一幅图片, 请描述可能有哪些缓存系统参与了该过程？

3. 请调研处理器和存储技术的发展趋势.

**提交:**

在线提交, 或者以doc,pdfl 附件提交到 [北京大学课程网](http://course.pku.edu.cn)

**提交截止:**

2017年10月10日23:59

<hr>


<a name="ihw2"></a>
### 概论作业2 (Sep/20/2017)

**内容:**

1. 某一音频信号采样频率为8000次/秒, 每次采样有256个不同的数据值, 求每秒
需要多少位来表示这个信号. 

2. 你有一个数码照片文件，希望在彩色激光打印机上打印出来，如何计算能打印的最
大尺寸？

3. 某基于 IEEE 754浮点数格式的 16 bit 浮点数表示, 有 9 个小数位, 请给出 
±0, ±1.0, 最大非规范化数, 最小非规范化数, 最小规范化浮点数, 最大规范化浮点数,  
±∞,  NaN 的二进制表示(表示形式请参照讲义).

**提交:**

在线提交, 或者以doc,pdfl 附件提交到 [北京大学课程网](http://course.pku.edu.cn)

**提交截止:**

2017年9月26日23:59

<hr>

-->

<a name="ihw1"></a>
### 概论作业1 (Sep/13/2017)

!!! note ""
    注: 本次作业不需提交

**内容:**

1. 记住键盘布局, 可以熟练盲打.

2. 拥有一个可靠的邮件帐号, 学习写邮件的礼仪，养成每天定时收发邮件的习惯.

3. 在 [https://github.com](https://github.com) 上注册一个账号, 了解如何使用
   [git](https://git-scm.com), 学习如何利用 github .

**提交:**

无

**提交截止:**

无


