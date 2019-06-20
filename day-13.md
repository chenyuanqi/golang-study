
### 一 Package 包
#### 1.1 package 与 import
在实际的开发中，我们往往需要在不同的文件中，去调用其它文件的定义的函数，比如 main.go 中，需要使用 "fmt" 包中的 Println() 函数：
```go
package main
import "fmt"
```
在 Go 中，Go 的每一个文件都是属于一个包的，也就是说 Go 是以包的形式来管理文件和项目目录结构。所以如果要导入某些第三方包，直接输入包所在地址即可。文件的包名通常和文件所在的文件夹名一致，一般为小写字母。
```
引入方式 1:import "包名"
引入方式 2:
import (
	"包名"
	"包名" 
)
```
- package 指令在 文件第一行，然后是 import 指令
- 在 import 包时，路径从 $GOPATH 的 src 下开始，不用带 src , 编译器会自动从 src 下开始引入
- 为了让其它包的文件，可以访问到本包的函数，则该函数名的首字母需要大写，类似其它语言的 public ,这样才能跨包访问
- 在访问其它包函数，变量时，其语法是 `包名.函数名`

#### 1.2 GoPath
GoPath 目录用来存放代码文件、可运行文件、编译后的包文件。  
从 1.1 版本到 1.7 版本必须设置这个变量，而且不能和 Go 的安装目录一样，1.8 版本后会有默认值：
```
Unix: $HOME/go
Windows: %USERPROFILE%/go。
```
GOPATH 允许多个目录，多个目录的时候，Windows 是分号，Linux 系统是冒号隔开。当有多个 GOPATH 时，默认会将 go get 的内容放在第一个目录下，$GOPATH 目录约定有三个子目录：  
- src: 存放源代码，一般一个项目分配一个子目录;  
- pkg: 编译后生成的文件，如 .a 文件  
- bin: 编译后生成的可执行文件,可以加入 $PATH 中  
> 注意：一般建议 package 的名称和目录名保持一致  

#### 1.3 包中的函数调用方式
函数调用的方式：
- 同包下：直接调用即可
- 不同包下：包名.函数名

### 二 go mod
#### 2.1 go mod 的使用
go 的项目依赖管理一直饱受诟病，在 go1.11 后正式引入了 `go mod` 功能，类似 nodejs 的 npm。在 go1.13 版本中将会默认启用。  
使用 go mod 可以让项目完全摆脱 GOPATH 的困扰。  
`go mod` 初步使用：  
```
# 开启 go mod
export GO111MODULE=on

# 在新建的项目根目录下（src）下使用该命令
go mod init 项目名                      # 此时会生成一个 go.mod 文件

# 使用
在项目中可以随时 import 依赖，当 go run 时候，会自动安装依赖，比如：
import (
	"github.com/gin-gonic/gin"
)
```

go run 后的 go.mod:
```
module api_server

go 1.12

require (
	github.com/gin-contrib/sse v0.0.0-20190301062529-5545eab6dad3 // indirect
	github.com/gin-gonic/gin v1.3.0 // indirect
	github.com/golang/protobuf v1.3.1 // indirect
	github.com/mattn/go-isatty v0.0.7 // indirect
	github.com/ugorji/go/codec v0.0.0-20190320090025-2dc34c0b8780 // indirect
	gopkg.in/go-playground/validator.v8 v8.18.2 // indirect
	gopkg.in/yaml.v2 v2.2.2 // indirect
)
```

使用 `go mod` 后，run 产生的依赖源码不会安装在当前项目中，而是安装在：`$GOPATH/pkg/mod`

#### 2.2 go mod 详细使用
详细使用地址：https://zhuanlan.zhihu.com/p/59687626

### 三 翻墙问题解决
#### 3.1 推荐方式 GOPROXY
从 Go 1.11 版本开始，还新增了 GOPROXY 环境变量，如果设置了该变量，下载源代码时将会通过这个环境变量设置的代理地址，而不再是以前的直接从代码库下载。 goproxy.io 这个开源项目帮我们实现好了我们想要的。该项目允许开发者一键构建自己的 GOPROXY 代理服务。同时，也提供了公用的代理服务 https://goproxy.io ，我们只需设置该环境变量即可正常下载被墙的源码包了：  

```
# 开发时设置 Golang 的 Prefrence-Go-proxy 即可

# linux 开启代理
export GOPROXY=https://goproxy.io			# 注意：必须开启 go mod 才能使用

# win 开启代理
$env:GOPROXY = "https://goproxy.io"

# 关闭代理
export GOPROXY=
```

#### 3.2 replace 方式
从 Go 1.11 版本开始，新增支持了 go modules 用于解决包依赖管理问题。该工具提供了 replace，就是为了解决包的别名问题，也能替我们解决 golang.org/x 无法下载的的问题。

go module 被集成到原生的 go mod 命令中，但是如果你的代码库在 $GOPATH 中，module 功能是默认不会开启的，想要开启也非常简单，通过一个环境变量即可开启 export GO111MODULE=on。

```go
module example.com/hello

require (
    golang.org/x/text v0.3.0
)

replace (
    golang.org/x/text => github.com/golang/text v0.3.0
)
```

#### 3.3 手动下载旧版 go 的解决
我们常见的 golang.org/x/... 包，一般在 GitHub 上都有官方的镜像仓库对应。比如 golang.org/x/text 对应 github.com/golang/text。所以，我们可以手动下载或 clone 对应的 GitHub 仓库到指定的目录下。
```bash
mkdir $GOPATH/src/golang.org/x
cd $GOPATH/src/golang.org/x
git clone git@github.com:golang/text.git
rm -rf text/.git
```
当如果需要指定版本的时候，该方法就无解了，因为 GitHub 上的镜像仓库多数都没有 tag。并且，手动嘛，程序员怎么能干呢，尤其是依赖的依赖，太多了。

### 四 go mod 引起的变化
引包方式变化：
- 不使用 go mod 引包："./test"  引入 test 文件夹
- 使用 go mod 引包："projectmodlue/test" 使用 go.mod 中的 modlue 名/包名

因为在 go1.11 后如果开启了 `go mod`，需要在 src 目录下存在 go.mod 文件，并书写主 module 名（一般为项目名），否则无法 build。

开启 `go mod` 编译运行变化：
- 使用 vscode 开发，必须在 src 目录下使用 `go build` 命令执行，不要使用 code runner 插件
- 使用 IDEA 开发，项目本身配置 go.mod 文件仍不能支持，开发工具本身也要开启 `go mod` 支持（位于配置的 go 设置中）


