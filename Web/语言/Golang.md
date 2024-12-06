- [9] **Golang是一种静态强类型、编译型语言**
- 编译型
- 类 C
- 不用分号
- 不能存在未使用的变量或包
# 基本
## 数据结构
赋值：
```go
var power int = 114514
// or
power := 114514
```
不能重复声明变量，但可以同时用 `:=` 给新旧变量赋值
### 数组
```go
var arr1[5] int
arr2 = new([5]int)
```
数组拷贝：
```go
arr2 = *arr1
```
### 切片
对数组部分的引用，长度可变
```go
var identifier []type
var slice1 []type = arr1[start:end]
scores := []int{1, 2, 3}
```
或相关数组还未定义：
```go
slice := make([]type, len, cap)
slice = new([cap]type)[start:end]
```
内存形式：
![[Pasted image 20240524084305.png|500]]
有 `0 <= len(s) <= cap(s)`
这个切片长度是 0，需要扩展：
```go
scores = append(scores, 5)
```
这样会把 5 加到 scores 的第一个元素，若要给第 6 个元素赋值，应该：
```go
scores = scores[0:6]
scores[5] = 114514
```
`append` 会以 `x2` 拓展不够的数组，类似 STL
#### copy()
```go
worst := make([]int, 5)
copy(worst, scores[:3])
```
多的不管，少的不动
### map
即字典
```go
// var map1 map[keytype]valuetype
var map1 map[string]int
lookup := make(map[string]int, 100) // 可选填初始容量
mapLit := map[string]int{"one": 1, "two": 2}
```
- key 必须可以用 `==` 或 `!=` 比较
- value 可以是任意类型
- 自动扩张
- **不要使用 `new` 来构造 map，永远用 `make`**
#### 检测元素
```go
val1, isPresent := map1["Key"]
```
`val1` 返回键值，`isPresent` 返回 bool 值，它们为：
- 值类型的空值，与 false
- 对应值，与 true
#### 删除元素
```go
delete(map1, key1)
```
- key 不存在也不出错
#### 遍历
```go
for key, value := range lookup {
    ...
}
```
朴实的添加方法：
```go
lookup["ANON"] = 1
```
---
## 结构体
```go
type Band struct {
    Name string
    member int
}
myGO := Band{
    Name: "MyGO",
    member: 5,
}
```
可以将函数与结构体绑定：
```go
func (s *Band) Super() { 
    s.member += 10000 
}
```
### 匿名字段
```go
type Band struct {
    int
    string
}
```
易知每种类型只有一个匿名字段
### 工厂
```js
func NewBand(name string, member int) *Band {
    return &Band{name, member}
}
```
使用
```go
myGO := NewBand("MyGO", 5)
```
对于结构体，`new(Band)` 和 `&Band{}` 等价
### tag
字段声明后面可以加字符串标签
```go
type Band struct {
    Name string "band name"
}
```
#### reflect
`reflect` 包用于自省类型、属性和方法 #跳转 ，要访问标签：
```go
t := reflect.TypeOf(Band{})
f1, _ := t.FieldByName("band name")
fmt.Println(f1.Tag)
```
也可以按顺序访问：
```go
ixField := t.Field(0)
fmt.Printf("%v\n", ixField.Tag)
```
---
## 函数
可以返回多个值：
```go
func power(name string) (int, bool) { }
value, exists := power("goku")
_, yes := power("goku")
```
- 不需要的赋值给 `_`
### 变长
```go
func myFunc(a, arg ...int) {}
```
### defer
不管函数在什么时候返回，都会执行 `defer` 语句，如 `defer file.Close()` 防止忘记关闭文件，或用于打印日志记录
- 多个 `defer` 后进先出
### 闭包
也即包装的匿名函数

---
## 方法
特殊的函数类型，将对象或指针作为第一个参数，称为 `receiver`
- `receiver` 基类型不能是指针或者接口
    - 也不能是跨包类型或原生类型（`int`，`map`，etc.）
