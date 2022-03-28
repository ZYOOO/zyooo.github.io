# Go_Day1


# Hello, golang!

## Hello world

```go
package main //有main函数的话这里一定要有main 

import "fmt"

//一个项目一个main函数，和c一样，与java不同
func main() {
	fmt.Println("Hello,World!")
	fmt.Println("Hello golang")
}

```



## 标识符

老规矩，不能数字开头、不能关键字、不能含运算符。



## 关键字

| break        | default         | func       | interface   | select     |
| ------------ | --------------- | ---------- | ----------- | ---------- |
| **case**     | **defer**       | **go**     | **map**     | **struct** |
| **chan**     | **else**        | **goto**   | **package** | **switch** |
| **const**    | **fallthrough** | **if**     | **range**   | **type**   |
| **continue** | **for**         | **import** | **return**  | **var**    |

除了以上介绍的这些关键字，Go 语言还有 36 个**预定义标识符**：

| append    | bool        | byte        | cap         | close      | complex  | complex64 | complex128 | uint16      |
| --------- | ----------- | ----------- | ----------- | ---------- | -------- | --------- | ---------- | ----------- |
| **copy**  | **false**   | **float32** | **float64** | **imag**   | **int**  | **int8**  | **int16**  | **uint32**  |
| **int32** | **int64**   | **iota**    | **len**     | **make**   | **new**  | **nil**   | **panic**  | **uint64**  |
| **print** | **println** | **real**    | **recover** | **string** | **true** | **uint**  | **uint8**  | **uintptr** |



## 空格

变量的声明必须使用空格隔开

```go
var age int //注意：代码中有未使用变量的话，将无法通过编译
//可用以下方法
age = age
_ = age
```



## 格式化字符串

Go中使用 fmt.Sprintf 格式化字符串并赋值给新串：

```go
package main

import (
    "fmt"
)

func main() {
   // %d 表示整型数字，%s 表示字符串
    var stockcode=123
    var enddate="2021-12-30"
    var url="Code=%d&endDate=%s"
    var target_url=fmt.Sprintf(url,stockcode,enddate)
    fmt.Println(target_url)
    //输出结果为 Code=123&endDate=2021-12-30
}
```



## 数据类型

### 布尔类型

```go
var b bool = true
```

### 数字类型

int、float32、float64，支持复数， 中位运算采用补码

### 字符串类型

字符串用单个字节连接起来的，字节使用UTF-8编码

### 派生类型

指针（Pointer）、数组、结构化类型、函数类型

Channel类型、切片类型、interface类型、Map类型

## 数字类型

uint8、uint16、uint32、uint64、int8、int16、int32、int64、float32、float64、complex64（32位实数和虚数）、complex128、byte（类似于uint8）、rune（类似于int32）、uint（32或64位）、uintptr（无符号整型，用于存放指针）

## 语言变量

```go
var a string = "string"
var b,c int = 1,2
var d int  //默认0
var e bool //默认false
var f string //默认为""
//一下几种为nil:
var a *int
var a []int
var a map[string] int
var a chan int
var a func(string) int
var a error //error is interface
//还可以根据值自行判断
var a = true //默认为bool
```



```go
//intVal := 1 相等于
var intVal int 
intVal = 1
```

多变量声明

```go
//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3

var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断

vname1, vname2, vname3 := v1, v2, v3 // 出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误


// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)
```

空白标识符 _ ,实际上是一个只写变量



## 常量

const LENGTH int = 10

用作枚举

const(

​	Unknown = 0

​	Female = 1

​	Male = 2

)

常量可以用len(), cap(), unsafe.Sizeof()函数计算表达式的值。常量表达式中，函数必须是内置函数，否则编译不过；

```go
package main

import "unsafe"
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)

func main(){
    println(a, b, c)
}
```

### iota

iota，特殊常量，可以认为是一个可以被编译器修改的常量。

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。

iota 可以被用作枚举值

```go
const (
    a = iota
    b = iota
    c = iota
)
const (
    a = iota
    b
    c
)

package main

import "fmt"

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
//结果 0 1 2 ha ha 100 100 7 8

```



```go
package main

import "fmt"
const (
    i=1<<iota
    j=3<<iota
    k
    l
)

func main() {
    fmt.Println("i=",i)
    fmt.Println("j=",j)
    fmt.Println("k=",k)
    fmt.Println("l=",l)
}
//i=1 j=6 k=12 l=24
```



## 运算符

基本一样， &取地址 ，* 指针变量

## 条件语句

### if语句

不用括号，并且可以在if中声明变量；

```go
if var a int; a - 50 < b{
	
}
```

### switch语句

case 后面不用beak；不会自动执行下一条语句，**fallthrough关键字会强制执行下一条case的语句**

### select语句

select 是 Go 中的一个控制结构，类似于用于通信的 switch 语句。每个 case 必须是一个通信操作，要么是发送要么是接收。

