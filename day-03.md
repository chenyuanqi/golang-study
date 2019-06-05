
### Type
Go 基本类型
<details>
<summary>布尔型</summary>

```go
var b bool
b  = true
fmt.Printf("b is of type %t\n", b)
e := bool(true)
fmt.Printf("e is of type %t\n", e)
```

- 长度：1字节
- 取值范围：true/false
- 只能使用true/false值，不能使用数字代替布尔值

</details>

<details>
<summary>整形 int/uint</summary>

```go
package main
import "fmt"
func main() {
  // n 是一个长度为 10 的数组
  var n [10]int 
  var i,j int

  /* 为数组 n 初始化元素 */         
  for i = 0; i < 10; i++ {
    n[i] = i + 100 /* 设置元素为 i + 100 */
  }

  /* 输出每个数组元素的值 */
  for j = 0; j < 10; j++ {
    fmt.Printf("Element[%d] = %d\n", j, n[j] )
  }
}
```

- int/uint
- 根据平台可能为32/64位

</details>

<details>
<summary>8位整型 int8/uint8</summary>

```go
u8 := []uint8{98, 99}

a := byte(255)  //11111111 这是byte的极限， 因为 a := byte(256)//越界报错， 0~255正好256个数，不能再高了
b := uint8(255) //11111111 这是uint8的极限，因为 c := uint8(256)//越界报错，0~255正好256个数，不能再高了
c := int8(127)  //01111111 这是int8的极限， 因为 b := int8(128)//越界报错， 0~127正好128个数，所以int8的极限只是256的一半
d := int8(a)    //11111111 打印出来则是-0000001，int8(128)、int8(255)、int8(byte(255))都报错越界，因为int极限是127，但是却可以写：int8(a)，第一位拿来当符号了
e := int8(c)    //01111111 打印出来还是01111111
fmt.Printf("%08b %d \n", a, a)
fmt.Printf("%08b %d \n", b, b)
fmt.Printf("%08b %d \n", c, c)
fmt.Printf("%08b %d \n", d, d)
fmt.Printf("%08b %d \n", e, e)
```

- int8/uint8
- 长度：1 字节
- 取值范围：-128~127/0~255

</details>

<details>
<summary>字节型 byte</summary>

```go
// 这里不能写成 b := []byte{"Golang"}，这里是利用类型转换。
b := []byte("Golang")
c := []byte("go")
d := []byte("Go")
println(b,c,d)
```

- byte(uint8别名)

**基本处理函数**

- `Contains()` 返回是否包含子切片
- `Count()` 子切片非重叠实例的数量
- `Map()` 函数，将byte 转化为Unicode，然后进行替换
- `Repeat()` 将切片复制count此，返回这个新的切片
- `Replace()` 将切片中的一部分 替换为另外的一部分
- `Runes()` 将 S 转化为对应的 UTF-8 编码的字节序列，并且返回对应的Unicode 切片
- `Join()` 函数，将子字节切片连接到一起。

