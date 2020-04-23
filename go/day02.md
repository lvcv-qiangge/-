Go安装

$GOPATH: 你写Go代码的工作区,保存你的Go代码的

***\*Go env\****

zq@czq-home:~$ go env

GO111MODULE=""

GOARCH="amd64"

GOBIN="/home/czq/go/bin"

GOCACHE="/home/czq/.cache/go-build"

GOENV="/home/czq/.config/go/env"

GOEXE=""

GOFLAGS=""

GOHOSTARCH="amd64"

GOHOSTOS="linux"

GOINSECURE=""

GONOPROXY=""

GONOSUMDB=""

GOOS="linux"

GOPATH="/home/czq/go"

GOPRIVATE=""

GOPROXY="https://proxy.golang.org,direct"

GOROOT="/usr/local/go"

GOSUMDB="sum.golang.org"

GOTMPDIR=""

GOTOOLDIR="/usr/local/go/pkg/tool/linux_amd64"

GCCGO="gccgo"

AR="ar"

CC="gcc"

CXX="g++"

CGO_ENABLED="1"

GOMOD=""

CGO_CFLAGS="-g -O2"

CGO_CPPFLAGS=""

CGO_CXXFLAGS="-g -O2"

CGO_FFLAGS="-g -O2"

CGO_LDFLAGS="-g -O2"

PKG_CONFIG="pkg-config"

GOGCCFLAGS="-fPIC -m64 -pthread -fmessage-length=0 -fdebug-prefix-map=/tmp/go-build303840672=/tmp/go-build -gno-record-gcc-switches"

 

 

 

GOPATH/bin 添加到环境变量:go install命名会吧生成的二进制可执行文件拷贝到GOPATH/bin

 

 

***\*GO命令\****

Go build:编译Go程序

Go build -o “文件名” 编译成一个可执行文件

Go run main.go :想执行脚本一样执行main.go文件

Go install:先编译后拷贝

 

 

***\*Go语言文件基础语法\****

存放go源代码 的文件名后缀名是.go

文件第一行: package关键字声明包含

如果要编译可执行文件,必须要有main包和main函数(入口函数)

//单行注释

/*

多行注释

*/

//Go语言函数外的语句必须以关键字开头

函数内部定义的变量必须使用

 

***\*变量和常量\****

3种声明方式:

\1. Var name string

\2. Var name = “陈正强”

\3. 函数内部专属:  name2 := “哈哈哈”

匿名变量(哑元变量):

当有些数据必须用变量接收但是又不使用它时,就可以用_来接收这个值.

 

 

***\*常量\****

Const p1 = 3.1415926

Const UserNotExitError = 1000

 

Iota实现枚举

两个要点

\1. iota在const关键字出现时将被重置为0

\2. const每新增一行常量声明.iota累加1

 

***\*流程控制\****

if

var age1 = 18

​	if age1 >= 18 {

​		fmt.Println("成年了")

​	}else if  age1 > 7{

​		fmt.Println("上小学")

​	}else{

​		fmt.Println("最快乐的时候")

​	}

 

***\*for 循环\****

标准for循环

for  i :=0; i < 10; i++{

 fmt.println(i)

} 

 

 

 

 

 

 

***\*基本数据类型\****

整形

无符号整型: unit8,unit16,unit32,unit64

带符号整型: int8,int16.int32.int64

Unit和Int:具体是32位还是64位看操作系统

Unitptr:表示指针

 

浮点型

Folat64和folat32

Go语言中浮点数默认是float64

 

## **数组**





 

```go
//存放元素的容器
//必须指定存放的元素的类型容量(长度)

//数组的长度是数组类型的一部分

func main() {

​	//数组定义

​	// var 数组变量名 [元素数量]元素类型

​	var a1 [3]bool // [true false true]

​	var a2 [4]bool // [true true false false]

​	fmt.Printf("a1:%T a2:%T\n", a1, a2)
```



 

