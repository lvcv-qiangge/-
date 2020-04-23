#                              sed基本介绍

*![image-20200415175458015](/home/czq/.config/Typora/typora-user-images/image-20200415175458015.png)*

sed是一个流编辑器,非交互式的编辑器,它一次处理一行内容

处理时,把当前处理的行存存储在临时缓冲区中,称为"模式空间"

接着用sed命令处理缓冲区的内容,处理完成后,把缓冲区的内容送往屏幕.

接着处理下一行,这样不断重复,指导文件末尾.

文件内容并没有改变,除非你使用重定向存储输出.

sed 要用来自动编辑一个或多个文件;简化对文件的反复操作:编写转换程序等

# sed选项参数

-e 允许多项编辑

````bash
sed '1,3d' test.txt | sed 's/Kubernetes/k8s/' #效果一样都是删除1到3行然后把k9s换成k8s
sed -e '1,3d' -e 's/Kubernetes/k8s/' test.txt ##推荐使用第二种
````

-n 取消默认的输出

-i 直接修改对应文件

-r 支持扩展元字符

# sed内置命令参数

## a

(after) 在当前行后添加一行或多行

````bash
czq@czq-home:~$ sed -i '4a k9s' test.txt  #在第4行后添加一行k9s
````

## c

(change) 在当前行进行替换修改

````bash
czq@czq-home:~$ sed -i '6c k9s' test.txt ##把第6行改成k9s
czq@czq-home:~$ sed -i '/^kube/c  kb' test.txt  ##把kube开头的内容换成kb
czq@czq-home:~$ sed -i '/kb/cKubernetes' test.txt   ##c后面不加空格也行
````

## d

(delete) 在当前行进行删除操作

````bash
czq@czq-home:~$ sed '4d' test.txt  ##删除第4行
Concepts
Overview
What is Kubernetes?
Kubernetes
Kubernetes
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes
Object Names a
czq@czq-home:~$ sed '3,$d' test.txt  ##删除第3到最后一行
Concepts
Overview
czq@czq-home:~$ sed '/Kubernetes/d' test.txt ##正则删除,把带有Kubernetes的行删除
Concepts
Overview
Object Names and IDs
Namespaces
Labels and Selectors
Annotations
Field Selectors
````

## i 

在当前行之前插入文本

````bash
czq@czq-home:~$ sed  '4i k9s' test.txt #在第4行之前插入k9s
Concepts
Overview
What is Kubernetes?
k9s
Kubernetes
````

## p 

打印匹配的行或指定行

````bash
czq@czq-home:~$ sed -n '/Kubernetes/p' test.txt  ##打印带有 Kubernetes的行
What is Kubernetes?
Kubernetes Components
The Kubernetes API
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes Object Management
czq@czq-home:~$ sed -n '3p' test.txt  ##打印第三行
What is Kubernetes?
czq@czq-home:~$ sed -n '$p' test.txt ##打印最后一行
Field Selectors
````

## w

写文件命令

````bash
czq@czq-home:~$ sed -n '/^Kube/w text1.txt' test.txt ##n取消默认输入 然后把带有Kube开头的行写入 text1.txt文件中
czq@czq-home:~$ sed -n '2w text1.txt' test.txt ##把test.txt第二行的内容写入到text1.txt中
````

## n 

 读取下一输入行,从下一条命令进行处理

````bash
czq@czq-home:~$ sed  '/What/{n;d}' test.txt  ##匹配带有what的行,然后把带有Waht的行的下一行删除
czq@czq-home:~$ sed '/What/{n;s/Kube/kb/}' test.txt ##匹配带有What的行,然后把下一行的kube替换成kb 
````

## ! 

对所选行以外的所有行应用命令

````bash
czq@czq-home:~$ sed '3!d' test.txt ##除了第三行,其他全部删除
What is Kubernetes?
````

## h  

把模式空间里的内容重定向到暂存区

