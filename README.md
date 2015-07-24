## 交流网站
> * [study golang](http://studygolang.com/) 中文站
> * [官方网站](https://golang.org/)

## 环境安装
> * 开发环境 centos6
> * go 1.4.2 版本号
> * 下载地址(https://storage.googleapis.com/golang/go1.4.2.linux-amd64.tar.gz)

## Linux、Mac OS X 和 FreeBSD 的安装包

```sh
tar -C /usr/local -zxzf go1.4.2.linux-amd64.tar.gz
```
要将 /usr/local/go/bin 添加到 PATH 环境变量， 你需要将此行添加到你的 /etc/profile.d/go.sh（全系统安装）或 $HOME/.profile 文件中：

```
export PATH=$PATH:/usr/local/go/bin
source /etc/profile.d/go.sh
```
*******

## Mac OS X安装包

打开此包文件 并跟随提示来安装Go工具。该包会将Go发行版安装到 /usr/local/go 中。
此包应该会将 /usr/local/go/bin 目录放到你的 PATH 环境变量中。 要使此更改生效，你需要重启所有打开的终端回话。
下载地址：[https://golang.org/dl/](https://golang.org/dl/)

**vim ~/.bash_profile**

```
export GOROOT=/usr/local/go
export GOPATH=~/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

## 代码的组织
go 工具为公共代码仓库中维护的开源代码而设计。 无论你会不会公布代码，该模型设置工作环境的方法都是相同的。

Go代码必须放在`工作空间`内。它其实就是一个目录，其中包含三个子目录：

* src 目录包含Go的源文件，它们被组织成包（每个目录都对应一个包），
* pkg 目录包含包对象，
* bin 目录包含可执行命令。

go 工具用于构建源码包，并将其生成的二进制文件安装到 pkg 和 bin 目录中。

src 子目录通常包会含多种版本控制的代码仓库（例如Git或Mercurial）， 以此来跟踪一个或多个源码包的开发。

以下例子展现了实践中工作空间的概念：

```
bin/
    streak                         # 可执行命令
    todo                           # 可执行命令
pkg/
    linux_amd64/
        code.google.com/p/goauth2/
            oauth.a                # 包对象
        github.com/nf/todo/
            task.a                 # 包对象
src/
    code.google.com/p/goauth2/
        .hg/                       # mercurial 代码库元数据
        oauth/
            oauth.go               # 包源码
            oauth_test.go          # 测试源码
    github.com/nf/
        streak/
        .git/                      # git 代码库元数据
            oauth.go               # 命令源码
            streak.go              # 命令源码
        todo/
        .git/                      # git 代码库元数据
            task/
                task.go            # 包源码
            todo.go                # 命令源码
```
此工作空间包含三个代码库（goauth2、streak 和 todo），两个命令（streak 和 todo） 以及两个库（oauth 和 task）。

命令和库从不同的源码包编译而来。稍后我们会对讨论它的特性。

## GOPATH 环境变量

GOPATH 环境变量指定了你的工作空间位置。它或许是你在开发Go代码时， 唯一需要设置的环境变量。

首先创建一个工作空间目录，并设置相应的 GOPATH。你的工作空间可以放在任何地方， 在此文档中我们使用 $HOME/go。注意，它绝对不能和你的Go安装目录相同。

```
export GOROOT=/usr/local/go
export GOPATH=~/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

## 新建一个github

```
cd ~/go/src
git clone https://github.com/liumingzhij26/go_base
cd go_base
```

## 第一个程序
**vim test.go**

```
package main

import "fmt"

func main() {
	fmt.Println("Hello")
}
```

**运行方法一:**

```
go run test.go
```

**结果:**
```
Hello
```

**运行方法二:**

```
go install
cd ~/go/bin
go_base

```
**结果:**
```
Hello
```

### IDE工具 LiteIDE X

下载地址：[http://mac.softpedia.com/get/Developer-Tools/LiteIDE-X.shtml#download](http://mac.softpedia.com/get/Developer-Tools/LiteIDE-X.shtml#download)

目前有很多Go的开发工具：LiteIDE、sublime、VIM、Emacs、Eclipse、Idea等工具，读者可以根据自己熟悉的工具进行配置，希望能够通过方便的工具快速的开发Go应用。

## 第一章 语言基础

学习地址：[https://github.com/astaxie/build-web-application-with-golang/blob/master/](https://github.com/astaxie/build-web-application-with-golang/blob/master/)

Go是一门类似C的编译型语言，但是它的编译速度非常快。这门语言的关键字总共也就二十五个，比英文字母还少一个，这对于我们的学习来说就简单了很多。先让我们看一眼这些关键字都长什么样：

```
break    default      func    interface    select
case     defer        go      map          struct
chan     else         goto    package      switch
const    fallthrough  if      range        type
continue for          import  return       var
```

![aa](https://github.com/astaxie/build-web-application-with-golang/raw/master/zh/images/navi2.png?raw=true)

### 重点详解
`package <pkgName>`（在我们的例子中是`package main`）这一行告诉我们当前文件属于哪个包，而包名`main`则告诉我们它是一个可独立运行的包，它在编译后会产生可执行文件。除了`main`包之外，其它的包最后都会生成`*.a`文件（也就是包文件）并放置在`$GOPATH/pkg/$GOOS_$GOARCH`中（以Mac为例就是`$GOPATH/pkg/darwin_amd64`）。

> * 每一个可独立运行的Go程序，必定包含一个package main，在这个main包中必定包含一个入口函数main，而这个函数既没有参数，也没有返回值。

为了打印`Hello, world...`，我们调用了一个函数`Printf`，这个函数来自于`fmt`包，所以我们在第三行中导入了系统级别的fmt包：`import "fmt"`。`

包的概念和Python中的package类似，它们都有一些特别的好处：模块化（能够把你的程序分成多个模块)和可重用性（每个模块都能被其它应用程序反复使用）。我们在这里只是先了解一下包的概念，后面我们将会编写自己的包。

在第五行中，我们通过关键字func定义了一个main函数，函数体被放在`{}`（大括号）中，就像我们平时写C、C++或Java时一样。

大家可以看到main函数是没有任何的参数的，我们接下来就学习如何编写带参数的、返回0个或多个值的函数。

第六行，我们调用了fmt包里面定义的函数`Printf`。大家可以看到，这个函数是通过`<pkgName>.<funcName>`的方式调用的，这一点和Python十分相似。

> * 前面提到过，包名和包所在的文件夹名可以是不同的，此处的`<pkgName>`即为通过`package <pkgName>`声明的包名，而非文件夹名。

最后大家可以看到我们输出的内容里面包含了很多非ASCII码字符。实际上，Go是天生支持UTF-8的，任何字符都可以直接输出，你甚至可以用UTF-8中的任何字符作为标识符。