# **day03上课笔记**

# **内容回顾**

## 运算符

### 算术运算符

+.*/

### 逻辑运算符

`&&` `||` `!`

### 位运算符

`>>` `<<` `|` `^` `&`

赋值运算符

`=` `+=` 等...

`++` 和`--` 是独立的语句,不属于赋值运算符

### 比较运算符

`<` `>=` `|=` 等...

## **数组**(Array)

`var ages [30]int`

`var names [30] string`

`var nums [40]int`	 

数组包含元素的类型和元素的个数, 元素的个数(数组的长度)属于s数组类型的一部分.

数组是值类型

## **切片**

切片的定义

1. 切片不存值,他就像一个框框,去底层数组框值.
2. 切片:指针,长度,容量

切片的扩容策略:

1. 如果申请的容量大于原来的的2倍,那就直接扩容至新申请的容量
2. 如果小于102就,那么就直接两倍
3. 如果大于大于1024,就按照1.25倍去扩容
4. 具体存储的值类型不同,扩容策略也有一定的不同.

append函数

````go
var l1 []int
	l1 = make([]int,1)
	l1[0] = 3
	fmt.Println(l1)
	l1 = append(l1, 2) //自动初始化切片
	fmt.Println(l1)
````



## **指针**

只需要记住两个符号 `&` 和 `*`

````go
var l1 []int
	l1 = make([]int,1)
	l1[0] = 3
	fmt.Println(l1)
	l1 = append(l1, 2) //自动初始胡切片
	fmt.Println(l1)
````

## **map**

map存储的是键值对

它也是需要申请内存的

````go
var uu map[string]int
	fmt.Println(uu == nil)
	uu = make(map[string]int, 10) //做初始化 , 键 值 容量
	uu["xxx"] = 2
	fmt.Println(uu)
	fmt.Println(uu["xxx"])  //如果key不存值返回的value对应类型的零值
````







# **今日内容**

## 函数

### **函数的定义**

#### **基本格式**

#### 参数的格式

有参数的函数

参数类型简写

可变参数

#### **返回值的格式**

有返回值

多返回值

多返回值

命名返回值

### 变量作用域

1. 全局作用域
2. 函数作用域
   1. 先在函数内部找变量,找不到往外层找
   2. 函数内部的变量,外部是访问不到的
3. 代码块作用域

###	**高阶函数**

函数也是一种类型,它可以作为参数,也可以作为返回值

### **匿名函数**

没有名字的函数,

### **闭包**

 *![image-20200404175101616](/home/czq/.config/Typora/typora-user-images/image-20200404175101616.png)*

今日难点:

1. 函数的定义
2. 高阶函数
3. 函数类型
4. 闭包
5. defer
6. panic/recover