可以参考下面列子来理解上面 7 个方法
```go
package main
import (
  "bytes"
  "fmt"
)
func main() {
  // 这里不能写成 b := []byte{"Golang"}，这里是利用类型转换。
  b := []byte("Golang")
  subslice1 := []byte("go")
  subslice2 := []byte("Go")
  // func Contains(b, subslice [] byte) bool
  // 检查字节切片b ，是否包含子字节切片 subslice
  fmt.Println(bytes.Contains(b, subslice1))
  fmt.Println(bytes.Contains(b, subslice2))


  s2 := []byte("同学们，上午好")
  m := func(r rune) rune {
    if r == '上' {
      r = '下'
    }
    return r
  }
  fmt.Println(string(s2))
  // func Map(mapping func(r rune) rune, s []byte) []byte
  // Map函数: 首先将 s 转化为 UTF-8编码的字符序列，
  // 然后使用 mapping 将每个Unicode字符映射为对应的字符，
  // 最后将结果保存在一个新的字节切片中。
  fmt.Println(string(bytes.Map(m, s2)))


  s3 := []byte("google")
  old := []byte("o")
  //这里 new 是一个字节切片，不是关键字了
  new := []byte("oo")
  n := 1
  // func Replace(s, old, new []byte, n int) []byte
  //返回字节切片 S 的一个副本， 并且将前n个不重叠的子切片 old 替换为 new，如果n < 0 那么不限制替换的数量
  fmt.Println(string(bytes.Replace(s3, old, new, n)))
  fmt.Println(string(bytes.Replace(s3, old, new, -1)))


  // 将字节切片 转化为对应的 UTF-8编码的字节序列，并且返回对应的 Unicode 切片。
  s4 := []byte("中华人民共和国")
  r1 := bytes.Runes(s4)
  // func Runes(b []byte) []rune
  fmt.Println(string(s4), len(s4))  // 字节切片的长度
  fmt.Println(string(r1), len(r1))  // rune 切片的长度


  // 字节切片 的每个元素，依旧是字节切片。
  s5 := [][]byte{
    []byte("你好"),
    []byte("世界"),  //这里的逗号，必不可少
  }
  sep := []byte(",")
  // func Join(s [][]byte, sep []byte) []byte
  // 用字节切片 sep 吧 s中的每个字节切片连接成一个，并且返回.
  fmt.Println(string(bytes.Join(s5, sep)))
}
```

</details>

<details>
<summary>16位整型 int16/uint16</summary>

- int16/uint16
- 长度：2字节
- 取值范围：-32768~32767/0~65535

</details>

<details>
<summary>32位整型 int32(别名rune)/uint32</summary>

- int32(别名rune)/uint32
- 长度：4字节
- 取值范围：-2^32/2~2^32/2-1/0~2^32-1

</details>

<details>
<summary>64位整型 int64/uint64</summary>

- int64/uint64
- 长度：8字节
- 取值范围：-2^64/2~2^64/2-1/0~2^64-1

</details>

<details>
<summary>浮点型 float32/float64</summary>

```go
package main
import "fmt"

func main() {
  var x float64
  x = 20.0
  fmt.Println(x)
  fmt.Printf("x is of type %T\n", x)

  a := float64(20.0)
  b := 42 
  fmt.Println(a)
  fmt.Println(b)
  fmt.Printf("a is of type %T\n", a)
  fmt.Printf("b is of type %T\n", b)
}
```

- float32/float64
- 长度：4/8字节
- 小数位：精确到 7/15 位小数

</details>

<details>
<summary>复数 complex64/complex128</summary>

- complex64/complex128
- 长度：8/16

</details>

<details>
<summary>指针整数型 uintptr</summary>

用于指针运算，GC 不把 uintptr 当指针，uintptr 无法持有对象。uintptr 类型的目标会被回收。

- uintptr
- 保存指正的 32 位或者 64 位整数型

```go
// 示例：通过指针修改结构体字段
package main
import (
  "fmt"
  "unsafe"
)

func main() {
  s := struct {
    a byte
    b byte
    c byte
    d int64
  }{0, 0, 0, 0}

  // 将结构体指针转换为通用指针
  p := unsafe.Pointer(&s)
  // 保存结构体的地址备用（偏移量为 0）
  up0 := uintptr(p)
  // 将通用指针转换为 byte 型指针
  pb := (*byte)(p)
  // 给转换后的指针赋值
  *pb = 10
  // 结构体内容跟着改变
  fmt.Println(s)

  // 偏移到第 2 个字段
  up := up0 + unsafe.Offsetof(s.b)
  // 将偏移后的地址转换为通用指针
  p = unsafe.Pointer(up)
  // 将通用指针转换为 byte 型指针
  pb = (*byte)(p)
  // 给转换后的指针赋值
  *pb = 20
  // 结构体内容跟着改变
  fmt.Println(s)

  // 偏移到第 3 个字段
  up = up0 + unsafe.Offsetof(s.c)
  // 将偏移后的地址转换为通用指针
  p = unsafe.Pointer(up)
  // 将通用指针转换为 byte 型指针
  pb = (*byte)(p)
  // 给转换后的指针赋值
  *pb = 30
  // 结构体内容跟着改变
  fmt.Println(s)

  // 偏移到第 4 个字段
  up = up0 + unsafe.Offsetof(s.d)
  // 将偏移后的地址转换为通用指针
  p = unsafe.Pointer(up)
  // 将通用指针转换为 int64 型指针
  pi := (*int64)(p)
  // 给转换后的指针赋值
  *pi = 40
  // 结构体内容跟着改变
  fmt.Println(s)
}
```

