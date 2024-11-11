---
title: Golang-start
date: 2022-07-21 22:08:11
categories: [Golang]
tags: Golang
---

# Golang用法

参考：

<!-- > https://learnku.com/docs/the-little-go-book
> https://geekr.dev -->
> http://books.studygolang.com/ 开源图书、资料、文档
> https://studygolang.com 中文标准库
> http://books.studygolang.com/The-Golang-Standard-Library-by-Example/
> https://pkg.go.dev 英文包库

## 运行命令

### run

编译并直接运行：

```go
go run main.go
```

### build

编译代码：

```go
go build main.go
```

## 常量

定义格式：

- const identifier [type] = value

### iota

一个特殊的常量定义时为0，当在const内出现时，const每新增一行将使iota计算一次：

```go
func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}

// 结果
0 1 2 ha ha 100 100 7 8
```

## 变量

### 声明

声明新变量可使用``var``或``:=``：

```go
var power int = 9000
power := 9000
```

``:=``会自动推断类型，但是只能用于局部变量，不能用于全局变量。

当声明并未初始化时为默认值，整形通常为``0``

### 数据类型

| 类型       | 说明                                                           |
| ---------- | -------------------------------------------------------------- |
| uint8      | 无符号 8 位整型 (0 到 255)                                     |
| uint16     | 无符号 16 位整型 (0 到 65535)                                  |
| uint32     | 无符号 32 位整型 (0 到 4294967295)                             |
| uint64     | 无符号 64 位整型 (0 到 18446744073709551615)                   |
| int8       | 有符号 8 位整型 (-128 到 127)                                  |
| int16      | 有符号 16 位整型 (-32768 到 32767)                             |
| int32      | 有符号 32 位整型 (-2147483648 到 2147483647)                   |
| int64      | 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |
| float32    | IEEE-754 32位浮点型数                                          |
| float64    | IEEE-754 64位浮点型数                                          |
| complex64  | 32 位实数和虚数                                                |
| complex128 | 64 位实数和虚数                                                |
| byte       | 类似 uint8                                                     |
| rune       | 类似 int32                                                     |
| uint       | 32 或 64 位                                                    |
| int        | 与 uint 一样大小                                               |
| uintptr    | 无符号整型，用于存放一个指针                                   |

格式化：

| 符号 | 介绍                                                                 |
| ---- | -------------------------------------------------------------------- |
| %v   | 值的默认格式表示。当输出结构体时，扩展标志（%+v）会添加字段名        |
| %+v  | 在 %v 基础上，对结构体字段名和值进行展开                             |
| %#v  | 值的Go语法表示                                                       |
| %T   | 值的类型的Go语法表示                                                 |
| %%   | 输出%符号                                                            |
| %t   | 布尔值                                                               |
| %b   | 二进制                                                               |
| %o   | 八进制                                                               |
| %d   | 十进制                                                               |
| %x   | 十六进制                                                             |
| %X   | 十六进制（字母使用大写）                                             |
| %p   | 指针（表示为十六进制）                                               |
| %f   | 浮点数（%9f 宽度9，%.2f 精度2，%9.2f 宽度9精度2）                    |
| %c   | unicode码值                                                          |
| %q   | 该值对应的单引号括起来的go语法字符字面值，必要时会采用安全的转义表示 |
| %U   | 表示为Unicode格式：U+1234，等价于"U+%04X"                            |
| %b   | 无小数部分、二进制指数的科学计数法，如-123456p-78                    |
| %G   | 根据实际情况采用%E或%F格式（以获得更简洁、准确的输出）               |
| %s   | 直接输出字符串或者[]byte                                             |
| %x   | 每个字节用两字符十六进制数表示（使用a-f）                            |
| %X   | 每个字节用两字符十六进制数表示（使用A-F                              |

### 初始化

```go
var power int
```

使用``:=``声明变量时，相同变量不能被声明两次，如果你尝试这么做，将得到一份错误：

```go
func main() {
  power := 9000
  fmt.Printf("It's over %d\n", power)

  // 编译器错误：
  //  := 左侧不是新的变量
  power := 9001
  fmt.Printf("It's also over %d\n", power)
}
```

正确做法是使用``=``重新赋值，但是声明包含新变量时，这样是可行的：

```go
func main() {
  name, power := "Goku", 9000
  fmt.Printf("%s's power is over %d\n", name, power)
}
```

使用``:=``多个变量赋值：

```go
name, power := "Goku", 9000
```

注意：变量被声明后必须使用，否则将会得到一份错误。