````bash
czq@czq-home:~$ cat test.txt 
Concepts
Overview
What is Kubernetes?
Kubernetes
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes
Object Names and IDs
Field Selectors
czq@czq-home:~$ sed '1h;$g' test.txt ##把第一行放进缓存区,再把缓冲区的第一行放到最后一行的模式空间,覆盖最后一行的内容
Concepts
Overview
What is Kubernetes?
Kubernetes
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes
Object Names and IDs
Concepts
````

## H

 把模式空间里的内容追加到暂存缓冲区(暂存区)

````bash
czq@czq-home:~$ sed '1H;$G' test.txt ##把第一行追加到暂存区(暂存区默认内容为空行),G把暂存区的内容追加到最后一行的模式空间
Concepts
Overview
What is Kubernetes?
Kubernetes
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes
Object Names and IDs
Concepts
Concepts

Concepts
````

## g  

取出缓存缓冲区的内容,将其复制到模式空间,覆盖该处原有内容

````bash
czq@czq-home:~$ sed '1H;$g' test.txt  ####把第一行追加到暂存区(暂存区默认内容为空行),g然后把缓存区的内容覆盖到最后一行
Concepts
Overview
What is Kubernetes?
Kubernetes
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes
Object Names and IDs
Concepts

Concepts
czq@czq-home:~$ sed '1h;2,$g' test.txt ##把第一行放进暂存区,再把暂存区域的内容放进模式空间的第2行到最后一行 
Concepts
Concepts
Concepts
Concepts
Concepts
Concepts
Concepts
````

## G 

取出缓存缓冲区的内容,将其复制到模式空间, 追加在原有内容后面

````bash
czq@czq-home:~$ sed -i '1h;$G' test.txt ##把第一行写入暂存区,然后把暂存区的内容追加到最后一行的模式空间
czq@czq-home:~$ cat test.txt 
Concepts
Overview
What is Kubernetes?
Kubernetes
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes
Object Names and IDs
Concepts
Concepts
czq@czq-home:~$ sed -r '1{h;d};$G' test.txt ##把第一行放到暂存区,再把模式空间的第一行给删除,最后再把暂存区域的内容追加到模式空间的最后一行
Overview
What is Kubernetes?
Kubernetes
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes
Object Names and IDs
Concepts
Concepts
Concepts
````

## s

替换

````bash
czq@czq-home:~/train$ sed 's/root/qg/' passwd ##替换每一行第一个出现的root为qg
qg:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
czq@czq-home:~/train$ sed -r '2s/.+/&ss/' passwd ##在第2行加一个ss &:匹配到的所有内容
````

## g

全局替换

````bash
czq@czq-home:~/train$ sed 's/root/czq/gi' passwd ##替换所有的root为czq,i:并且忽略大小写
czq:x:0:0:czq:/czq:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
````

## 后项引用

````bash
czq@czq-home:~/train$ ifconfig wlp0s20f3 | sed -n '2p' | sed -r 's#(^.*et) (.*)(net.*$)#\2#g' ##打印第二行,然后把以et以及前面的内容的net以及net后面的内容都不要,只取中间的值
192.168.1.6  
````

## 删除文件

````bash
czq@czq-home:~/train$ sed '/^#/d' passwd ##删除文件中#号开头的注释行,tab和空格无法删除
Root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
czq@czq-home:~/train$ sed '/^[ \t]*#/d' passwd ##删除文件中含有tab键的注释行
Root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
czq@czq-home:~/train$ sed '/^[ \t]*$/d' passwd ##删除无内容空行
Root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
````

## 添加注释

````bash 
czq@czq-home:~/train$ sed '2,5s/^/#/' passwd  ##给2到6行添加注释
Root:x:0:0:root:/root:/bin/bash
#daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
#bin:x:2:2:bin:/bin:/usr/sbin/nologin
#sys:x:3:3:sys:/dev:/usr/sbin/nologin
#sync:x:4:65534:sync:/bin:/bin/sync
czq@czq-home:~/train$ sed -r '2s/.+/#&/' passwd  ##给第2行添加注释,&匹配到的所有内容
Root:x:0:0:root:/root:/bin/bash
#daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
````