</details>

<details>
<summary>数组类型 array</summary>

数组声明语法

```go
var variable_name [SIZE]variable_type
```

数组是具有相同唯一类型的一组已编号且长度固定的数据项序列，这种类型可以是任意的原始类型例如整形、字符串或者自定义类型。  

```go
package main
import "fmt"
func main() {
  // 声明一个长度为5的整数数组
  // 一旦数组被声明了，那么它的数据类型跟长度都不能再被改变。
  var array1 [5]int
  
  fmt.Printf("array1: %d\n\n", array1)

  // 声明一个长度为5的整数数组
  // 初始化每个元素
  array2 := [5]int{12, 123, 1234, 12345, 123456}
  array2[1] = 5000
  fmt.Printf("array2: %d\n\n", array2[1])
  
  // n 是一个长度为 10 的数组
  var n [10]int 
  var i,j int

  /* 为数组 n 初始化元素 */         
  for i = 0; i < 10; i++ {
    n[i] = i + 100 /* 设置元素为 i + 100 */
  }

  /* 输出每个数组元素的值 */
  for j = 0; j < 10; j++ {
    fmt.Printf("Element[%d] = %d\n", j, n[j] )
  }

  /* 数组 - 5 行 2 列*/
  var a = [5][2]int{ {0,0}, {1,2}, {2,4}, {3,6},{4,8}}
  var e, f int

  /* 输出数组元素 */
  for  e = 0; e < 5; e++ {
    for f = 0; f < 2; f++ {
        fmt.Printf("a[%d][%d] = %d\n", e,f, a[e][f] )
    }
  }
}
```

初始化数组中 {} 中的元素个数不能大于 [] 中的数字。
如果忽略 [] 中的数字不设置数组大小，Go 语言会根据元素的个数来设置数组的大小：

```go
var array1 = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

数组元素可以通过索引（位置）来读取。格式为数组名后加中括号，中括号中为索引的值。例如：

```go
float32 salary = array1[9]
```

以上实例读取了数组`array1`第`10`个元素的值。

多维数组，下面例子

```go
// 三行四列
a = [3][4]int{  
 {0, 1, 2, 3} ,   /*  第一行索引为 0 */
 {4, 5, 6, 7} ,   /*  第二行索引为 1 */
 {8, 9, 10, 11}   /*  第三行索引为 2 */
}
```

访问多维数组

```go
// 访问第2行第3列
int val = a[2][3]
```

</details>

<details>
<summary>结构类型 struct</summary>

```go
package main
import "fmt"
type Vertex struct {
	X int
	Y int
}
func main() {
  fmt.Println(Vertex{1, 2})
  
  // 结构体字段使用点号来访问。
	v := Vertex{1, 2}
	v.X = 4
  fmt.Println(v.X)
  
  // 结构体字段可以通过结构体指针来访问。
	e := Vertex{1, 2}
	p := &e
	p.X = 1e9
  fmt.Println(e)
  
  
  var (
    v1 = Vertex{1, 2}  // 类型为 Vertex
    v2 = Vertex{X: 1}  // Y:0 被省略
    v3 = Vertex{}      // X:0 和 Y:0
    p  = &Vertex{1, 2} // 类型为 *Vertex , 特殊的前缀 & 返回一个指向结构体的指针
  )
  fmt.Println(v1, p, v2, v3)
}
```

简单的结构体

```go
type T struct {a, b int}
```

结构体里的字段都有 名字，像 `field1`、`field2` 等，如果字段在代码中从来也不会被用到，那么可以命名它为 `_`。上面简单的结构体定义，下面调用方法：

```go
var s T
s.a = 5
s.b = 8
```

数组可以看作是一种结构体类型，不过它使用下标而不是具名的字段。

```go
var t *T
t = new(T)
```

上面简单的管用语句方法`t := new(T)`，变量 `t` 是一个指向 `T` 的指针，此时结构体字段的值是它们所属类型的零值。

声明 `var t T` 也会给 `t` 分配内存，并零值化内存，但是这个时候 `t` 是类型`T`。在这两种方式中，`t` 通常被称做类型 `T` 的一个实例（instance）或对象（object）。

一个非常简单的例子[structs_fields.go](./example/structs/structs_fields.go)运行例子查看结果：

```bash
→ go run test/structs_fields.go