select 随机执行一个可运行的 case。如果没有 case 可运行，它将阻塞，直到有 case 可运行。一个默认的子句应该总是可运行的

```go
select {
    case communication clause  :
       statement(s);      
    case communication clause  :
       statement(s);
    /* 你可以定义任意数量的 case */
    default : /* 可选 */
       statement(s);
}
/*
以下描述了 select 语句的语法：

每个 case 都必须是一个通信
所有 channel 表达式都会被求值
所有被发送的表达式都会被求值
如果任意某个通信可以进行，它就执行，其他被忽略。
如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行。
否则：
如果有 default 子句，则执行该语句。
如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值。
*/
```

## 循环语句

和 C 语言的 for 一样：

for init; condition; post { }

和 C 的 while 一样：

for condition { }

和 C 的 for(;;) 一样：

for { }

for 循环的 range 格式可以对 slice、map、数组、字符串等进行迭代循环。格式如下：

for key, value := range oldMap {     newMap[key] = value }



## 函数

```go
func swap(x, y string) (string, string) {
   return y, x
}
```

如果函数返回值声明了变量，那return后面可以不加东西

```go
func add(x,y int) (sum int){
	sum = x + y;
    return
}
```

## 初始化数组

```go
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
//数组长度不确定时，可以用...代替数组的长度，编译器会自行推断。
//如果忽略 [] 中的数字不设置数组大小，Go 语言会根据元素的个数来设置数组的大小。
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance := [5]float32{1:2.0,3:7.0}
```

## 结构体

结构体定义需要使用 type 和 struct 语句。struct 语句定义一个新的数据类型，结构体中有一个或多个成员。type 语句设定了结构体的名称。结构体的格式如下：

```go
type struct_variable_type struct {
   member definition
   member definition
   ...
   member definition
}
```

variable_name := structure_variable_type {value1, value2...valuen} 

或 

variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}



## 切片slice

### 概念

切片可看作一种对数组的包装形式，他的包装数组称为该切片的底层数组。反过来讲，切片是针对其底层数组中某个连续片段的描述。切片的长度是可变的，并不是类型的一部分，只要元素类型相同，两个切片的类型就是相同的，一个切片类型的零值总是nil（空指针），此零值的长度和容量都为0。

### 定义切片

```go
var identifier []type	
```

切片不需要说明长度

或使用 **make()** 函数来创建切片:

```go
var slice1 []type = make([]type, len)

//也可以简写为

slice1 := make([]type, len)

//也可以指定容量，其中 capacity 为可选参数。

make([]T, length, capacity)
```

### 切片初始化

```go
s :=[] int {1,2,3 } 
直接初始化切片，[] 表示是切片类型，{1,2,3} 初始化值依次是 1,2,3，其 cap=len=3。

s := arr[:] 
初始化切片 s，是数组 arr 的引用。

s := arr[startIndex:endIndex] 
将 arr 中从下标 startIndex 到 endIndex-1 下的元素创建为一个新的切片。

s := arr[startIndex:] 
默认 endIndex 时将表示一直到arr的最后一个元素。

s := arr[:endIndex] 
默认 startIndex 时将表示从 arr 的第一个元素开始。

s1 := s[startIndex:endIndex] 
通过切片 s 初始化切片 s1。

s := make([]int,len,cap) 
通过内置函数 make() 初始化切片s，[]int 标识为其元素类型为 int 的切片。
```

### len() and cap() functions

```go
切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。

以下为具体实例：

实例
package main

import "fmt"

func main() {
   var numbers = make([]int,3,5)

   printSlice(numbers)
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}

以上实例运行输出结果为:

len=3 cap=5 slice=[0 0 0]

一个切片在未初始化之前默认为 nil，长度为 0，实例如下
package main

import "fmt"

func main() {
	s := [7]int{1, 2, 3, 4, 5, 6, 7}
	var numbers = s[3:5]
	printSlice(numbers)
}
func printSlice(x []int) {
	fmt.Printf("len=%d,cap=%d,slice=%v", len(x), cap(x), x)
}

//输出结果
len=2,cap=4,slice=[4 5]
```

## 范围range

用于for循环中迭代数组、切片、通道、或map，在数组和切片中返回索引和索引对应的值，在mao返回key-value值

```go
package main
import "fmt"
func main() {
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    //range也可以用在map的键值对上。
    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }
    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```

sum: 9 

index: 1 

a -> apple 

b -> banana 

0 103 

1 111



## map

```go
package main

import "fmt"

func main() {
    var countryCapitalMap map[string]string /*创建集合 */
    countryCapitalMap = make(map[string]string)

    /* map插入key - value对,各个国家对应的首都 */
    countryCapitalMap [ "France" ] = "巴黎"
    countryCapitalMap [ "Italy" ] = "罗马"
    countryCapitalMap [ "Japan" ] = "东京"
    countryCapitalMap [ "India " ] = "新德里"

    /*使用键输出地图值 */
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }

    /*查看元素在集合中是否存在 */
    capital, ok := countryCapitalMap [ "American" ] /*如果确定是真实的,则存在,否则不存在 */
    /*fmt.Println(capital) */
    /*fmt.Println(ok) */
    if (ok) {
        fmt.Println("American 的首都是", capital)
    } else {
        fmt.Println("American 的首都不存在")
    }
}
```

