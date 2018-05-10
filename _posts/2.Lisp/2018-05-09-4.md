<Lisp>[SBCL中文文档](https://github.com/xishvai/sbcl-doc-zh) Via: xishvai
# 1 获得支持和报告Bug
    1.1 志愿者支持
    1.2 商业支持
    1.3 报告Bug
        1.3.1 怎么有效地报告Bug
        1.3.2 信号相关的Bug

## 1.1 志愿者支持
你的首选SBCL支持应该是`sbcl-help`邮件列表：为了帮助其他使用者，SBCL开发者管理该邮件列表并可以给出一些建议。作为一种反垃圾邮件措施，订阅是必需的：

[https://lists.sourceforge.net/lists/listinfo/sbcl-help][ref1]

记住：回答你问题的人是志愿者，所以当你问出一个好问题时，你得到好的解答的机会就会大。

在发送邮件之前，在

[http://sourceforge.net/mailarchive/forum.php?forum_name=sbcl-help][ref2]

或者在

[http://news.gmane.org/gmane.lisp.steel-bank.general][ref3]

检查列表文档来查看你的问题是否已被回答过。检查bug数据库也是值得，以明确该bug是否已经被审查通过。见1.3小结[报告Bug]。

为了提出好问题，可以参考一般建议，见

[http://www.catb.org/~esr/faqs/smart-questions.html][ref4]

## 1.2 商业支持
目前没有正式的组织开发SBCL，但是如果你需要付费支持或者定制SBCL开发，我们接下来会维护公司和客户的名单。它会用特长技能和兴趣点来标识服务提供商，并可直接联系他们。

SBCL工程不能确定这些信息的准确性或这些名单上人的能力，而且他们已经提供了他们自己的简介如下：你必须自己从可用的信息中做出合适的需求判断。这些信息可以是他们提供的链接，CREDITS文件，邮件列表档案，CVS提交信息等等。请随时在`sbcl-help`邮件列表询求建议。

(目前，在本手册中，还没有公司或顾客希望为付费支持或定制SBCL开发而打广告。)

## 1.3 报告Bug
SBCL使用Launchpad来追踪Bug。Bug数据库可以从这里访问

[https://bugs.launchpad.net/sbcl][ref5]

这需要在Launchpad注册后方能报告Bug。然而，Bug也可以在邮件列表`sbcl-bugs`中报告，这相对来说方便一些，但不需要订阅该邮件。

简单地把Bug报告邮件发送到`sbcl-bugs@lists.sourceforge.net`，这些Bug会被检查并且被SBCL维护者加到Launchpad中。

### 1.3.1 怎么有效地报告Bug
请在一个Bug报告中包含足够的信息，以使人读到它的时候能再现这个问题，例如，不要写成这样

>Subject: apparenug in PRINT-OBJECT (or *PRINT-LENGTH*?)
>
>PRINT-OBJECT doesn't see to work with *PRINT-LENGTH*. Is this a bug?

而应该写成这样

>Subject: apparent bug in PRINT-OBJECT (or *PRINT-LENGTH*?)
>
>In sbcl-1.2.3 running under OpenBSCD 4.5 on my Alpha box, when
>
>I compile and load the file
>
    (DEFSTRUCT (FOO (:PRINT-OBJECT (LAMBDA (X Y)
                                     (LET ((*PRINT-LENGTH* 4))
                                       (PRINT X Y)))))
      X Y)
>then at the command line type
    (MAKE-FOO)
>the program loops endlessly instead of printing the object.

关于有效报告Bug，更深的讨论见

[http://www.chiark.greenend.org.uk/~sgtatham/bugs.html][ref6]

### 1.3.2 信号相关的Bug
如果你碰到一个信号(signal)相关的Bug，你会得到致命错误，例如信号N被阻塞(blocked)或非阻塞(unblocked)又或挂起(hang)。且，你想发送一个有用的Bug报告，那么

1. 用`ldb`支持来编译SBCL，并且将`src/runtime/runtime.h`文件中的`#define QSHOW_SIGNAL 0`改成`#define QSHOW_SIGNAL 1`。
2. 构建一个独立的小测试用例，运行它。
3. 如果它只是挂起，用`SIGABRT`杀掉它：`kill -ABRT <pidof sbcl>`。
4. 输入`ba`来打印调用堆栈(backtrace)。
5. 连接(Attach)到GDB：`gdb -p <pidof sbcl>`并且得到所有线程的调用堆栈：`thread apply all ba`。
6. 如果有多个线程在跑，且当时仍在gdb中，尝试得到所有线程的Lisp调用堆栈：`thread apply all call backtrace_from_fp($ebp, 100)`。在x86-64机中，替换`$ebp`为`$rbp`。调用堆栈会出现在SBCL进程的标准输出中。
7. 将SBCL产生的调用堆栈和输出（stdout和stderr）发送到报告中。
8. 不要忘了包含OS和SBCL的版本。
9. 如果可能，包含相同测试在不同版本SBCL/OS/...中的输出。

[ref1]: https://lists.sourceforge.net/lists/listinfo/sbcl-help
[ref2]: http://sourceforge.net/mailarchive/forum.php?forum_name=sbcl-help
[ref3]: http://news.gmane.org/gmane.lisp.steel-bank.general
[ref4]: http://www.catb.org/~esr/faqs/smart-questions.html
[ref5]: https://bugs.launchpad.net/sbcl
[ref6]: http://www.chiark.greenend.org.uk/~sgtatham/bugs.html

# 2 介绍
    2.1 ANSI一致性
    2.2 扩展性
    2.3 特质
        2.3.1 声明
        2.3.2 FASL格式
        2.3.3 编译器的实现
        2.3.4 定义常量
        2.3.5 风格警告
    2.4 开发工具
        2.4.1 编辑器集成
        2.4.2 语言参考
        2.4.3 产生可执行文件
    2.5 更多SBCL信息
        2.5.1 SBCL主页
        2.5.2 在线文档
        2.5.3 附加文档
        2.5.4 内部文档
    2.6 更多Common Lisp信息
        2.6.1 互联网社区
        2.6.2 第三方库
        2.6.3 Common Lisp书籍
    2.7 SBCL的历史及实现

SBCL是一种最符合ANSI Common Lisp标准的实现。这份手册将焦点集中在SBCL的特有行为(behavior)上，而不是ANSI Common Lisp的通用行为上。

## 2.1 ANSI一致性
基本上，不一致的每种类型都被认为是一个错误。(例外情况是，内在标准不一致的地方。)见1.3小节[报告错误]。

## 2.2 扩展性
SBCL带有大量的扩展，一些在core中，另一些可以用`require`来导入。不幸的是，并非所有这些扩展都有适当的文档。

**系统定义工具**
`asdf`是一个Daniel Barlow写的可扩展的并且非常流行的面向协议的系统定义工具。见Info文件`asdf`的‘Top’节点以获取更多信息。

**外部函数接口**
`sb-alien`包允许与C代码交互，导入共享对象文件等。见第8章[外部函数接口]。
`sb-grovel`可以部分地自动生成外部函数接口定义。见16.4小节[sb-grovel]。

**递归事件循环**
SBCL提供了一个递归事件循环(`server-event`)来处理多流(multiple stream)中的非阻塞IO，而不是用线程。

**元对象协议**
`sb-mop`包为Common Lisp Object System提供了元对象协议，像《Art of Metaobject Protocal》描述的那样。

**扩展序列**
SBCL允许使用者定义`sequence`类的子类。见7.6小节[扩展序列]。

**本地线程**
SBCL在x86/Linux上拥有本地线程，可以利用多核机器的SMP优点。见第12章[线程]。

**网络接口**
`sb-bsd-sockets`是底层网络接口，提供TCP和UDP套接字。见第14章[网络]。

**自省的设施**
`sb-introspect`模块给出了大量自省扩展，包括函数lambda列表的访问和交叉引用设施。

**操作系统接口**
`sb-ext`包含大量函数来运行外部进程，访问环境变量等。
`sb-posix`模块为POSIX设施提供了lisp化的接口。

**扩展流**
`sb-gray`是一种Gray Stream的实现。见10.3小节[Gray Streams]。
`sb-simple-streams`是一种简单流API的实现，这种简单流API由Franz Inc提出。

**性能分析**
`sb-profile`是一个精确的针对每个函数的性能分析器。见15.1小节[Deterministic Profiler]。
`sb-sprof`是一种统计分析器，能生成调用图和指令级别的分析，而后者也支持分配内存分析。见15.2小节[统计分析器]。

**自定义钩子**
SBCL包含大量标准外的自定义钩子，以调整系统的行为。见7.8小节[定制用户的钩子函数]。
`sb-aclrepl`为SBCL提供了一个Allegro CL风格的toplevel，来作为类CMUCL风格的候选。见16.1小节[sb-aclrepl]。

**CLTL2移植层**
`sb-cltl2`模块提供了`compiler-let`和环境访问功能(见《Common Lisp The Language，2nd Edition》的描述)，这在ANSI的标准化进程中从语言中去除了。

**可执行文件交付**
函数`sb-ext:save-lisp-and-die`的`:executable`参数能产生一个独立的可执行文件，包含一个当前Lisp会话的映像和一个SBCL运行时。

**位旋转**
`sb-rotate-byte`为整数的位旋转提供了一个很有效率的原语。位旋转，例如被大量图形加密算法使用，但在ANSI Common Lisp中却并不能以原语来使用。见16.8小节[sb-rotate-byte]。

**Harness测试**
`sb-rt`模块是一个简单但吸引人的回归测试和单元测试框架。

**MD5校验和**
 `sb-md5`是一个MD5报文摘要算法的Common Lisp实现，使用了SBCL提供的模块化的算术优化。见16.5小节[sb-md5]。

## 2.3 特质
本小节的信息描述了ANSI标准放弃的而SBCL实现的一些方法。

### 2.3.1 声明
声明通常被当成断言。这个通用的原则和它的含义，以及Bug可以免除编译器退出也满足这个原则。这些在4.2.1小节[声明和断言]中被讨论。

### 2.3.2 FASL格式
SBCL的fasl格式是二进制可移植的，但只严格针对特定SBCL版本(译者注：意味着你最多能将fasl格式文件在相同SBCL版本号的不同平台上运行)。尽管这是次优的，却被证明比试图维持不同版本的fasl兼容性更具鲁棒性：不小心打碎的东西太容易了，并会导致难以诊断错误。

对于基于ASDF的系统，下面的代码片段处理fasl的自动重编译，包含在用户或系统的初始化文件中以备需要(见3.4小节[初始化文件])。
    (require :asdf)
    ;;; If a fasl was stale, try to recompile and load (once).
    (defmethod asdf:perform :around ((o asdf:load-op)
                                     (c asdf:cl-source-file))
        (handler-case (call-next-method o c)
            ;; If a fasl was stale, try to recompile and load (once).
            (sb-ext:invalid-fasl ()
                (asdf:perform (make-instance 'asdf:compile-op) c)
                (call-next-method))))

### 2.3.3 编译器的实现
本质上来说，SBCL只是一个Common Lisp的编译器实现。这意味着，除了一些特殊的情况，在所有情况下，`eval`创建一个lambda表达式，用`compile`调用该lambda表达式来产生一个编译后的函数，然后用`funcall`调用该编译后的函数。在缺省的构建中，更传统一点解释器也是能得到的；它只是在内部调用。这是明确地由ANSI标准允许的，但是会导致一些问题，例如在默认设置中，`functionp`和`compiled-function-p`是等价的，并且当SBCL构建中没有解释器时，二者会具有相同的功能。

### 2.3.4 定义常量
SBCL对于ANSI中`defconstant`的定义是非常严格的。ANSI说对于相同的符号(symbol)使用`defconstant`多于一次是未定义(undefined)的，除非新值`eql`旧值。当在使用像`string=`或`equal`这样更弱的测试函数的情况下，该“常量”值就是常量的时候，符合这样的规范就是一个滋扰。

这是很恼人的，因为在SBCL中，`defconstant`不仅仅在导入期(load time)而且在编译期(compile time)发挥作用，所以仅仅编译并导入合理的代码，就像
    (defconstant +foobyte+ '(1 4))
会导致未定义的行为。许多Common Lisp的实现会尝试帮助程序员处理这些烦扰，通过默默地接受这些未定义的代码并尝试做程序员可能想做的事。

相反，SBCL把这些未定义行为视为错误。经常地，这些代码在可移植ANSI Common Lisp中被重写，它们有预期的行为。例如，上面的代码可以通过用`defparameter`或使用正确的自定义宏来替换`defconstant`，从而给出一个定义的确切含义，例如
    (defmacro define-constant (name value &optional doc)
        '(defconstant ,name (if (boundp ',name) (symbol-value ',name) ,value)
                            ,@(when doc (list doc))))
或者可能顺着SBCL自身的实现中，内部使用`defconstant-eqx`宏这条线来继续下去。在某些情况下，这是不合适的，程序员可以处理条件类型`sb-ext:defconstant-uneql`，并可适当地选择继续或终止重启。

### 2.3.5 风格警告
SBCL可以给出各种完全合法的代码的风格警告，例如
- `defmethod`之前没有`defgeneric`
- 在不同单元中多个defun具有相同符号
- 特殊变量没有命名为`*foo*`这种方便的风格，并且词法变量反而命名为`*foo*`这种不方便的风格
这会导致一些人的微词，他们指出，其他的代码组织方式(尤其是避免使用`defgeneric`)一样美观时尚。然而，这些警告不应解读为“警告，检测到坏的编码风格，没有风格”而是应该是“警告，这种风格不像你想的那样让编译器来理解”。这意味着，除非编译器对此条件发出警告，否则编译器没有其他方法来警告编程错误，那样的话这些错误会很容易地被忽略。(相关Bug：当你编译并随后导入一个函数，这个函数包含一个包裹(wrapped)在`eval-when`中的`defun`。这种情况下，多`defun`的警告是全无必要的恼人的，理想情况下是应该被屏蔽掉的，但是在SBCL 0.7.6中依然没有去除)

## 2.4 开发工具
### 2.4.1 编辑器集成
尽管SBCL可以“裸体”运行，推荐的开发模式是使用一个连接到SBCL的编辑器，这个编辑器不仅支持基本的lisp编辑(模式匹配等)，也提供集成的调试器，可交互的编译，自动文档查看等特性。

当前SLIME(Super Lisp Interaction Mode for Emacs)和Emacs是被推荐和SBCL一起使用的，尽管存在其他选择。
(注：历史上，http://ilisp.cons.org/ 上的ILISP包提供了相同的功能，但是它不支持现代的SBCL版本)

SLIME可以从这里下载[http://www.common-lisp.net/project/slime/][ref1]。

### 2.4.2 语言参考
CLHS(Common Lisp Hyperspec)是一份ANSI标准的超文本版本。它是被LispWorks制作的----无价的参考，可以自由使用。
见：[http://www.lispworks.com/reference/HyperSpec/index.html][ref2]

### 2.4.3 产生可执行文件
SBCL可以生成单独的可执行文件。生成的可执行文件包含SBCL运行时，所以在程序功能上不会产生限制。例如，一个分发的程序可以调用`compile`和`load`，这要求编译器必须在可执行文件中存在。更进一步的信息，见3.2.3小节[sb-ext:save-lisp-and-die]。

## 2.5 更多SBCL信息
### 2.5.1 SBCL主页
SBCL网站[http://www.sbcl.org/][ref3]含有一些通用的信息，加之用于SBCL的邮件列表连接和这些邮件列表的归档。推荐订阅`sbcl-help`和`sbcl-announce`的邮件列表：二者均轻量且能帮助你了解SBCL的开发。

### 2.5.2 在线文档
各种命令的非ANSI扩展的文档从SBCL可执行文件本身获得。具有各自命令提示(例如debugger，inspect)的功能扩展可以再其命令提示符下输入help以获取文本文档。不具有命令提示符的功能扩展(例如trace)在其文档字符串中被描述，除非你的SBCL在编译时加入了不包含文档字符串的选项，这种情况下只可读取源码的文档字符串。

### 2.5.3 附加文档
除了用户手册，SBCL的源码和二进制分发都包含一些其他SBCL相关的文档文件，它们应该随手册被安装在你的系统中，例如在`/usr/local/share/doc/sbcl/`。
>COPYING Licence和Copyright简述
>CREDITS SBCL各个部分的作者信息
>INSTALL 包含如何从源码和二进制分发包来安装SBCL的内容，并有一些相关的安装出错信息。
>NEWS    总结各种SBCL版本之间的变化

### 2.5.4 内部文档
如果你对SBCL系统自身的开发比较感兴趣，那么订阅`sbcl-devel`是个好主意。

SBCL内部文档----除了源码的注释外，目前也被维护为一个类似wiki的网站：[http://sbcl-internals.cliki.net/][ref4]。

一些描述从CMUCL到SBCL转换的编程细节的底层信息可以在SBCL分发下的`doc/FOR-CMUCL-DEVELOPERS`文件中访问到，尽管这个文件不是默认安装的。

## 2.6 更多Common Lisp信息
### 2.6.1 互联网社区
Common Lisp的互联网社区相当多：[news://comp.lang.lisp][ref5]是一个相当人群密集的新闻组，但却有很低的信噪比。各种特殊兴趣的邮件列表和IRC趋向于提供更多内容和更少的吵闹。[http://www.lisp.org][ref6]和[http://www.cliki.net][ref7]包含了lisper们谈老本行时网络中大量的可参考的地方。

### 2.6.2 第三方库
为了获得大量免费(自由)的Common Lisp库和工具的信息，我们建议查看CLiki：[http://www.cliki.net][ref7]。

### 2.6.3 Common Lisp书籍
如果你不是一个程序员并且想学Lisp，有许多入门的Lisp书籍。当然我们没有特殊的偏好。如果你不能决定，请查看Usenet [news://comp.lang.lisp][ref8] 来获得最新建议。

如果你是一个对其他语言有经验的程序员但是需要学习Common Lisp，一些书籍比较显眼：

*Practical Common Lisp, by Peter Seibel*

它是对语言的一个很好的介绍，包括基本内容和“高级主题”，例如宏，CLOS和包。可以在此获得可打印格式和网页格式：[http://www.gigamonkeys.com/book/][ref9]

*Paradigms Of Artificial Intelligence Programming, by Peter Norvig*
 是一般的Common Lisp编程的好资料，有许多常见的例子。无论你的工作是否是AI，这都是一本值得看的很好的书。

*On Lisp, by Paul Graham*
关于宏的深层次的解剖。但不推荐作为入门书籍，鉴于它比ANSI稍稍早一些，你需要提防非标准的用法。且它并非想涵盖一个该语言的整体而是仅仅聚焦在宏上。可以从[http://www.paulgraham.com/onlisp.html][ref10]下载。

*Object-Oriented Programming In Common Lisp, by Sonya Keene*
除了《Practical Common Lisp》之外，许多入门书籍都不强调CLOS。这本则强调了。即使你在抽象思维中对面向对象编程知识渊博，但是如果你要用Common Lisp处理任何的OO，这本书都很值得看。CLOS中的一些抽象(尤其是多调度)超越了任何你会在大多数OO系统中的所见，并存在许多微小差异。这本可以帮助缓解此种文化冲击(culture shock)。


*Art Of Metaobject Programming, by Gregor Kiczales et al*
目前是SBCL所支持的Common Lisp的元对象协议的信息的主要来源。第二部分(第5,6章)可以在此免费获得[http://www.lisp.org/mop/][ref11]。

## 2.7 SBCL的历史及实现
你可以用SBCL直接用于工作而不用去知道，或知晓关于它从哪儿来，它是怎么实现的，或它怎样扩展ANSI Common Lisp标准的任何事。然而，一点点知识可以是很有用的。如，理解错误信息，捕捉并解决问题，理解为什么系统的一些部分比其他更容易调试，预测已知的Bug，已知的性能问题和失踪的扩展可能被修复，调节或添加。

SBCL是从CMUCL继承下来的。而后者是从Spice Lisp继承下来的。包括在IBM RT Mach操作系统上的早期实现，这追溯到1980年代了。当时的一些设计上的决定仍然反映在现在的实现当中:

- 该系统期望被导入虚拟内存中的具有固定编译时间的内置上，且期望其堆存储的位置可以再编译时被指定。
- 该系统过量使用内存，从系统分配大量的地址空间(通常超过可用的虚拟内存空间)，然后挂掉。这是因为使用了过量的分配存储。
- 该系统是一个C程序实现，并负责提供底层服务且可以导入Lisp的核心(core)文件。

SBCL也从CMUCL继承了一些新的架构特点。最重要的是，在一些架构上，有一个通过垃圾收集器(GC)，它对性能具有各种影响(大多数是好的方面)。这些内容在第6章[效率]中被讨论。

SBCL从CMUCL中独立出来是因为SBCL现在是Common Lisp的一种编译器实现。这是在实现策略上的改变，这充分利用了ANSI说明书3.1节“Evaluation”中所保证的“任何设施可以共享相同的执行策略”的自由意志所带来的特点。这并非意味着SBCL不能被交互使用，而事实上这个改变很大程度上对一般用户而言是不可见的，因为SBCL自始至终都能够并且确实交互地执行代码的，这是通过按照输入代码的顺序而编译完成的(如果你知道如何来查看，这将会是可见的，比如使用`compiled-function-p`或SBCL不会出现很多Bug的方式，后者会导致行为的不同，这是因为被解释的代码比被编译的代码在行为表现上更加不同)。这在SBCL而言意味着，`eval`函数唯一真正`解释`几种简单的形式，例如`boundp`为真的符号。更复杂的形式是通过调用编译，然后在返回的结果上调用`funcall`来执行的。

SBCL的直接祖先是CMUCL的x86分支。这个分支在某些方面是CMUCL分支中最“拼凑”的分支，因为产生了许多奇怪的改变以支持寄存器功能较差(register-poor)的x86架构。一些东西(如跟踪和调试)在此不能很好地工作。SBCL应该能改善这些领域(在某些领域已经改善了)，但需要一段时间。

x86上的SBCL----像x86上的CMUCL，使用一个保守的GC。这意味着，它不会严格区分标记的(tagged)和未标记的(untagged)数据，而是将一些未标记的数据(例如原始的浮点数)作为可能标记的(possibly-tagged)数据，并且不会回收任何它们指向的Lisp对象。这对平均时间效率有一些负面影响(尽管可能不会比试图像x86那样在寄存器功能较差的处理器架构上实现一个严格的GC的影响更坏)，且对最坏情况下的内存效率有一些潜在的不可限(unlimited)的影响。在实践中，保守垃圾收集器的工作表现相当不错，没有出现任何的极限临近情况。但它能偶尔引奇模式(odd pattern)的内存使用。

自CMUCL而来的分支(fork)，是基于系统引导过程的大部分重写。CMUCL多年来容忍了一个不寻常的“构建(build)”过程，实际上它并没有从零完整地构建整个系统，而是在新版本上逐渐覆盖运行系统的一些部分。这种拟构建(quasi-build)过程能导致各种古怪的引导挂起(bootstrapping hangup)问题，尤其是要对系统产生一个重要变化的时候。这也使得当前源码和当前可执行文件的连接比其他软件系统更脆弱----这非常容易意外地“构建”一个包含没有反映在源码当前版本的某种特点的CMUCL系统。

其他自CMUCL分支出来后所产生的重要改变包括

- SBCL从核心系统删除了许多CMUCL扩展(例如，IP网络，远程过程调用，Unix系统接口，X11接口)。然而，它们大多数可以作为分发模块(用SBCL发布的)或第三方模块来得到。
- SBCL已经删除或废弃一些非标准的特性和代码复杂度，这可有效降低维护成本。例如，SBCL编译器内部不会实现内存池(这样会更简单并且更容易维护，但却产生了更多垃圾并且运行地更慢)，且各种对于语言的模块编译效率提升(block-compilation efficiency-increasing)扩展以及被删除或者不会再被SBCL自身的实现中使用。

[ref1]: http://www.common-lisp.net/project/slime/
[ref2]: http://www.lispworks.com/reference/HyperSpec/index.html
[ref3]: http://www.sbcl.org/
[ref4]: http://sbcl-internals.cliki.net/
[ref5]: news://comp.lang.lisp
[ref6]: http://www.lisp.org
[ref7]: http://www.cliki.net
[ref8]: http://www.cliki.net/
[ref9]: http://www.gigamonkeys.com/book/
[ref10]: http://www.paulgraham.com/onlisp.html
[ref11]: http://www.lisp.org/mop/

# 3 启动和停止
    3.1 启动SBCL
        3.1.1 从Shell到Lisp
        3.1.2 从Emacs运行
        3.1.3 Shebang脚本
    3.2 停止SBCL
        3.2.1 退出
        3.2.2 文件结尾
        3.2.3 保存一个核心映像
        3.2.4 出现错误时退出
    3.3 命令行选项
        3.3.1 运行时选项
        3.3.2 顶层选项
    3.4 初始化文件
    3.5 初始化和退出钩子

## 3.1 启动SBCL
### 3.1.1 从Shell到Lisp
从命令行输入`sbcl`来运行SBCL。

之后你应该处于REPL(读取，求值，打印，循环)，在此你可以输入表达式以和SBCL交互。

    $ sbcl
    This is SBCL 0.8.13.60, an implementation of ANSI Common Lisp.
    More information about SBCL is available at <http://www.sbcl.org/>.

    SBCL is free software, provided as is, with absolutely no warranty.
    It is mostly in the public domain; some portions are provided under
    BSD-style licenses. See the CREDITS and COPYING files in the
    distribution for more information.
    * (+ 2 2)

    4
    * (exit)
    $

见3.3小节[命令行选项]和3.2小节[停止SBCL]。

### 3.1.2 从Emacs运行
为了从Emacs下来运行SBCL，你的`.emacs`应该像这样

    ;;; The SBCL binary and command-line arguments
    (setq inferior-lisp-program "/usr/local/bin/sbcl --noinform")

用Emacs使用SBCL的更多信息，见2.4.1小节[编辑器集成]。

### 3.1.3 Shebang脚本
标准的Unix工具就是Unix的解析器。解析器跟随一个通用的命令行协议，为了让“shebang脚本”工作，这是有必要的。SBCL通过`--script`命令行选项来支持它。

示例文件(hello.lisp):

    #!/usr/local/bin/sbcl --script
    (wirte-line "Hello, World!")

使用示例：

    $ ./hello.lisp
    Hello, World!
    $ sbcl --script hello.lisp
    Hello, World!

## 3.2 停止SBCL
### 3.2.1 退出
SBCL可以在任何时候通过调用`sb-ext:exit`而停止，选择性地(optionally)返回一个指定的数值给调用进程。见第12章[线程]以获取终止单个线程的信息。

sb-ext:exit &key code abort timeout [函数]

终止进程，导致`sbcl`以退出码`code`退出。当`abort`为假时，`code`默认为0，否则为1。

当`abort`为假(默认)，当前线程首先解开(unwound)，`*exit-hooks*`开始运行，其他线程被终止，且标准输出流会在`sbcl`调用`exit(3)`前被刷新，在`exit(3)`函数被调用时，`atexit(3)`函数会运行。如果多个线程调用`exit`时`abort`为假，第一个调用它的线程会走完上述流程(protocol)。

当`abort`为真时，`sbcl`会调用`_exit(2)`立即退出而不会解开(unwinding)堆栈，或者调用退出钩子。注意，`_exit(2)`不会像`exit(3)`那样调用`atexit(3)`。

递归地调用`exit`会导致`exit`的行为像`abort`参数为真那样。

当`abort`为`nil`时，`timeout`控制等待其他线程终止的时间。一旦当前线程被解开(unwound)且`*exit-hooks*`已经运行，创建新线程就会被阻止且会通过调用`terminate-thread`来终止其他线程。然后，系统会使用`join-thread`来等待这些线程结束，最多是等待所有线程合并的时间的总和。那些不能及时结束的线程在退出流程进行的时候，会被简单地忽略。`timeout`默认被设置为`*exit-timeout`，而后者的默认值为60。`timeout`为`nil`意味着无限期等待。

注意，`timeout`只适用于`join-thread`，而不是`exit-hooks`。由于终止`terminate-thread`是异步的，使得对于有复杂清理过程的多线程应用程序的正确终止非常棘手。为了有序同步地关闭，请使用退出钩子，而不是依赖隐式的线程终止。

如果在从`*exit-hooks*退出异常错误的过程中发生严重的情况，后果则是不确定的。这会导致收到信号的钩子的警告和钩子停止执行。如果不是这样，钩子会允许退出进程正常地继续。

### 3.2.2 文件结尾
默认SBCL会在输入的结尾退出，这个结尾或者是使用者在相关的终端输入了`Control-D`
或者是SBCL作为shell管道的一端遇到输入结尾而引起的。

### 3.2.3 保存一个核心映像
SBCL有保存它的状态成一个文件以备以后执行的能力。这项功能对它的启动进程非常重要，而且作为一项扩展提供给用户。

sb-ext:save-lisp-and-die core-file-name &key toplevel executable [函数]


### 3.2.4 出现错误时退出


## 3.3 命令行选项


### 3.3.1 运行时选项
**--core corefilename**
**--dynamic-space-size megabytes**
**--control-stack-size megabytes**
**--noinform**
**--disable-ldb**
**--lose-on-corruption**
**--script filename**
**--merge-core-pages**
**--no-merge-core-pages**
**--default-merge-core-pages**
**--help**
**--version**

### 3.3.2 顶层选项
**--sysinit filename**



**--no-sysinit**
**--userinit filename**
**--no-userinit**
**--eval command**
**--load filename**
**--noprint**
**--disabled-debugger**
**script filename**


## 3.4 初始化文件
SBCL进程初始化文件使用`read`和`eval`，而不是`load`；并且初始化文件可以用来设置启动的`*package*`和`*readtable*`，且可以表明全局的优化策略。

**系统初始化文件**

默认在`$SBCL_HOME/sbclrc`，如果不存在，看看`etc/sbclrc`。可以被命令行选项`--sysinit`或`--no-sysinit`覆盖。

系统初始化文件时为系统管理员和软件包而准备的，来配置第三方模块的安装位置等。

**用户初始化文件**

默认在`$HOME/.sbclrc`。可以被命令行选项`--userinit`或`--no-userinit`覆盖。

用户初始化文件是为个人的个性定义而准备的，例如在启动时导入特定的模块，定义实用函数来使用REPL，处理FASL的自动重新编译(见2.3.2[FASL格式])等。

## 3.5 初始化和退出钩子
SBCL提供了系统的初始化和退出的钩子函数。

- sb-ext:*init-hooks* [变量]
这是一个函数列表，他们会在系统初始化后一个保存的核心映像启动的时候，以不确定的顺序来被调用。`sbcl`自身不会使用该钩子：是保留给用户和应用程序的。

- sb-ext:*exit-hooks* [变量]
这是一个函数列表，他们会在`sbcl`退出的时候，以不确定的顺序来被调用。`sbcl`自身不会使用该钩子：是保留给用户和应用程序的。使用(sb-ext:exit :abort t)，或者直接调用exit(3)会规避这些钩子。




