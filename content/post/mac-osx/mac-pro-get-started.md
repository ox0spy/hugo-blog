---
title: "mac pro初体验"
date: 2017-12-15T19:22:00+08:00
tags: ["mac"]
author: "ox0spy"
categories: ["mac-osx"]
---

貌似如今不玩下Mac总觉得逼格不够，最近08年买的Thinkpad R60也坏掉了，就入手一台Mac Pro。
下面记录下环境配置和最近使用感受。


## Xcode

### 安装Xcode命令行工具:

    :::bash
    $ xcode-select --install

### Xcode包管理工具[Alcatraz](http://alcatraz.io/)安装:

    :::bash
    $ curl -fsSL https://raw.githubusercontent.com/supermarin/Alcatraz/deploy/Scripts/install.sh | sh

### Xcode安装Solarized-Dark-for-Xcode主题:
- 打开Xcode，Window -> Package Manager，选择Color Themes，搜索solar，安装ArtSabintsev/Solarized-Dark-for-Xcode
- 重启Xcode，Xcode -> Preference -> Fonts & Colors，选择刚才安装的Solarized-Dark-for-Xcode
- 再重启Xcode生效

> 注：如果想修改字体及字体大小，请手动修改`~/Library/Developer/Xcode/UserData/FontAndColorThemes/Solarized\ Dark\ for\ Xcode.dvtcolortheme`


## Homebrew

