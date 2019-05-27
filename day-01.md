
### Hello golang
```
package main

import 'fmt'

func main() {
  fmt.Println('Hello, golang ~')
}
```

### Install it
```
# mac
brew update && brew upgrade
brew install go

# linux
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
gvm install go1.8.3
gvm use go1.8.3

# windows
# 安装完成后默认会在环境变量 Path 后添加 Go 安装目录下的 bin 目录 C:\Go\bin\
# 并且，添加环境变量 GOROOT，值为 Go 安装根目录 C:\Go\
下载页：https://golang.org/dl/
```

### Workspace
- $GOPATH 目录约定有三个子目录
> src 存放源代码（比如：.go .c .h .s 等）  
> pkg 编译后生成的文件（比如：.a）  
> bin 编译后生成的可执行文件（为了方便，可以把此目录加入到 $PATH 变量中，如果有多个 gopath，那么使用${GOPATH//://bin:}/bin添加所有的 bin 目录）  

### Command
```bash
# 更新所有的依赖包 
$ go mod download
# 热启动项目 
fresh -c ./fresh.conf

# Go 编译器命令
go command [arguments]                              # go 命令 [参数]
go build                                            # 编译包和依赖包
go clean                                            # 移除对象和缓存文件
go doc                                              # 显示包的文档
go env                                              # 打印 go 的环境变量信息
go bug                                              # 报告 bug
go fix                                              # 更新包使用新的 api
go fmt                                              # 格式规范化代码
go generate                                         # 通过处理资源生成 go 文件
go get                                              # 下载并安装包及其依赖
go install                                          # 编译和安装包及其依赖
go list                                             # 列出所有包
go run                                              # 编译和运行 go 程序
go test                                             # 测试
go tool                                             # 运行给定的 go 工具
go version                                          # 显示 go 当前版本
go vet                                              # 发现代码中可能的错误

godoc --http=:8080                                  # 本地查看go官网
```

