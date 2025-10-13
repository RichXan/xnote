- Go可以用json.Marshal将结构体序列化成byte[]
- 用json.Ummarshal将byte[]转换成map数组

- Go没有while go数组的长度不可以改变，但是切片可以。切片可以追加元素。

- 如果你想要==交换两个变量的值==，则可以简单地使用==a, b = b, a==。 
```go
// foreach 循环
for key, value := range oldMap {

	newMap[key] = value
}
```
- 
```go
// 第二个参数为函数!!!
func visit(list []int, f func(int)) {
 for _, v := range list {
 // 执行回调函数
 f(v)
 }
}

func main() {
 // 使用匿名函数直接做为参数
 visit([]int{1, 2, 3, 4}, func(v int) {
 fmt.Println(v)
 })
}
```

- defer是在return之前进行使用
- 切片与数组的区分
	- 数组的长度不可改变
		- 数组初始化`var variable_name [SIZE] variable_type`
	- 切片的长度是不固定的，可以追加元素。
		- 切片初始化`var identifier []type`
		- `var slice1 []type = make([]type, len) `
		- 简写：`slice1 := make([]type, len)`
		- `make([]T, length, capacity)`其中capacity为可选参数。这里 len 是数组的长度并且也是切片的初始长度。
- Map集合：是一种无序的键值对的集合
	- Map定义： `var map_variable map[key_data_type]value_data_type`
		make定义Map：`map_variable = make(map[key_data_type]value_data_type)`




| 占位符 | 说明                       | 举例                  | 输出                        |
| ------ | -------------------------- | ------------------- | --------------------------  |
| %v     | 相应值的默认格式。         | Printf("%v", people)  | {zhangsan}                   |
| %+v    | 打印结构体时，会添加字段名  | Printf("%+v", people) | {Name:zhangsan}              |
| %#v    | 相应值的Go语法表示        | Printf("#v", people)  | main.Human{Name:"zhangsan"}  |
| **%T**     | 相应值的类型的Go语法表示   | Printf("%T", people)  |  main.Human                  |
	


# 1. Go语法基础
## 1.1 编码规范
Go在命名时以字母a到Z或a到Z或下划线开头，后面跟着零或更多的字母、下划线和数字(0到9)。Go不允许在命名时中使用@、$和%等标点符号。Go是一种==区分大小写==的编程语言。因此，Manpower和manpower是两个不同的命名。

> 1.  当命名（包括常量、变量、类型、函数名、结构字段等等）以一个==大写字母开头==，如：Group1，那么使用这种形式的标识符的对象就==**可以被外部包的代码所使用**==（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；
>   
> 2.  **命名如果以==小写字母开头==，则==对包外是不可见==的，但是他们在整个包的内部是可见并且可用的**（像面向对象语言中的 private ）

| 类型         | 规范                                                          | 例子                                                                         |
| ------------ | ------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 包命名       | package名字和目录保持一致，简短，有意义，尽量和标准库不要冲突 | `package demo`                                                               |
| 文件命名     | 全小写用_划分                                                 | `my_test.go`                                                                   |
| 结构体       | 驼峰（首字母根据访问控制大/小写）                             | `type User struct`                                                           |
| 接口         | 驼峰（首字母根据访问控制大/小写）                             | `type Reader interface`                                                      |
| 变量命名     | 驼峰（首字母根据访问控制大/小写）特有名词看情况               | 私有且特有名词：`apiClinet`，其他情况保持原有写法`APIClinet  repoID  UserID` |
| bool型变量   | 名称以Has/is/allow开头                                        | `var isExist/hasConflict/canManage/allowGitHook bool`                        |
| 常量命名     | 全部用大写字母且使用下划线                                    | `const APP_VER = "1.0"`                                                      |
| 枚举类型常量 | 首先要创建相应类型                                            | 如下                                                                         |
```go
// Scheme是string类型的，相当于给string重命名了而已
type Scheme string

// Scheme是枚举常量的类型
const (
    HTTP  Scheme = "http"
    HTTPS Scheme = "https"
)
```


## 1.2 变量的声明
`c : = 10`
> 这种方式它只能被用在函数体内，而不可以用于全局变量的声明与赋值

```go
package main
// 指定变量类型并赋值
var a string = "World"
// 指定变量类型，声明后若不赋值，使用默认值为false
var b bool
// 根据值自行判定变量类型(类型推断Type inference)
var c = "Hello"
// 省略var, 注意 :=左侧的变量不应该是已经声明过的(多个变量同时声明时，至少保证一个是新变量)，否则会导致编译错误(简短声明)
d := 10
func main(){
    println(a, b, c)
}
```
运行结果
```go
Hello World false
```



## 1.3 iota常量
iota，特殊常量，可以认为是一个可以被编译器修改的常量
iota 可以被用作枚举值：
```go
const (
    a = iota
    b = iota
    c = iota
)
```
第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1；所以 a=0, b=1, c=2 可以简写为如下形式：
```go
const (
    a = iota
    b
    c
)
```
**iota 用法**
```go
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
			d = "ha"   //独立值，iota += 1   iota = 3
            e          //"ha"   iota += 1   iota = 4
            f = 100    //iota +=1           iota = 5
            g          //100  iota +=1      iota = 6
            h = iota   //7,恢复计数           iota = 7
            i          //8                   iota = 8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```
运行结果：
`0 1 2 ha ha 100 100 7 8`
point:
- 如果中断iota自增，则必须显式恢复。且后续自增值按行序递增
- 自增默认是int类型，可以自行进行显示指定类型
- 数字常量不会分配存储空间，无须像变量那样通过内存寻址来取值，因此无法获取地址

scanf：读取键盘的数据是阻塞式的
阻塞：程序走到这里不动了，类似于堵车，如果没有读到数据就一直在这里等。只有等到键盘输入数据后才可以解除这个阻塞，程序才会往下执行。