- 满足大小驼峰原则
```go
func (t TypeName) MethodName (ParamList ) (Returnlist) { }

func (t *TypeName) MethodName (ParamList) (Returnlist) { }
```
实际上就是：
```go
func TypName_MethodName(t TypeName , otherParamList) (Returnlist) { }

func TypName_MethodName (t *TypeName , otherParamList) (Returnlist) { }
```
- 相当于专门给某个类型用的函数，且不同类型可存在同名方法，然后供接口使用，体现**多态**性
### 方法值
<font color="grey" style="font-style: italic">method value</font>
若 `x` 有静态类型 `T`，而 `M` 是 `T` 的一个方法，那么
```go
f := x.M
```
`x.M` 是一个闭包，称为方法值
### 方法表达式
<font color="grey" style="font-style: italic">method expression</font>
有
```go
func (t T) Get() int {
    return t.a
}

func (t *T) Set(i int) int {
    t.a = i
    return t.a
}
```
称 `T.Get` 和 `(*T).Set` 为方法表达式，可以看做函数名，并作为右值赋给函数变量
```go
f1 := T.Get
f2 := (*T).Set
```
有以下三种情况：
1. 实际和形参相同类型：
```go
T{1}.Set(2) //  Point
pptr.Set(2) // *Point
```
2. 实参是类型T，但形参是类型 `*T`：
```go
p.Set(2) // implicit (&p)
```
3. 实参是类型 `*T`，形参是类型T。隐式解引用：
```go
pptr.Set(2) // implicit (*pptr)
```
---
## 接口
```go
type Namer interface {
    Method1(param_list) return_type
    Method2(param_list) return_type
    ...
}
```
接口提供一组方法集，不包含实现，也不包含具体的变量
- 通常包含 0~3 个方法
- 一般用 `-er` 名词后缀命名
- 核心是创建一个接口，给它传一个结构体等变量，然后调用方法
- 可以嵌套
比如：
```go
type Shaper interface {
    Area() float32
}

type Square struct {
    side float32
}
```
- 其中有方法：
```go
func (sq *Square) Area() float32 {
    return sq.side * sq.side
}
```
### 使用接口
```go
sq1 := new(Square)
sq1.side = 5

var areaIntf Shaper
areaIntf = sq1
```
- 或更简洁：`areIntf := Shaper(sq1)`
    - 或更更简洁：`areIntf := sq1`
- 要使用：`areIntf.Area()`
- 同一个接口的方法可以对多种类型生效，如：
```go
type Rectangle struct {
    length, width float32
}

func (r Rectangle) Area() float32 {
    return r.length * r.width
}
```
然后
```go
    r := Rectangle{5, 3}
    q := &Square{5}
    shapes := []Shaper{r, q}
    
    for n, _ := range shapes {
        fmt.Println("Shape details: ", shapes[n])
        fmt.Println("Area of this shape is: ", shapes[n].Area())
    }
```
### 类型断言
1. 测试接口 `varI` 是否包含类型 `T`
```go
v, ok := varI.(T)
```
-  `v` 返回类型值（`%T` 输出）或者对应 T 的零值，`ok` 返回 bool 值
2. 直接获取类型，通常搭配 `switch`
```go
switch t := areaIntf.(type) { 
    case *Square: 
        fmt.Printf("Type Square %T with value %v\n", t, t)
```
### Sorter 接口排序 #待补充 
### 空接口
```go
var val interface{}
```
可以向 `val` 赋任意类型的值，其占据两个字长：一个用来存储它包含的类型，另一个用来存储它包含的数据或者指向数据的指针
#### 通用数组
```go
type Element interface {}

type Vector struct {
    a []Element
}

func (p *Vector) At(i int) Element {
    return p.a[i]
}

func (p *Vector) Set(i int, e Element) {
    p.a[i] = e
}
```
#### 通用节点
```go
type Node struct {
    le   *Node
    data interface{}
    ri   *Node
}

func NewNode(left, right *Node) *Node {
    return &Node{left, nil, right}
}

func (n *Node) SetData(data interface{}) {
    n.data = data
}
```
### 反射包
- `reflect.TypeOf()` 返回数据类型
- `reflect.ValueOf()` 值
- `v.Kind()` 返回底层类型
- `v.Interface()` 返回接口值
#### 修改值
- `v.CanSet()` 返回值是否可以设置（bool）
- `v = v.Elem()` 使其变为可以设置
#### 结构体
- `v.NumField()` 结构体的字段数量
- `v.Field(i)` 第 i 个字段
- 注意仅大写的字段是可以设置的
# 代码结构
## 包
- 一个包可以在多个源文件中声明
### 示例
- `/src`
    - `/shopping`
        - `pricecheck.go`
        - `/db`
            - `db.go`
        - `/main`
            - `main.go`
        - `/models`
            - `item.go`
