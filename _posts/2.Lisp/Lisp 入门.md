# <Lisp>Lisp 入门

LISP 是 LISt Processor 的缩写，是“列表处理语言”意思。
Lisp语言最初是由美国的 John McCarthy 在 1958 年提出来的，是最早的计算机语言之一。然而，半个多世纪后的今天，Lisp 语言仍然在使用，并且还会继续被使用，这和它独特的结构是分不开的。Lisp的基本框架可以容下任何修订或扩充。而且 LISP 语言在符号处理方面的优势，LISP 最初使用于人工智能处理。（早期有部分人工智能的研究者认为：“符号演算系统可以衍生出智能。”）
《黑客与画家》的作者 Paul Graham 就对 Lisp 语言赞誉有加，认为大部分的现代语言都在向 Lisp 靠近。

## 安装 Mac 环境
在已安装 HomeBrew 前提下
在 Terminal 键入

```Shell
brew install sbcl
```
或者

```Shell
brew install clisp
```

据说Clisp比sbcl更加正宗？

## 开始
Terminal 中键入

```shell
sbcl
```

会有

```shell
This is SBCL 1.4.6, an implementation of ANSI Common Lisp.
More information about SBCL is available at <http://www.sbcl.org/>.

SBCL is free software, provided as is, with absolutely no warranty.
It is mostly in the public domain; some portions are provided under
BSD-style licenses.  See the CREDITS and COPYING files in the
distribution for more information.
*
```
这个就是 Lisp 的解释器，在这里我们首先要知道退出是 (quit)。

解译器的功能就是对一个输入的表达式求值的东西。

## 函数
在 Python 中1+2

```Python
1 + 2
```

而在 Lisp 中1+2

```Lisp
(+ 1 2)
```

在Python中 + 是运算符，而在 Lisp 中 + 是运算符，同时也是函数，但它是前缀表达。

在数学中，一听到函数就是 f(x) 或者 f(x,y)，而 Lisp 中我们会表示成

```Lisp
(f x)

(f x y)
```

加法可以看成一个二元函数写作

```
+(1, 2)
```

而在 Lisp 中写作

```Lisp
(+ 1 2)
```

在上面已经出现过。

## Lisp 7大公理
1. Quote(')
2. Atom
3. Eq
4. Car
5. Cdr
6. Cons
7. Cond

所谓说7大公理，就是说所有 Lisp 中的函数均能用以上7个表达式或者说函数来表达。

Qutoe 函数能防止 Lisp 对括号内的内容(括号内的内容又称为Lisp的列表)求值。

Eq 函数只能对原子求是否相同，即是否相同的对象。

## 一些函数和结果
```Lisp
(car '(1 2 3));;;1 # 实际是取二叉树的左支
(cdr '(1 2 3));;;(2 3) # 实际是取二叉树的右支

;;;一些鸡贼的表达
(cadr '(1 2 3));;;2 # 相当于(car (cdr '(1 2 3))
(caddr '(1 2 3));;;3

(cons 1 '(2 3));;;(1 2 3) # 合并表
(cons 1 2);;;(1 . 2) # . 用于区分二叉树的左支和右支，所以实际上表是二叉树
(cons '(1 2) '(3 4));;;((1 2) 3 4) # 省略了 . 但 (1 2) 依旧是左支

(append '(3 3) '(4 4));;;(3 3 4 4)
(append '((3)) '(4 4));;;((3) 4 4) # 所以 append 是去掉一层括号然后合并

(list 1 2 3 4);;;(1 2 3 4)
(list '(1 2) '(3) 4);;;((1 2) (3) 4) # List 是把所有参数放入一个表中返回

(first '(1 2 3));;;1
(last '(1 2 3));;;(3)
```

Lisp中有一个叫原子的东西，还不清楚是什么，反正不可再分，是一个很基础的概念。

> 原子可以是任何数，分数，小数，自然数，负数等等。
> 
> 原子可以是一个字母排列，当然其中可以夹杂数字和符号。
> 
> 空表就是原子NIL。在 LISP 解释器中输入引用符（单引号）紧接着输入一个原子，可以返回这个原子本身，就像对列表的操作一样。即：

```Lisp
'abc;;;abc
```

好像是除了表和所有函数以外均是原子。

```Lisp
(atom (+ 1 1));;;T
(atom '(3));;;nil
(atom nil);;;T

```

**SETQ运算符**
Setq 可用于隐性设定全局变量，而 Let 可设置局域变量

键入

```Lisp
a
```

会报错「The variable A is unbound.」

再键入

```Lisp
(setq a 5);;;5
```

然后键入

```Lisp
a;;;5
```

## 断言函数
atom 判是否原子
null 判是否nil
equal 判是否相同
除此之外还有
Listp 判是否列表

在 Lisp 中有许多判断均已 P 结尾，而且均为断言函数。P 即 Predicate

Lisp 中一切非空(nil)一般为真

```Lisp
(null nil);;;T
(equal 's 's);;;T
```

## Cond 操作符(定理）
(cond 分支列表1 分支列表2 ... 分支列表N)

分支列表的构成: (条件P 值e)

cond 将对每一个条件P求值，如果为Nil就接着求下一个分支列表；如果为T，就返回对应的『值e』；如果没有一个真值(T)，cond 操作符就返回Nil。

## 自定函数
**DEFUN**

例：

```Lisp
(defun 2nd (x)
  (car (cdr x))
)

(2nd '(1 2 3));;;2
```

## 七大公理的牛逼之处
所有函数均有七大公理衍生出来

Null函数

```Lisp
(defun null2 (x)
  (cond 
    ((equal x nil) t)
    (t nil)
  )
)
```

And函数

```Lisp
(defun and2 (x y)
  (cond
    ((equal x nil) nil)
    ((not (equal y nil)) t)
    (t nil)
  )
)
```

Or函数

```Lisp
(defun or2 (x y)
  (cond
    ((equal x t) t)
    ((equal y t) t)
  )
)
```

Last 函数

```Lisp
(defun last2 (x)
  (cond
    ((equal (cdr x) nil) x)
    (t (last2 (cdr x)))
  )
)
```

Length函数

```Lisp
(defun len (x)
  (cond
    ((null x) 0)
    (t (+ (len (cdr x)) 1))
  )
)
```

Append函数

```Lisp
(defun append2 (x y)
  (cond
    ((eq x nil) y)
    (t (cons (car x) (append2 (cdr x) y)))
  )
)
```

Equal函数

```Lisp
(defun equal2 (x y) 
  (cond 
    ((null x) (not y))
    ((null y) (not x))
    ((atom x) (eq x y))
    ((atom y) (eq x y))
    ((not (equal2  (car x) (car y))) nil)
    (t (equal2 (cdr x) (cdr y)))
  )
)
```

If函数

```Lisp
(defun if2 (p e1 e2)
  (cond
    (p e1)
    (t e2)
  )
)
```

## 参考来源
《ANSI Commons Lisp》
[Lisp入门](https://zh.wikibooks.org/wiki/Lisp_入門)


