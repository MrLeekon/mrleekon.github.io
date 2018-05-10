# <Vim>.vimrc
```
set nocompatible "关闭与VI的兼容
filetype off "必须要添加

" 设置包括vundle和初始化相关的runtime path
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" 另一种选择, 指定一个vundle安装插件的路径
"call vundle#begin('~/some/path/here')

" 让vundle管理插件版本,必须
Plugin 'VundleVim/Vundle.vim'

" 以下范例用来支持不同格式的插件安装.
" 请将安装插件的命令放在vundle#begin和vundle#end之间.
" Github上的插件
" 格式为 Plugin '用户名/插件仓库名'
" Plugin 'tpope/vim-fugitive'
" 来自 http://vim-scripts.org/vim/scripts.html 的插件
" Plugin '插件名称' 实际上是 Plugin 'vim-scripts/插件仓库名' 只是此处的用户名可以省略
" Plugin 'L9'
" 由Git支持但不再github上的插件仓库 Plugin 'git clone 后面的地址'
" Plugin 'git://git.wincent.com/command-t.git'
" 本地的Git仓库(例如自己的插件) Plugin 'file:///+本地插件仓库绝对路径'
" Plugin 'file:///home/gmarik/path/to/plugin'
" 插件在仓库的子目录中.
" 正确指定路径用以设置runtimepath. 以下范例插件在sparkup/vim目录下
" Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" 安装L9，如果已经安装过这个插件，可利用以下格式避免命名冲突
" Plugin 'ascenator/L9', {'name': 'newL9'}

Plugin 'hylang/vim-hy'

" 你的所有插件需要在下面这行之前
call vundle#end()            " 必须
filetype plugin indent on    " 必须 加载vim自带和插件相应的语法和文件类型相关脚本
" 忽视插件改变缩进,可以使用以下替代:
"filetype plugin on
"
" 常用的命令
" :PluginList       - 列出所有已配置的插件
" :PluginInstall     - 安装插件,追加 `!` 用以更新或使用 :PluginUpdate
" :PluginSearch foo - 搜索 foo ; 追加 `!` 清除本地缓存
" :PluginClean      - 清除未使用插件,需要确认; 追加 `!` 自动批准移除未使用插件
"
" 查阅 :h vundle 获取更多细节和wiki以及FAQ
" 将你自己对非插件片段放在这行之后

set nobackup "设置不备份
set helplang=cn "中文帮助文档(前提是下了中文包)  
set number "显示行号
" set relativenumber "设置相对行号
colorscheme desert
" au BufRead,BufNewFile *.py,*.pyw,*.c,*.h match BadWhitespace /\s\+$/ "把空白字符红色显示
syntax enable
syntax on "代码高亮 
set history=400 "vim记住的历史操作的数量，默认的是20 

set tabstop=4
set autoread "当文件在外部被修改时，自动重新读取
" set autoindent "自动对齐，使用上一行的对齐方式

set backspace=2 "解决退格失效

" 缩进: 每一行都和前一行有相同的缩进量,
" 同时这种缩进形式能正确的识别出花括号,当遇到右花括号（}）,
" 则取消缩进形式。此外还增加了识别C语言关键字的功能。
" 如果一行是以#开头的(比如宏)，那么这种格式将会被特殊对待而不采用缩进格式
set smartindent
" set shiftwidth=4 "每一级自动缩进的空格数
set showmatch "设置匹配模式，类似当输入一个左括号时匹配上相应的那个右括号

" set expandtab
" 如果只想在Python文件中将Tab展开成空格，就改换成下面这句
" autocmd FileType python set expandtab
autocmd FileType python setlocal tabstop=4 shiftwidth=4 softtabstop=4 textwidth=79

" 设置内部字符编码方式
set encoding=utf-8
set langmenu=zh_CN.UTF-8
language message zh_CN.UTF-8

set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1 "设置为文档编码打开，防止乱码
set incsearch "聪明的搜索
set vb t_vb= "关闭声音
set ruler "在底下设置行号
" :read $VIMRUNTIME/vimrc_example.vim
" set pythondll=/Users/.pyenv/versions/2.7.14/lib/libpython2.7.dylib

" 改键位
inoremap jk <esc>
inoremap <C-[> <ESC>
```

