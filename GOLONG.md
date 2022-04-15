

# GOLONG

## 一、基础语法

#### 注意事项

* import必须跟在package声明后

* 编译器会把换行符转化为分号，不能乱加，不然会影响Go的正确解析

### 分支指令

#### 循环

Go语言只有for循环这一种循环语句。for循环有多种形式，其中一种如下所示：

```go
for initialization; condition; post {
	// zero or more statements
}
```

for循环三个部分==不需括号包围。大括号强制要求，左大括号必须和*post*语句在同一行==。

当condition为true时执行循环体，post在循环体结束后执行，之后再对condition求值

#####  for 是 Go 中的 “while”

此时你可以去掉分号，因为 C 的 `while` 在 Go 中叫做 `for`。再去掉条件就是无限循环。

```go
func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

#### if

Go 的 `if` 语句与 `for` 循环类似，表达式外无需小括号 `( )` ，而大括号 `{ }` 则是必须的。

 `if` 语句可以在条件表达式前执行一个简单的语句。

该语句声明的变量作用域仅在 `if` 之内。

在 `if` 的简短语句中声明的变量同样可以在任何对应的 `else` 块中使用。

```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// 这里开始就不能使用 v 了
	return lim
}
```

#### switch

Go 的 switch 语句只运行选定的 case，而非之后所有的 case。 实际上，Go 自动提供了在这些语言中每个 case 后面所需的 `break` 语句。 除非以 `fallthrough` 语句结束，否则分支会自动终止。**Go 的另一点重要的不同在于 switch 的 case 无需为常量，且取值不必为整数 **。



没有条件的 switch 同 `switch true` 一样。

这种形式能将一长串 if-then-else 写得更加清晰。

```go
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

#### defer

defer 语句会将函数推迟到外层函数返回之后执行。

推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}

```

输出为：

```
hello
world
```

推迟的函数调用会被压入一个栈中。当外层函数返回时，被推迟的函数会按照后进先出的顺序调用。

---

### 函数

##### 变量声明

```go
func add(x int, y int) int {
	return x + y
}
```

类型在变量名之后

当连续两个或多个函数的已命名形参类型相同时，除最后一个类型以外，其它都可以省略。

在本例中，

```go
x int, y int
```

被缩写为

```go
x, y int
```

##### 多值返回

函数可以返回任意数量的返回值

```go 
func swap(x, y string) (string, string) {
	return y, x
}
```

---

### 变量

##### 变量声明

`var` 语句可以出现在包或函数级别。

```go
import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```



变量声明可以包含初始值，每个变量对应一个。

如果初始化值已存在，则可以省略类型；变量会从初始值中获得类型。

```go
var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```

**在函数中**，简洁赋值语句 `:=` 可在类型明确的地方代替 `var` 声明。

```go
func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

##### 类型转换

```go
i := 42
f := float64(i)
u := uint(f)
```

#### 数组

类型 `[n]T` 表示拥有 `n` 个 `T` 类型的值的数组。

表达式

```go
var a [10]int
```

会将变量 `a` 声明为拥有 10 个整数的数组。

每个数组的大小都是固定的。而切片则为数组元素提供动态大小的、灵活的视角。在实践中，切片比数组更常用。

#### 切片

类型 `[]T` 表示一个元素类型为 `T` 的切片。

切片通过两个下标来界定，即一个上界和一个下界，二者以冒号分隔：

```go
a[low : high]
```

它会选择一个半开区间，包括第一个元素，但排除最后一个元素。**即左闭右开**

以下表达式创建了一个切片，它包含 `a` 中下标从 1 到 3 的元素：

```
a[1:4]
```

切片像是引用，更改切片的元素会修改其底层数组中对应的元素。与它共享底层数组的切片都会观测到这些修改。

* 切片的长度就是它所包含的元素个数。

 	   切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

​		切片 `s` 的长度和容量可通过表达式 `len(s)` 和 `cap(s)` 来获取。

* 切片的零值是nil

* `make` 函数会分配一个元素为零值的数组并返回一个引用了它的切片

  [Go 切片：用法和本质 - Go 语言博客 (go-zh.org)](https://blog.go-zh.org/go-slices-usage-and-internals)

  ##### range

  `for` 循环的 `range` 形式可遍历切片或映射。

  当使用 `for` 循环遍历切片时，每次迭代都会返回两个值。第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

  ```go
  func main() {
  	for i, v := range pow {
  		fmt.Printf("2**%d = %d\n", i, v)
  	}
  }
  ```

  i是下标，v是值，只要下标或者只要值都是可以的，_传参即可

---

### 常量

常量的声明与变量类似，只不过是使用 `const` 关键字。

常量可以是字符、字符串、布尔值或数值。

常量不能用 `:=` 语法声明。

---

### 指针

类型 `*T` 是指向 `T` 类型值的指针。其零值为 `nil`。

```
var p *int
```

`&` 操作符会生成一个指向其操作数的指针。

```
i := 42
p = &i
```

`*` 操作符表示指针指向的底层值。

```
fmt.Println(*p) // 通过指针 p 读取 i
*p = 21         // 通过指针 p 设置 i
```

这也就是通常所说的“间接引用”或“重定向”。

与 C 不同，Go 没有指针运算。



##### 结构体指针

结构体指针不用（*P）来解引用，可以隐式间接引用

```go
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

