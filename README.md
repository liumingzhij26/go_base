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

