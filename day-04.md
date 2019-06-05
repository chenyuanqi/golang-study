
### Operator
<details>
<summary>算术运算符</summary>

```go
package main
import "fmt"
func main() {
  var a int = 21
  var b int = 10
  var c int
  c = a + b
  fmt.Printf("第一行 - c 的值为 %d\n", c ) // 第一行 - c 的值为 31
  c = a - b
  fmt.Printf("第二行 - c 的值为 %d\n", c ) // 第二行 - c 的值为 11
  c = a * b
  fmt.Printf("第三行 - c 的值为 %d\n", c ) // 第三行 - c 的值为 210
  c = a / b
  fmt.Printf("第四行 - c 的值为 %d\n", c ) // 第四行 - c 的值为 2
  c = a % b
  fmt.Printf("第五行 - c 的值为 %d\n", c ) // 第五行 - c 的值为 1
  a++
  fmt.Printf("第六行 - c 的值为 %d\n", a ) // 第六行 - c 的值为 22
  a--
  fmt.Printf("第七行 - c 的值为 %d\n", a ) // 第七行 - c 的值为 21
}
```

下表列出了所有Go语言的算术运算符。假定 A 值为 10，B 值为 20。

| 运算符 | 描述 | 实例 |
| ---- | ---- | ---- |
| + | 相加  | A + B 输出结果 30 |
| - | 相减  | A - B 输出结果 -10 |
| * | 相乘  | A * B 输出结果 200 |
| / | 相除  | B / A 输出结果 2 |
| % | 求余  | B % A 输出结果 0 |
| ++ | 自增 | A++ 输出结果 11 |
| -- | 自减 | A-- 输出结果 9 |

注意：Go 中只有后-- 和后++，且自增自减不能用于表达式中，只能独立使用。  

</details>

<details>
<summary>关系运算符</summary>

```go
package main
import "fmt"
func main() {
   var a int = 21
   var b int = 10
   if( a == b ) {
      fmt.Printf("第一行 - a 等于 b\n" )
   } else {
      fmt.Printf("第一行 - a 不等于 b\n" )
   }
   if ( a < b ) {
      fmt.Printf("第二行 - a 小于 b\n" )
   } else {
      fmt.Printf("第二行 - a 不小于 b\n" )
   } 
   
   if ( a > b ) {
      fmt.Printf("第三行 - a 大于 b\n" )
   } else {
      fmt.Printf("第三行 - a 不大于 b\n" )
   }
   /* 让我们改变a和b的值 */
   a = 5
   b = 20
   if ( a <= b ) {
      fmt.Printf("第四行 - a 小于等于 b\n" )
   }
   if ( b >= a ) {
      fmt.Printf("第五行 - b 大于等于 a\n" )
   }
}
```

下表列出了所有Go语言的关系运算符。假定 A 值为 10，B 值为 20。

| 运算符 | 描述 | 实例 |
| ---- | ---- | ---- |
| ==  | 检查两个值是否相等，如果相等返回 True 否则返回 False。 |  (A == B) 为 False |
| !=  | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。 |  (A != B) 为 True |
| > | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。 |  (A > B) 为 False |
| < | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。 |  (A < B) 为 True |
| >=  | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 |  (A >= B) 为 False |
| <=  | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 | A <= B) 为 True |

</details>

<details>
<summary>逻辑运算符</summary>

```go
package main
import "fmt"
func main() {
  var a bool = true
  var b bool = false
  if ( a && b ) {
    fmt.Printf("第一行 - 条件为 true\n" )
  }
  if ( a || b ) {
    fmt.Printf("第二行 - 条件为 true\n" )
  }
  /* 修改 a 和 b 的值 */
  a = false
  b = true
  if ( a && b ) {
    fmt.Printf("第三行 - 条件为 true\n" )
  } else {
    fmt.Printf("第三行 - 条件为 false\n" )
  }
  if ( !(a && b) ) {
    fmt.Printf("第四行 - 条件为 true\n" )
  }
}
```

下表列出了所有Go语言的逻辑运算符。假定 A 值为 `True`，B 值为 `False` d。

| 运算符 | 描述 | 实例 |
| ---- | ---- | ---- |
| && | 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。 | (A && B) 为 False |
| \|\| | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。|	(A || B) 为 True |
| ! | 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。 |	!(A && B) 为 True |

</details>

<details>
<summary>位运算符</summary>

