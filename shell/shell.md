![image-20200406154308390](/home/czq/.config/Typora/typora-user-images/image-20200406154308390.png)

# 什么是shell

 命令解释器,负责翻译我们输入的命令,执行成功返回给用户

面试题:linux默认的shell是(bash)

交互式: 用户输入命令,bash负责吧我们的命令翻译成内核可识别的语言,返回结果到屏幕

非交互式:不于用户进行交互,执行文本里的内容,执行到文件的末尾.结束

## 什么是shell脚本	

命令的集合 很多可执行命令放在文本中称为shell脚本(包含条件表达式 判断语句 数组等等)

shell脚本也是解释性语言,需要解释器,所以会有点慢

## shell脚本规范(为自动化准备)

​	1) 必须放在统一的目录

​	2)脚本必须以.sh结尾

​	3)脚本开头有注释#!/bin/sh 必须是第一行 

​	4)脚本的注释信息包含相关作者,脚本时间,和脚本作用

​	5)建议注释使用英文

​	6)成对的符号一次性书写完毕

​	7)脚本名称的命名 最好见名知其意

## shell脚本的执行方式

​	1.使用命令解释器执行 bash sh

​	2.使用全路径执行脚本 注意 需要加执行权限

​	3.使用source或者.执行脚本

   4.sh -x 脚本 可查看脚本运行顺序过程

# 环境变量	

## 1.什么是环境变量

x=1 y=x+1   x y\=\=\=变量名称  等号右边==变量值    = 赋值

右边一堆内容,使用一个名字来代替称为环境变量

## 2.环境变量分类

1.环境变量(全局变量)    2.普通变量(局部变量)

按照生命周期划分  两类

永久的 需要在环境变量文件中配置 /etc/profile 永久生效

临时的 使用export声明即可 临时生效 重启失效

test=czq 只对当前的bash生效

export test=czq 对(当前)所用的bash生效

写入/etc/profile针对所有用户和bash生效

如何查看变量

env

set

变量的生效顺序

/etc/profile  开机所有用户加载此文件

. ~/.bashrc

.bash_profile

.bashrc 

. /etc/bashrc

## 3.定义环境变量

name=czq

名称如何定义: 以下划线 数字和字母的组合, 须以字母或下划线开头,见名知其意,等号两边不允许有空格

oldboy_age 第一种书写方式

OLDBOY_AGE 第一种书写方式

Oldboy_Age 大驼峰语法

oldboy_Age 小驼峰语法

````bash
[root@db1 scripts]# old= 1
-bash: 1: 未找到命令
[root@db1 scripts]# old =1
-bash: old: 未找到命令
[root@db1 scripts]# old = 1
-bash: old: 未找到命令
[root@db1 scripts]# old=1
````

## 4.变量值的定义

数值的定义

name_age = 123  必须是连续的数字

字符串的定义

name="oldboy" 

""字符串必须加双引号,不知道加什么就加双引号 解析变量

' ' 单引号 所见既所得 吃什么拉什么 不解析变量

命令的定义

time=\`date` 反引号解析命令

time=$( )    \$ ()解析命令



注意命令的定义 是把结果复制给了变量 如果想改变结果 再次赋值

````bash
[root@db1 scripts]# czq=`date`
[root@db1 scripts]# echo $czq
2020年 04月 06日 星期一 22:14:30 CST
[root@db1 scripts]# date
2020年 04月 06日 星期一 22:14:39 CST
````



# shell特殊位置变量

## `$0`   \*\*\*\*\*

代表了脚本的名称 如果全路径执行 则脚本名称带全路径  

使用方法  给用户提示

`echo $"Usage: $0 {start|stop|status|restart|reload|force-reload}"`

## `$n` 

脚本的第n个参数 0被脚本名称占用 从1开始   \$ 1开始  \$1 \$2 $9后面的参数需要加{}

```shell
echo $1 $2 $3 $4 $5 $6 $7 $8 $9 ${10} ${11} ${12}
```

##   `$#`      \*\*\*\*\*

获取脚本传参的总个数 

也可以控制脚本传参的个数

```shell
[ $# -ne 2 ] && echo "请输入两个参数" && exit ##如果传递的参数不等与2输出"请输入两个参数"并退出
```

## ``$*``

获取脚本所有的参数 不加引号和$@相同 加上双引号 则把参数视为一个参数\$1\$2\$3

## `$@`

获取脚本所有的参数 不加双引号和$*相同,加上双引号则把参数视为独立的参数\$1 \$2 \$3

\$*和\$@在脚本中相同 在循环体内不同

````bash
[root@db1 scripts]# echo $*
i am bay guy
[root@db1 scripts]# echo $@
i am bay guy
[root@db1 scripts]# set -- i am bay guy
[root@db1 scripts]# for i in $*;do echo $i;done
i
am
bay
guy
[root@db1 scripts]# for i in $@;do echo $i;done
i
am
bay
guy
[root@db1 scripts]# for i in "$@";do echo $i;done
i
am
bay
guy
[root@db1 scripts]# for i in "$*";do echo $i;done
i am bay guy
````

## `$?`  \*\*\*\*\*

**获取上一条命令的返回结果 0为成功 非0失败**   

可指定返回结果

