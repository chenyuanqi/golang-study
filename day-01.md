
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
```

