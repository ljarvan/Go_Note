# os.Args
程序获取运行他时给出的参数，可以通过os包来实现。
```
package main

import (
	"fmt"
	"os"
	"strconv"
)

func main()  {
	for index, args := range os.Args{
		fmt.Println("参数" + strconv.Itoa(index)+":", args )
	}

}
```
```
 go run test.go 12 df 2.3
参数0: /var/folders/7c/4gjt355x5s956tbhsvy_0vl80000gn/T/go-build374208526/b001/exe/test
参数1: 12
参数2: df
参数3: 2.3
```
命令行参数包括了程序路径本身，以及通常意义上的参数。  
程序中os.Args的类型是 []string ，也就是字符串切片。所以可以在for循环的range中遍历。  
如果不想要输出os.Args的第一个值，也就是可执行文件本身的信息，则
```
for idx, args := range os.Args[1:] { 

}
```
输出切片所有元素
```
fmt.Println(os.Args[1:])
```
# flag包
## 2.1定义参数
使用flag包，首先要定义待解析命令行参数，也就是以"-"开头的参数。  
-help不需要特别指定，可以自动处理。
```
func Bool(name string, value bool, usage string) *bool
func String(name string, value string, usage string) *string
```
```
var b = flag.Bool("b", false, "bool类型参数")
var s = flag.String("s", "", "string类型参数")
```

通过flag.Bool和flag.String,建立了两个指针b和s，分别指向bool类型和string类型的变量。所以要通过*b和*s来使用变量。  
|参数|	功能
|---|-
|name|	命令行参数名称，比如 -b, -help
|value|	默认值，未显式指定的参数，给出隐式的默认值，比如本例中-b未给出的话，*b=false
|usage|	提示信息，如果给出的参数不正确或者需要查看帮助 -help，那么会给出这里指定的字符串
## 解析参数
```
flag.Parse()
```
## 使用参数
```
fmt.Println("-b:", *b)
fmt.Println("-s:", *s)
```
## 未参与解析参数
参数中没有能够按照预定义的参数解析的部分，通过`flag.Args()`即可获取，是一个字符串切片。  
需要注意的是，从第一个不能解析的参数开始，后面的所有参数都是无法解析的。即使后面的参数中含有预定义的参数。

## 例子
```
ppackage main

import (
	"flag"
	"fmt"
)

func main() {
	var username = flag.String("u","","用户名,默认为空")
	var password = flag.String("p", "", "密码,默认为空")
	var host = flag.String("h", "127.0.0.1", "主机名,默认 127.0.0.1")
	var port = flag.Int( "P", 3306, "端口号,默认为空")
	flag.Parse()
	fmt.Printf("username=%v  password=%v  host=%v  port=%d \n", *username, *password, *host, *port)

}

go run test.go -u root -p 1234
username=root  password=1234  host=127.0.0.1  port=3306 

go run test.go -u root -p 123456 -h localhost -P 3307
username=root  password=123456  host=localhost  port=3307 
```