````bash
[ $# -ne 2 ] && echo "请输入两个参数" && exit 50
````

## `$$` 

获取脚本的pid

````bash
echo $$ > /tmp/nginx_log.pid 
````

## `$!`

获取上一个在后台运行脚本的PID 调试使用

## `$_`

获取命令行最后一个参数s   相当于ESC.









# 脚本传参的三种方式

第一种传参方式 赋值

````shell
name=$1
age=$2
echo $name $age
````

第二种传参方式 直接传参

````shell
echo $1 $2
````

第三种传参方式 read

````shell
read -p "请输入你的名字: " name
echo $name
````

取消变量赋值 

unset

````bash
[root@db scripts]# czq=1
[root@db scripts]# echo $czq
1
[root@db scripts]# unset czq
[root@db scripts]# echo $czq
````



# 环境变量字串

## 字符切片

切片时第一个数表示从第几个索引开始, 后面的数表示切出来几个

````bash
[root@db1 scripts]# echo $test
I am czq student
[root@db1 scripts]# echo ${test:0:2}
I
[root@db1 scripts]# echo ${test:0:3}
I a
[root@db1 scripts]# echo ${test:2:2}
am
[root@db1 scripts]# echo ${test:1:3}
am
[root@db1 scripts]# echo ${test:6:10}
zq student
[root@db1 scripts]# echo ${test:5:10}
czq studen
````

## 字符串长度

如何统计字符串长度

````bash
[root@db1 scripts]# echo ${#test}
16
[root@db1 scripts]# echo $test | wc -L
16
[root@db1 scripts]# echo $test | awk '{print length}'
16

````

## 面试题

统计一下字符串 输出小于3的字符

第一种方法

````shell
[root@db1 scripts]# cat for.sh 
#!/bin/bash
for i in I am czq student i am 18
do
	[ ${#i}  -lt 3 ] && echo $i
done
````

第二种方法 awk列方式

```bash
[root@db1 scripts]# echo "I am czq student i am 18" | xargs -n1 | awk '{if(length<3)print}'
I
am
i
am
18
```

第三种方法 横向判断

```bash
[root@db1 scripts]# echo "I am czq student i am 18" | awk '{for(i=1 ;i<=NF;i++)if(length($i)<3)print $i}'
I
am
i
am
18
```

# 字符的删除和替换

### #从前往后匹配删除##贪婪匹配

````bash
[root@db1 scripts]# echo $url
www.qg.com.cn
[root@db1 scripts]# echo ${url#*.}
qg.com.cn
[root@db1 scripts]# echo ${url#*.*.}
com.cn
[root@db1 scripts]# echo ${url#*.*.*.}
cn
[root@db1 scripts]# echo ${url##*.}
cn
````

### %从后往前匹配删除%%贪婪匹配

````bash
[root@db1 scripts]# echo $url
www.qg.com.cn
[root@db1 scripts]# echo ${url%.*}
www.qg.com
[root@db1 scripts]# echo ${url%.*.*}
www.qg
[root@db1 scripts]# echo ${url%%.*}
www
````

### /替换 //贪婪替换

````bash
[root@db1 scripts]# echo $url
www.qg.com.cn
[root@db1 scripts]#  echo ${url/www/aaa}
aaa.qg.com.cn
[root@db1 scripts]# echo ${url/w/a}
aww.qg.com.cn
[root@db1 scripts]# echo ${url//w/a}
aaa.qg.com.cn
[root@db1 scripts]# 
````

### 二次赋值

````bash
[root@db1 scripts]# cat oldboy.sh 
#!/bin/bash
dir=/etc/passwd
read -p "请输入文件名称: " dir
echo $dir $dir
[root@db1 scripts]# sh oldboy.sh 
请输入文件名称: /etc/hosts
/etc/hosts /etc/hosts   ##一开始定义的dir变量被后面获取到的输入内容给提取了
````



# 数值运算

## expr

expr 整数运算

````bash
[root@db1 scripts]# expr 1 + 1
2
[root@db1 scripts]# expr 1 - 1
0
[root@db1 scripts]# expr 4 * 2
expr: 语法错误
[root@db1 scripts]# expr 4 / 2
2
[root@db1 scripts]# expr 4 \* 2
8
````

## $(())

$(()) 整数运算  效率最高

````bash
[root@db1 scripts]# echo $((10 + 10))
20
[root@db1 scripts]# echo $((10 - 10))
0
[root@db1 scripts]# echo $((10 * 10))
100
[root@db1 scripts]# echo $((10 / 10))
1

````

## $[]

$[]  整数运算

````bash
[root@db1 scripts]# echo $[10 + 10]
20
[root@db1 scripts]# echo $[10 - 10]
0
[root@db1 scripts]# echo $[10 / 10]
1
[root@db1 scripts]# echo $[10 * 10]
100
````

## bc

bc 整数运算 小数运算

````bash
[root@db1 scripts]# echo 10 + 10.5 | bc
20.5
[root@db1 scripts]# echo 10 - 10.5 | bc
-.5
[root@db1 scripts]# echo 10 * 10.5 | bc
(standard_in) 1: syntax error
[root@db1 scripts]# echo 10*10.5 | bc
105.0
[root@db1 scripts]# echo 10/10.5 | bc
0
````

## awk  

awk  整数小数运算

````bash
[root@db1 scripts]# awk 'BEGIN{print 1+1}'
2
[root@db1 scripts]# awk 'BEGIN{print 1+1.5}'
2.5
[root@db1 scripts]# awk 'BEGIN{print 1*1.5}'
1.5
[root@db1 scripts]# awk 'BEGIN{print 1/1.5}'
0.666667
[root@db1 scripts]# echo 20.5 10 | awk '{print $1 + $2}'
30.5
[root@db1 scripts]# echo 20.5 10 | awk '{print $1 - $2}'
10.5
[root@db1 scripts]# echo 20.5 10 | awk '{print $1 * $2}'
205
[root@db1 scripts]# echo 20.5 10 | awk '{print $1 / $2}'
2.05

````

## python

python 整数小数运算

````bash
[root@db1 scripts]# python
Python 2.7.5 (default, Aug  4 2017, 00:39:18) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-16)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 1+1
2
>>> 1+1.5
2.5
>>> 33-1.5
31.5
>>> 33*1.5
49.5
>>> 33/1.5
22.0
````

## 判断输入的是否为整数

````shell
read -p "请输入第一个数字: " num
expr 1 + $num > /dev/null 2>&1
[ $? -ne 0 ] && echo "请输入整数" && exit 1
read -p "请输入第二个数字: " num1
expr 1 + $num1 > /dev/null 2>&1
[ $? -ne 0 ] && echo "请输入整数" && exit 2
echo "$num+$num"=$[$num + $num1]
````

# 条件表达式

## 文件测试

test = [ ]

[ ]  常用

test -e file 不常用

-e file 存在则为真

````bash
[root@db1 scripts]# [ -e /etc/hosts ] && echo ok || echo erroe
ok
[root@db1 scripts]# [ -e /etc/hosts ] && echo ok || echo error
ok
[root@db1 scripts]# [ -e /etc/hossts ] && echo ok || echo error
error
````

-f file 是否为文件   \*\*\*\*\*

````bash
[root@db1 scripts]# [ -f /etc/hosts ] 
[root@db1 scripts]# echo $?
0
[root@db1 scripts]# [ -f /etc/hostss ] 
[root@db1 scripts]# echo $?
1
[root@db1 scripts]# [ -f /etc/hosts ] && echo ok || echo error
ok
[root@db1 scripts]# [ -f /etc/hossts ] && echo ok || echo error
error
````

-d file 是否为目录 是否存在  \*\*\*\*\*

````bash
[root@db1 scripts]# [ -d /etc/hossts ] && echo ok || echo error
error
[root@db1 scripts]# [ -d /etc/ ] && echo ok || echo error
ok
[root@db1 scripts]# [ -d /backup/xx ] && echo ok || mkdir /backup/xx  ##不存在就创建
[root@db1 scripts]# ls /backup/
bin.log  czq  full_2020-02-26.sql  xx
````

-x file 是否可执行

-r file 是否可读

-w file 是否可写

案例:-f

````bash
[root@db1 scripts]# cat test.sh  ##函数库一般都是用在脚本里
#!/bin/sh
[ -f /etc/init.d/functions ] && . /etc/init.d/functions  ##判断函数库是否存在,存在并调用它
[ -d /backup ] && action "yes" /bin/true || action "directory not found" /bin/false
[root@db1 scripts]# . /etc/init.d/functions  ##调用函数
[root@db1 scripts]# [ -d /backup ] && action "yes" /bin/true || action "directory not found" /bin/false
yes                                                        [  确定  ]
````



## 数值比较

语法格式 [ 数值1 比较符 数值2]

比较符

-eq 等于

-ne 不等于

-gt 大于

-lt 小于

-ge 大于等于

-le 小于等于

[[ ]] 建议使用 == != > <  >= <= 比较符

````bash
[root@db1 scripts]# [ 1 -eq 2 ]
[root@db1 scripts]# echo $?
1
[root@db1 scripts]# [ 1 -eq 1 ]
[root@db1 scripts]# echo $?
0
[root@db1 scripts]# [ 1 -eq 1 ] ; echo $?  ##等于
0
[root@db1 scripts]# [ 10 -ne 10 ] ; echo $? ##不等于
1
[root@db1 scripts]# [ 10 -gt 10 ] ; echo $? ##大于
1
[root@db1 scripts]# [ 10 -lt 10 ] ; echo $? ##小于
1 
[root@db1 scripts]# [ 10 -ge 10 ] ; echo $? ## 大于等于
0
[root@db1 scripts]# [ 10 -le 10 ] ; echo $?  ##小于等于
0
[root@db1 scripts]# [ 2 -gt 5 ] && echo "表达式成立" || echo "表达式不成立"
表达式不成立
[root@db1 scripts]# [ 2 -gt 5 ] && ls || pwd
/server/scripts
````

题目:统计磁盘使用率 如果磁盘大于5%则发邮件报警 小于则提示ok

### 案例1

1. 如何取出磁盘当前使用率
2. 用数值表达式判断大小 如果大与发邮件 如果小小于提示ok
3. 测试脚本

````bash
[root@db1 scripts]# cat usedisk.sh 
#!/bin/bash
disk=`df -hT | grep /$ | awk '{print $(NF-1)}'`
[ ${disk%\%} -gt 80 ] && echo "当前磁盘使用率是$disk,已经超过%5 | mail -s "test" 2484576482@qq.com" || echo "当前使用率为$disk,状态良好"
````

### 案例2

内存使用率

````bash
[root@db1 scripts]# cat usemem.sh 
#!/bin/bash
Mem=`free -h | grep ^Mem | awk '{print $3}'`
lessmem=`echo ${Mem} | awk '{print 5 - $1 }'`
maxmem=`echo ${Mem} | awk '{print $1 - 2 }'`
[ ${Mem%.*} -gt 2 ] && echo "当前内存不租,使用了$Mem,超过满额$maxmem"G || echo "当前内存良好,内存为$Mem,离超出还有$lessmem"G
[root@db1 scripts]# cat usemem.sh 
#!/bin/bash
[ `free -m | awk 'NR==2{print $3}' | awk -F "" '{print $1}'` -gt 5 ] && echo mail || echo ok 
  ## 判断是否大于5 ,不大于就执行 || 后面的ok,大于就执行&&后面的mail ,不大于就执行||后面的ok
````

### 案例3

统计服务器负载,负载1分钟的值大于2则报警.小于则提示ok

````bash
ab -n 2000000 -c 20000 http://127.0.0.1/index.html
[root@db1 ~]# [ `uptime | awk -F "[ ,.]+" '{print $11}'` -gt 2 ] && echo mail || echo ok
ok
````

### 多整数比较

-a and

-o or

````bash
[root@db1 ~]# [ 10 -eq 10 -a  10 -lt 100 ] && echo 成立 || echo 不成立
成立
[root@db1 ~]# [ 10 -eq 10 -o  10 -lt 100 ] && echo 成立 || echo 不成立
成立
[root@db1 ~]# [ 10 -ne 10 -o  10 -gt 100 ] && echo 成立 || echo 不成立
不成立
[root@db1 ~]# [ 10 -ne 10 -o  10 -gt 100 -o 10 -le 20 ] && echo 成立 || echo 不成立
成立
[root@db1 ~]# [ 10 -ne 10 -o  10 -gt 100 -o 10 -le 20 ] && echo 成立 || echo 不成立
成立
````

在中括号内使用&&并且 == and || h或 ==== or

````bash
[root@db1 ~]# [[ 10 -ne 10 && 20 -gt 10 && 50 -le 60 ]] && echo 成立 || echo 不成立
不成立
[root@db1 ~]# [[ 10 -eq 10 && 20 -gt 10 && 50 -le 60 ]] && echo 成立 || echo 不成立
成立
[root@db1 ~]# [[ 10 -eq 10 || 20 -gt 10 || 50 -le 60 ]] && echo 成立 || echo 不成立
成立
[root@db1 ~]# [[ 10 -eq 10 || 20 -gt 10 && 50 -le 60 ]] && echo 成立 || echo 不成立
成立
````

### 字符串比较

字符串比较必须加双引号

````bash
[root@db1 ~]# [ "$USER" = "root" ] && echo 成立 || echo 不成立
成立
[root@db1 ~]# [ "$USER" = "czq" ] && echo 成立 || echo 不成立
不成立
[root@db1 ~]# [ "$USER" != "czq" ] && echo 成立 || echo 不成立
成立
````

-z 字符串长度为0 则为真

-n 字符串长度不为0 则为真

````bash
[root@db1 ~]# czq="xxxx"
[root@db1 ~]# [ -z $czq ]
[root@db1 ~]# echo $?
1
[root@db1 ~]# [ -n $czq ]
[root@db1 ~]# echo $?
0
````

-z案例: ##判断是否输入内容

````bash
[root@db1 scripts]# cat name.sh 
#!/bin/bash
read -p "请输入要更改的主机名: " name
[ -z $name ] && echo "请输入主机名" && exit  ##判断name输入的是否为空值.
````

案例: 判断是否为整数

方法1:

案例  expr 1 +  变量

[ $? -ne 0 ]  && exit

方法2:

````bash
[[ $test =~ ^[0-9]+$ ]]  ##判断是否以整数开头或结尾
````

````bash
[root@db1 scripts]# cat name.sh 
#!/bin/bash
read -p "请输入要更改的主机ip地址主机位: " ip
expr 1 + $ip > /dev/null 2>&1
[ $? -ne 0 ] && echo "请输入主机位" && exit
````

### 正则比对

需要使用 [[ ]]

````bash
 ##  ~波浪线表示开始匹配 ^以什么开头
[root@db1 scripts]# [[ $USER =~ ^r ]]
[root@db1 scripts]# echo $?
0
[root@db1 scripts]# [[ $USER =~ ^l ]]
[root@db1 scripts]# echo $?
1
[root@db1 scripts]# [[ ! $USER =~ ^l ]]
[root@db1 scripts]# echo $?
0

##判断是否以整数开头或结尾
[root@db1 scripts]# test=1234
[root@db1 scripts]# [[ $test =~ ^[0-9]+$ ]]
[root@db1 scripts]# echo $?
0
[root@db1 scripts]# test=1234qq
[root@db1 scripts]# [[ $test =~ ^[0-9]+$ ]]
[root@db1 scripts]# echo $?
1
````

# 笔试题

:linux添加默认路由

```bash
route add default gw 172.16.210.10
routedel default gw 172.16.210.10
ip route add 0/0 via 172.16.210.10
ip route del 0/0 via 172.16.210.10
```

 网卡添加多个ip地址

```` bash
ip addr add 172.16.210.36/24 dev ens192
for i in {10..200}; do ip add add 172.16.210.$i/24 dev ens192
````

策略路由  vpn拨号

ip ro list 查看策略路由

查看策略路由表  

````bash
[root@db1 yum.repos.d]# cat /etc/iproute2/rt_tables 
#
# reserved values
#
255	local
254	main
253	default
0	unspec
#
# local
#
#1	inr.ruhep

````

添加一个表 给这个表设置一个路由

````bash
ip route add 0/0 via 172.16.210.254 table test
ip rule add from 172.16.210.1 table test
````

脚本的方式获取主机信息

````bash
[root@db1 scripts]# cat homework.sh 
#/bin/bash
hostname=`hostnamectl  | grep hostname | awk -F " " '{print $(NF)}'`
version=`hostnamectl | grep "System" | awk -F ":" '{print $2}'`
kernel=`hostnamectl | awk -F ": " 'NR==9{print $2}'`
virt=`hostnamectl | awk -F ": " 'NR==6{print $2}'`
ip=$(hostname -i)
lo=$(ifconfig lo  | awk 'NR==2{print $2}')
exnet=$(curl -s cip.cc | grep "IP" | awk -F ": " '{print $2}')
echo "当前的主机名是: $hostname"
echo "当前系统的版本是: $version"
echo "当前系统的内核是: $kernel"
echo "当前系统的虚拟平台是: $virt"
echo "当前ip地址是: $ip"
echo "当前lo地址是: $lo"
echo "当前外网地址是: $exnet"
````



# for循环

for  i in [取值列表] 数字 字符串 命令的结果`` 序列  1 2 3 4 5 6

 do

​	 要执行什么命令

done

案例:测试1-255有多少个ip地址在线(能ping通则在线)10.0.0.1-255

````bash
[root@db scripts]#  cat ping.sh 
#!/bin/sh
for i in {1..254}
do 
  {    ##开启多线程
   ping -c1 -W1 172.16.210.$i > /dev/null 2>&1
   [ $? -eq 0 ] && echo "172.16.210.$i"
   } & ##&放在后台执行
done 
wait ##等待前面的命令执行完再开始后面的命令
echo "在线ping测试完成"
````

使用变量的方式

````bash
[root@db scripts]# cat ping.sh 
#!/bin/sh
for i in {1..254}
do 
  IP=172.16.210.$i
  {
   ping -c1 -W1 $IP > /dev/null 2>&1
   [ $? -eq 0 ] && echo "$IP"
   } &
done 
wait
echo "在线ping测试完成"
````

创建用户

````bash
[root@db scripts]# cat createuser.sh 
#!/bin/bash
read -p "请输入要创建的用户名和个数: " user int
[ -z $user ] && echo "请重新输入创建的用户名和个数" && exit
[[ ! $int =~ ^[0-9]+$ ]] && echo "个数必须是数字和非空" && exit
for i in `seq $int`
do 
  useradd $user$i > /dev/null 2>&1
  [ $? -eq 0 ] && echo "创建$user$i成功" || echo "创建$user$i失败" 
  echo "123456" | passwd --stdin $user$i > /dev/null 2>&1
done 
````

````bash
[root@db scripts]# cat createuser2.sh 
#!/bin/bash
read -p "请输入要创建的用户名: " user 
[ -z $user ] && echo "请重新输入创建的用户名和个数" && exit
read -p "请输入要创建的个数: "  int
[[ ! $int =~ ^[0-9]+$ ]] && echo "个数必须是数字和非空" && exit
xx=`seq  $int`
for i in $xx
do 
  useradd $user$i > /dev/null 2>&1
  [ $? -eq 0 ] && echo "创建$user$i成功" || echo "创建$user$i失败" 
  echo "123456" | passwd --stdin $user$i > /dev/null 2>&1
done 
````

# if判断

````bash
if  [ 你会shell ]  ##单分支
then
      echo  "加200块钱"
fi

if  [你会shell ]  ##双分支
then  
	 echo "加200块钱"
else
	echo "也加200块钱"
fi

if  [你会shell];then  ##多分支
echo "加200块钱"
elif [你会go];then
echo "加200块钱"
elif [你会k8s];then
echo "加800块钱"
else [你会nginx];then    ##可以是else结尾,也可以是elif结尾
echo "加600块钱"
fi
````

总结:

单分支:  一个条件  一个结果

双分支   一个条件  两个结果

多分支  多个条件  多个结果

案例: 判断输入的两个数字的大小 输出结果

````bash
[root@db scripts]# cat dirrnum.sh 
#!/bin/bash
read -p "请输入两个数字:" num1 num2
if [ $num1 -gt $num2 ];then
    echo "$num1 > $num1"
elif [ $num1 -lt $num2 ];then
    echo "$num1 < $num2"
else
   echo "$num1 = $num2"
fi
````

案例: 先输出一个随机数,read 猜随机数

如果你输入的小了 则提示小了 大了就提示大了 如果成功则提示 猜对了

````bash
[root@db scripts]# cat guessnum.sh 
#!/bin/sh
ran=`echo $((RANDOM%100+1))`
while true
do
 let i++
read -p "请输入你要输入的数值: " num
if [ $num -gt $ran ];then
 echo "大了"
elif [ $num -lt $ran ];then
 echo "小了"
else
   echo "猜对了,猜了$i"
   exit
fi
done
````



案例: 根据版本安装yum源

````bash
[root@db scripts]# cat version.sh 
#!/bin/bash
ver=`cat /etc/redhat-release | awk '{print $(NF-1)}'`
if [ ${ver%%.*} -eq 7 ];then  ##如果版本=7
   which wget  > /dev/null 2>&1  ##判断当前系统是否有wget命令
   [ $? -ne 0 ] && yum -y install wget ##如果没有.就安装wget
wait ##等上面语句结束
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup ##备份原有的yum源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
elif [ ${ver%%.*} -eq 8 ];then
   which wget > /dev/null 2>&1
   [ $? -ne 0 ] && yum -y install wget
wait 
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo
else
which wget > /dev/null 2>&1
[ $? -ne 0 ] && yum -y install wget
wait
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
fi
````

# sort排序

列出使用次数最多的10条命令

````bash
# uniq 去除重复行 #-c 打印每行在文本重复出现的次数
#sort 排序命令
#-n 依照数值的大小排序
#-r 相反顺序排序
#head 前10行
czq@czq-home:~/train$ sort ~/.bash_history | uniq -c | sort -nr | head
     97 ls
     61 ip a
     40 ssh s
     30 sudo pgyvpn
     28 cd
     25 ssh root@al
     21 sudo du -h --max-depth=1
     20 cat test.txt 
     17 df -hT
     11 sudo dpkg -i sogoupinyin_2.3.1.0112_amd64.deb 
````



# 菜单

````bash
[root@db scripts]# cat mum.sh 
#~/bin/bash
echo -e "\t\t######################################"
echo -e "\t\t########### 1.PHP install 5.5 ########"
echo -e "\t\t########### 2.PHP install 5.7 ########"
echo -e "\t\t########### 3.PHP install 7.1 ########"
echo -e "\t\t########### 4.PHP install 7.2 ########"
echo -e "\t\t######################################"
cat <<EOF
			       1. INSTALLPHP5.5	
			       2. INSTALLPHP5.6
			       3. INSTALLPHP7.1
			       4. INSTALLPHP7.2
EOF
````

*![image-20200408214218946](/home/czq/.config/Typora/typora-user-images/image-20200408214218946.png)*

````bash
[root@db scripts]# cat mum.sh 
#~/bin/bash
cat <<EOF
			       1. INSTALLPHP5.5	
			       2. INSTALLPHP5.6
			       3. INSTALLPHP7.1
			       4. INSTALLPHP7.2
EOF
read -p "请输入要安装的版本: " ver
[[ ! $ver =~ ^[0-9]+$  ]] && echo "请输入对应的选项" && exit
if [ $ver -eq 1 ];then
echo "install PHP 5.5................"
elif [ $ver -eq 2 ];then
echo "install PHP 5.6..............."
elif [ $ver -eq 3 ];then
echo "install PHP 7.1..............."
elif [ $ver -eq 4 ];then
echo "install PHP 7.2..........."
else 
echo "请输入对应的选项"
fi
````



# case

结构

变量 === 名字 自己起的 脚本的传参 \$1 \$2    sh case.sh  czq

case $1 in                                                 if [ \$1 == czq ];then

​          模式1)                                             echo  ok

​                    命令

​					;;

​			模式2)