### 类型转换

显式转换：

```go
stra := "the spice must flow"
byts := []byte(stra)
strb := string(byts)
```

断言转换：

```go
m uint := 1
i, ok := m.(int)
// 第一个是对应类型的值，第二个判断类型是否正确
```

断言switch转换：

```go
var a interface{} =10
switch a.(type){
case int:
        fmt.Println("int")
case float32:
        fmt.Println("string")
}
```

## 结构体

Golang中不存在对象，通常使用结构体的``组合``来实现继承、多态，返回构造体实例实现构造函数。

### 声明

```go
type Saiyan struct {
  Name string
  Power int
}
```

声明不含``,``逗号。

### 初始化

```go
goku := Saiyan{
  Name: "Goku",
  Power: 9000,
}
```

结尾``,``是必须的。 \
初始化可以选择一部份：

```go
goku := Saiyan{}

// or

goku := Saiyan{Name: "Goku"}
goku.Power = 9000
```
未初始化的值为默认值。

字段可以是任何类型：

```go
type Saiyan struct {
    Name string
    Power int
    Father *Saiyan
}
gohan := &Saiyan{
    Name: "Gohan",
    Power: 1000,
    Father: &Saiyan {
        Name: "Goku",
        Power: 9001,
        Father: nil,
    },
}
```

### 关联指针

Golang传递参数总是会使用参数的副本，传递指针以保证数据内存一致。

```go
func main() {
  goku := &Saiyan{"Power", 9000}
  Super(goku)
  fmt.Println(goku.Power)
}

func Super(s *Saiyan) {
  s.Power += 10000
}
```

### 结构体上的函数

我们把一个函数绑定到一个结构体上，我们可以把结构体看成“对象”，函数为“方法”：

```go
type Saiyan struct {
  Name string
  Power int
}

func (s *Saiyan) Super() {
  s.Power += 10000
}
```

通过初始化调用“方法”：

```go
goku := &Saiyan{"Goku", 9001}
goku.Super()
fmt.Println(goku.Power) // 将会打印出 19001
```

### 构造函数

结构体没有构造器，可以创建返回期望的实例实现：

```go
func NewSaiyan(name string, power int) *Saiyan {
  return &Saiyan{
    Name: name,
    Power: power,
  }
}
```

### New

``new``函数用于给结构体分配内存，``new(x)``结果与``&x{}``效果相同：

```go
goku := new(Saiyan)
// same as
goku := &Saiyan{}
```

通常选择后种，因为更具有可读性：

```go
goku := new(Saiyan)
goku.name = "goku"
goku.power = 9001

//vs

goku := &Saiyan {
  Name: "goku",
  Power: 9000,
}
```

### 组合

``Saiyan``继承``Person``，并且可以直接使用：

```go
type Person struct {
  Name string
}

func (p *Person) Introduce() {
  fmt.Printf("Hi, I'm %s\n", p.Name)
}

type Saiyan struct {
  *Person
  Power int
}

// 使用它
goku := &Saiyan{
  Person: &Person{"Goku"},
  Power: 9001,
}
goku.Introduce()

// 隐式访问
goku := &Saiyan{
  Person: &Person{"Goku"},
}
fmt.Println(goku.Name)
fmt.Println(goku.Person.Name)
```

## 函数

### 可变参数

可变参数需要放在最又边使用，普通参数放左边：

```go
func main() {
   //multiParam 可以接受可变数量的参数
   multiParam("jerry", 1)
   multiParam("php", 1, 2)
}
func multiParam(name string, args ...int) {
   fmt.Println(name)
   //接受的参数放在args数组中
   for _, e := range args {
      fmt.Println(e)
   }
}
```


## 数组

### 创建

```go
var scores [10]int
scores[0] = 339

// 或

scores := [4]int{9001, 9333, 212, 33}
```

我们可以使用``len``函数来获取数组长度，``range``函数来迭代遍历：

```go
for index, value := range scores {

}
```

通常使用功能更强的切片来替代。

## 切片

### 创建

使用``make()``函数创建，创建一个长度为0，容量为10的切片：

```go
scores := make([]int, 0, 10)
```

基于数组创建切片：

```go
all := months[:]          //  将所有元素切片
firsthalf := months[:6]   //  将前6个元素切片
secondhalf := months[6:]  //  从第6个元素开始跟后续元素切片
```

### 动态操作

使用``len()``函数获取长度，``cap()``函数获取容量：

```go
fmt.Println(len(newSlice))
fmt.Println(cap(newSlice))
```

