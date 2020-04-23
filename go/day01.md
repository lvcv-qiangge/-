# **编译**

使用***\*go build\****

2. 在其他路径上执行 go build,需要在后面加上项目的路径(项目路径从GOPATH/src后开始写起,编译之后的可执行文件就保存在当前目录下)

3. Go build -o hello.exe



***\*Go run\****

像执行脚本文件一样执行Go代码

 

***\*Go install\****

Go install 分为两步

\1. 先编译得到一个可执行文件

\2. 将可执行文件拷贝到GOPATH/bin

 

交叉编译

Go支持跨平台编译一个能在linux平台执行的可执行文件

SET CGO_ENABLED=0  // 禁用CGO

SET GOOS=linux  // 目标平台是linux

SET GOARCH=amd64  // 目标处理器架构是amd64

Mac 下编译 Linux 和 Windows平台 64位 可执行程序：

CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build

CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build

Linux 下编译 Mac 和 Windows 平台64位可执行程序：

CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build

CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build

Windows下编译Mac平台64位可执行程序：

SET CGO_ENABLED=0

SET GOOS=darwin

SET GOARCH=amd64

go build

 

Go 语言文件的基本结构

```go
//导入语句
import "fmt"

//函数外只能防止标识符(变量\常量\函数\类型)的声明
//fmt.Println("hello") //非法
//程序的入口函数
func main() {
  fmt.Println("hello world!")
}
```





 

 

变量

Go语言中的每一个变量都有自己的类型，***\*并且变量必须经过声明才能开始使用。\****

声明变量

Var s1 string: 声明一个保存字符串类型数据的s1变量

***\*注意事项\****

*1.变量名推荐使用驼峰式*

*2.变量必须先声明再使用*

*3.变量声明且赋值后必须使用*

 

***\*字符串\****

Go语言中字符串是用双引号包裹的！！！

Go语言中单引号包裹的是字符！！！

 

//字符串

S := “hello 沙河”

//单独的字母,汉子,符号表示一个字符

C1 := ‘h’

C2 := ’1’

C3 := ‘沙’

//字节:  1字节=8bit(8个二进制位)

//1个字符’A’=1个字节

//1个utf8编码的汉子 ‘沙’ = 一般占3个字节