​                     命令

​                      ;;

​			模式3)

​					命令

​					;;

​		     	*)

​                 echo 没有匹配到

esac

## 案例: 批量删除用户(if判断也可以)

1.删除用户的前缀 read -p "please input prefix: " pre

2.删除用户的数量 read -p "please input userss: " pre

````bash
[root@db scripts]# cat deluser.sh 
#!/bin/bash
read -p "请输入要删除的用户和个数: " user int
[ -z $user ] && echo "请重新输入要删除的用户名和个数" && exit
[[ ! $int =~ ^[0-9]+$ ]] && echo "个数必须是数字和非空" && exit
xx=`seq  $int | xargs`
for i in $xx
do 
   userdel -r $user$i 
done 
[root@db scripts]# cat case.sh 
#!/bin/sh
read -p "please input prefix: " pre
read -p "please input users: " num
for i in `seq $num`
do
   echo "$pre$i" 
done
read -p "你是否要删除这些用户[y/Y/yes | /n/N/no]: " real
case $real in 
        y|Y|yes)
for i in `seq $num`
do
          id $pre$i > /dev/null  2>&1
	  if [ $? -ne 0 ];then
	     echo  id: "$pre$i": no such user
	  else
	     userdel -r $pre$i
	    [ $? -eq 0 ] && echo id "$pre$i del is ok"
	 fi
