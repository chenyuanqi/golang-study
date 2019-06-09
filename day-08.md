
### 一 Struct 结构体
#### 1.0 Go 设计思想  
Go 语言通过用自定义的方式形成新的类型。  
Go 语言使用结构体和结构体成员来描述真实世界。  
Go 语言没有类的概念，也不支持类的继承等面向对象思想。  
Go 语言的结构体内嵌配合接口比面向对象具有更高的扩展性和灵活性。  
Go 语言不仅认为结构体能拥有方法，且每种自定义类型也可以拥有自己的方法。  

#### 1.1 结构体的作用
结构体可以用来声明新的类型，作为其他类型的属性/字段的容器，如下定义一个学生结构体：
```go
	type Student struct {
		name string
		age int
	}

	//按顺序初始化：每个成员都必须初始化
	var s1 Student = Student{"lisi", 20}

	//制定成员初始化：没有被初始化的，自动赋零值
	s2 := Student{age:30}
	
	//new 申请结构体
	s3 := new(Student)      // 被 new 生成的结构体实例其实是指针类型
	s3.name = "zs"          // 这里的.语法只是语法糖，将 s3.name 转换成了 (*s3).name
	s3.age = 27
	
	//直接声明
	var s4 Student
	s4.name = "ww"
	s4.age = 30
```
struct 结构中的类型可以是任意类型，且存储空间是连续的，其字段按照声明时的顺序存放。  
如果结构体的所有的成员都是可以比较的，那么结构体本身也是可以比较的，使用 == != ，不支持 > 和 <。  
注意：如果结构体的成员要被包外调用，需要大写首字母。  


#### 1.2 取结构体地址与实例化
对结构体的 new 其实是生成了一个指针类型。其实对结构体进行 `&` 取地址操作时，也可以视为对该类型进行一次 `new` 的实例化操作。  
Go 语言的 new 函数，使用它来分配类型所需要的内存。 new(X) 的结果与 &X{} 相同。
```go
ins := new(T)
// same as
ins := &T{}

# T 是结构体类型
# ins 为结构体的实例，类型为 *T，是指针类型
```

#### 1.3 匿名字段
struct 的字段名与类型一一对应，如果不提供名字，则为匿名字段。  
如果匿名字段是一个 struct 时，这个 struct 拥有的全部字段都被隐式引入了当前的struct。
```go
    type Human struct {
        name string
        age int
        weight int
    }
    
    type Student struct {
        Human // 匿名字段，那么默认Student就包含了Human的所有字段
        speciality string
    }
```

不仅仅是 struct，其他所有内置类型和自定义类型都可以作为匿名字段：
```go
    type Human struct {
        name string
        age int
        weight int
    }
    
    type Student struct {
        Human // 匿名字段，struct
        Skills // 匿名字段，自定义的类型 string slice
        int // 内置类型作为匿名字段
        speciality string
    }
    
    func main() {
        // 初始化学生 Jane
        jane := Student{Human:Human{"Jane", 35, 100}, speciality:"Biology"}
        // 现在我们来访问相应的字段
        fmt.Println("Her name is ", jane.name)
        fmt.Println("Her age is ", jane.age)
        fmt.Println("Her weight is ", jane.weight)
        fmt.Println("Her speciality is ", jane.speciality)
        // 我们来修改他的 skill 技能字段
        jane.Skills = []string{"anatomy"}
        fmt.Println("Her skills are ", jane.Skills)
        fmt.Println("She acquired two new ones ")
        jane.Skills = append(jane.Skills, "physics", "golang")
        fmt.Println("Her skills now are ", jane.Skills)
        // 修改匿名内置类型字段
        jane.int = 3
        fmt.Println("Her preferred number is", jane.int)
    }
```

这里有一个问题：如果 human 里面有一个字段叫做 phone，而 student 也有一个字段叫做 phone，那么该怎么办呢？Go 里面很简单的解决了这个问题，最外层的优先访问，也就是当你通过 student.phone 访问的时候，是访问 student 里面的字段，而不是 human 里面的字段。

#### 1.4 内嵌结构体
当前结构体可以直接访问其内嵌结构体的内部字段：
```go
package main

import "fmt"

type Animal struct {
	Age int
}

type Person struct {
	Animal
	Name string
}

type Student struct {
	Person
	ClassName string
}

func main() {

	// 初始化方式 1
	s1 := Student{
		Person{
			Animal: Animal {
				Age: 15,
			},
			Name:"lisi",
		},
		"一班",
	}
	fmt.Println(s1.Age)				// 正确输出 15
	fmt.Println(s1.Person.Name)		// 正确输出 lisi

	// 初始化方式 2
	var s2 Student
	s2.Name = "zs"
	s2.Age = 30
	s2.ClassName = "二班"
	fmt.Println(s2.Age)				// 正确输出 30
	fmt.Println(s2.Person.Name)		// 正确输出 zs
}
```

#### 1.5 结构体上的函数
我们可以把一个方法关联在一个结构体上
```go
type Saiyan struct {
  Name string
  Power int
}

// 可以这么理解，*Saiyan 类型是 Super 方法的接受者
func (s *Saiyan) Super() {
  s.Power += 10000
}

// 我们可以通过下面的代码去调用 Super 方法
goku := &Saiyan{"Goku", 9001}
goku.Super()
fmt.Println(goku.Power) // 将会打印出 19001
```

### 二 关于类型别名
#### 2.1 类型别名的使用
Go 在 1.9 版本加入了类型别名。主要用于代码升级、迁移中类型的兼容问题（C/C++ 中使用宏来解决重构升级带来的问题）。  
Go1.9 之前的版本内建类型：
```go
type byte uint8
type rune int32
```

Go1.9 之后的版本内建类型：
```go
type byte = uint8
type rune = int32
```

类型定义是定义了一个全新的类型的类型。    
类型别名只是某个类型的小名，并非创造了新的类型。    
```go
	var a1 MyInt
	fmt.Printf("a1 type: %T\n", a1)			//main.MyInt

	var a2 AliasInt
	fmt.Printf("a2 type: %T\n", a2)			// int
```

#### 2.2 不同包下的类型别名
注意：不能为不在同一个包中的类型上定义方法。
```go
package main

import (
	"time"
)

type MyDuration = time.Duration

func (m MyDuration) Test(str string) {

}

func main() {

}
```
解决方案：使用别名定义（type MyDuration time.Duration），且将定义放在time包中
