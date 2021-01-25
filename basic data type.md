# 标识符和关键字
## 标识符
在编程语言中标识符就是程序员定义的具有特殊意义的词，比如变量名、常量名、函数名等等。 Go语言中标识符由`字母数字`和`_`(下划线）组成，并且只能以字母和`_`开头。 举几个例子：abc, _, _123, a123。

## 关键字
关键字是指编程语言中预先定义好的具有特殊含义的标识符。 关键字和保留字都不建议用作变量名。  
Go语言中有25个关键字：
```
    break        default      func         interface    select
    case         defer        go           map          struct
    chan         else         goto         package      switch
    const        fallthrough  if           range        type
```
此外，Go语言中还有37个保留字。  
```
 Constants:       true  false  iota  nil

        Types:    int  int8  int16  int32  int64  
                  uint  uint8  uint16  uint32  uint64  uintptr
                  float32  float64  complex128  complex64
                  bool  byte  rune  string  error

    Functions:   make  len  cap  new  append  copy  close  delete
                 complex  real  imag
                 panic  recover
```
# 变量
## 变量类型
变量（Variable）的功能是存储数据。不同的变量保存的数据类型可能会不一样。经过半个多世纪的发展，编程语言已经基本形成了一套固定的类型，常见变量的数据类型有：整型、浮点型、布尔型等。
 
## 变量声明
Go语言中的变量需要声明后才能使用，同一作用域内不支持重复声明。 并且`Go语言的变量声明后必须使用`。
### 标准声明
Go语言的变量声明格式为：
```
var 变量名 变量类型
```
### 批量声明
每声明一个变量就需要写`var`关键字会比较繁琐，go语言中还支持批量变量声明：
```
var (
    a string
    b int
    c bool
    d float32
)
```
### 变量的初始化
Go语言在声明变量的时候，会自动对变量对应的内存区域进行初始化操作。每个变量会被初始化成其类型的默认值，例如： 整型和浮点型变量的默认值为`0`。 字符串变量的默认值为`空字符串`。 布尔型变量默认为`false`。 切片、函数、指针变量的默认为`nil`。
```
var 变量名 类型 = 表达式
```
或者一次初始化多个变量
```
var i, j, k, int
var b, f, s = true, 2.3, "four"
```
一组变量也可以通过调用一个函数，由函数的返回的多个返回值进行初始化
```
var f, err = os.open(name)
```
### 类型推导
有时候我们会将变量的类型省略，这个时候编译器会根据等号右边的值来推导变量的类型完成初始化。
```
var name = "zhangsan"
var age = 18
```
### 短变量声明
在`函数内部`，可以使用更简略的 `:= `方式声明并初始化变量。  
`var`形式的声明语句往往是用于需要显示指定变量类型的地方
```
package main

import (
	"fmt"
)
// 全局变量m
var m = 100

func main() {
	n := 10
	m := 200 // 此处声明局部变量m
	fmt.Println(m, n)
}
```
`注意:``:=`是一个变量声明语句，而`:`是一个变量赋值语句。 

简短变量声明语句中 必须至少要声明一个新的变量。下面代码无法通过。
```
f, err = os.open(name)
f, err = os.create(outfile)
```
### 匿名变量
在使用多重赋值时，如果想要忽略某个值，可以使用`匿名变量（anonymous variable）`。 匿名变量用一个下划线`_`表示，例如：
```
func foo() (int, string) {
	return 10, "Q1mi"
}
func main() {
	x, _ := foo()
	_, y := foo()
	fmt.Println("x=", x)
	fmt.Println("y=", y)
}
```
匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。  
`注意：` 1.函数外的每个语句都必须以关键字开始（var、const、func等）  
        2.:=不能使用在函数外。  
        3._多用于占位，表示忽略值。  
# 常量
相对于变量，常量是恒定不变的值，多用于定义程序运行期间不会改变的那些值。 常量的声明和变量声明非常类似，只是把`var`换成了`const`，`常量在定义的时候必须赋值`。  
所有常量的运算都可以在编译期完成，这样一些运算时的错误也可以在编译时发现  
```
const pi = 3.1415
```
声明了pi这两个常量之后，在整个程序运行期间它的值都不能再发生变化了。  

多个常量也可以一起声明：  
```
const (
    pi = 3.1415
    e = 2.7182
)
```
const同时声明多个常量时，如果省略了值则表示和上面一行的值相同。 例如：
```
const (
    n1 = 100
    n2
    n3
)
```
上面示例中，常量n1、n2、n3的值都是100。
# inta
`iota`是go语言的常量计数器，只能在常量的表达式中使用。  
`iota`在const关键字出现时将被重置为0。const中每新增一行常量声明将使iota计数一次(iota可理解为const语句块中的行索引)。 使用iota能简化定义，在定义枚举时很有用。
```
const (
		n1 = iota //0
		n2        //1
		n3        //2
		n4        //3
	)
```
## 常见例子
使用`_`跳过某些值
```
const (
		n1 = iota //0
		n2        //1
		_
		n4        //3
	)
```
`iota`声明中间插队
```
const (
		n1 = iota //0
		n2 = 100  //100
		n3 = iota //2
		n4        //3
	)
	const n5 = iota //0
```
定义数量级 （这里的<<表示左移操作，1<<10表示将1的二进制表示向左移10位，也就是由1变成了10000000000，也就是十进制的1024。同理2<<2表示将2的二进制表示向左移2位，也就是由10变成了1000，也就是十进制的8。）   
```
const (
		_  = iota
		KB = 1 << (10 * iota)
		MB = 1 << (10 * iota)
		GB = 1 << (10 * iota)
		TB = 1 << (10 * iota)
		PB = 1 << (10 * iota)
	)

```
多个`iota`定义在一行
```
const (
		a, b = iota + 1, iota + 2 //1,2
		c, d                      //2,3
		e, f                      //3,4
	)
```
# 整型
GO语言提供了`有符号`和`无符号`类型的整数运算，`int8、int16、int32、int64` 四种截然不同大小的有符号整型数类型，`uint8、uint16、uint32、uint64` 无符号整型数类型。Unicode字符`rune`类型和`int32`是等价类型，`uint8`就是我们熟知的`byte`型，byte类型一般用于强调数值是一个原始数据而不是一个小的整数。`int16`对应C语言中的`short`型，`int64`对应C语言中的`long`型。  

有符号整数采用2的补码形式表示，值域是-2^(n-1) ~ 2^(n-1)-1,无符号整数的所有bit位都只用来表示非负数。 


|类型|	      描述|   
|---|------------|
|uint8|	    无符号 8位整型 (0 到 255)|  
|uint16|	    无符号 16位整型 (0 到 65535)|  
|uint32|	    无符号 32位整型 (0 到 4294967295)|  
|uint64|	    无符号 64位整型 (0 到 18446744073709551615)|  
|int8|	    有符号 8位整型 (-128 到 127)|  
|int16|	    有符号 16位整型 (-32768 到 32767)|  
|int32|	    有符号 32位整型 (-2147483648 到 2147483647)|  
|int64|	    有符号 64位整型 (-9223372036854775808 到 9223372036854775807)|   


## 特殊整型
|类型|	描述|
|---|------|  
|uint|	32位操作系统上就是uint32，64位操作系统上就是uint64|  
|int|	32位操作系统上就是int32，64位操作系统上就是int64|  
|uintptr|	无符号整型，没有指定具体的bit大小但足以容纳一个指针|  

`注意`：在使用`int`和 `uint`类型时，不能假定它是32位或64位的整型，而是考虑int和uint可能在不同平台上的差异。  
## 数字字面语法
Go1.13版本之后引入了数字字面量语法，这样便于开发者以二进制、八进制或十六进制浮点数的格式定义数字，例如：  

v := 0b00101101， 代表二进制的 101101，相当于十进制的 45。 v := 0o377，代表八进制的 377，相当于十进制的 255。 v := 0x1p-2，代表十六进制的 1 除以 2²，也就是 0.25。

而且还允许我们用 _ 来分隔数字，比如说： v := 123_456 表示 v 的值等于 123456。

我们可以借助fmt函数来将一个整数以不同进制形式展示。
```
package main
 
import "fmt"
 
func main(){
	// 十进制
	var a int = 10
	fmt.Printf("%d \n", a)  // 10
	fmt.Printf("%b \n", a)  // 1010  占位符%b表示二进制
 
	// 八进制  以0开头
	var b int = 077
	fmt.Printf("%o \n", b)  // 77
 
	// 十六进制  以0x开头
	var c int = 0xff
	fmt.Printf("%x \n", c)  // ff
	fmt.Printf("%X \n", c)  // FF
}
```
# 浮点数
Go语言支持两种浮点型数：`float32`和`float64`。这两种浮点型数据格式遵循IEEE 754标准： float32 的浮点数的最大范围约为 `3.4e38`，可以使用常量定义：`math.MaxFloat32`。 float64 的浮点数的最大范围约为 `1.8e308`，可以使用一个常量定义：`math.MaxFloat64`。  

一个float32类型的浮点数可以提供大约6个十进制数的精度，float64类型的浮点数提供大约15个，所以优先使用float64类型。 

小数点前面或后面的数可以被省略。很大或很小的数最好用科学计数法书写，通过e或E来指定指数部分。
```
const Avogadro = 6.02214129e23 //阿伏伽德罗常数
const Planck = 6.62606957e-34 //普朗克常数
```
使用`%e`(带指数)或`%f`的形式打印浮点数。
```
for x := 0; x < 8; x++{
	fmt.printf("x = %d e^x = %8.3f\n", x, math.Exp(float64(x)))
}
```
上面代码打印e的幂，打印精度是小数点后三个小数精度和8个字符宽度。  
# 复数
GO语言提供了两种精度的复数类型：`complex64`和`complex128`.   
内置的complex函数用于构建复数，内建的`real`和`imag`函数分别返回函数的`实部`和`虚部`。  
```
func main()  {
	var x complex128 = complex(1,2)
	var y complex128 = complex(3,4)
	fmt.Println(x*y)
	fmt.Println(real(x*y))
	fmt.Println(imag(x*y))
}
```
复数也可以用==和!=进行相等比较，只有实部和虚部都相等时，复数才相等。  

复数有实部和虚部，complex64的实部和虚部为32位，complex128的实部和虚部为64位。  
# 布尔型
Go语言中以`bool`类型进行声明布尔型数据，布尔型数据只有`true`（真）和`false`（假）两个值。  

`注意：`  布尔类型变量的默认值为false。  

布尔值并不会隐式转化为数字值0或1，反之亦然。可包装成一个函数比较方便。
```
//botoi returns 1 if b is true and 0 if b is false  
fuc btoi (b bool) int{
	if b{
		return 1
	}
	return 0
}  
```
```
fun itob(i int) bool{
	return i !=0
}
```
# 字符串
字符串可以包含任意的数据，使用字符串就像使用其他原生数据类型（int、bool、float32、float64 等）一样。 Go 语言里的字符串的内部实现使用`UTF-8`编码。 字符串的值为`双引号`(" ")中的内容.  

第i个字节并不一定是字符串的第i个字符。  

子字符串操作s[i:j]基于原始的s字符串的`第i个字节开始到第j-1个字节`生成的一个新字符串。
## 字符串转译符
|转义符|	含义|  
|-----|--------|
|\a|  响铃|  
|\b|  退格|  
|\f|  换页 | 
|\r|	回车符（返回行首）|  
|\n|	换行符（直接跳到下一行的同列位置）|  
|\t|	制表符|  
|\v|  垂直制表符|  
|\'|	单引号|  
|\"|	双引号|  
|\\|	反斜杠|  
## 多行字符串
Go语言中要定义一个多行字符串时，就必须使用`反引号`字符：
```
s1 := `第一行
第二行
第三行
`
fmt.Println(s1)
```
反引号间换行将被作为字符串中的换行，但是所有的转义字符均无效，文本将会原样输出。
## 字符串的常用操作
|方法|	介绍|
|---|-
|len(str)|	求长度|
|+或fmt.Sprintf|	拼接字符串|
|strings.Split|	分割|
|strings.contains|	判断是否包含|
|strings.HasPrefix,strings.HasSuffix|	前缀/后缀判断|
|strings.Index(),strings.LastIndex()|	子串出现的位置|
## 修改字符串
要修改字符串，需要先将其转换成`[]rune`或`[]byte`，完成后再转换为`string`。无论哪种转换，都会重新分配内存，并复制字节数组。
```
func changeString() {
	s1 := "big"
	// 强制类型转换
	byteS1 := []byte(s1)
	byteS1[0] = 'p'
	fmt.Println(string(byteS1))

	s2 := "白萝卜"
	runeS2 := []rune(s2)
	runeS2[0] = '红'
	fmt.Println(string(runeS2))
}
```
## 类型转换
Go语言中只有强制类型转换，没有隐式类型转换。该语法只能在两个类型之间支持相互转换的时候使用。

强制类型转换的基本语法如下：
```
T(表达式)
```
其中，T表示要转换的类型。表达式包括变量、复杂算子和函数返回值等.  

比如计算直角三角形的斜边长时使用math包的Sqrt()函数，该函数接收的是float64类型的参数，而变量a和b都是int类型的，这个时候就需要将a和b强制类型转换为float64类型。
```
func sqrtDemo() {
	var a, b = 3, 4
	var c int
	// math.Sqrt()接收的参数是float64类型，需要强制转换
	c = int(math.Sqrt(float64(a*a + b*b)))
	fmt.Println(c)
}
```
## 字符串和数字的转换
### 整数转化为字符串
一种使用fmt.sprintf返回一个格式化的字符串，另一种方法是用strconv.Itoa  
```
func  main() {
	x := 123
	y := fmt.Sprintf("%d",x)
	fmt.Println(y, strconv.Itoa(x)) // 123 123
}
```


