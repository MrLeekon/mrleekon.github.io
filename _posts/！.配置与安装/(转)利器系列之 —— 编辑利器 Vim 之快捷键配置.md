# <Vim>利器系列之 —— 编辑利器 Vim 之快捷键配置 Via：[时习之](http://blog.guorongfei.com/2015/09/03/vim-shortcut/)

无论我多爱 Vim，不容否认的一点是它的快捷键对于现在的大部分人来说比较诡异， 这其中的有很深的历史渊源。这篇文章将会重点介绍 `Vim` 的快捷键的由来以及个 人偏好的一些快捷键设置。

# 诡异的快捷键

看似诡异的 Vim 快捷键其实并不诡异，这一切的一切要从 `Bill Joy` 和那台 `ADM-3A` 说起。所有学计算机的人，或者说所有用电脑的人都应该记住 `Bill Joy` 这个人，因为他主导开发了 `BSD Unix` 系统。这个系统是整个操作系统发展史上的 一座丰碑，它给我们提供了虚拟内存、网络套接字、`ex`、`C shell` 还有 `Vi` 等 等强悍的功能，而这些东西大部分都是 `Bill Joy` 写的。虽然　`BSD` 已经停止开 发，但是它的后代还在不断的发挥余热，苹果的操作系统的内核 Darwin 就是基于 BSD 的。

`Bill Joy` 的作品太多，这里要说的是它的神作 `Vi`，这个后来发展成为 `Vim` 传世神器。Vim 的快捷键之所以这么诡异就是拜它所赐，当年 `Bill Joy` 编写 `Vi` 的时候年代还比较久远，那时候他们用的不是普通的键盘而是终端机，当时 `Bill Joy` 使用的终端机叫做 `ADM-3A`，它的键盘的按键分布和我们现在的普通键 盘的按键分布大有不同，我在 `wikipedia` 上找到一张它的键盘分布图如下：

![adm3a](../../img/adm3a.svg)

看完这张图你大概会明白为什么 `Vim` 使用 `hjkl` 作为方向键；为什么把如此常 用的回到普通模式功能绑定在现在很难输入的 `Esc` 按键上；为什么大部分的常用 键都绑定为单字符快捷键而命令输入快捷键要绑定在 `:` 这个需要按住 `shift` 才 能输入的按键上面；为什么用来绑定快捷键的 `mapleader` 会是 `\` 这个按键，毕 竟快捷键是为了输入快捷，但是 `\` 这个字符在大部分的键盘上输入都比较慢。

# 快捷键的修改

`Vim` 源自 `Vi`，它沿用了后者的快捷键，而这些快捷键中有一些目前显得太过于 别扭了。幸运的是 `Vim` 是一个高度可配置的编辑，你可以随意的更改它的快捷键 ，也可以创建自己喜欢的快捷键。

## 递归绑定和非递归绑定

`Vim` 的快捷键绑定分为递归和非递归两种，比如：

```
" 非递归方式
noremap <space> :
noremap : /

" 递归方式
map <space> :
map : /

```

第一个绑定最终的结果是输入空格会变成命令模式 `:`，输入 `:` 则会变成搜索按 键 `/`。第二个绑定最终的结果是输入空格最终变成搜索按键，而你没有办法再输入 `:` 进入命令模式（千万不要做这种配置，这里只是举了一个不恰当的例子）。

## 绑定模式

`Vim` 的快捷键绑定比较复杂，因为它可以基于不同的模式绑定不同的快捷键，而 `Vim` 包含六类的 `mapping` 模式，下面这段话摘自 `Vim` 的帮助文档：

> There are six sets of mappings
> 
> * For *Normal mode*: When typing commands.
> * For *Visual mode*: When typing commands while the Visual area is highlighted.
> * For *Select mode*: like Visual mode but typing text replaces the selection.
> * For *Operator-pending mode*: When an operator is pending (after “d”, “y”, “c”, etc.). See below: |omap-info|.
> * For *Insert mode*. These are also used in Replace mode.
> * For *Command-line mode*: When entering a “:” or “/“ command.

其中用的比较多的是 `Normal`, `Insert`, `Command-line` 这三种，这篇文章中主 要讲解的也是这几种模式的快捷键配置。这几种按键绑定模式的用法如下：

```
inoremap jk <Esc>
nnoremap <space> :
cnoremap <C-a> <Home>

```

这些绑定最前面的字符 `[inc]` 表示不同的模式。如果你没有指明你使用的是哪一种 模式，比如你直接使用

```
noremap <space> :

```

那么它表示同时绑定到 `normal`, `visual`, `operator-pending` 三种模式。

# 常用的快捷键绑定

## 避免输入 Esc

在 `ADM-3A` 中 `Esc` 这个按键在目前的 `Tab` 键的位置，输入起来非常方便，但 是现在的键盘中这个按键输入起来非常的不方便，为了避免把手指移动到左上角，你 可以使用下面几种方式。

1. 在你的系统中 `Caps Lock` 映射到 `Esc` 中。这个方式我不是很推荐，因为我 更喜欢把 `Caps Lock` 映射到 `Ctrl` 按键。

2. 在输入普通模式命令之前先输入 `Alt`，因为大部分的终端编辑器会在输入 `Alt` 之后产生一个 `Esc` 按键，所以你输入 `Alt-o` 就可以直接在输入模式 中在当前行下面新建一行开始编辑。

3. 绑定其他按键到 `Esc` 上，比如我把 `jk` 绑定为 `Esc`。

    inoremap jk

    经过这个绑定，我可以直接在 `insert mode` 下输入 `jk` 退出编辑模式。

## 替换 `:`

`Vim` 的命令输入快捷键 `:` 需要输入两个字符（你需要先按住 SHIFT），对于一 个常用的快捷键来说，速度太慢了一点。我习惯把 绑定为 `:` 这样，我可 以直接输入空格进入命令模式。

```
noremap <space> :

```

## 快速切换到行首行尾

`Vim` 切换到行首和行尾的快捷键分别是 `^` 和 `

***** 
 
<Esc>代表Escape键
<CR>代表Enter键
<D>代表Command键
Alt键可以使用<M-key>或<A-key>来表示
<C>代表Ctrl
对于组合键，可以用<C-Esc>代表Ctrl-Esc
使用<S-F1>表示Shift-F1.