```go
package main
import "fmt"
func main() {

  var a uint = 60	/* 60 = 0011 1100 */  
  var b uint = 13	/* 13 = 0000 1101 */
  var c uint = 0          

  c = a & b       /* 12 = 0000 1100 */ 
  fmt.Printf("第一行 - c 的值为 %d\n", c ) // 第一行 - c 的值为 12

  c = a | b       /* 61 = 0011 1101 */
  fmt.Printf("第二行 - c 的值为 %d\n", c )  // 第二行 - c 的值为 61

  c = a ^ b       /* 49 = 0011 0001 */
  fmt.Printf("第三行 - c 的值为 %d\n", c ) // 第三行 - c 的值为 49

  c = a << 2     /* 240 = 1111 0000 */
  fmt.Printf("第四行 - c 的值为 %d\n", c ) // 第四行 - c 的值为 240

  c = a >> 2     /* 15 = 0000 1111 */
  fmt.Printf("第五行 - c 的值为 %d\n", c )  // 第五行 - c 的值为 15
}
```

Go 语言支持的位运算符如下表所示。假定 A 为60，B 为13：

| 运算符 | 描述 | 实例 |
| ---- | ---- | ---- |
| & | 按位与运算符"&"是双目运算符。 其功能是参与运算的两数各对应的二进位相与。 |	(A & B) 结果为 12, 二进制为 0000 1100 |
| \| | 按位或运算符 \| 是双目运算符。 其功能是参与运算的两数各对应的二进位相或。 |	(A \| B) 结果为 61, 二进制为 0011 1101 |
| ^ | 按位异或运算符"^"是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。 |	(A ^ B) 结果为 49, 二进制为 0011 0001 |
| << | 左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方。 其功能把"<<"左边的运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。 | A << 2 结果为 240 ，二进制为 1111 0000 |
| >> | 右移运算符">>"是双目运算符。右移n位就是除以2的n次方。 其功能是把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数。| A >> 2 结果为 15 ，二进制为 0000 1111 |

</details>

<details>
<summary>赋值运算符</summary>

```go
package main
import "fmt"
func main() {
  var a int = 21
  var c int

  c =  a
  fmt.Printf("第 1 行 - =  运算符实例，c 值为 = %d\n", c )
  // 第 1 行 - =  运算符实例，c 值为 = 21
  c +=  a
  fmt.Printf("第 2 行 - += 运算符实例，c 值为 = %d\n", c )
  // 第 2 行 - += 运算符实例，c 值为 = 42
  c -=  a
  fmt.Printf("第 3 行 - -= 运算符实例，c 值为 = %d\n", c )
  // 第 3 行 - -= 运算符实例，c 值为 = 21
  c *=  a
  fmt.Printf("第 4 行 - *= 运算符实例，c 值为 = %d\n", c )
  // 第 4 行 - *= 运算符实例，c 值为 = 441
  c /=  a
  fmt.Printf("第 5 行 - /= 运算符实例，c 值为 = %d\n", c )
  // 第 5 行 - /= 运算符实例，c 值为 = 21
  c  = 200; 
  c <<=  2
  fmt.Printf("第 6 行  - <<= 运算符实例，c 值为 = %d\n", c )
  // 第 6 行  - <<= 运算符实例，c 值为 = 800
  c >>=  2
  fmt.Printf("第 7 行 - >>= 运算符实例，c 值为 = %d\n", c )
  // 第 7 行 - >>= 运算符实例，c 值为 = 200
  c &=  2
  fmt.Printf("第 8 行 - &= 运算符实例，c 值为 = %d\n", c )
  // 第 8 行 - &= 运算符实例，c 值为 = 0
  c ^=  2
  fmt.Printf("第 9 行 - ^= 运算符实例，c 值为 = %d\n", c )
  // 第 9 行 - ^= 运算符实例，c 值为 = 2
  c |=  2
  fmt.Printf("第 10 行 - |= 运算符实例，c 值为 = %d\n", c )
  // 第 10 行 - |= 运算符实例，c 值为 = 2
}
```

| 运算符 | 描述 | 实例 |
| ---- | ---- | ---- |
| = | 简单的赋值运算符，将一个表达式的值赋给一个左值s | C = A + B 将 A + B 表达式结果赋值给 C |
| += | 相加后再赋值s | C += A 等于 C = C + A |
| -= | 相减后再赋值s | C -= A 等于 C = C - A |
| *= | 相乘后再赋值s | C *= A 等于 C = C * A |
| /= | 相除后再赋值s | C /= A 等于 C = C / A |
| %= | 求余后再赋值s | C %= A 等于 C = C % A |
| <<= | 左移后赋值s | C <<= 2 等于 C = C << 2 |
| >>= | 右移后赋值s | C >>= 2 等于 C = C >> 2 |
| &= | 按位与后赋值s | C &= 2 等于 C = C & 2 |
| ^= | 按位异或后赋值s | C ^= 2 等于 C = C ^ 2 |
| \|= | 按位或后赋值s | C \|= 2 等于 C = C \| 2 |

