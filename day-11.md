### 一 defer 延迟执行
#### 1.1 defer 延迟执行修饰符
在函数中，程序员经常需要创建资源(比如:数据库连接、文件句柄、锁等) ，为了在函数执行完毕后，及时的释放资源，Go 设计者提供了 `defer`(延时机制):
```go
func main() {
	// 当执行到 defer 语句时，暂不执行，会将 defer 后的语句压入到独立的栈中,当函数执行完毕后，再从该栈按照先入后出的方式出栈执行
	defer fmt.Println("defer1...")
	defer fmt.Println("defer2...")
	fmt.Println("main...")
}
```

上述代码执行结果：
```
main...
defer2...
defer1...
```

`defer` 将语句放入到栈时，也会将相关的值拷贝同时入栈:
```go
func main() {
	num := 0
	defer fmt.Println("defer 中：num=", num)
	num = 3
	fmt.Println("main中：num=",num)
}
```

输出结果：
```
main 中：num= 3
defer 中：num= 0
```

#### 1.2 defer 最佳实践
defer 最佳实践：用于关闭资源，比如：`defer connect.close()`。  

案例一：defer 处理资源  
没有使用 defer 时打开文件处理代码：
```go
f,err := os.Open(file)
if err != nil {
	return 0
}

info,err := f.Stat()
if err != nil {
	f.Close()
	return 0
}

f.Close()
return 0;

```

使用 defer 优化：
```go

f,err := os.Open(file)
if err != nil {
	return 0
}

defer f.Close()

info,err := f.Stat()

if err != nil {
	// f.Close()			// 这句已经不需要了
	return 0
}

// 后续一系列文件操作后执行关闭
// f.Close()			// 这句已经不需要了
return 0;
```


案例二：并发使用 map 的函数。  
无 defer 代码：
```go
var (
	mutex sync.Mutex
	testMap = make(map[string]int)
)
func getMapValue(key string) int {
	mutex.Lock()						// 对共享资源加锁
	value := testMap[key]
	mutex.Unlock()
	return value
}
```

上述案例是很常见的对并发 map 执行加锁执行的安全操作，使用 defer 可以对上述语义进行简化：
```go
var (
	mutex sync.Mutex
	testMap = make(map[string]int)
)
func getMapValue(key string) int {
	mutex.Lock()						//对共享资源加锁
	defer mutex.Unlock()
	return testMap[key]
}
```

#### 1.3 defer 无法处理全局资源
使用 defer 语句, 可以方便地组合函数/闭包和资源对象，即使 panic 时，defer 也能保证资源的正确释放。但是上述案例都是在局部使用和释放资源，如果资源的生命周期很长， 而且可能被多个模块共享和随意传递的话，defer 语句就不好处理了，需要下面的方式。  

Go 的 `runtime` 包的 `func SetFinalize(x, f interface{}) 函数可以提供类似 C++ 析构函数的机制，比如我们可以包装一个文件对象，在没有人使用的时候能够自动关闭：
```go
type MyFile struct {
	f *os.File
}

func NewFile(name string) (&MyFile, error){
	f, err := os.Open(name)
	if err != nil {
		return nil, err
	}
	runtime.SetFinalizer(f, f.f.Close)
	return &MyFile{f: f}, nil
}
```
在使用 `runtime.SetFinalizer` 时, 需要注意的地方是尽量要用指针访问内部资源，这样的话, 即使 `*MyFile` 对象忘记释放, 或者是被别的对象无意中覆盖, 也可以保证内部的文件资源可以正确释放。