done
	;;
	n)
	exit
	;;
	*)
	echo "请输入[y/n]"
esac
````

````bash
[root@db scripts]# cat menu.sh 
#!/bin/sh
menu(){
cat <<EOF
			1.help查看帮助
			2.显示内存使用情况
			3.显示磁盘使用
			4.显示系统负载
			5.显示登陆用户
			6.查看ip地址
			7.查看ersion
			8.退出
EOF
}
menu
while true
do 
read	-p "请输入要查看系统的编号: " num
case  $num in
	1)
	clear
	menu
	;;
	2)
	free -h
	;;
	3)
	df -hT
    	;;
	4)
	uptime
	;;
	5)
	echo $USER
        ;;
	6)
	hostname -i 
	;;
	7)
	cat /etc/redhat-release
	;;
	8)
	exit
	;;
	*)
	echo "请输入对应编号"
esac
done
````

## 控制nginx状态

````bash
[root@db scripts]# cat startnginx2.0.sh
#!/bin/bash
[ -f /etc/init.d/functions ] && . /etc/init.d/functions 
te=$1  ##全局变量
start(){
	/usr/sbin/nginx > /dev/nul 2>&1
}
stop(){
	/usr/sbin/nginx -s stop > /dev/nul 2>&1
}
restart(){
	start
	sleep 1
	stop
}
reload(){
	/usr/sbin/nginx -s reload > /dev/nul 2>&1
}
status(){
   echo	"当前nginx监听端口为:`netstat -lnptu | grep nginx  | awk  'NR==1 {print $4}'`"
   echo	"当前nginxPID为:`ps aux | grep nginx | grep master | awk '{print $2}'`"
}
cc(){
	if [ $? -eq 0 ];then
	action "nginx $te is" /bin/true
	else
	action "nginx $te is " /bin/false
	fi
}
xx=`[ $? -eq 0 ] && action "nginx $te is" /bin/true || action "nginx $te is " /bin/false`
case $1 in
	start)  ##可以按回车分割,也可以按空格分割
	start   ##
	echo $xx
	;;
	stop) stop ; echo $xx  ;; ##命令之间用;分割
	restart) restart  cc ;; ##函数之间不用
	reload) reload cc ;;
	status) status ;;
	*) echo "USAGE: $0 {start|stop|status|restart|reload}"
