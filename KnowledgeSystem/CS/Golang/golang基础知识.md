- 切片是没有大小的数组
- 参数传递
	- 值类型的数据：操作的是数据本身 int、string、bool、float64、array... ...
	- 引用类型的数据：操作的是数据的地址 slice、map、chan、func... ... 
- 局部变量遵循**就近原则**
- 递归函数：一个函数中自己调用自己
	- 递归函数需要一个出口
	- 逐渐向出口靠近
	- 没有出口会导致死循环
- defer：一个函数或方法的执行被延迟了
	- 当函数执行到最后时，这些defer语句会按照逆序执行（栈，先进后出）
	- defer函数中传入的值是当时传入的值内容，不会到最后再重新去取最新的内容 
- func函数本身也是一个数据类型（变量）`func`、
- 闭包
	- 一个外层函数中，有内层函数，该内层函数中，会操作外层函数的局部变量并且该外层函数的返回值就是这个内层函数。这个内层函数和外层函数的局部变量，统称为闭包结构
	- 局部变量的生命周期就会发生改变，正常的局部变量会随着函数的调用而创建，随着函数的结束而销毁但是闭包结构中的外层函数的局部变量并不会随着外层函数的结束而销毁，因为内层函数还在继续使用
```go
// 闭包
func main(){
	r1 := increment()
	fmt.Println(r1)
	fmt.Println(r1()) // 1
	fmt.Println(r1()) // 2
	fmt.Println(r1()) // 3
	fmt.Println(r1()) // 4
	fmt.Println(r1()) // 5
	r2 := increment()
	fmt.Println(r2()) // 1
	fmt.Println(r1()) // 6
	fmt.Println(r2()) // 2
	fmt.Println(r2()) // 3
	fmt.Println(r2()) // 4
	fmt.Println(r2()) // 5
}

func increment()func() int{
	var i = 0
	fun := func() int {
		i ++
		return i
	}
	return fun
}
```