The int is: 10
The float is: 15.500000
The string is: Chris
&{10 15.5 Chris}
```

**使用 new**

</details>

<details>
<summary>字符串类型 string</summary>

```go
var str string //声明一个字符串
str = "Go lang"  //赋值
ch :=str[0]    //获取第一个字符
len :=len(str) //字符串的长度,len是内置函数 ,len=5
```

len函数是Go中内置函数，不引入strings包即可使用。len(string)返回的是字符串的字节数。len函数所支持的入参类型如下：

- len(Array) 数组的元素个数
- len(*Array) 数组指针中的元素个数,如果入参为nil则返回0
- len(Slice) 数组切片中元素个数,如果入参为nil则返回0
- len(map) 字典中元素个数,如果入参为nil则返回0
- len(Channel) Channel buffer队列中元素个数

</details>

<details>
<summary>接口类型 inteface</summary>

```go
package main
import (
   "fmt"
   "math"
)

/* 定义一个 interface */
type shape interface {
   area() float64
}

/* 定义一个 circle */
type circle struct {
   x,y,radius float64
}

/* 定义一个 rectangle */
type rectangle struct {
   width, height float64
}

/* 定义一个circle方法 (实现 shape.area())*/
func(circle circle) area() float64 {
   return math.Pi * circle.radius * circle.radius
}

/* 定义一个rectangle方法 (实现 shape.area())*/
func(rect rectangle) area() float64 {
   return rect.width * rect.height
}

/* 定义一个shape的方法*/
func getArea(shape shape) float64 {
   return shape.area()
}

func main() {
   circle := circle{x:0,y:0,radius:5}
   rectangle := rectangle {width:10, height:5}

   fmt.Printf("circle area: %f\n",getArea(circle))
   fmt.Printf("rectangle area: %f\n",getArea(rectangle))
}
```

</details>

<details>
<summary>函数类型 func</summary>

```go
package main
import "fmt"
type functinTyoe func(int, int) // 声明了一个函数类型
func (f functinTyoe)Serve() {
  fmt.Println("serve2")
}
func serve(int,int) {
  fmt.Println("serve1")
}
func main() {
  a := functinTyoe(serve)
  a(1,2)
  a.Serve()
}
```

</details>

<details>
<summary>引用类型 func</summary>


**切片**

> 是一种可以动态数组，可以按我们的希望增长和收缩。

- slice

**Map**

> 是一种无序的键值对的集合。是一种集合，所以我们可以像迭代数组和 slice 那样迭代它。

- map

```go
// 通过 make 来创建
dict := make(map[string]int)
// 通过字面值创建
dict := map[string]string{"Red": "#da1337", "Orange": "#e95a22"}

// 给 map 赋值就是指定合法类型的键，然后把值赋给键
colors := map[string]string{}
colors["Red"] = "#da1337"

// 不初始化 map , 就会创建一个 nil map。nil map 不能用来存放键值对，否则会报运行时错误
var colors map[string]string
colors["Red"] = "#da1337"
// Runtime Error:
// panic: runtime error: assignment to entry in nil map

