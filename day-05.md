
### String
#### 1.0 字符型

Golang 中没有专门的字符类型，如果要存储单个字符(字母)，一般使用 byte 来保存，且使用单引号包裹。  
字符类型可以以 d% 打印为整型。  
```go
var c1 byte = 'a'
var c2 byte = '0'

fmt.Println("c1=", c1)					//输出 97
fmt.Println("c2=", c2)					//输出48
fmt.Printf("c1=%c,c2=%c\n", c1, c2)	    //输出原值 a 0

//var c3 byte = '北'
//fmt.Printf("c3=%c", c3)					//溢出错误
```

说明：
- 如果我们保存的字符在 ASCII 表的,比如 [0-1, a-z,A-Z..] 直接可以保存到 byte
- 如果我们保存的字符对应码值大于 255,这时我们可以考虑使用 int 类型保存
- 如果我们需要安装字符的方式输出，这时我们需要格式化输出，即 fmt.Printf(“%c”, c1)
- 字符可以和整型进行运算


#### 1.1 基本使用

字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。也就是说对于传统的字符串是由字符组成的，而 Go 的字符串不同，它是由字节组成的。  

字符串在 Go 语言中是基本类型，内容在初始化后不能修改。Go 中的字符串都是采用 UTF-8 字符集编码，使用一对双引号（""）或反引号` `定义。  

使用 ` ` 可以额外解析换行，即其没有字符转义功能。  

```go
str := "Hello "
str2 := " World!"
// str[0] = 'c'         //编译报错： cannot assign to 因为字符串不可变
fmt.Println(str[0])     //输出字符串第一个字符 72
fmt.Println(len(str))   //输出长度 6
fmt.Println(str + str2) //输出不带空格的
```

#### 1.2 字符串修改

Go 中的字符串不可改变，有两种修改办法：

办法一：通过转换为 []byte 类型，构造一个临时字符串
```go
str := "hello"
strTemp := []byte(str)
strTemp[0] = 'c';
strResult := string(strTemp) // cello
```

办法二：字符串可以进行切片操作
```go
str := "hello"
str = "c"+ str[1:]		// 1: 表示从第1位开始到最后
```

Go 和 Java 等语言一样，字符串默认是不可变的，这样保证了线程安全，大家使用的都是只读对象，无须加锁，且能很方便的共享内存，不必使用写时复制。

#### 1.3 字符串遍历

遍历方式一：使用字节数组，注意每个中文在 UTF-8 中占据 3 个字节
```go
str := "hello"
for i := 0; i < len(str); i++ {
	fmt.Println(i,str[i])
}
```

遍历方式二：range 关键字只是第一种遍历方式的简写
```go
str := "你好"
for i,ch := range str {
	fmt.Println(i,ch)
}
```

注意：Unicode 字符遍历需要使用 range，原因见 len() 函数部分讲解。

#### 1.4 字符串转换

字符串转化的函数在 strconv 中，如下也只是列出一些常用的：

- Append 系列函数将整数等转换为字符串后，添加到现有的字节数组中。

```go

package main

import (
	"fmt"
	"strconv"
)

func main() {
	str := make([]byte, 0, 100)
	str = strconv.AppendInt(str, 4567, 10)
	str = strconv.AppendBool(str, false)
	str = strconv.AppendQuote(str, "abcdefg")
	str = strconv.AppendQuoteRune(str, '单')
	fmt.Println(string(str)) // 4567false"abcdefg"'单'
}
```

- Format 系列函数把其他类型的转换为字符串
```go

package main

import (
	"fmt"
	"strconv"
)

func main() {
	a := strconv.FormatBool(false)
	b := strconv.FormatFloat(123.23, 'g', 12, 64)
	c := strconv.FormatInt(1234, 10)
	d := strconv.FormatUint(12345, 10)
	e := strconv.Itoa(1023)
	fmt.Println(a, b, c, d, e) // false 123.23 1234 12345 1023
}

```

- Parse 系列函数把字符串转换为其他类型

```go

package main

import (
	"fmt"
	"strconv"
)

func checkError(e error) {
	if e != nil {
		fmt.Println(e)
	}
}

func main() {
	a, err := strconv.ParseBool("false")
	checkError(err)
	b, err := strconv.ParseFloat("123.23", 64)
	checkError(err)
	c, err := strconv.ParseInt("1234", 10, 64)
	checkError(err)
	d, err := strconv.ParseUint("12345", 10, 64)
	checkError(err)
	e, err := strconv.Atoi("1023")
	checkError(err)
	fmt.Println(a, b, c, d, e) // false 123.23 1234 12345 1023
}

```

