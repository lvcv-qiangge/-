# awk

1. awk是一种编成语言,用与在'linux/unix'下对文本和数据进行处理
2. awk数据可以来自标准输入,一个或多个文件,或其他命令的输出
3. awk通常是配合脚本使用,是一个强大的文本处理工具

### awk的处理数据的方式

1. 进行逐行扫描,从第一行到最后一行
2. 寻找匹配的特定模式的行,在行上进行操作
3. 如果没有处理动作,则把匹配的行显示到标准输出
4. 如果没有指定模式,则所有被操作的行都被处理

![image-20200411083624106](/home/czq/.config/Typora/typora-user-images/image-20200411083624106.png)

### 选项

-F 第年一输入字段分割符号, 默认的分割符, 空格或tab键



命令 command

行处理前    行处理    行处理后

BEGING{}     {}    END{}



#### BEGIN发生在读文件之前

````bash
[root@czq ~]# awk 'BEGIN{print 1/2}'
0.5
````

#### BEGIN在行处理前,修改字段分割符

````bash
[root@czq ~]# awk 'BEGIN{FS=":"} {print $1}' /etc/passwd
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
operator
games
ftp
````

#### BEGIN在行处理前,修改字段输入和输出分割符

````bash
[root@czq ~]# awk 'BEGIN{FS=":";OFS="----"} {print $1,$2}' /etc/passwd
root----x
bin----x
daemon----x
adm----x
lp----x
sync----x
shutdown----x
halt----x
[root@czq ~]# awk 'BEGIN{print 1/2} {print "ok"} END {print "game over"}' /etc/passwd
0.5   ##先打印一个0.5 
ok   ## 然后每有一行数据就打印一个ok
ok   
ok           
ok
ok
ok
game over  ##最后再打印一个game over

````

#### awk的匹配

awk '/匹配内容/' 文件名

````bash
[root@czq ~]# awk '/root/' /etc/passwd ##匹配/etc/passwd 带有root的行
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin

````

#### awk的处理动作

awk '{动作}' 文件名

````bash
[root@czq ~]# awk -F ":" '{print $1}'  /etc/passwd
root
bin
daemon
adm
lp
sync
````

#### awk的匹配+处理动作

````bash
[root@czq ~]# awk -F ":" '/root/{print $1}' /etc/passwd 
root   ##先匹配带有root的行 再打印第一列
operator
````

#### awk加判断条件

````bash
[root@db ~]# df -m | awk '/\/$/ { if ($3<7000) print $2}'
35943
[root@db ~]# df -m | awk '/\/$/ { if ($3<7000) print $2}'
[root@db ~]# awk -F: '{if($3>1000){print $3} else  print$1}' /etc/passwd
root
bin     ##如果(也就是第三列)uid大于1000就打印第三列 不大于就打印第一列
daemon
adm 
lp
sync
dbus
polkitd
postfix
sshd
mysql
apache
1001
1002
1003
1004
[root@db scripts]# awk -F : '{ if($3==0){print $1 " is admin"}}' /etc/passwd
root is admin  ##如果第三列的uid=0 就打印 并且后面跟上admin
[root@db scripts]# awk -F: '{ if($3>0 && $3<1000){i++}} END {print i}' /etc/passwd  
20  ##统计普通用户的个数
````

#### awk以空格冒号tab作为字段分割

````bash
[root@db ~]# awk -F '[ :\t]' '{print NF, $NF}' /etc/passwd
7 /bin/bash
7 /sbin/nologin
7 /sbin/nologin
7 /sbin/nologin
7 /sbin/nologin
7 /bin/sync
````

#### awk使用'OFS'指定输出分割符

逗号映射为OFS, 初始情况下OFS变量是空格

````bash
[root@db ~]# awk 'BEGIN{FS=":"; OFS="+++"} /^root/{print $1,$2}' /etc/passwd
root+++x  ##第年一开始定义分格符单位是: 然后指定分割符为+++ ,匹配以root开头的行,并打印1 2列 以+++作为它们的分割符
````

#### awk使用print格式化输出

````bash
[root@db ~]# date | awk '{print $2,"四月份""\n",$NF,"今年"}'
04月 四月份
 CST 今年
````

#### awk使用while循环

````bash
[root@czq ~]# awk 'BEGIN{ i=1; while(i<=5){print i;i++}}'
1
2
3
4
5
[root@czq ~]# awk -F : '{i=1; while(i<=NF){print $i; i++ }}' /etc/passwd
[root@czq ~]# awk -F : '{i=1; while(i<=NF){print $i; i++ }}' /etc/passwd
root
x     ##把/etc/passwd每一行每一列的信息都输出出来
0
0
root
/root
/bin/bash
bin
x
1
1
bin
/bin
/sbin/nologin
daemon
x
2
2
daemon
/sbin
````

#### awk使用for循环

````bash
[root@czq ~]# awk 'BEGIN{for(i=1;i<=5;i++){print i}}'
1
2
3
4
5
[root@czq ~]# awk -F: '{ for(i=1;i<=3;i++) {print $0}}' /etc/passwd
root:x:0:0:root:/root:/bin/bash   ##$0打印所有列  循环了3次 等于每行打印3行
root:x:0:0:root:/root:/bin/bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
bin:x:1:1:bin:/bin:/sbin/nologin
bin:x:1:1:bin:/bin:/sbin/nologin
````