[Homebrew](http://brew.sh)是Mac中非常受欢迎的包管理工具，熟悉Linux的朋友一定知道rpm/yum, dpkg/apt等。

    :::bash
    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    $ brew doctor
    $ man brew                     # manpage for brew
    $ brew install <package-name>  # install package
    $ brew update
    $ brew outdated
    $ brew upgrade <package-name>
    $ brew cleanup
    $ brew list --versions

使用`清华大学Tuna镜像源`：[Homebrew 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)

## Homebrew Cask

通过Homebrew可以方便的管理命令行程序，Homebrew Cask可以管理图形界面程序。

    :::bash
    $ brew tap caskroom/cask
    $ brew install brew-cask
    $ brew cask install google-chrome
    $ brew update && brew upgrade brew-cask && brew cleanup


## iTerm2

- 从[官网](https://www.iterm2.com/)下载、安装iTerm2。
- 关闭烦人的bell: `iTerm 2: Preferences -> Profiles -> Tab: Terminal -> Notifications -> Silence Bell`


## Zsh

    :::bash
    $ brew install zsh zsh-completions
    $ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    $ chsh -s /bin/zsh

Zsh还配置了powerline，开始Zsh、vim都使用powerline，后来vim使用vim-airline。
powerline要显示一些Unicode字符，需要安装打过patch的powerline字体。

安装powerline: https://powerline.readthedocs.org/en/latest/installation/linux.html
powerline font: https://github.com/powerline/fonts

字体安装后需要在iTerm2中选用刚安装的打过powerline patch的字体。

## GNU command line tools

Mac默认的命令行工具太蛋疼了，至少对我这样非常熟悉Linux的人来说是这样的。下面介绍下如何安装GNU命令行工具:

    :::bash
    brew install coreutils
    brew tap homebrew/dupes
    brew install binutils
    brew install diffutils
    brew install ed --default-names
    brew install findutils --with-default-names
    brew install gawk
    brew install gnu-indent --with-default-names
    brew install gnu-sed --with-default-names
    brew install gnu-tar --with-default-names
    brew install gnu-which --with-default-names
    brew install grep --with-default-names
    brew install gzip
    brew install watch
    brew install wdiff --with-gettext
    brew install wget


## pip

个人经常写点Python代码，需要安装Python模块管理工具-`pip`

[Installing with get-pip.py](https://pip.pypa.io/en/stable/installing/)

pypi源设置请参考：[清华大学Tuna的pypi 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)


## CocoaPods

[CocoaPods](https://cocoapods.org/)是Xcode项目的依赖库管理工具。

我将它安装到自己的$HOME目录下，也可以直接安装到系统目录。

将下面内容添加到 $HOME/.zshrc中

> export GEM_HOME=$HOME/.gem
> export PATH=$GEM_HOME/ruby/2.0.0/bin:$PATH

通过下面命令安装CocoaPods:

    :::bash
    $ source ~/.zshrc  # 获取GEM_HOME, PATH配置
    $ gem sources -l  # 列出当前ruby gem源地址
    $ gem sources --remove https://rubygems.org/  # 删除官方源
    $ gem sources -a https://ruby.taobao.org/  # 使用淘宝ruby gem镜像
    $ gem install cocoapods --user-install  # 安装CocoaPods
    $ pod install

使用CocoaPods国内镜像索引:

    :::bash
    $ pod repo  # 查看repo信息，下面修改repo master git URL
    $ pod repo remove master
    $ pod repo add master https://gitcafe.com/akuandev/Specs.git
    $ pod repo update

CocoaPods的更多介绍请阅读[用CocoaPods做iOS程序的依赖管理](http://blog.devtang.com/blog/2014/05/25/use-cocoapod-to-manage-ios-lib-dependency/)


## vim

常用vim，在mac上配置下，曾经自己的vim配置有点乱了，发现［k-vim](https://github.com/wklken/k-vim)不错，就使用它了。


    :::bash
    $ brew install macvim  # 只使用了macvim，没有用原生vim
    $ ln -s /usr/local/bin/mvim /usr/local/bin/vim  # 否则YouCompleteMe等plugin会报无法安装需要 vim 7.4 之类的
    $ git clone https://github.com/wklken/k-vim.git ~/k-vim
    $ cd ~/k-vim && bash -x install.sh

注：
1. k-vim没有适配最新版的vbundle，导致运行安装程序报错，基本就是Bundle修改为Plugin。。。
2. YouCompleteMe安装总失败，后来手动下载安装


## 安装Java JDK

    $ brew cask install java

## Intellij IDEA

IDEA运行需要JDK。

从IDEA -> Project Default -> Project Structure -> SDKS中添加JDK和Intellij Platform Plugin SDK。（需要选择JDK、Intellij的目录）

安装go-lang-idea-plugin插件，可以从[官网](https://plugins.jetbrains.com/plugin/5047)下载安装；也可以通过下面方式编译安装。

    :::bash
    $ git clone https://github.com/go-lang-plugin-org/go-lang-idea-plugin.git

IDEA中打开该plugin代码，配置IDEA Platform SDK版本，Build出jar包。然后，选择jar包安装。

配置IDEA的golang开发环境浪费了很多时间，尝试了不同版本的[go-lang-idea-plugin](https://github.com/go-lang-plugin-org/go-lang-idea-plugin)，总是报错“The selected directory is not a valid home for Go SDK”。
最后运行下面命令再选择GO SDK目录终于可以了。

    :::bash
    $ echo $GOROOT
    /Users/<my-home>/Projects/golang/sdk
    $ echo $GOPATH
    /Users/<my-home>/Projects/golang/mygo
    $ cd $GOROOT
    $ git checkout -b go1.4.2 go1.4.2
    $ cd src && ./all.bash
    $ cp $GOPATH/bin/go $GOROOT/bin/
    $ cp $GOPATH/bin/gofmt $GOROOT/bin/

注：
1. GOROOT是go的源代码，下载地址：https://github.com/golang/go
2. GOPATH是go workspace，自己的代码都$GOPATH/src下


## 最后

1. mac的屏的确要比我之前用过的笔记本好很多
2. 严重怀疑网上那些把mac捧得太高的人只用过Windows，根本没用过Ubuntu之类的桌面Linux (至少目前自己还没体会到mac比Linux高效在哪里)
3. 刚开始使用难免出错，也许上面的都是错的。。