esac
````

## jumpserver

````bash
[root@db scripts]# cat jumpserver.sh | wc -l
31
[root@db scripts]# cat jumpserver.sh 
#!/bin/bash
wd=172.16.210.
memu(){
cat <<EOF
####################################################################################
				 1.master 172.16.210.11
				 2.node1 172.16.210.12
				 3.node2 172.16.210.13
				 4.controller 172.16.210.10
####################################################################################
EOF
}
memu
trap "echo "请不要乱按"" INT TSTP HUP ##trap 一定是要在while true的上面执行
while true
do
read -p "请输入你要连接的服务器编号: " num
case $num in
 	1)
	ssh root@${wd}11 ;;
 	2)
	ssh root@${wd}12 ;;
 	3)
	ssh root@${wd}13 ;;
 	4)
	ssh root@${wd}10 ;;
	vip) exit ;;
	*)
	read -p "请输入你要连接的服务器编号: " num ##如果判断语句判断到这条,这个read -p 不会判断case里面的语句,因为case已经结束了,然后经过while循环又从最上面的read -p开始判断.这条read -p起到的只有提示作用.
esac
done
````

# while循环

for 

for i in 取值列表

do 

​	执行命令

done



while [ 条件表达式 ]

do 



done

````bash
[root@db scripts]# cat user.txt 
czq 123
cxy 456
xzs 78
[root@db scripts]# cat while.sh 
#!/bin/bash
while read line
do
  echo $line
