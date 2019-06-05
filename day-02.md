
### Golang Link
[Go 官网](https://golang.org/)  
[Go pkg](https://golang.org/pkg/)  
[Go api](https://gowalker.org/)  

[Go 编程基础](https://github.com/Unknwon/go-fundamental-programming)  
[Go 指南](https://tour.go-zh.org/list)  
[Go 学习](https://miek.nl/go/)  
[Go 练习](https://play.golang.org/)  
[Go 示例](https://gobyexample.com/)  
[Go 正则](http://regoio.herokuapp.com/)  

[Go 论坛](https://studygolang.com/)  
[Go 知识社区](https://learnku.com/golang)  

[Go 建议](https://github.com/cristaloleg/go-advices)  
[Go 知识图谱](https://github.com/gocn/knowledge)  
[Go 语言圣经](https://yar999.gitbooks.io/gopl-zh/content/)  
[awesome-go](https://awesome-go.com/)  

### Golang Identifier
标识符用来命名变量、类型等程序实体。  

- 关键字（25 个）
```
if      for     func    case        struct      import               
go      type    chan    defer       default     package
map     const   else    break       select      interface
var     goto    range   return      switch      continue     fallthrough
```

- 预定义名字（30+ 个）
```
内建常量：  
        true        false       iota        nil
内建类型：  
        int         int8        int16       int32       int64
        uint        uint8       uint16      uint32      uint64      uintptr
        float32     float64 
        complex128  complex64
bool：      
        byte        rune        string 	    error
内建函数：   
        make        delete      complex     panic       append      copy    
        close       len         cap	        real        imag        new   	    recover
```

### Golang Basic Programming
<details>
<summary>行分隔符</summary>

- 在 Go 程序中，一行代表一个语句结束，不需要分隔符。
- 打算将多个语句写在同一行，它们则必须使用 `;` 人为区分，并不鼓励这种做法。
</details>

<details>
<summary>注释方法</summary>

```go
// 单行注释

/*
  多行注释
*/
```
</details>

<details>
<summary>包引用 import</summary>

```go
import "fmt"
import "os"
import "io"
```

简写方式如下

```go
import (
  "fmt"
  "os"
  "io"
)
```

**包引用介绍**

```bash
.
├── cal
│   ├── add.go
│   ├── multi
│   │   └── multiply.go
│   └── subtract.go
└── main.go
```

注意：package 文件夹复制到 `$GOPATH/src/` 目录下，不然运行报错哦
```bash
go run $GOPATH/src/package-demo/main.go
```

main.go 中如何调用 add.go、subtract.go 或者是 multiply.go 文件中的函数。
> `add.go` 和 `subtract.go` 文件中，包名为 cal `package cal`  
> `multiply.go` 在 multi 文件夹下，所以程序的包名为 multi `package multi`  
> 如果 mian 函数要调用`add.go`或者`subtract.go`中的函数，必须要引入包"cal" `import "package-demo/cal"`  
> 要调用 `multiply.go` 中的函数，必须要引入包 "multi"，`import "package-demo/cal/multi"`  
> Go 中如果函数名的首字母大写，表示该函数是公有的，可以被其他程序调用，如果首字母小写，该函数就是是私有的

</details>

<details>
<summary>包别名</summary>

```go
import(
  ff "fmt"
)

// 或者
import ff "fmt"

// 别名包调用
ff.Println('Hello World!')
```

</details>

<details>
<summary>省略调用</summary>

```go
import(
  . "fmt"
)
func main() {
  // 省略调用
  Println('Hello World!')
}
```

</details>

<details>
<summary>可见性规则</summary>

Go语言中约定使用 **大小写** 来决定常量、变量、类型、接口、结构或函数是否可以被外部包所调用

- 函数名字首字母 **小写** 即为 `private` 私有的
- 函数名字首字母 **大写** 即为 `public` 公有

</details>

### Golang Basic Structure
```golang
// 当前程序的包名
 package main

// 导入其它的包
 import std "fmt"

// 常量的定义
 const PI = 3.14

// 全局变量的声明与赋值
 var name = "gopher"

// 一般类型声明
 type newType int

// 结构的声明
 type gopher struct{}

// 接口的声明
 type golang interface{}

// 由 main 函数作为程序入口点启动
 func main() {
	std.Println("Hello world! 你好，世界！")
}
```