使用``append()``会自动扩展切片，增加超过容量后会返回切片副本：

```go
func main() {
  scores := make([]int, 0, 10)
  scores = append(scores, 5)
  fmt.Println(scores) // prints [5]
}
```

使用``copy()``函数复制切片：

```go
slice1 := []int{1, 2, 3, 4, 5} 
slice2 := []int{5, 4, 3}

// 复制 slice1 到 slice 2
copy(slice2, slice1) // 只会复制 slice1 的前3个元素到 slice2 中
// slice2 结果: [1, 2, 3]
// 复制 slice2 到 slice 1
copy(slice1, slice2) // 只会复制 slice2 的 3 个元素到 slice1 的前 3 个位置
// slice1 结果：[5, 4, 3, 4, 5]
```

切片删除元素：

```go
slice3 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
slice3 = slice3[:len(slice3) - 5]  // 删除 slice3 尾部 5 个元素
slice3 = slice3[5:]  // 删除 slice3 头部 5 个元素

slice3 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    
slice4 := append(slice3[:0], slice3[3:]...)  // 删除开头三个元素
slice5 := append(slice3[:1], slice3[4:]...)  // 删除中间三个元素
slice6 := append(slice3[:0], slice3[:7]...)  // 删除最后三个元素
slice7 := slice3[:copy(slice3, slice3[3:])]  // 删除开头前三个元素
```

更多操作：https://ueokande.github.io/go-slice-tricks/

## 指针

### 创建

创建并初始化一个指针，通过``&``获取变量所在的内存地址，再使用``*``获取指针所指向内存地址的值，初始化未指定值时为``nil``：

```go
a := 100
var ptr *int       // 声明指针类型
ptr = &a          // 初始化指针类型值为变量 a 
fmt.Println(ptr)  //  0xc0000a2000
fmt.Println(*ptr) //  100

a := 100
var ptr *int  //  声明指针类型
ptr = &a      // 初始化指针类型值为变量 a 

ptr := &a     //  使用 := 初始化
```

通过指针传值，使用指针有效节省内存空间：

```go
func swap(a, b *int)  {
    *a, *b = *b, *a
    fmt.Println(*a, *b)
}

func main() {
    a := 1
    b := 2
    swap(&a, &b)
    fmt.Println(a, b)
}
```

## map字典

### 声明

``string``为键的类型，``int``为值的类型：

```go
var testMap map[string]int
```
### 初始化

初始化时与数组、切片一样使用``make()``函数分配内存，通过第二个参数指定初始容量：

```go
testMap = make(map[string]int, 100)
```

### 元素操作

使用``ok``判断元素：

```go
value, ok := testMap["one"] 
if ok { // 找到了
  // 处理找到的value 
}
```

使用``delete()``删除元素：

```go
delete(testMap, "four")
```

这个调用不会出现副作用。

使用``range``遍历：

```go
testMap := map[string]int{
    "one": 1,
    "two": 2,
    "three": 3,
}

for key, value := range testMap {
    fmt.Println(key, value)
}
```

## 类型断言

使用``.(type)``来进行接口和结构体断言。

接口断言：

```go
var num1 Number = 1;
var num2 Number2 = &num1;
if num3, ok := num2.(Number1); ok {
    fmt.Println(num3.Equal(1))
}
```

结构体断言：

```go
var animal = NewAnimal("中华田园犬")
var pet = NewPet("泰迪")
var ianimal IAnimal = NewDog(&animal, pet)
if dog, ok := ianimal.(Dog); ok {
    fmt.Println(dog.GetName())
    fmt.Println(dog.Call())
    fmt.Println(dog.FavorFood())
}
```

基于反射在运行时动态断言：

```go
func myPrintf(args ...interface{}) {
    for _, arg := range args {
        switch reflect.TypeOf(arg).Kind() {
        case reflect.Int:
            fmt.Println(arg, "is an int value.")
        case reflect.String:
            fmt.Printf("\"%s\" is a string value.\n", arg)
        case reflect.Array:
            fmt.Println(arg, "is an array type.")
        default:
            fmt.Println(arg, "is an unknown type.")
        }
    }
}
```

## 流程控制

### 条件语句

包含``if``、``else``、``else if``、``switch``、``case``、``select``：

