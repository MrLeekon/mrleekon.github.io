# <Vim>Vim编译安装
## 网上脚本(不可行)
configure

```Shell
#! /bin/sh

# This is just a stub for the Unix configure script, to provide support for
# doing "./configure" in the top Vim directory.

PY_CONFIG=`pyenv prefix 2.7.14/lib/python2.7/config`
PY3_PREFIX=`pyenv prefix 3.6.5`
PY3_CONFIG=`$PY3_PREFIX/bin/python-config --configdir`

cd src && exec ./configure                  \
    --with-features=huge                    \
    --enable-multibyte                      \
    --enable-pythoninterp=dynamic           \
    --enable-python3interp=dynamic          \
    --with-python-config-dir=$PY_CONFIG     \
    --with-python3-config-dir=$PY3_CONFIG   \
    --enable-cscope                         \
    --prefix=/usr/local $@

# cd src && exec ./configure "$@"
```

**In Shell**

```
$ env PYTHON_CONFIGURE_OPTS="--enable-shared" pyenv install -fk 2.7.14
$ env PYTHON_CONFIGURE_OPTS="--enable-shared" pyenv install -fk 3.6.5

$ mkdir -p ~/lib
$ cd ~/lib
$ ln -s ../.pyenv/versions/2.7.14/lib/libpython2.7.dylib
$ ln -s ../.pyenv/versions/3.6.5/lib/libpython3.6m.dylib
```

```shell
# 说明
--with-features=huge：支持最大特性
--enable-rubyinterp：打开对ruby编写的插件的支持
--enable-pythoninterp：打开对python编写的插件的支持
--enable-python3interp：打开对python3编写的插件的支持
--enable-luainterp：打开对lua编写的插件的支持
--enable-perlinterp：打开对perl编写的插件的支持
--enable-multibyte：打开多字节支持，可以在Vim中输入中文
--enable-cscope：打开对cscope的支持
--with-python-config-dir=/usr/lib/python2.7/config/ 指定python 路径
--with-python-config-dir=/usr/lib/python3.6/config-3.6m/ 指定python3路径
--prefix=/usr/local/vim：指定将要安装到的路径(自行创建)
```

## 分析并换成自己的(还没完）
用Pyenv获取Python2.7的Config用于编译
PY_CONFIG=`pyenv prefix 2.7.14/lib/python2.7/config`
/Users/Leekon/.pyenv/versions/2.7.14/lib/python2.7/config

用Pyenv获取Python3的路径
PY3_PREFIX=`pyenv prefix 3.6.5`
/Users/Leekon/.pyenv/versions/3.6.5

用Python获取自己的Config用于编译
PY3_CONFIG=`$PY3_PREFIX/bin/python-config --configdir`
/Users/Leekon/.pyenv/versions/3.6.5/lib/python3.6/config-3.6m-darwin


最终Make的Configure，因为Vim-hy的原因只能用python2

```
./configure \
    --with-features=huge \
    --enable-multibyte \
    --enable-pythoninterp=dynamic \
    --enable-python3interp=dynamic \
    --with-python-config-dir=/Users/Leekon/.pyenv/versions/2.7.14/lib/python2.7/config \
    --with-python3-config-dir=/Users/Leekon/.pyenv/versions/3.6.5/lib/python3.6/config-3.6m-darwin \
    --enable-cscope \
    --prefix=/usr/local/
```

Shell中

```
--enable-static：生成静态链接库
--enable-shared：生成动态链接库
```

## 实操
最终直接用上面的configure也不行，主要是Shell没有设定好Python版本

**以安装Vim为例**，同时支持Python和Python3
在Shell先执行一下(我是用Pyenv管理Python）

```
$ Pyenv shell 2.7.14 3.6.5
$ mkdir .mybuild # 非必要步骤
$ cd .mybuild    # 非必要步骤
$ git clone https://github.com/vim/vim
$ cd vim
$ make clean     # 非必要步骤
```

然后

```
./configure \
    --with-features=huge \
    --enable-multibyte \
    --enable-pythoninterp=dynamic \
    --enable-python3interp=dynamic \
    --with-python-config-dir=/Users/Leekon/.pyenv/versions/2.7.14/lib/python2.7/config \
    --with-python3-config-dir=/Users/Leekon/.pyenv/versions/3.6.5/lib/python3.6/config-3.6m-darwin \
    --enable-cscope \
    --prefix=/usr/local
```

接着

```
$ make
$ make install
```

最后在`.vimrc`加入`set pythondll=/Users/.pyenv/versions/2.7.14/lib/libpython2.7.dylib`

## 另外几个测试
**动态加载Python2&Python3**

```
./configure \
    --with-features=huge \
    --enable-multibyte \
    --enable-pythoninterp=dynamic \
    --enable-python3interp=dynamic \
    --with-python-config-dir=/Users/Leekon/.pyenv/versions/2.7.14/lib/python2.7/config \
    --with-python3-config-dir=/Users/Leekon/.pyenv/versions/3.6.5/lib/python3.6/config-3.6m-darwin \
    --enable-cscope \
    --prefix=/usr/local
```

**动态加载Python3 静态加载python2**

```
./configure                                 \
  --with-features=huge                      \
  --enable-python3interp=dynamic            \
  --enable-pythoninterp                     \
  --with-python-config-dir=/usr/lib/python2.7/config/ \
  --with-python3-config-dir=/Users/Leekon/.pyenv/versions/3.6.5/lib/python3.6/config-3.6m-darwin
  --enable-multibyte                        \
  --prefix=/usr/local
```

**动态加载Python2**

```
./configure                                 \
    --with-features=huge                    \
    --enable-multibyte                      \
    --enable-pythoninterp=dynamic           \
    --with-python-config-dir=/Users/Leekon/.pyenv/versions/2.7.14/lib/python2.7/config \
    --prefix=/usr/local
```

**动态加载Python3**

```
./configure                                 \
    --with-features=huge                    \
    --enable-multibyte                      \
    --enable-python3interp=dynamic          \
    --prefix=/usr/local
```

**静态加载Python2**

```
./configure                                 \
    --with-features=huge                    \
    --enable-multibyte                      \
    --enable-pythoninterp                   \
    --with-python-config-dir=/usr/lib/python2.7/config/ \
    --prefix=/usr/local
```

