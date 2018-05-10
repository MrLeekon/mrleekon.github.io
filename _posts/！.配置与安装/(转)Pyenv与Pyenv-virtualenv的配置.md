# <配置>Pyenv与Pyenv-virtualenv的配置
## [pyenv](https://github.com/pyenv/pyenv)

> 使用Pyenv方便管理/切换Python版本

### 1、安装

```source-shell
# 通过 Homebrew 安装
brew update
brew install pyenv
```

### 2、配置

1. 编辑配置文件

```source-shell
# 使用 zsh shell
vim ~/.zshrc

# 使用系统默认
vim ~/.bash_profile
```

在 **.zshrc** 或 **.bash_profile** 文件最后写入：

```source-shell
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"

if which pyenv > /dev/null;
  then eval "$(pyenv init -)";
fi

# 可把if语句换为以下一句
eval "$(pyenv init -)"
```

1. 使配置生效

```source-shell
source ~/.zshrc
# or
source ~/.bash_profile
```

### 3、使用

* 查看可安装的版本

```source-shell
pyenv install --list
```

* 安装python版本

```source-shell
pyenv install 3.6.3
```

* 查看当前已安装的python版本

```source-shell
pyenv versions
```

[![2017-11-01-14-52-01](media/15224256858097/32269178-a8af1d5a-bebf-11e7-904a-e8273a32742d.png)](media/15224256858097/32269178-a8af1d5a-bebf-11e7-904a-e8273a32742d.png)

*已安装3.6.2 与 3.6.3 版本, 当前使用的版本: 3.6.3*

* 设置python版本

```source-shell
# 对所有的Shell全局有效，会把版本号写入到~/.pyenv/version文件中
pyenv global 3.6.3

# 只对当前目录有效，会在当前目录创建.python-version文件
pyenv local 3.6.3

# 只在当前会话有效
pyenv shell 3.6.3

# 可通过配置PYENV_VERSION环境变量或编辑~/.python-version文件设置会话默认使用的python版本
echo "3.6.3" > ~/.python-version
# or
echo 'export PYENV_VERSION="3.6.3"' >> ~/.zshrc && source ~/.zshrc
```

* 重置版本设置

> 只有 pyenv shell 和 pyenv local 命令有--unset参数

```source-shell
pyenv shell --unset
pyenv local --unset
```

* 卸载版本

```source-shell
pyenv uninstall 3.6.3
```

## [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)

> pyenv-virtualenv 是pyenv的插件，为pyenv设置的python版本提供隔离的虚拟环境，设置虚拟环境后，在当前目录下面安装的第三方库都不会影响其他环境

### 1、安装

```source-shell
brew update
brew install pyenv-virtualenv
```

### 2、配置

1. 编辑配置文件

```source-shell
# 使用 zsh shell
vim ~/.zshrc

# 使用系统默认
vim ~/.bash_profile
```

在 **.zshrc** 或 **.bash_profile** 文件最后写入：

```source-shell
# pyenv-virtualenv
if which pyenv-virtualenv-init > /dev/null;
  then eval "$(pyenv virtualenv-init -)";
fi
# 实验可得只添加下面一句就好了
eval "$(pyenv virtualenv-init -)"
```

1. 使配置生效

```source-shell
source ~/.zshrc
# or
source ~/.bash_profile
```

### 3、使用

* 从当前版本创建virtualenv

```source-shell
# 当前版本为3.6.3
pyenv virtualenv xxx-3.6.3
```

* 指定版本创建virtualenv

```source-shell
#pyenv virtualenv 版本号 虚拟环境名
pyenv virtualenv 3.6.3 xxx-3.6.3
```

* 查看已创建的virtualenv

```source-shell
pyenv versions
```

[![2017-11-01-17-20-55](media/15224256858097/32269156-981852c2-bebf-11e7-9cdb-65fba2598f87.png)](media/15224256858097/32269156-981852c2-bebf-11e7-9cdb-65fba2598f87.png)

* 激活和停用virtualenv

```source-shell
# 手动激活
pyenv activate 虚拟环境名
pyenv deactivate

# 自动激活
# 使用pyenv local 虚拟环境名
# 会把`虚拟环境名`写入当前目录的.python-version文件中
# 关闭自动激活 -> pyenv deactivate
# 启动自动激活 -> pyenv activate xxx-3.6.3
pyenv local xxx-3.6.3
```

* 删除现有virtualenv

```source-shell
pyenv uninstall 虚拟环境名
```