```go
score := 100
if score > 90 {
    fmt.Println("Grade: A")
} else if score > 80 {
    fmt.Println("Grade: B")
} else if score > 70 {
    fmt.Println("Grade: C")
} else if score > 60 {
    fmt.Println("Grade: D")
} else {
    fmt.Println("Grade: F")
}

score := 100
switch {
case score >= 90:
    fmt.Println("Grade: A")
case score >= 80 && score < 90:
    fmt.Println("Grade: B")
case score >= 70 && score < 80:
    fmt.Println("Grade: C")
case score >= 60 && score < 70:
    fmt.Println("Grade: D")
default:
    fmt.Println("Grade: F")
}
```

### 循环语句

包含``for``、``range``：

```go
a := []int{1, 2, 3, 4, 5, 6} 
for i, j := 0, len(a) – 1; i < j; i, j = i + 1, j – 1 { 
    a[i], a[j] = a[j], a[i] 
}
fmt.Println(a)

// 第一个值为key，第二个值为value
for _, k := range a {
    fmt.Println(k)
}
```

``break``跳出循环，``continue``跳出当前循环继续下一次循环。

### 跳转语句

包含``goto``：

```go
arr := [][]int{{1,2,3},{4,5,6},{7,8,9}}

for i := 0; i < 3; i++ {
    for j := 0; j < 3; j++ {
        num := arr[i][j]
        if j > 1 {
            goto EXIT
        }
        fmt.Println(num)
    }
}   

EXIT:
fmt.Println("Exit.")
```

不推荐使用，容易造成代码混乱。

## 包管理

使用``package``声明包，使用``import``导入包：

```go
package db

import (
  "shopping/models"
)

func LoadItem(id int) *models.Item {
  return &models.Item{
    Price: 9.001,
  }
}
```

### 可见性

用``首字母大写``使外部可以访问，小写不可访问：

```go
func NewItem() *Item {
  // ...
}
```

所有可调用的函数或变量或结构体都遵循这一项。

## 接口

Golang使用结构体来定义接口：

```go
type Logger interface {
  Log(message string)
}
```

> 一个类型是否满足一个接口是自动确定的。

可以细化接口并组合使用：

```go
type Server struct {
  logger Logger
}
```

### 空接口

空接口用``interface{}``表示，用来表达任意类型数据：

```go
var v1 interface{} = 1 // 将 int 类型赋值给 interface{} 
var v2 interface{} = "学院君" // 将 string 类型赋值给 interface{} 
var v3 interface{} = true  // 将 bool 类型赋值给 interface{}

var v4 interface{} = &v2 // 将指针类型赋值给 interface{} 
var v5 interface{} = []int{1, 2, 3}  // 将切片类型赋值给 interface{} 
var v6 interface{} = struct{   // 将结构体类型赋值给 interface{}
    id int
    name string
}{1, "学院君"} 

func Printf(fmt string, args ...interface{}) 
func Println(args ...interface{}) ...
func (p *pp) printArg(arg interface{}, verb rune)
```

## 反射

Golang提供``reflect ``api包来支持反射。
链接：https://studygolang.com/pkgdoc

## 泛型

参考：https://segmentfault.com/a/1190000041634906

定义``T``占位符为形参，``int|float32``约束类型，``[]T``返回类型实参：

```go
type Slice[T int|float32|float64 ] []T
```

使用时必须传入实参进行实例化：

```go
// 这里传入了类型实参int，泛型类型Slice[T]被实例化为具体的类型 Slice[int]
var a Slice[int] = []int{1, 2, 3}  
fmt.Printf("Type Name: %T",a)  //输出：Type Name: Slice[int]

// 传入类型实参float32, 将泛型类型Slice[T]实例化为具体的类型 Slice[string]
var b Slice[float32] = []float32{1.0, 2.0, 3.0} 
fmt.Printf("Type Name: %T",b)  //输出：Type Name: Slice[float32]

// ✗ 错误。因为变量a的类型为Slice[int]，b的类型为Slice[float32]，两者类型不同
a = b  

// ✗ 错误。string不在类型约束 int|float32|float64 中，不能用来实例化泛型类型
var c Slice[string] = []string{"Hello", "World"} 

// ✗ 错误。Slice[T]是泛型类型，不可直接使用必须实例化为具体的类型
var x Slice[T] = []int{1, 2, 3} 
```

```

## defer资源回收

在进行io操作时使用兜底避免程序中断，在函数结束时执行如有多个defer将按倒序依次执行：

```go
package main

import (
  "fmt"
  "os"
)