1. `db.go`：
```go
package db  
import "shopping/models"
func LoadItem(id int) *models.Item ...
```
2. `pricecheck.go`：
```go
package shopping
import (
    "shopping/db"
)
func PriceCheck(itemId int) ...
```
3. `main.go`：
主程序
```go
package main
import (
    "fmt"
    "shopping"
)
shopping.PriceCheck(4343)
```
4. `item.go`
```go
package models
type Item struct ...
```
在此定义 `Item` 结构，避免了循环导入
### 可见性
类型、函数和结构体：大写可以通过导入包访问，小写只能在本包访问（大小驼峰）
```go
import . "package"
```
- 使用包内函数或变量不须写出包名
```go
import _ "package"
```
- 只执行 `init` 函数并初始化全局变量
# 特性
## 标签
- 不能定义但不使用
```go
LABEL1:
    for i := 0; i <= 5; i++ {
        for j := 0; j <= 5; j++ {
            if j == 4 {
                continue LABEL1
            }
            fmt.Printf("i is: %d, and j is: %d\n", i, j)
        }
    }
```
- 这里 `continue` 会调到下一个 `i` 值
    - 若 `continue` 改为 `goto`，则会重置 `i` 值
- 若标签位于 `goto` 之后，则在这之间不能定义变量
## 格式化
```bash
go fmt ./...
```
## if 初始化
```go
if err := process(); err != nil { 
    return err 
} else {
    ...
    return err
}
```
`err` 只能在此 `if` 块使用
## 函数类型
```go
type FuncType func(argument_list) return_type
```
使用例：
```go
type MathFunc func(int, int) int

func (f *MathFunc) put(str string) {
    str = str + " Lane"
    fmt.Println(str)
}

func add(x, y int) int {
    return x + y
}

func sub(x, y int) int {
    return x - y
}

func mul(x, y int) int {
    return x * y
}

func doFive(f MathFunc) int {
    return f(5, 5)
}

func main() {
    var f MathFunc
    f = add
    fmt.Println(f(1, 2))   // 1 + 2
    fmt.Println(doFive(f)) // 5 + 5
    f.put("Azur")          // Azur Lane
}
```
# 错误处理
## error
一般用返回值处理错误：
```go
    n, err := strconv.Atoi(os.Args[1])
    if err != nil { 
        fmt.Println("not a valid number") 
    } else { 
        fmt.Println(n) 
    }
```
创建错误类型：
```go
type error interface {
    Error() string
}
```
使用 `error` 包：
```go
import "error"
if ... {
    return error.New("Error description")
} else {
    return nil
}
```
## panic
`panic` 语句在程序死亡时打印些东西，如：
```go
if err != nil {
    panic("ERROR occurred:" + err.Error())
}
```
- Panicking：在多层嵌套的函数调用中调用 panic，可以马上中止当前函数的执行，所有的 defer 语句都会保证执行并把控制权交还给接收到 panic 的函数调用者。这样向上冒泡直到最顶层，并执行（每层的） defer，在栈顶处程序崩溃，并在命令行中用传给 panic 的值报告错误情况
## recover
恢复终止中的程序，只能在 defer 修饰的函数中使用，取得 panic 传递的错误，在某个函数发生错误时，会执行这些东西：
```go
defer func() {
    if e := recover(); e != nil {
        fmt.Printf("Panicing %s\r\n", e)
    }
}()
```

# 协程
## goroutine
协程，一个小小线程，调用时会立即执行接下去的代码，并同步执行协程代码
```go
func num() {
    for i := 1; i < 5; i++ {
        fmt.Println(i)
        time.Sleep(250 * time.Millisecond)
    }
}

go num()
```
## channel
一种数据类型，在 goroutine之间通信
- 同时只有一个协程可以访问 channel
```go
var ch0 chan int
ch0 = make(chan int)
ch1 := make(chan string)
```
### 操作符 `<-`
-  `ch <- i1` 用通道发送变量
-  `i2 = <- ch` 接收通道发送的变量
    - 单独的 `<- ch` 获取通道的下一个值，丢弃当前值
- 给需要通信的协程传同一个通道
### 阻塞
发送和接收操作在没有对应操作之前会阻塞，阻塞时协程会休眠，直到另一头出现
### 缓冲
```go
buf := 114514
ch = make(chan int, buf)
```
未满的通道不会因无接收者而阻塞
### 信号量 #待补充 
### select
监听来自不同通道的数据
```go
func Suck(ch1, ch2 chan) {
    select {
    case v := <-ch1:
        fmt.Printf("Received on channel 1: %d\n", v)
    case v := <-ch2:
        fmt.Printf("Received on channel 2: %d\n", v)
    }
}
```