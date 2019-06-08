
### 一 Map 映射
#### 1.0 map 算法
需要使用任意类型的关联，就需要用到集合的映射，比如学号和名字。Go 语言提供了映射关系容器是 map，内部使用散列表（hash）实现。  

大多数语言中映射关系容器使用两种算法：散列表和平衡树。  
散列表可以简单的描述为一个数组，数组的每个元素都是列表，根据散列函数获得每个元素的特征值，将特征值作为映射的键。如果特征值重复，表示元素发生了碰撞，需要尽量避免碰撞，这样就需要多容器扩容，每次扩容，元素都需要重新放入，较为耗时。  
平衡树类似于有父子关系的一棵数据数，每个元素在放入树时，都要与一些节点进行比较。

#### 1.1 map 的创建
Go 内置了 map 类型，map 是一个无序键值对集合（也有一些书籍翻译为字典）。  
普通创建：
```
//声明一个map类型，[]内的类型指任意可以进行比较的类型 int 指值类型
m := map[string]int{"a":1,"b":2}
fmt.Print(m["a"])
```

make 方式创建 map：
```go
type Person struct{
	ID string
	Name string
}

func main() {
	var m map[string] Person				
	m = make(map[string] Person)
	m["123"] = Person{"123","Tom"}
	p,isFind := m["123"]
	fmt.Println(isFind)		// true
	fmt.Println(p)			// {123 Tom}
}
```

注意：key 可以是什么类型？
golang 中的 map，key 可以是很多种类型，比如 bool, 数字，string, 指针, channel , 还可以是只包含前面几个类型的接口, 结构体, 数组。  
通常 key 为 int 、string。  

#### 1.2 map 的使用
map 的读取和设置也类似 slice 一样，通过 key 来操作，只是 slice 的 index 只能是｀int｀类型，而 map 多了很多类型，可以是 int，可以是 string 及所有完全定义了 == 与 != 操作的类型。
```go
// 声明一个key是字符串，值为int的字典,这种方式的声明需要在使用之前使用make初始化
var numbers map[string]int
// 另一种map的声明方式
numbers = make(map[string]int)
numbers["one"] = 1 // 赋值
numbers["ten"] = 10 // 赋值
numbers["three"] = 3

fmt.Println("第三个数字是: ", numbers["three"]) // 读取数据
// 打印出来如:第三个数字是: 3
```

map 的遍历：同数组一样，使用 for-range 的结构遍历  
注意：
map 是无序的，每次打印出来 map 都会不一样，它不能通过 index 获取，而必须通过 key 获取；  
map 的长度是不固定的，也就是和 slice 一样，也是一种引用类型	 
内置的 len 函数同样适用于 map，返回 map 拥有的 key 的数量  
map 的值可以很方便的修改，通过 numbers["one"]=11 可以很容易的把 key 为 one 的字典值改为 11  
map 和其他基本型别不同，它不是 thread-safe，在多个 go-routine 存取时，必须使用 mutex lock 机制
```go
lookup := map[string]int{
  "goku": 9001,
  "gohan": 2044,
}

for key, value := range lookup {
  ...
}
```

#### 1.3 删除元素
```go
// 初始化一个字典
rating := map[string]float32{"C":5, "Go":4.5, "Python":4.5, "C++":2 }
// map有两个返回值，第二个返回值，如果不存在key，那么ok为false，如果存在ok为true
csharpRating, ok := rating["C#"]
if ok {
    fmt.Println("C# is in the map and its rating is ", csharpRating)
} else {
    fmt.Println("We have no rating associated with C# in the map")
}

delete(rating, "C") // 删除 key 为 C 的元素
```

注意：go 没有提供清空元素的方法，可以重新 make 一个新的 map，不用担心垃圾回收的效率，因为 go 中并行垃圾回收效率比写一个清空函数高效很多。

#### 1.4 sync.Mpa
Go 内置的 map 只读是线程安全的，读写是线程不安全的，并发安全的 map 可以使用标准包 sync 中的 map。 

演示并发洗 map 的问题：
```go
package main

func main() {

	m := make(map[int]int)

	go func() {			
		for {				//无限写入
			m[1] = 1
		}
	}()

	go func() {
		for {				//无限读取
			_ = m[1]
		}
	}()

	for {}					//无限循环，让并发程序在后台执行

}
```

错误提示：`fatal error: concurrent map read and map write`，即出现了并发读写，因为用两个并发程序不断的对 map 进行读和写，产生了竞态问题。map 背部会对这种错误进行检查并提前发现。  

需要并发读写时，一般都是加锁，但是这样做性能不高在 go1.9 版本中提供了更高效并发安全的 sync.Map。  

sync.Map的特点：
- 无须初始化，直接声明即可
- sync.Map 不能私用 map 的方式进行取值和设置操作，而是使用 sync.Map 的方法进行调用。Store 表示存储，Load 表示获取，Delete 表示删除。 
- 使用 Range 配合一个毁掉函数进行遍历操作，通过回调函数返回内部遍历出来的值，需要继续迭代时，返回 true，终止迭代返回 false。
```go
package main

import (
	"fmt"
	"sync"
)

func main() {

	var scene sync.Map

	//保存键值对
	scene.Store("id",1)
	scene.Store("name","lisi")

	//根据键取值
	fmt.Println(scene.Load("name"))			

	//遍历
	scene.Range(func(k, v interface{}) bool{
		fmt.Println(k,v)
		return true
	})

}
```
注意：map 没有提供获取 map 数量的方法，可以在遍历时手动计算。sync.Map 为了并发安全，损失了一定的性能。  