</details>

<details>
<summary>其他运算符</summary>

```go
package main
import "fmt"
func main() {
  var a int = 4
  var b int32
  var c float32
  var ptr *int

  /* 运算符实例 */
  fmt.Printf("第 1 行 - a 变量类型为 = %T\n", a ); // 第 1 行 - a 变量类型为 = int
  fmt.Printf("第 2 行 - b 变量类型为 = %T\n", b ); // 第 2 行 - b 变量类型为 = int32
  fmt.Printf("第 3 行 - c 变量类型为 = %T\n", c ); // 第 3 行 - c 变量类型为 = float32

  /*  & 和 * 运算符实例 */
  ptr = &a	/* 'ptr' 包含了 'a' 变量的地址 */
  fmt.Printf("a 的值为  %d\n", a);   // a 的值为  4
  fmt.Printf("*ptr 为 %d\n", *ptr); // *ptr 为 4
}
```

| 运算符 | 描述 | 实例 |
| ---- | ---- | ---- |
| & | 返回变量存储地址 | &a; 将给出变量的实际地址。 |
| * | 指针变量。 | *a; 是一个指针变量 |

</details>

<details>
<summary>运算符优先级</summary>

```go
package main
import "fmt"
func main() {
  var a int = 20
  var b int = 10
  var c int = 15
  var d int = 5
  var e int;
  // 通过使用括号来临时提升某个表达式的整体运算优先级。
  e = (a + b) * c / d;      // ( 30 * 15 ) / 5
  fmt.Printf("(a + b) * c / d 的值为 : %d\n",  e );
  e = ((a + b) * c) / d;    // (30 * 15 ) / 5
  fmt.Printf("((a + b) * c) / d 的值为  : %d\n" ,  e );
  e = (a + b) * (c / d);   // (30) * (15/5)
  fmt.Printf("(a + b) * (c / d) 的值为  : %d\n",  e );
  e = a + (b * c) / d;     //  20 + (150/5)
  fmt.Printf("a + (b * c) / d 的值为  : %d\n" ,  e );  
}
```

有些运算符拥有较高的优先级，二元运算符的运算方向均是从左至右。下表列出了所有运算符以及它们的优先级，由上至下代表优先级由高到低：

| 优先级 | 运算符 |
| ---- | ---- |
| 7 | ^ ! |
| 6 | * / % << >> & &^ |
| 5 | + - \| ^ |
| 4 | == != < <= >= > |
| 3 | <- |
| 2 | && |
| 1 | \|\| |

</details>

### Process Control
<details>
<summary>for 循环语句</summary>

```go
package main
import "fmt"
func main() {
  sum := 0
  // 如果条件表达式的值变为 false，那么迭代将终止。
  for i := 0; i < 10; i++ {
    sum += i
  }
  fmt.Println(sum)
  
  // 循环初始化语句和后置语句都是可选的。
  // for 是 Go 的 “while”
  // 基于此可以省略分号：C 的 while 在 Go 中叫做 for 。
  // 如果省略了循环条件，循环就不会结束，因此可以用更简洁地形式表达死循环。
  sum2 := 1
  for ; sum2 < 1000; {
    sum2 += sum2
  }
  fmt.Println(sum2)
}
```
基本的 for 循环包含三个由分号分开的组成部分：

1. 初始化语句：在第一次循环执行前被执行
1. 循环条件表达式：每轮迭代开始前被求值
1. 后置语句：每轮迭代后被执行


常用的跳出循环关键字：  

1. break 用于函数内跳出当前 for、switch、select 语句的执行  
1. continue 用于跳出 for 循环的本次迭代。  
1. goto 可以退出多层循环  
```go
// break 跳出循环案例 (continue 同下)
OuterLoop:
   for i := 0; i < 2; i++ {
      for j := 0; j < 5; j++ {
         switch j {
            case 2:
               fmt.Println(i,j)
               break OuterLopp
            case 3:
               fmt.Println(i,j)
               break OuterLopp
         }
      }
   }

// goto 跳出多重循环案例
for x:=0; x<10; x++ {
 
   for y:=0; y<10; x++ {

        if y==2 {
            goto breakHere
         }
   }
   
}
breakHere:
   fmt.Println("break")

// goto 也可以用来统一错误处理
if err != nil {
   got onExit
}
onExit:
   fmt.Pritln(err)
   exitProcess()
```

</details>

<details>
<summary>if 语句</summary>

```go
package main
import (
  "fmt"
  "math"
)
func sqrt(x float64) string {
  if x < 0 {
    return sqrt(-x) + "i"
  }
  return fmt.Sprint(math.Sqrt(x))
}
func main() {
  fmt.Println(sqrt(2), sqrt(-4))
}
```

