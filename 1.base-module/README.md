# go 程序基础

## 文件命名规范

- Go 源文件总是用全小写字母形式的短小单词命名，并且以 .go 扩展名结尾。
- 如果要在源文件的名字中使用多个单词，我们通常直接是将多个单词连接起来作为源文件名，而不是使用其他分隔符，比如下划线。也就是说，我们通常使用 helloworld.go 作为文件名而不是 hello_world.go。
- 尽量不要用两个以上的单词组合作为文件名，否则就很难分辨了。

## 程序结构

```go
package main
// 包是 go 语言的基本组成单元，通常使用单个的小写单词命名，一个 go 程序本质上就是一组包的集合。
// 
// * 整个 go 程序中仅允许存在一个 main 包。

import "fmt"
// fmt 表示的是标准库下的 fmt 目录

func main() {
    fmt.Println("hello, world")
  // 首字母为大写的标识符才是导出的（Exported），才能对包外的代码可见；如果首字母是小写的，那么就说明这个标识符仅限于在声明它的包内可见。
}
```

## 程序编译与运行

### 编译

`go build main.go`

### 直接运行

`go run main.go`

## 依赖管理 Go module

> Go module 是 Go 默认的包依赖管理机制和 Go 源码构建机制。

一个 module 就是一个包的集合，这些包和 module 一起打版本、发布和分发。go.mod 所在的目录被我们称为它声明的 module 的根目录。
go.mod 记录直接依赖的包的信息
go.sum 记录直接依赖和间接依赖包的相关版本的 hash 值，用来校验本地包的真实性。在构建的时候，如果本地依赖包的 hash 值与 go.sum 文件中记录的不一致，就会被拒绝构建。

### .mod 文件内容示例

```go
module hellomodule

go 1.19

require (
  github.com/valyala/fasthttp v1.28.0
  go.uber.org/zap v1.18.1
)
```

### 自动添加依赖

`go mod tidy`

### 配置国内代理
  
`go env -w GOPROXY=https://mirrors.aliyun.com/goproxy` # 阿里云镜像
或着
`go env -w GOPROXY=https://goproxy.cn,direct` # goproxy.cn

`go env|grep GOPROXY` # 验证

## Q&A

### module path 和 依赖安装位置

Go Module 的概念我还是不怎么理解,很多教程都用`github.com...` 做例子， 你说的是”路径“，路径不应该是
`（win10）"D:\goprojects\hellomodule"`吗？我自己自定义一个任意的名字也可以正常运行？我感觉很奇怪，为什么很多人用`githubcom/...`做module呢?
还有`go mod tidy` 命令后，下载的包在哪？在Go语言安装路径的src文件夹中？还是在哪?

  > 作者回复:
    1. 使用`github.com/...`作为module path是因为多数实用级module多是上传到github上的。用这种示例便于后续与真实生产接驳。但对于本地开发使用的简单示例程序而言，你说的没错。module path可以任意起，比如：`module demo1`也是ok的。
    2. go mod tidy下载的第三方包一般在GOPATH/pkg/mod下面。如果没有设置GOPATH环境变量，其默认值为你的home路径下的go文件夹。这样第三方包就在go文件夹的pkg/mod下面。
