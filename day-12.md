
### 一 错误 Error
#### 1.1 Go 自带的错误接口
error 是 go 语言声明的接口类型：
```go
type error interface {
	Error() string
}
```
所有符合 Error()string 格式的方法，都能实现错误接口，Error() 方法返回错误的具体描述。

#### 1.2 自定义错误
返回错误前，需要定义会产生哪些可能的错误，在 Go 中，使用 errors 包进行错误的定义，格式如下：
```go
var err = errors.New("发生了错误")
```
提示：错误字符串相对固定，一般在包作用于声明，应尽量减少在使用时直接使用 errors.New 返回。

下面这个例子演示了如何使用 `errors.New`:
```go
func Sqrt(f float64) (float64, error) {
	if f < 0 {
		return 0, errors.New("math: square root of negative number")
	}
	// implementation
}
```

在 C 语言里面，是通过返回 -1 或者 NULL 之类的信息来表示错误，但是对于使用者来说，不查看相应的 API 说明文档，根本搞不清楚这个返回值究竟代表什么意思，比如:返回 0 是成功，还是失败,而 Go 定义了一个叫做 error 的类型，来显式表达错误。在使用时，通过把返回的 error 变量与 nil 的比较，来判定操作是否成功。例如`os.Open`函数在打开文件失败时将返回一个不为 nil 的 error 变量。

```Go
func Open(name string) (file *File, err error)
```

下面这个例子通过调用 `os.Open` 打开一个文件，如果出现错误，那么就会调用 `log.Fatal` 来输出错误信息：
```Go
f, err := os.Open("filename.ext")
if err != nil {
	log.Fatal(err)
}
```
类似于 `os.Open` 函数，标准包中所有可能出错的 API 都会返回一个 error 变量，以方便错误处理。

#### 1.3 自定义错误案例
案例一：简单的错误字符串提示
```go
package main

import (
	"errors"
	"fmt"
)

// 定义除数为 0 的错误
var errByZero = errors.New("除数为0")

func div(num1, num2 int) (int, error) {

	if num2 == 0 {
		return 0, errByZero
	}

	return num1 / num2, nil

}

func main() {
	fmt.Println(div(1, 0))
}
```

案例二：实现错误接口
```go
package main

import (
	"fmt"
)

// 声明一种解析错误
type ParseError struct {
	Filename string
	Line int
}

// 实现 error 接口，返回错误描述
func (e *ParseError) Error() string {
	return fmt.Sprintf("%s:%d", e.Filename, e.Line)
}

// 创建一些解析错误
func newParseError(filename string, line int) error {
	return &ParseError{filename, line}
}

func main() {

	var e error

	e = newParseError("main.go", 1)

	fmt.Println(e.Error())

	switch detail := e.(type) {
	case *ParseError:
		fmt.Printf("Filename: %s Line:%d \n", detail.Filename, detail.Line)
	default: 
		fmt.Println("other error")
	}

}
```

#### 1.4 errors 包分析
Go 中的 errors 包对 New 的定义非常简单:
```go

// 创建错误对象
func New(text string) error {
	return &errorString{text}
}

// 错误字符串
type errorString struct {
	s string
}

// 返回发生何种错误
func (e *errorString) Error() string {
	return e.s
}
```
错误对象都要事先 error 接口的 Error() 方法，这样，所有的错误都可以获得字符串的描述。

### 二 panic 宕机
#### 2.1 手动触发宕机
Go 语言可以在程序中手动触发宕机，让程序崩溃，这样开发者可以及时发现错误。  
Go 语言程序在宕机时，会将堆栈和 goroutine 信息输出到控制台，所以宕机可以更方便知晓发生错误的位置。如果在编译时加入的调试信息甚至连崩溃现场的变量值、运行状态都可以获取，那么如何触发宕机呢？  
```go
package main

func main() {
	panic("crash")
}
```

运行结果是：
```
panic: crash

goroutine 1 [running]:
main.main()
	/Users/username/Desktop/TestGo/src/main.go:5 +0x39
exit status 2
```

使用 `panic` 函数可以制造崩溃，panic 声明如下；
```go
func panic(v interface{})
```
panic() 参数可以是任意类型。  
注意：手动触发宕机并不是一种偷懒的方式，反而能迅速报错，终止程序继续运行，防止更大的错误产生，但是如果任何错误都使用宕机处理，也不是一个良好的设计。  

#### 2.2 defer 与 panic
当 panic() 发生宕机，panic() 后面的代码将不会被执行，但是在 panic 前面已经运行过的 defer 语句依然会在宕机时发生作用：
```go
package main

import "fmt"

func main() {
	defer fmt.Println("before")
	panic("crash")
}
```

### 三 recover 宕机恢复
#### 3.1 让程序在崩溃时继续执行
无论是代码运行错误由 Runtime 层抛出的 panic 崩溃，还是主动触发的 panic 崩溃，都可以配合 defer 和 recover 实现错误捕捉和处理，让代码在发生崩溃后允许继续执行。  

在其他语言里，宕机往往以异常的形式存在，底层抛出异常，上层逻辑通过 try/catch 机制捕获异常，没有被捕获的严重异常会导致宕机，捕获的异常可以被忽略，让代码继续执行。Go 没有异常系统，使用 panic 触发宕机类似于其他语言的抛出异常，recover 的宕机恢复机制就对应 try/catch 机制。  

#### 3.2 使用 defer 与 recover 处理错误
```go
func test(num1 int, num2 int){
	defer func(){
		err := recover()	// recover 内置函数，可以捕获异常
		if err != nil {
			fmt.Println("err=", err);
		}
	}()
	fmt.Println(num1/num2)
}

func main() {
	test(2,0)
}
```

#### 3.3 panic recover 综合示例
```go

package main

import (
	"fmt"
	"runtime"
)

// 崩溃时需要传递的上下文信息
type panicContext struct {
	function string
}

// 保护方式允许一个函数
func ProtectRun(entry func()) {

	defer func() {
		err := recover()	// 发生宕机时，获取 panic 传递的上下文并打印
		switch err.(type) {
		case runtime.Error:
			fmt.Println("runtime error:", err)
		default:
			fmt.Println("error:", err)
		}
	}()
	
	entry()

}

func main() {

	fmt.Println("运行前")

	ProtectRun(func(){

		fmt.Println("手动宕机前")

		panic(&panicContext{"手动触发panic",})

		fmt.Println("手动宕机后")

	})

	ProtectRun(func(){

		fmt.Println("赋值宕机前")

		var a *int
		*a = 1

		fmt.Println("赋值宕机后")

	})

	fmt.Println("运行后")

}
```

运行结果：
```
运行前
手动宕机前
error: &{手动触发panic}
赋值宕机前
runtime error: runtime error: invalid memory address or nil pointer dereference
运行后
```

#### 3.4 panic 和 recover 的关系
panic 和 recover 的组合：
- 有 panic 没有 recover，程序宕机
- 有 panic 也有 recover，程序不会宕机，执行完对应的 defer 后，从宕机点退出当前函数后继续执行