### 	**数组的初始化**

```go
	//如果不初始化:默认元素都是零值(布尔值: false, 整型和浮点型都是0, 字符串: "")

​	fmt.Println(a1, a2)

​	//1.初始化方式1

​	a1 = [3]bool{true, true, true}

​	fmt.Println(a1)

​	//2.初始化方式2:根据初始值自动推算数组的长度是多少

​	//a10 := [9]int{0, 1, 2, 3, 4, 5, 6, 7, 8,}

​	a10 := [...]int{0, 2, 3, 4, 5, 6, 7}

​	fmt.Println(a10)

​	//3,初始化方式3:根据索引来初始化

​	a9 := [6]int{0: 2, 5: 2}

​	fmt.Println(a9)

 

​	//数组的便利

​	citys := [...]string{"广西", "柳州", "橙汁"}

​	//1.根据索引遍历

​	for i := 0; i < len(citys); i++ {

​		fmt.Println(citys[i])

​	}

​	//2.for range遍历

​	for i, v := range citys {

​		fmt.Println(i, v)

​	}
```





 

### 	**多维数组**

```go
	//[[1 2] [3 4] [5 6]]

​	var a11 [3][2]int //表示3个数组,每个数组有2个元素

​	a11 = [3][2]int{

​		[2]int{1, 2},

​		[2]int{3, 4},

​		[2]int{5, 6},

​	}

​	fmt.Println(a11)

 

​	//多维数组的遍历

​	for _, v1 := range a11 {

​		fmt.Println(v1)

​		for _, v2 := range v1 {

​			fmt.Println(v2)

​		}

​	}

​	//数组是值类型

​	b1 := [3]int{1, 2, 3} // [1 2 3]

​	b2 := b1	// [1 2 3] Ctrl+C Ctrl+v => 把word文档从文件夹A拷贝到文件夹B

​	b2[0] = 100// b2:[100 2 3]

​	fmt.Println(b1, b2)

 

切片(selice)

切片指向了一个底层的数组

切片的长度就是它元素的个数.

切片的容量是底层数组从切片的第一个元素到最后一个元素的数量.

 

var s []int //定义了一个存放int类型的切片

​	var s2 []string //定义了一个存放string 类型的切片

​	fmt.Println(s,s2)

​	fmt.Println(s == nil) //true

​	fmt.Println(s2 == nil) //true

 

​	//初始化

​	s = []int{1,2,3}

​	s2 = []string{"广西", "柳州", "橙汁"}

​	fmt.Println(s,s2)

​	fmt.Println(s == nil) //false

​	fmt.Println(s == nil) //false

​	//长度和容量

​	fmt.Printf("(len)s:%d (cap)s:%d\n", len(s), cap(s))

​	fmt.Printf("(len)s2:%d (cap)s2:%d\n", len(s2), cap(s2))
```



 

​	//由数组得到切片

​	a := [...]int{1, 3, 5, 7, 8, 9, 11, 12}

​	a1 := a[0:4] //1 3 5 7

​	//基于一个数组切割,左包含,右不包含,左开右避

​	fmt.Println(a1)

​	a2 := a [1:5]

​	fmt.Println(a2)

​	a3 := a[:3]	// => [0:3] [1 3 5 ] len 3 cap 8

​	a4 := a [3:]	// => [3:len(a)] [7 8 9 11 12] len 5 cap5

​	a5 := a [:]		// => [0:len(a)]

​	fmt.Println(a3, a4, a5)

​	//切片的容量是底层数组的容量

​	fmt.Printf("len(a3):%d map(a3):%d\n", len(a3), cap(a3))

​	fmt.Printf("len(a4):%d map(a4):%d\n", len(a4), cap(a4))

​	//切片再切割

​	s8 := a4[3:] //[11 12 ]

​	fmt.Printf("len(a4):%d map(a4):%d\n", len(s8), cap(s8))