func main() {
  file, err := os.Open("a_file_to_read")
  defer file.Close()
  if err != nil {
    fmt.Println(err)
    return
  }
  // 读取文件
}
```

## 错误处理


使用``panic``显式抛出错误，使用``recover``捕获错误避免程序崩溃：

```go
//  panic
func main() {
    defer func() {
        fmt.Println("代码清理逻辑")
    }()

    var i = 1
    var j = 0
    if j == 0 {
        panic("除数不能为0！")
    }
    k := i / j
    fmt.Printf("%d / %d = %d\n", i, j, k)
}

//recover
func divide() {
    defer func() {
        if err := recover(); err != nil {
            fmt.Printf("Runtime panic caught: %v\n", err)
        }
    }()
    var i = 1
    var j = 0
    k := i / j
    fmt.Printf("%d / %d = %d\n", i, j, k)
}
func main() {
    divide()
    fmt.Println("divide 方法调用完毕，回到 main 函数")
}
```

一般函数返回第二个值为错误消息：

```go
n, err := Foo(0)

if err != nil { 
    // 错误处理 
}
```

### 自定义错误

使用``New()``函数：

```go
type error interface {
  Error() string
}

import (
  "errors"
)

func process(count int) error {
  if count < 1 {
    return errors.New("Invalid count")
  }
  ...
  return nil
}
```

使用``fmt.Error()``抛出错误：

```go
func test() error {
  return fmt.Error("抛出错误: %w", err)
}
```

## 并发

### 协程

使用``go``关键字创建协程，主线程结束时协程也将被终止：

```go
func main() {
  fmt.Println("start")
  go process()
  time.Sleep(time.Millisecond * 10) // 延迟等待协程运行
  fmt.Println("done")
}

func process() {
  fmt.Println("processing")
}
```

### 通道

使用关键字``chan``创建通道类型，使用``make()``函数初始化，第二个参数为通道缓冲：

```go
c := make(chan int, 10)
```

创建单向通道：

```go
var ch1 chan int       // ch1是一个正常的channel，是双向的
var ch2 chan<- float64 // ch2是单向channel，只用于写float64数据
var ch3 <-chan int     // ch3是单向channel，只用于读int数据
```

使用``len()``函数获取缓存中剩余元素，``cap()``函数获取缓存容量大小：

```go
func main() {
    c := make(chan int, 3) //带缓冲的通道

    //内置函数 len 返回未被读取的缓冲元素数量， cap 返回缓冲区大小
    fmt.Printf("len(c)=%d, cap(c)=%d\n", len(c), cap(c))

    go func() {
        defer fmt.Println("子go程结束")

        for i := 0; i < 3; i++ {
            c <- i
            fmt.Printf("子go程正在运行[%d]: len(c)=%d, cap(c)=%d\n", i, len(c), cap(c))
        }
    }()

    time.Sleep(2 * time.Second) //延时2s
    for i := 0; i < 3; i++ {
        num := <-c //从c中接收数据，并赋值给num
        fmt.Println("num = ", num)
    }
    fmt.Println("main进程结束")
}
```

使用``close()``函数手动关闭通道：

```go
func main() {
    c := make(chan int)

    go func() {
        for i := 0; i < 5; i++ {
            c <- i
        }
        close(c)
    }()

    for {
        //ok为true说明channel没有关闭，为false说明管道已经关闭
        if data, ok := <-c; ok {
            fmt.Println(data)
        } else {
            break
        }
    }

    fmt.Println("Finished")
}

```

使用``select``关键字设置超时避免阻塞，当没有通道可用会执行``default``，如果没有通道将会一直阻塞直到通道可用：

```go
select {
    case <- chan1:
        // 如果chan1成功读到数据，则进行该case处理语句
    case chan2 <- 1:
        // 如果成功向chan2写入数据，则进行该case处理语句
    default:
        // 如果上面都没有成功，则进入default处理流程
    }

func main() {
    c := make(chan int)
    out := make(chan bool)
    go func() {
        for {
            select {
            case v := <-c:
                fmt.Println(v)
            case <-time.After(5 * time.Second):
                fmt.Println("timeout")
                out <- true
                return
            }
        }
    }()
    //c <- 666 // 注释掉，引发 timeout
    <-out
}
```

### 锁

参考：https://learnku.com/docs/bettercoding/1.0/locks-and-condition-variables/6832

Golang提供``sync``api包操作锁，高级操作用通道替代更好，链接：https://studygolang.com/pkgdoc

## 设计模型

参考：

> https://www.topgoer.cn/docs/golang-design-pattern/golang-design-pattern-1cbgha2ltg796 地鼠文档 \
> https://github.com/senghoo/golang-design-pattern Github \
> https://www.runoob.com/design-pattern/design-pattern-tutorial.html 菜鸟教程