done<user.txt
[root@db scripts]# sh while.sh   可以看到while循环是按行打印的
czq 123
cxy 456
xzs 789
[root@db scripts]# for i in `cat user.txt` ; do echo $i ; done  ##for循环是按回车打印的
czq
123
cxy
456
xzs
789
````

while 循环创建用户并指定密码

````bash
[root@db scripts]# cat user.sh 
#!bin/bash
line
while read line
do    
	user=`echo $line | awk '{print $1}'`
	pass=`echo $line | awk '{print $2}'`
	useradd $user
	echo $pass | passwd --stdin $user
done < user.txt

````

# shell内置命令

exit 结束循环

continue 继续执行

break  跳出循环体 执行循环外的命令

````bash
[root@db scripts]# cat exit.sh 
#!/bin/bash
for i in `seq 10`
do
	useradd czq$i
	if [ $? -eq 0 ];then
	echo "create $i Success"
	else
	  exit
	fi
done
echo done.................
[root@db scripts]# sh exit.sh 
create 1 Success
create 2 Success
create 3 Success
create 4 Success
useradd：用户“czq5”已存在
[root@db scripts]# cat continue.sh 
#!/bin/bash
for i in `seq 10`
do
	useradd czq$i
	if [ $? -eq 0 ];then
	echo "create $i Success"
	else
	  continue
	fi