//选择是只返回值，然后判断是否是零值来确定键是否存在。
value := colors["Blue"]
if value != "" {
  fmt.Println(value)
}
```

在函数间传递 map 不是传递 map 的拷贝。所以如果我们在函数中改变了 map，那么所有引用 map 的地方都会改变

```go
func main() {
  colors := map[string]string{
     "AliceBlue":   "#f0f8ff",
     "Coral":       "#ff7F50",
     "DarkGray":    "#a9a9a9",
     "ForestGreen": "#228b22",
  }
  for key, value := range colors {
      fmt.Printf("Key: %s  Value: %s\n", key, value)
  }
  removeColor(colors, "Coral")
  for key, value := range colors {
      fmt.Printf("Key: %s  Value: %s\n", key, value)
  }
}
func removeColor(colors map[string]string, key string) {
    delete(colors, key)
}
```

<details>
<summary>通道 chan</summary>

</details>


Go 的值类型与引用类型
```
值类型：
    整型（int8,uint等）                 # 基础类型之数字类型
    浮点型（float32，float64）          # 基础类型之数字类型
    复数()                             # 基础类型之数字类型
    布尔型（bool）                      # 基础类型
    字符串（string）                    # 基础类型
    数组                               # 复合类型 
    结构体（struct）                    # 复合类型

引用类型：即保存的是对程序中一个变量的或状态的间接引用，对其修改将影响所有该引用的拷贝
    指针
    切片（slice）
    字典（map）
    函数
    管道（chan）
    接口（interface）
```
注意：
- 基本数据类型是 Go 语言实际的原子
- 复合数据类型是由不同的方式组合基本类型构造出来的数据类型，如：数组，slice，map，结构体
- 没有字符型，使用 byte 来保存单个字母

Go 类型判断
```go
a := 1
fmt.Printf("%T",a);                 // 输出变量 a 的类型，int
```
注意：int32 和 int 是两种不同的类型，编译器不会自动转换，需要类型转换。  

Go 常见格式化输出如下
```
%%	%字面量
%b	二进制整数值，基数为 2，或者是一个科学记数法表示的指数为 2 的浮点数
%c	字符型
%d	十进制数值，基数为 10
%e	科学记数法 e 表示的浮点或者复数
%E	科学记数法 E 表示的浮点或者附属
%f	标准计数法表示的浮点或者附属
%o	8 进制度
%p	十六进制表示的一个地址值
%s	字符串
%T	输出值的类型
```

Go 允许使用类型别名
```go
type bigint int64	            // 支持使用括号，同时起多个别名
var a bigint
```

Go 多数据分组书写
```go
const(
    i = 100
    pi = 3.1415
    prefix = "Go_"
)

var(
    i int
    pi float32
    prefix string
)
```

### Var
Go 变量声明的三种方式：
```go
var a int		    // 声明一个整形变量，默认为0
var b = 10		    // 声明并初始化，且自动推导类型
c := 20			    // 初始化，且自动推导
```
注意：
- `:=`定义变量只能在函数内部使用，所以经常用 var 定义全局变量
- Go 对已经声明但未使用的变量会在编译阶段报错：`** not used`
- Go 中的标识符以字母或者下划线开头，大小写敏感
- Go 推荐使用驼峰命名

Go 多变量同时声明
```go
var a,b string
var a1,b1 string = "哼","哈"
var a2,b2 int = 1,2             // 类型可以直接省略
c,d := 1,2
var(
	e int
	f bool
)
```

Go 的默认值机制  
Go 变量初始化会自带默认值，不像其他语言为空，下面列出各种数据类型对应的 0 值：  
```go
int     0
int8    0
int32   0
int64   0
uint    0x0
rune    0           // rune的实际类型是 int32
byte    0x0         // byte的实际类型是 uint8
float32 0           // 长度为 4 byte
float64 0           // 长度为 8 byte
bool    false
string  ""
```

Go 变量值互换
```go
m,n = n,m			// 变量值互换
temp,_ = m,n		// 匿名变量：变量值互换，且丢弃变量n 
```
注意，`_` 是个特殊的变量名，任何赋予它的值都会被丢弃。该变量不占用命名空间，也不会分配内存。
```go
_, b := 34, 35      // 将值`35`赋予`b`，并同时丢弃`34`：
```

Go 变量声明，正确示例如下
```go
in, err := os.Open(file)
out, err := os.Create(file)     // 此处的 err其实是赋值
```
但是，如果在第二行赋值的变量名全部和第一行一致，则编译不通过：
```go
in, err := os.Open(file)
in, err := os.Create(file)     // 即 := 必须确保至少有一个变量是用于声明
```
注意： `:=` 只有对已经在同级词法域声明过的变量才和赋值操作语句等价，如果变量是在外部词法域声明的，那么 `:=` 将会在当前词法域重新声明一个新的变量。

Go 变量的类型转换
```go
// 只能类型显式转换
var a float32 = 1.1