## Vundle模板
```
set nocompatible              " 去除VI一致性,必须要添加
filetype off                  " 必须要添加

" 设置包括vundle和初始化相关的runtime path
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" 另一种选择, 指定一个vundle安装插件的路径
"call vundle#begin('~/some/path/here')

" 让vundle管理插件版本,必须
Plugin 'VundleVim/Vundle.vim'

" 以下范例用来支持不同格式的插件安装.
" 请将安装插件的命令放在vundle#begin和vundle#end之间.
" Github上的插件
" 格式为 Plugin '用户名/插件仓库名'
" Plugin 'tpope/vim-fugitive'
" 来自 http://vim-scripts.org/vim/scripts.html 的插件
" Plugin '插件名称' 实际上是 Plugin 'vim-scripts/插件仓库名' 只是此处的用户名可以省略
Plugin 'L9'
" 由Git支持但不再github上的插件仓库 Plugin 'git clone 后面的地址'
Plugin 'git://git.wincent.com/command-t.git'
" 本地的Git仓库(例如自己的插件) Plugin 'file:///+本地插件仓库绝对路径'
Plugin 'file:///home/gmarik/path/to/plugin'
" 插件在仓库的子目录中.
" 正确指定路径用以设置runtimepath. 以下范例插件在sparkup/vim目录下
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" 安装L9，如果已经安装过这个插件，可利用以下格式避免命名冲突
Plugin 'ascenator/L9', {'name': 'newL9'}

" 你的所有插件需要在下面这行之前
call vundle#end()            " 必须
filetype plugin indent on    " 必须 加载vim自带和插件相应的语法和文件类型相关脚本
" 忽视插件改变缩进,可以使用以下替代:
"filetype plugin on
"
" 常用的命令
" :PluginList       - 列出所有已配置的插件
" :PluginInstall     - 安装插件,追加 `!` 用以更新或使用 :PluginUpdate
" :PluginSearch foo - 搜索 foo ; 追加 `!` 清除本地缓存
" :PluginClean      - 清除未使用插件,需要确认; 追加 `!` 自动批准移除未使用插件
"
" 查阅 :h vundle 获取更多细节和wiki以及FAQ
" 将你自己对非插件片段放在这行之后
```

## 自己常用的插件
```
Plugin 'vim-hy'

```

# 最简单版
```
set nocompatible
filetype off

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" 安装插件写在这之后

Plugin 'VundleVim/Vundle.vim'
Plugin 'hylang/vim-hy'

" 安装插件写在这之前
call vundle#end()            " required
filetype plugin on    " required

" 常用命令
" :PluginList       - 查看已经安装的插件
" :PluginInstall    - 安装插件
" :PluginUpdate     - 更新插件
" :PluginSearch     - 搜索插件，例如 :PluginSearch xml就能搜到xml相关的插件
" :PluginClean      - 删除插件，把安装插件对应行删除，然后执行这个命令即可
" h: vundle         - 获取帮助

" vundle的配置到此结束，下面是你自己的配置

" 一般设置
set helplang=cn
set nobackup
set autoread
set number
colorscheme desert
syntax enable
syntax on
set history=400
set vb t_vb=
set ruler
set backspace=2 "解决退格失效

" 缩进设置
set tabstop=4
set showmatch
set autoindent
set smartindent
set shiftwidth=4

" Python专用
autocmd FileType python setlocal tabstop=4 shiftwidth=4 softtabstop=4 textwidth=79

" 编码问题
set encoding=utf-8
set langmenu=zh_CN.UTF-8
language message zh_CN.UTF-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1

" 改键位
inoremap jk <esc>
inoremap <C-[> <ESC>
```