#### 1.5 len() 函数

len() 函数是 go 语言的内置函数，可以用来获取切片、字符串、通道等的长度。

```go

package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	str1 := "hello world"
	str2 := "你好，"

	fmt.Println(len(str1))							// 11
	fmt.Println(len(str2))							// 9
	fmt.Println(utf8.RuneCountInString(str2))		// 3
}
```

第一个函数输出 11 很容易理解，第二个函数却输出了 9，理论上我们会认为应该是 3 才对。这时因为 Go 的字符串都是以 UTF-8 格式保存，每个中文占据 3 个字节。Go 中计算 UTF-8 字符串格式的长度应该使用 `utf8.RuneCountInString`。

#### 1.6 字符串索引
```go
package main

import (
	"fmt"
	"strings"
)

func main() {

	str := "hello,world!"

	index := strings.Index(str, "w")

	fmt.Println(index)					// 6
	fmt.Println(str[index])				// 119 ASCII中的值
	fmt.Println(str[index:])			// world!

}
```

- strings.Index: 正向搜索字符串
- strings.LastIndex: 反向搜索字符串

#### 1.7 字符串连接

使用 `+` 能够连接字符串。但是该操作并不高效。Go 中也拥有类似 Java 的 StringBuilder 机制来进行高效字符串连接：
```go
	str1 := "hello"
	str2 := " world"

	//创建字节缓冲
	var stringBuilder bytes.Buffer

	//把字符串写入缓冲
	stringBuilder.WriteString(str1)
	stringBuilder.WriteString(str2)

	//将缓冲以字符串形式输出
	fmt.Println(stringBuilder.String()) // hello world
```

注意：bytes.Buffer可以写入各种字节数组，字符串也是一种字节数组。

#### 1.8 其他操作

```go
//查找s在字符串str中的索引
Index(str, s string) int    示例：strings.Index(str, ",")

//判断str是否包含s
Contains(str, s string) bool

//通过字符串str连接切片 s
Join(s []string, str string) string

//替换字符串str中old字符串为new字符串，n表示替换的次数，小于0全部替换
Replace(str,old,new string,n int) string

//字符串str按照s分割，返回切片
Splite(str,s string)[]string
//Append系列函数将整数等转换为字符串后，添加到现有的字节数组中
//Format系列函数可以把其他类型转换为字符串
//Trim函数可以去除头部、尾部指定的字符串
//Fields函数可以去除空格，返回切片
```

### Array
#### 2.1 数组的声明

数组是一段固定长度的连续内存区域。数组的长度定义后不可更改，长度使用 len() 获取。

```go
var arr1 [10]int					//定义长度为10的整型数组，很少这样使用
arr2 [5]int := [5]int{1,2,3,4,5}	//定义并初始化
arr3 := [5]int{1,2,3,4,5}			//自动推导并初始化
arr4 := [5]int{1,2}					//指定总长度，前几位被初始化，没有的使用零值
arr5 := [5]int{2:10, 4:11}			//有选择的初始化，没被初始化的使用零值
arr6 := [...]int{2,3,4}				//自动计算长度
```

#### 2.2 数组常用操作

```go
arr[:]      代表所有元素
arr[:5]     代表前五个元素
arr{5:}     代表从第5个开始，不包含第5个
len(arr)    数组的长度
```

#### 2.3 数组的遍历

方式一：for 循环遍历
```go
arr := [3]int{1,2,3}

for i := 0; i < len(arr); i++ {
	fmt.Println(arr[i])
}
```
方式二：for-range 遍历
```go
arr := [3]int{1,2,3}

for i, v := range arr {
	fmt.Println(i)	//元素位置	
	fmt.Println(v)	//元素值
}
```

#### 2.4 数组使用注意事项
数组创建完长度就固定，不可以再追加元素；  
长度是数组类型的一部分，因此 [3]int 与 [4]int 是不同的类型；  
数组之间的赋值是值的赋值，即当把一个数组作为参数传入函数的时候，传入的其实是该函数的副本，而不是他的指针。