// 省略 var, 简短形式，使用 := 赋值操作符
b := int(a)
// 不兼容的类型不能转换类型
```

- 关于全局变量和局部变量
> 全局变量名 以大写开头  
> 全局变量不可以省略 var ，可以使用并行的方式  
> 所有变量都可以使用类型推断  
> 局部变量不可以使用 var() 简写的形式  

### Const
常量：在编译阶段就确定下来的值，程序运行时无法改变。  

Go 常量定义的方式如下：
```go
const A = 3
const PI float32 = 3.1415
const mask = 1 << 3						// 常量与表达式
//const Home = os.GetEnv(“HOME”);		// 错误写法：常量赋值是一个编译期行为，右边的值不能出现在运行时才能得到结果的值。 

import "unsafe"
// 常量可以用len(), cap(), unsafe.Sizeof()常量计算表达式的值。
// 常量表达式中，函数必须是内置函数，否则编译不过
const (
  a = "abc"
  b = len(a)
  c = unsafe.Sizeof(a)
) 
```

Go 的关键字 iota  
iota 是特殊常量，可以认为是一个可以被编译器修改的常量。关键字 iota 声明初始值为 0，每行递增 1  
```go
const (
	a = iota    	        // 0
    b =	iota 		        // 1        
    c = iota 		        // 2
)

const (
	d = iota    	        // 0
    e 				        // 1        
    f 				        // 2
)

// 如果 iota 在同一行，则值都一样
const (
	g = iota    	        //0
    h,i,j = iota,iota,iota 	       
    k 				        // 2
)

const (
	// 第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1；
	// 所以 a=0, b=1, c=2 可以简写为如下形式：
	a = iota // 0
	b        // 1
	c        // 2
	d = "ha" // 独立值"ha"，iota += 1
	e        // 独立值"ha"，iota += 1
	f = 100  // 独立值 100，iota += 1
	g        // 独立值 100，iota += 1
	h = iota // 7, 恢复计数
	i        // 8
)

const (
	i1 = -1
	j1 = iota // 1，由于 iota 在 const 的第二行出现，因此是 1
	k1
)
```
为什么设计 iota？  
在 Go 中有一种情况，我们并不关心值，而只是设计一个常量，能够区分与别的常量不同就可以了。这其实就像是在别的语言中使用枚举类，但是在 Go 中没有枚举类，于是可以用 iota 创造出一组值不同的常量。比如 for 循环、excel 表格里，用自增整数可以作出那些千变万化的计算公式。  
```go
// 实现枚举例子
type State int

// iota 初始化后会自动递增
const (
    Running State = iota    // value --> 0
    Stopped                 // value --> 1
    Rebooting               // value --> 2
    Terminated              // value --> 3
)

func (this State) String() string {
    switch this {
    case Running:
        return "Running"
    case Stopped:
        return "Stopped"
    default:
        return "Unknown"
    }
}

func main() {
    state := Stopped
    fmt.Println("state", state) // state Stopped，没有重载 String 函数的情况下则输出 state 1
}
```

根据不同的表达式，可以很快创建出很多不同的、有规律的的常量组。
```go
// 偶数常量组
const (
    even1    = (iota+1)*2  //2
    even2                  //4
    even3                  //6
    even4                  //8
    even5                  //10
)
```