## 接口

```go
package main

import (
    "fmt"
)

type Phone interface {
    call()
}

type NokiaPhone struct {
}

func (nokiaPhone NokiaPhone) call() {
    fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {
}

func (iPhone IPhone) call() {
    fmt.Println("I am iPhone, I can call you!")
}

func main() {
    var phone Phone

    phone = new(NokiaPhone)
    phone.call()

    phone = new(IPhone)
    phone.call()

}
```



## 错误

```go
package main

import (
    "fmt"
)

// 定义一个 DivideError 结构
type DivideError struct {
    dividee int
    divider int
}

// 实现 `error` 接口
func (de *DivideError) Error() string {
    strFormat := `
    Cannot proceed, the divider is zero.
    dividee: %d
    divider: 0
`
    return fmt.Sprintf(strFormat, de.dividee)
}

// 定义 `int` 类型除法运算的函数
func Divide(varDividee int, varDivider int) (result int, errorMsg string) {
    if varDivider == 0 {
            dData := DivideError{
                    dividee: varDividee,
                    divider: varDivider,
            }
            errorMsg = dData.Error()
            return
    } else {
            return varDividee / varDivider, ""
    }

}

func main() {

    // 正常情况
    if result, errorMsg := Divide(100, 10); errorMsg == "" {
            fmt.Println("100/10 = ", result)
    }
    // 当除数为零的时候会返回错误信息
    if _, errorMsg := Divide(100, 0); errorMsg != "" {
            fmt.Println("errorMsg is: ", errorMsg)
    }

}

100/10 =  10
errorMsg is:  
    Cannot proceed, the divider is zero.
    dividee: 100
    divider: 0
```

## 并发

Go 语言支持并发，我们只需要通过 go 关键字来开启 goroutine 即可。

goroutine 是轻量级线程，goroutine 的调度是由 Golang 运行时进行管理的。

goroutine 语法格式：

go 函数名 (参数列表）

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go say("world")
	say("hello")
}
func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

world
hello
hello
world
world
hello
hello
world
world
hello
```

### 通道channel

通道（channel）是用来传递数据的一个数据结构。

通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 <- 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。

```go
ch <- v    // 把 v 发送到通道 ch
v := <-ch  // 从 ch 接收数据
           // 并把值赋给 v

声明一个通道很简单，我们使用chan关键字即可，通道在使用前必须先创建：

ch := make(chan int)
```

**注意**：**默认情况**下，通道是不带缓冲区的。发送端发送数据，同时必须有接收端相应的接收数据。

以下实例通过两个 goroutine 来计算数字之和，在 goroutine 完成计算后，它会计算两个结果的和：

```go
package main

import "fmt"

func main() {
	s := []int{7, 2, 7, 2, 1, 3, 5, 6, -1, 1}
	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c
	fmt.Println(x, y, x+y)
}
func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // sum发送到通道c
}

14 19 33
```

### 通道缓冲区

第二个参数为缓存区大小

```go
ch := make(chan int,100)
```

带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据。

不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

**注意**：如果通道**不带缓冲**，发送方会**阻塞**直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；如果**缓冲区已满**，则意味着需要等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直**阻塞**。

```go
package main

import "fmt"

func main() {
    // 这里我们定义了一个可以存储整数类型的带缓冲通道
        // 缓冲区大小为2
        ch := make(chan int, 2)

        // 因为 ch 是带缓冲的通道，我们可以同时发送两个数据
        // 而不用立刻需要去同步读取数据
        ch <- 1
        ch <- 2

        // 获取这两个数据
        fmt.Println(<-ch)
        fmt.Println(<-ch)
}
```

### 遍历通道与关闭通道

range来实现遍历读到的数据

```go
v,ok := <-ch
```

如果通道接收不到数据后 **ok 就为 false**，这时通道就可以使用 **close()** 函数来关闭。

```go
package main

import (
        "fmt"
)

func fibonacci(n int, c chan int) {
        x, y := 0, 1
        for i := 0; i < n; i++ {
                c <- x
                x, y = y, x+y
        }
        close(c)
}

func main() {
        c := make(chan int, 10)
        go fibonacci(cap(c), c)
        // range 函数遍历每个从通道接收到的数据，因为 c 在发送完 10 个
        // 数据之后就关闭了通道，所以这里我们 range 函数在接收到 10 个数据
        // 之后就结束了。如果上面的 c 通道不关闭，那么 range 函数就不
        // 会结束，从而在接收第 11 个数据的时候就阻塞了。
        for i := range c {
                fmt.Println(i)
        }
}

0
1
1
2
3
5
8
13
21
34
```