done
echo done.................
[root@db scripts]# sh continue.sh 
create 1 Success
create 2 Success
create 3 Success
create 4 Success
useradd：用户“czq5”已存在
create 6 Success
create 7 Success
create 8 Success
create 9 Success
create 10 Success
done.................
[root@db scripts]# cat break.sh 
#!/bin/bash
for i in `seq 10`
do
	useradd czq$i
	if [ $? -eq 0 ];then
	echo "create $i Success"
	else
	  break
	fi
done
echo done.................
[root@db scripts]# sh break.sh 
create 1 Success
create 2 Success
create 3 Success
create 4 Success
useradd：用户“czq5”已存在
done.................
````

# 函数

````bash
[root@db scripts]# cat fun.sh 
#!/bin/sh
test(){ ##比较推荐使用这种
	echo "第一种定义方式"
}
function test1 (){
	echo "第二种定义方式"
}
function test2 { ##可以用空格代替()
	echo "第三种定义方式"
}
test 
test1
test2
````

脚本之间的调用

````bash
[root@db scripts]# cat fun3.sh 
#!/bin/bash
count01(){
num=10
for i in `seq 10`
do
total=$[$i + $num]
done
echo "$total"
}
count01
[root@db scripts]# cat funset.sh 
#!/bin/sh
. /server/scripts/fun3.sh > /dev/null 2>&1 ##调用上一个脚步并将输出丢掉
echo " 调用结果`count01`"  ##调用上一个脚本的count01函数
echo "上一个脚本的变量 $num" ##调用上一个脚本的sum变量
[root@db scripts]# sh funset.sh 
 调用结果20