就像 for 循环一样，Go 的 if 语句也不要求用 ( ) 将条件括起来，同时， { } 还是必须有的。

**if 的便捷语句**

```go
package main
import (
  "fmt"
  "math"
)

func pow(x, n, lim float64) float64 {
  if v := math.Pow(x, n); v < lim { // 这里的 v < liml 才是真正的 if 判断表达式
    return v
  }
  return lim
}
func main() {
  fmt.Println(
    pow(3, 2, 10),
    pow(3, 3, 20),
  )
}
```

</details>

<details>
<summary>if 和 else 语句</summary>

```go
package main
import (
  "fmt"
  "math"
)
func pow(x, n, lim float64) float64 {
  if v := math.Pow(x, n); v < lim {
    return v
  } else {
    fmt.Printf("%g >= %g\n", v, lim)
  }
  // 这里开始就不能使用 v 了
  return lim
}

func main() {
  // 两个 pow 调用都在 main 调用 fmt.Println 前执行完毕了。
  fmt.Println(
    pow(3, 2, 10),
    pow(3, 3, 20),
  )
}
```

在 if 的便捷语句定义的变量同样可以在任何对应的 else 块中使用。

</details>

<details>
<summary>switch 语句</summary>

```go
package main
import (
  "fmt"
  "runtime"
)
func main() {
  fmt.Print("Go runs on ")
  switch os := runtime.GOOS; os {
    case "darwin":
      fmt.Println("OS X.")
    case "linux":
      fmt.Println("Linux.")
    default:
      // freebsd, openbsd,
      // plan9, windows...
      fmt.Printf("%s.", os)
  }
}
```

在 if 的便捷语句定义的变量同样可以在任何对应的 else 块中使用。

**switch 的执行顺序：** 条件从上到下的执行，当匹配成功的时候停止。  
Go 保留了 break，用来跳出 switch 语句，默认书写了该关键字；  
Go 也提供 fallthrough，代表不跳出 switch，后面的语句无条件执行。  

```go
package main
import (
  "fmt"
  "time"
)
func main() {
  fmt.Println("When's Saturday?")
  today := time.Now().Weekday()
  switch time.Saturday {
    case today + 0:
      fmt.Println("Today.")
    case today + 1:
      fmt.Println("Tomorrow.")
    case today + 2:
      fmt.Println("In two days.")
    default:
      fmt.Println("Too far away.")
  }
}
```

**没有条件的 switch 同 switch true 一样。**

```go
package main
import (
  "fmt"
  "time"
)
func main() {
  t := time.Now()
  switch {
    case t.Hour() < 12:
      fmt.Println("Good morning!")
    case t.Hour() < 17:
      fmt.Println("Good afternoon.")
    default:
      fmt.Println("Good evening.")
  }
}
```

</details>

<details>
<summary>defer 语句</summary>

```go
package main
import "fmt"
func main() {
  // 2. 在输出 world
  defer fmt.Println("world")
  // 1. 先输出 hello
  fmt.Println("hello")
}
```

> defer 语句会延迟函数的执行直到上层函数返回。
> 延迟调用的参数会立刻生成，但是在上层函数返回前函数都不会被调用。

**defer 栈**

延迟的函数调用被压入一个栈中。当函数返回时， 会按照后进先出的顺序调用被延迟的函数调用。

```go
package main
import "fmt"
func main() {
  fmt.Println("counting")
  for i := 0; i < 10; i++ {
    defer fmt.Println(i)
  }
  fmt.Println("done")
}
```

可以运行demo [defer](example/defer/defer.go) 查看效果。

</details>

### Struct
<details>
<summary>结构体字段</summary>

结构体字段使用点号来访问。

```go
package main
import "fmt"
type Vertex struct {
  X int
  Y int
}
func main() {
  v := Vertex{1, 2}
  v.X = 4
  fmt.Println(v.X)
}
```

</details>

<details>
<summary>结构体指针</summary>

结构体指针使用 & 来访问。

```go
package main
import "fmt"
type Vertex struct {
  X int
  Y int
}
func main() {
  v := Vertex{1, 2}
  p := &v
  p.X = 1e9
  fmt.Println(v)
}

```

</details>

<details>
<summary>结构体文法</summary>

> 结构体文法表示通过结构体字段的值作为列表来新分配一个结构体。
> 使用 Name: 语法可以仅列出部分字段。（字段名的顺序无关。）  
> 特殊的前缀 & 返回一个指向结构体的指针。

```go
package main
import "fmt"
type Vertex struct {
  X int
  Y int
}
func main() {
  v := Vertex{1, 2}
  p := &v
  p.X = 1e9
  fmt.Println(v)
}
```

</details>
