# <Brew>[Brew的工作细节](http://phenmod.com/blog/2016/01/20/brew-internal/) Via:Phenmod Zhu

本文将大略讲一下 brew 的工作细节。对普通的使用者来说，只要看明白使用文档就好，了不了解这些内容并不重要。有些内容（用斜体标出的部分）我也不太确定是否正确，还待后续修改。

### brew 本身的文件位置

通过 `which brew` 命令，可以发现 brew 命令的可执行文件的路径为 /usr/local/bin/brew，它是个 shell 脚本。用文本编辑软件打开，可以发现它实际上是读取系统使用的 ruby 的路径，来执行 /usr/local/Library/brew.rb。

brew 软件本身的各个文件 *全部* 在 /usr/local/Library 目录下，可以在它的三个子目录 bin/、Library/、share/ 下找到安装后的 brew 文件。比如，/usr/local/bin/brew 就是 brew 的可执行文件。具体每个brew的子命令都是一个个的 ruby 脚本，比如，`brew search` 就是 /usr/local/Library/Homebrew/cmd/search.rb。

每个使用brew安装的软件都有一个对应的ruby脚本，我们称为 Formula，`brew install <package_name>`安装命令其实就是读取这个脚本里的配置，下载源码，然后运行配置里写好的安装命令。这些脚本放在 /usr/local/Library/Formula 下，使用 `brew update` 从github拉取最新版本的 brew 时，会在命令行运行结束后看到提示，哪些软件有版本更新，新增了哪些软件。

### brew 安装软件的细节

软件下载下来之后，缓存在 /Library/Caches/Homebrew 目录中，你会在这个目录下发现一堆你下载过的软件包。如果你已安装某软件，然后用 `brew reinstall <package_name>` 重新安装该版本，就不需要重新下载了，它会直接使用已下载的软件进行安装。

brew 大多是直接下载源码然后编译安装，安装命令如 `./configure && make install`。并且，在安装本软件之前，会先安装脚本里指明的本项目的依赖库。

软件安装的目录是 /usr/local/Cellar///，比如 /usr/local/Cellar/autojump/22.2.4/。源码编译完成后，生成的文件都会放在这里，如果是直接下载的可执行文件，也会直接放在这里。每个版本的软件都会放在对应版本名称的文件夹下。一般的软件可能会包含可执行文件、供其它库使用的头文件、运行库、文档等文件或目录，为了方便升级管理，，brew 会把这些文件或目录分别软链接到 /usr/local/bin/、/usr/local/include/、/usr/local/lib/、/usr/local/share/ 等目录下。

如果你用 `brew upgrade <package_name>` 更新过几次该软件，你所有安装过的版本都会在 /usr/local/Cellar// 目录下找到。每次升级时，brew都是先把可执行文件等都编译到当前版本目录下，撤销之前版本的软链接，然后再将新版本软链接到 /usr/local 的各子文件夹下。这样，我们就会使用到最新版本了。

你也可以使用 `brew swith` 来切换使用软件的某具体版本。比如，`brew swith autojump 22.1.4` 。

时间久了，你的安装文件会越来越多，这时候可以用 `brew cleanup` 清理掉不想用的旧版本，这样会会节省出来大量硬盘空间。如果你不想直接删掉，可以使用 `brew cleanup -n` 命令，它相当于预演一下，告诉你真正运行 `brew cleanup` 时会干些什么，而不会真正执行。检查完毕确定可以删除后，你再 `brew cleanup` 就安全多了。brew 的很多命令都支持 `-n` 参数，也就是 `--dry-run` 参数，这些就需要你自己去翻文档了。

brew 还提供了很多其他命令，有需要的话自己 `man brew` 查看吧。