上一个脚本的变量 10
````

# 数组

````bash
[root@czq ~]# czq[1]=linux
[root@czq ~]# czq[2]=mysql
[root@czq ~]# czq[3]=k8s
[root@czq ~]# czq[0]=docker
[root@czq ~]# echo $czq
docker
[root@czq ~]# echo ${czq[0]}
docker
[root@czq ~]# echo ${czq[1]}
linux
[root@czq ~]# echo ${czq[2]}
mysql
[root@czq ~]# echo ${czq[3]}
k8s
[root@czq ~]# echo ${czq[@]}#获取数组元素的元素
docker linux mysql k8s
[root@czq ~]# echo ${czq[*]} 
docker linux mysql k8s
[root@czq ~]# echo ${!czq[*]} #获取数组元素的索引
0 1 2 3
[root@czq ~]# echo ${#czq[*]} #获取数组元素的总个数
4
````

取消数组定义(变量的定义也是这么做的)

```bash
[root@czq ~]# unset czq
```

第二种定义数组方式

````bash
[root@czq ~]#czq=([0]=linux [1]=Shell [2]=Mysql [3]=KVM)
[root@czq ~]# echo ${czq[1]}
Shell
[root@czq ~]# echo ${czq[0]}
linux
[root@czq ~]# echo ${czq[2]}
Mysql
[root@czq ~]# echo ${czq[3]}
KVM
[root@czq ~]# echo ${czq[*]}
linux Shell Mysql KVM
[root@czq ~]# echo ${!czq[*]}
0 1 2 3
````

**比较常用且推荐的**

````bash
[root@czq ~]# czq=(nginx shell mysql k8s)
[root@czq ~]# echo ${czq[*]}
nginx shell mysql k8s
[root@czq ~]# echo ${czq}
nginx
[root@czq ~]# echo ${!czq[*]}
0 1 2 3
````

案例

````bash
[root@czq scripts]# cat arrar.sh  ##for循环比较适合有序循环  
#!/bin/bash
IP=(        ##数组则可以作无序循环
127.0.0.1
172.16.210.2
172.16.210.3
)
for i in ${IP[*]}
do
	ping -c 1 -W1 $i >> /dev/null 2>&1
	[ $? -eq 0 ] && echo "ping $i sucess " || echo "ping $i falid"
done
````

## 关联数组

字符串作为索引

declare -A 数组名 声明是一个关联数组    (awk中不需要定义关联数组)

declare -A 查看关联数组

declare -A czq

czq=([ind]=linux [ind1]=shell [index2]=kvm [suoyiqi]=MYSQL)

````bash
[root@czq scripts]# declare -A czq
[root@czq scripts]# let czq[m]++
[root@czq scripts]# let czq[m]++
[root@czq scripts]# let czq[m]++
[root@czq scripts]# let czq[m]++
[root@czq scripts]# let czq[x]++
[root@czq scripts]# let czq[x]++
[root@czq scripts]# let czq[x]++
[root@czq scripts]# let czq[f]++
[root@czq scripts]# let czq[m]++
[root@czq scripts]# echo ${czq[m]}
5
[root@czq scripts]# echo ${czq[x]}
3
[root@czq scripts]# echo ${czq[f]}
1
[root@czq scripts]# cat sex.txt 
m
m
m
f
f
f
x
[root@czq scripts]# cat array.sh ##统计字符串出现次数
#!/bin/sh
declare -A array
while read line
do
	let array[$line]++
done<sex.txt
for i in  ${!array[*]}
do 
  echo $i=${array[$i]} 
done
[root@
````

````bash
[root@czq scripts]# cat countbash.sh ##用数组统计用户有多少个
#!/bin/bash
declare -A czq
xx=`awk -F ":" '{print $(NF)}' /etc/passwd` ##取出/etc/passwd/的最后一个值
for i in $xx
do
	let czq[$i]++
done
echo "root用户有${czq[/bin/bash]}个"
echo "普通用户有${czq[/sbin/nologin]}个" 
````

````bash
[root@czq scripts]# cat array.sh  ##第二种方法统计用户
#!/bin/sh
declare -A array
while read line
do
	xx=`echo $line | awk -F: '{print $NF}'`
	let array[$xx]++
done</etc/passwd
for i in ${!array[*]}
do
echo $i = ${array[$i]}
done
[root@czq scripts]# sh array.sh 
/sbin/nologin = 19
/bin/sync = 1
/bin/bash = 1
/sbin/shutdown = 1
/sbin/halt = 1
````