​	//切片是引用类型,都执行了底层的一个数组.

​	a[2] = 23 //修改了底层数组的值

 

 

 

### **Make**

***\*切片的本质\****

切片就是一个框, 框住了一块连续的内存,

属于引用类型,真正的数据都是保存在底层数组里的

 ```go
//make()()函数创造切片

func main()  {
	s1 :=make([]int, 5)
	fmt.Printf("s1=%v len=(%d) cap=(%d)\n", s1, len(s1), cap(s1))
	s2 :=make([]int, 0, 10)
	fmt.Printf("s1=%v len=(%d) cap=(%d)\n", s2, len(s2), cap(s2))
	//切片的赋值
	s3 := []int{1,3,5}
	s4 := s3  //s3和s4都指向了同一个底层数组
	fmt.Println(s3, s4)
	s3[1] = 333
	fmt.Println(s3, s4)


	//切片的遍历
	//1. 索引遍历
	for i :=0; i < len(s3); i++{
		fmt.Println(s3[i])
	}
	//2. range遍历
	for v,i := range s3{
		fmt.Println(v, i)
	}
}
 ```



***\*切片的判断\****

所以要判断一个切片是否是空的，要是用len(s) == 0来判断，不应该使用s == nil来判断。

 

### **apeend申请容量**

```go
//make()()函数创造切片

func main()  {
	s1 :=make([]int, 5)
	fmt.Printf("s1=%v len=(%d) cap=(%d)\n", s1, len(s1), cap(s1))
	s2 :=make([]int, 0, 10)
	fmt.Printf("s1=%v len=(%d) cap=(%d)\n", s2, len(s2), cap(s2))
	//切片的赋值
	s3 := []int{1,3,5}
	s4 := s3  //s3和s4都指向了同一个底层数组
	fmt.Println(s3, s4)
	s3[1] = 333
	fmt.Println(s3, s4)


	//切片的遍历
	//1. 索引遍历
	for i :=0; i < len(s3); i++{
		fmt.Println(s3[i])
	}
	//2. range遍历
	for v,i := range s3{
		fmt.Println(v, i)
	}
}
```

 

![img](file:////tmp/wps-czq/ksohtml/wpsy2M86o.jpg) 



# **copy**

```go
func main() {
	a1 :=[]int{1, 3, 5}
	a2 := a1
	var a3 = make([]int, 3, 3)
	copy(a3, a1)  //使用copy()函数将切片a1中的元素复制到切片a3
	fmt.Println(a1, a2, a3)
	a1[0] = 222
	fmt.Println(a1, a2, a3)

	//切片删除
	//将b1中的索引为2的3这个元素删掉
	b1 :=[]int{1, 2, 3, 4, 5, 6}
	b1 =append(b1[:2], b1[3:]...)
	fmt.Println(b1)
	fmt.Println(cap(b1)) //切片不存值,所以容量是底层的数组的6

	c1 :=[...]int{1, 3, 5} //数组
	h1 := c1[:]   	//切片
	fmt.Println(c1, len(h1), cap(h1))
	//1.切片不保存原来的值
	//2.切片对应一个底层数组
	//3.底层数组都是占用一块连续的内存
	h1 = append(h1[:1], h1[2:]...) //修改了底层数组!
	fmt.Println(h1, len(h1), cap(h1))
	h1[0] = 233 //修改底层数组
	fmt.Println(c1)//[1 5 5]?
}
```



# **指针**

Go语言不存在指针操作.只需要记住两个符号

1.`&`取地址

2.`*`根据地址取值



###	**make和new的区别**

1. make和new都是用来申请内存的
2. new很少用,一般用来给基本数据类型申请内存,`string` ,`int`,返回的是对应类型的指针(*string, *int)。
3. make是用来给 `map` 、`slice` 、`chan  `申请内存的,make函数返回的是对应的这三个类型本身

