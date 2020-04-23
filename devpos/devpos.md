# 																			Devpos

*![image-20200418220741759](/home/czq/.config/Typora/typora-user-images/image-20200418220741759.png)*

devpos是一种理念:所有参与到这个项目的人共同协作

开发 `development`

运维 `operations`

## Devpos能干嘛?

1.提高产品质量

2.自动化测试

3.持续集成

4.代码质量管理工具

## Devpos如何实现

````bash
设计架构规划--代码的存储-构建-测试,预生产,部署,监控
````

# Git版本控制系统

vcs `version control system`

版本控制系统是一种记录一个或若干个文件内容变化,以便将来查阅版本内容i情况的系统

记录文件的所有历史变化

随时可恢复到任何一个历史状态

多人协作开发

## 为什么需要版本控制系统?

*![image-20200419223427057](/home/czq/.config/Typora/typora-user-images/image-20200419223427057.png)*

平时写一个文件有些改动就需要再复制一个,但是有版本控制系统就不需要这样做了

*![image-20200419223548879](/home/czq/.config/Typora/typora-user-images/image-20200419223548879.png)*缺点:本地的不能多人协作

## 常见版本管理工具

### svn

svn:企业用的少

集中式的版本控制系统,只有一个中央数据库,如果数据仓库挂了或者不可访问,所有的使用者无法使用svn,无法进行提交或备份文件.

*![](/home/czq/.config/Typora/typora-user-images/image-20200419224201025.png)*





### Git

分布式的版本控制系统,在每个使用者电脑上就有一个完整的数据仓库,没有网络依然可以使用Git,当然为了习惯及团队协作,会將本地数据同步到Git服务器或者GitHub等代码仓库

*![image-20200419225201141](/home/czq/.config/Typora/typora-user-images/image-20200419225201141.png)*

# GIT的配置

1.安装git

`````bash
[root@czq ~]# yum install -y git
`````

````bash
 root@czq ~]# git config  ##获取一些git选项
--global              使用全局配置文件
--system              使用系统级配置文件
 --local               使用版本库级配置文件
````

基础配置

````bash
[root@czq ~]# git config --global user.name 'czq' ##设置用户名
[root@czq ~]# git config --global user.email 'czq@qq.com' ##设置邮箱
[root@czq ~]# git config --global color.ui true ##设置语法高亮
````

查看配置信息

````bash
[root@czq ~]# git config --list
user.name=czq
user.email=czq@qq.com
color.ui=true
[root@czq ~]# cat .gitconfig 
[user]
	name = czq
	email = czq@qq.com
[color]
	ui = true
````

git初始化

````bash
[root@czq ~]# mkdir gitdata
[root@czq ~]# cd gitdata/
[root@czq gitdata]# git init  ##把当前目录初始化为git仓库
初始化空的 Git 版本库于 /root/gitdata/.git/
[root@czq gitdata]# git status ##查看状态
# 位于分支 master
#
# 初始提交
#
无文件要提交（创建/拷贝文件并使用 "git add" 建立跟踪） 
````

*![image-20200420130033791](/home/czq/.config/Typora/typora-user-images/image-20200420130033791.png)*

````bash
[root@czq gitdata]# touch a b  c ##创建3个文件
[root@czq gitdata]# git status  ##查看状态
# 位于分支 master
#
# 初始提交
#
# 未跟踪的文件:
#   （使用 "git add <file>..." 以包含要提交的内容）
#
#	a
#	b
#	c
提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）
[root@czq gitdata]# git add a ##提交a文件到暂存区
[root@czq gitdata]# git add . ##提交当前所有文件到暂存区
[root@czq gitdata]# git rm --cached c  ##把c文件从暂存区撤回
rm 'c'
[root@czq gitdata]# git rm -rf b  ##把工作区和暂存区的b文件都删除
rm 'b'
[root@czq gitdata]# git commit -m "add newfile a"  ##提交文件到仓库 
[master（根提交） 8d80079] add newfile a
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a
````

小结:如何真正意义上通过版本控制系统 管理文件

1.工作目录必须有个代码文件

2.通过git add file 添加到暂存区域

3.通过 git commit -m "你自己输入的信息" 添加到本地仓库

`````bash
[root@czq gitdata]# git mv a.txt a ##把工作区和暂存区的文件同时改名
[root@czq gitdata]# git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	重命名：    a.txt -> a
[root@czq gitdata]# git commit -m "mv a.txt a" ##再提交
[master ce467a2] mv a.txt a
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename a.txt => a (100%)
[root@czq gitdata]# git status
# 位于分支 master
无文件要提交，干净的工作区

`````

````bash
[root@czq gitdata]# git diff   ##没任何输出,表示没有不同
[root@czq gitdata]# echo index > a ##添加一些内容到a文件
[root@czq gitdata]# git diff
diff --git a/a b/a
index e69de29..9015a7a 100644
--- a/a
+++ b/a 
@@ -0,0 +1 @@
+index ##工作目录的a多加了一行index
[root@czq gitdata]# git add a  ##把工作目录的a提交到暂存区
[root@czq gitdata]# git diff  ##此时再对比就没有什么不同了
[root@czq gitdata]# git diff --cached  ##没输入表示暂存区域和本地仓库是一样的
[root@czq gitdata]# git diff --cached
diff --git a/a b/a
index e69de29..9015a7a 100644
--- a/a
+++ b/a
@@ -0,0 +1 @@
+index ##暂存区域的a多了一行index
[root@czq gitdata]# git commit -m "add index" ##把暂存区域的a提交到仓库
[master 6a6690c] add index
 1 file changed, 1 insertion(+)
[root@czq gitdata]# git diff 
[root@czq gitdata]# git diff --cached
````

如果某个文件已经被仓库管理,想再更改此文件,直接一条命令即可

```bash
[root@czq gitdata]# echo 123 >> a
[root@czq gitdata]# git commit -am "add 123" > a 
```

git commit #相当于虚拟机的镜像,任何操作都被做了一次快照,可恢复到任何一个位置

````bash
[root@czq gitdata]# git log
commit 2de0e8c83313e7920cf29b31de27126bc4ad02ae
Author: czq <czq@qq.com>
Date:   Mon Apr 20 01:51:07 2020 -0400

    add 123  ##每次commit的描述信息

commit 6a6690c0dcffc877a675af4ab5aa688ea466c100
Author: czq <czq@qq.com>
Date:   Mon Apr 20 01:01:04 2020 -0400

    add index

commit ce467a21f0b8efa8c6bf5d57150341cf695547cb
Author: czq <czq@qq.com>
Date:   Mon Apr 20 00:43:38 2020 -0400

    mv a.txt a

commit 20d7119dc1a520399ef5e437315ea19a50ec8051
Author: czq <czq@qq.com>
Date:   Mon Apr 20 00:40:21 2020 -0400

    modifiled a a.txt
[root@czq gitdata]# git log --oneline  ##以一行显示提交的信息
2de0e8c add 123
6a6690c add index
ce467a2 mv a.txt a
20d7119 modifiled a a.txt
8d80079 add newfile a
[root@czq gitdata]# git log --oneline --decorate ##显示当前的指针指向哪里
2de0e8c (HEAD, master) add 123
6a6690c add index
ce467a2 mv a.txt a
20d7119 modifiled a a.txt
8d80079 add newfile a
[root@czq gitdata]# git log -p  ##显示每一次的变化
commit 2de0e8c83313e7920cf29b31de27126bc4ad02ae
Author: czq <czq@qq.com>
Date:   Mon Apr 20 01:51:07 2020 -0400

    add 123

diff --git a/a b/a
index 9015a7a..e69de29 100644
--- a/a
+++ b/a
@@ -1 +0,0 @@
-index

commit 6a6690c0dcffc877a675af4ab5aa688ea466c100
Author: czq <czq@qq.com>
Date:   Mon Apr 20 01:01:04 2020 -0400

    add index

diff --git a/a b/a
index e69de29..9015a7a 100644
--- a/a
+++ b/a
@@ -0,0 +1 @@
+index

commit ce467a21f0b8efa8c6bf5d57150341cf695547cb
Author: czq <czq@qq.com>
Date:   Mon Apr 20 00:43:38 2020 -0400

    mv a.txt a
[root@czq gitdata]#  git log -p --oneline ##一行显示每一次的变化和内容
2de0e8c add 123
diff --git a/a b/a
index 9015a7a..e69de29 100644
--- a/a
+++ b/a
@@ -1 +0,0 @@
-index
6a6690c add index
diff --git a/a b/a
index e69de29..9015a7a 100644
--- a/a
+++ b/a
@@ -0,0 +1 @@
+index
ce467a2 mv a.txt a
diff --git a/a b/a
new file mode 100644
index 0000000..e69de29
diff --git a/a.txt b/a.txt
deleted file mode 100644
index e69de29..0000000
20d7119 modifiled a a.txt
diff --git a/a b/a
[root@czq gitdata]# git log -1 -p  ##查看最近一次commit的详细改动
commit 2de0e8c83313e7920cf29b31de27126bc4ad02ae
Author: czq <czq@qq.com>
Date:   Mon Apr 20 01:51:07 2020 -0400

    add 123

diff --git a/a b/a
index 9015a7a..e69de29 100644
--- a/a
+++ b/a
@@ -1 +0,0 @@
-index
````

回滚数据到某一提交

````bash
[root@czq gitdata]# git reset --hard 8d80079 
````

查看历史提交

```bash
[root@czq gitdata]# git reflog
2de0e8c HEAD@{0}: reset: moving to 2de0e8c
8d80079 HEAD@{1}: reset: moving to 8d80079
2de0e8c HEAD@{2}: commit: add 123
6a6690c HEAD@{3}: commit: add index
ce467a2 HEAD@{4}: commit: mv a.txt a
20d7119 HEAD@{5}: commit: modifiled a a.txt
8d80079 HEAD@{6}: commit (initial): add newfile a
```

# 分支

*![image-20200421144322232](/home/czq/.config/Typora/typora-user-images/image-20200421144322232.png)*

一般在实际的项目开发中,我们要尽量保证master分支是非常稳定的,仅用于发布新版本,平时不要随便直接修改里面的数据文件,而工作的时候则可以新建不同的工作分支,等到工作完成后合并到master分支上面,所以团队的合作分支看起来会像上面图那样.

````bash
[root@czq gitdata]# git branch
* master
  testing
[root@czq gitdata]# touch aaa bbb ccc
[root@czq gitdata]# git add aaa
[root@czq gitdata]# git commit -m "add aaa"
[master c8a0ab1] add aaa
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 aaa
[root@czq gitdata]# git add bbb
[root@czq gitdata]# git commit -m "add bbb"
[master 63b3277] add bbb
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 bbb
[root@czq gitdata]# git add ccc
[root@czq gitdata]# git commit -m "add ccc"
[master ac801f6] add ccc
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 ccc
[root@czq gitdata]# git branch testing  ##创建分支
[root@czq gitdata]# git branch  ##查看分支
* master  ##*代表当前在那个分支上
  testing
[root@czq gitdata]# git checkout testing  ##切换分支
切换到分支 'testing'
[root@czq gitdata]# git branch 
  master
* testing[root@czq gitdata]# git branch -d testing ##删除分支
已删除分支 testing（曾为 6a6690c）。
[root@czq gitdata]# git checkout -b testing ##创建并切换分支
切换到一个新分支 'testing'
[root@czq gitdata]# git branch
  master
* testing
[root@czq gitdata]# ll
总用量 4
-rw-r--r--. 1 root root 6 4月  20 02:15 a
-rw-r--r--. 1 root root 0 4月  21 21:08 aaa
-rw-r--r--. 1 root root 0 4月  21 21:08 bbb
-rw-r--r--. 1 root root 0 4月  21 21:08 ccc
[root@czq gitdata]# git merge testing  ##合并分支代码冲突
自动合并 aaa
冲突（内容）：合并冲突于 aaa
自动合并失败，修正冲突然后提交修正的结果。
[root@czq gitdata]# cat aaa
<<<<<<< HEAD
master
=======
testing
>>>>>>> testing
[root@czq gitdata]# git status 
# 位于分支 master
# 您有尚未合并的路径。
#   （解决冲突并运行 "git commit"）
#
# 未合并的路径：
#   （使用 "git add <file>..." 标记解决方案）
#
#	双方修改：     aaa
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
[root@czq gitdata]# vim aaa  ##把<<<<<<< HEAD=======>>>>>>> testing 给删除掉
[root@czq gitdata]# cat aaa
master   ##保留你想要留下的代码
testing
[root@czq gitdata]# git commit -am "merge testting"
[master 64329aa] merge testting
````

# git标签使用

````bash
[root@czq gitdata]# git tag -a v1.0 13554e6 -m "tag v1.0 add"  ##给提交打标签
[root@czq gitdata]# git tag 
v1.0
[root@czq gitdata]# git show v1.0  ##查看v1.0的内容
tag v1.0
Tagger: czq <czq@qq.com>
Date:   Tue Apr 21 22:43:51 2020 -0400

tag v1.0 add

commit 13554e6aec9dcd402b3694353690ba59dacd3272
Author: czq <czq@qq.com>
Date:   Tue Apr 21 21:34:51 2020 -0400

    add netfile test-add

diff --git a/test-add b/test-add
new file mode 100644
index 0000000..e69de29
[root@czq gitdata]# git reset --hard v1.0  ##切换到v1.0
HEAD 现在位于 13554e6 add netfile test-add
[root@czq gitdata]# git tag -a "v2.0" -m "xxxx" ##给当前所在的提交打标签 ##-a 指定标签名字 -m 描述信息
[root@czq gitdata]# git tag ##查看所有标签
v1.0
v2.0
[root@czq gitdata]# git tag -d v1.0  ##删除标签
已删除 tag 'v1.0'（曾为 b6fcdeb
````

# github

github是一个git版本库的托管服务,github可以托管各种git版本仓库,还拥有了更美观的web界面,你的代码可以被任何人克隆,使得开发者为开源项目贡献代码变得更加容易

````bash
[root@czq gitdata]# git remote add origin git@github.com:lvcv-qiangge/git_data.git ##远程链接github库
[root@czq gitdata]# git remote 查看当前的远程库
origin
[root@czq gitdata]# git push -u origin master #推送master分支到到远程库
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint is MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.  ##没有上传密钥开始报错
[root@czq gitdata]# ssh-keygen ##生成密钥
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:jQcjXgoeGmIS7PyVG1LeYFI+ZL38lUuakY8SvuSGUL4 root@czq
The key's randomart image is:
+---[RSA 2048]----+
|o   .+.          |
| o .++ .         |
|=.. Bo* = . .    |
|o+ = X.O B +     |
|  o * * S @ .    |
|   o o + * o     |
|    . = o        |
|     E +         |
|      .          |
+----[SHA256]-----+
[root@czq gitdata]# cat ~/.ssh/
id_rsa       id_rsa.pub   known_hosts  
[root@czq gitdata]# cat ~/.ssh/id_rsa.pub ##把公钥复制到gitbuh上
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCz3MHCAac2h9S+ALwbydUiT1dNVbok8VR1qW1thGQaFB7kF2/Q+MOfIP8oeZ8WO5Uq0T9s918uwzt10/JB2S08GUtSGGV+Cx3Uhp7L3Hv/kQ7+GRdLRKSi8U/to6KZmr2f0mgJGbVEl5rdBlWNOT9qrx/9NjYgbxI4L6t45rr8ka79E+T7dNOR5jZyb8PKEHQFD3TGtJ2ZSehesAcJbx3PmBe1kZfB32u4e2XjnS19rHRGX/b4GxGyqm8+beGe5Q5ebziJfhlzsHCfICwyM+vpb6o4G1Wc2OihO4q9b+mCYgX0bHVRvAmMB6AydDtO2tHtXPHqusMy3i3ahfNdnC5Z root@czq
[root@czq gitdata]# git push -u origin master ##开始推送
Counting objects: 30, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (22/22), done.
Writing objects: 100% (30/30), 2.32 KiB | 0 bytes/s, done.
Total 30 (delta 9), reused 0 (delta 0)
remote: Resolving deltas: 100% (9/9), done.
To git@github.com:lvcv-qiangge/git_data.git
 * [new branch]      master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
[root@czq gitdata]# cd /tmp/
[root@czq tmp]# git clone git@github.com:lvcv-qiangge/git_data.git ##把远程库的代码复制到本地
正克隆到 'git_data'...
Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
remote: Enumerating objects: 30, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 30 (delta 9), reused 30 (delta 9), pack-reused 0
接收对象中: 100% (30/30), done.
处理 delta 中: 100% (9/9), done.
[root@czq tmp]# ls ##查看文件
git_data
systemd-private-49ee4b8488554c1a97f58ee5c1798a82-chronyd.service-bQlO1k
systemd-private-d9c277825fc5463fb3090a42a4b08446-vmtoolsd.service-k7A3si
yum_save_tx.2020-04-19.22-51.ZcEllD.yumtx
yum_save_tx.2020-04-21.22-12.hj5zer.yumtx
[root@czq tmp]# cd git_data/
[root@czq git_data]# ls
a  aaa  bbb  ccc  master-e  test-add
````

# gitab

gitlab是一个用于仓库管理系统的开源项目,使用git作为代码管理工具,并在此基础上搭建起来的web服务,可以通过web界面进行访问公开的或者私人项目,它拥有与github类似的功能,能够浏览源代码,管理缺陷和注释.可以管理团队对仓库的访问,他非常易与浏览提交过的版本并提供一个文件历史库,团队成员可以利用内置的简单聊天程序(wall)进行交流,它还提供一个代码片段收集功能可以轻松实现代码复用

## gitlab的安装

1.关闭防火墙和selinux

````bash
[root@czq ~]# systemctl stop firewalld
[root@czq ~]# systemctl disable firewalld
[root@czq ~]# setenforce 0
[root@czq ~]# sed -i.bak '7s/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux 
````

2.安装相关依赖软件

````bash
[root@czq ~]# yum install -y curl policycoreutils-python openssh-server
````

3.安装gitlab

````bash
[root@czq ~]# rpm -ivh gitlab-ce-11.6.2-ce.0.el7.x86_64.rpm
````

4.修改web访问地址

````bash
vim /etc/gitlab/gitlab.rb 
external_url 'http://192.168.26.20' ##改成本地的ip
````

5.初始化gitlab

````bash
[root@czq ~]# gitlab-ctl configure
````

![image-20200422192329996](/home/czq/.config/Typora/typora-user-images/image-20200422192329996.png)

6.web访问

![image-20200422191042226](/home/czq/.config/Typora/typora-user-images/image-20200422191042226.png)

## gitlab基础配置

### 1.美化界面

![image-20200422192958332](/home/czq/.config/Typora/typora-user-images/image-20200422192958332.png)

点击`Appearance`

![image-20200422193027846](/home/czq/.config/Typora/typora-user-images/image-20200422193027846.png)

![image-20200422193327918](/home/czq/.config/Typora/typora-user-images/image-20200422193327918.png)

### 2.配置库

1.创建组

![image-20200422200030328](/home/czq/.config/Typora/typora-user-images/image-20200422200030328.png)

![image-20200422200252390](/home/czq/.config/Typora/typora-user-images/image-20200422200252390.png)

2.创建项目

![image-20200422200440820](/home/czq/.config/Typora/typora-user-images/image-20200422200440820.png)

![image-20200422200617139](/home/czq/.config/Typora/typora-user-images/image-20200422200617139.png)

3.上传公钥

![image-20200422200821932](/home/czq/.config/Typora/typora-user-images/image-20200422200821932.png) ![image-20200422200855149](/home/czq/.config/Typora/typora-user-images/image-20200422200855149.png)

![image-20200422200948705](/home/czq/.config/Typora/typora-user-images/image-20200422200948705.png)

把服务器的`~/.ssh/id.rsa.pub`粘贴到gitlab

![image-20200422201555521](/home/czq/.config/Typora/typora-user-images/image-20200422201555521.png)

4.服务器添加远程库

`````bash
[root@czq gitdata]# git remote add origin git@192.168.26.20:test/git_date.git
[root@czq gitdata]# git push origin master ##把master分支推送到远程库
The authenticity of host '192.168.26.20 (192.168.26.20)' can't be established.
ECDSA key fingerprint is SHA256:EPxGNl8K0PS9Jl7FqgpsVWjeb4BRoDXIzrjh4OXhHuc.
ECDSA key fingerprint is MD5:39:a4:72:a6:66:a5:b8:10:e0:51:86:ea:8f:ef:9f:7f.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.26.20' (ECDSA) to the list of known hosts.
Counting objects: 30, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (22/22), done.
Writing objects: 100% (30/30), 2.32 KiB | 0 bytes/s, done.
Total 30 (delta 9), reused 0 (delta 0)
To git@192.168.26.20:test/git_date.git
 * [new branch]      master -> master
`````

此时gitlab就已经收到代码了

![image-20200422201915300](/home/czq/.config/Typora/typora-user-images/image-20200422201915300.png)

尝试有新需求,需要推送代码 

````bash
[root@czq gitdata]# touch test.tx
[root@czq gitdata]# git add .
[root@czq gitdata]# git commit -m "newfile test.txt"
[master dc522bf] newfile test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.tx
[root@czq gitdata]# git push -u origin master 
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 244 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
To git@192.168.26.20:test/git_date.git
   64329aa..dc522bf  master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
````

### 3.创建用户给开发

![image-20200422202606580](/home/czq/.config/Typora/typora-user-images/image-20200422202606580.png)

![image-20200422202735032](/home/czq/.config/Typora/typora-user-images/image-20200422202735032.png)

其他保持默认值,再点击`create user`



点击`edit` 设置密码

![image-20200422202855012](/home/czq/.config/Typora/typora-user-images/image-20200422202855012.png)

![image-20200422202952586](/home/czq/.config/Typora/typora-user-images/image-20200422202952586.png)

其他保持默认值,点击![image-20200422203010363](/home/czq/.config/Typora/typora-user-images/image-20200422203010363.png)



换个浏览器登录czq用户

![image-20200422203259210](/home/czq/.config/Typora/typora-user-images/image-20200422203259210.png)

初始化密码

![image-20200422203353128](/home/czq/.config/Typora/typora-user-images/image-20200422203353128.png)

点击![image-20200422203417675](/home/czq/.config/Typora/typora-user-images/image-20200422203417675.png)

再重新登录

![image-20200422203502904](/home/czq/.config/Typora/typora-user-images/image-20200422203502904.png)



回到管理员浏览器

点击test组

![image-20200422203743419](/home/czq/.config/Typora/typora-user-images/image-20200422203743419.png)

 

把czq加入到组里

![image-20200422203853215](/home/czq/.config/Typora/typora-user-images/image-20200422203853215.png) 

再给个开发者权限

![image-20200422203952729](/home/czq/.config/Typora/typora-user-images/image-20200422203952729.png) 

再点击`ADD uers to group`

![image-20200422204054316](/home/czq/.config/Typora/typora-user-images/image-20200422204054316.png) 

![image-20200422204137638](/home/czq/.config/Typora/typora-user-images/image-20200422204137638.png) 

回到用户浏览器刷新查看项目

![image-20200422204241311](/home/czq/.config/Typora/typora-user-images/image-20200422204241311.png) 

开发把自己服务器的密钥上传到gitlab里

![image-20200422204559513](/home/czq/.config/Typora/typora-user-images/image-20200422204559513.png) 

填入密钥

![image-20200422204723383](/home/czq/.config/Typora/typora-user-images/image-20200422204723383.png) 

复制clone码

![image-20200422205024168](/home/czq/.config/Typora/typora-user-images/image-20200422205024168.png) 

````bash
[root@czq cc]# git clone git@192.168.26.20:test/git_date.git ##开发把项目拉到自己服务器
正克隆到 'git_date'...
remote: Enumerating objects: 32, done.
remote: Counting objects: 100% (32/32), done.
remote: Compressing objects: 100% (24/24), done.
remote: Total 32 (delta 10), reused 0 (delta 0)
接收对象中: 100% (32/32), done.
处理 delta 中: 100% (10/10), done.
[root@czq cc]# cd git_date/ ##进入项目目录
[root@czq git_date]# touch newfile  ##模拟一个新代码
[root@czq git_date]# git add .  ##添加到暂存区
[root@czq git_date]# git commit -m "add newfile" ##提交到本地库
[master 725a37d] add newfile
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 newfile
[root@czq git_date]# git push -u origin master ##开始推送  
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 229 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
To git@192.168.26.20:test/git_date.git
   dc522bf..725a37d  master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。 ##生产环境不建议这么做,因为master分支是主分支,代码直接发送到主分支不太严谨,所以推荐先创建一个分支
[root@czq git_date]# git checkout -b czq ##创建并切换分支
切换到一个新分支 'czq'
[root@czq git_date]# git push -u origin czq  ##把czq分支推上去
Total 0 (delta 0), reused 0 (delta 0)
remote: 
remote: To create a merge request for czq, visit:
remote:   http://192.168.26.20/test/git_date/merge_requests/new?merge_request%5Bsource_branch%5D=czq
remote: 
To git@192.168.26.20:test/git_date.git
 * [new branch]      czq -> czq
分支 czq 设置为跟踪来自 origin 的远程分支 czq。
````

### 4.master分支保护

1.点项目

2.点击`settings`

3.点击`repository`

4.点击`Protected Branches`旁边的expand

![image-20200422211438979](/home/czq/.config/Typora/typora-user-images/image-20200422211438979.png)

![image-20200422211931343](/home/czq/.config/Typora/typora-user-images/image-20200422211931343.png) 

点击`protect` 就出现了保护的分支

![image-20200422212009083](/home/czq/.config/Typora/typora-user-images/image-20200422212009083.png) 

再尝试推送

````bash
[root@czq git_date]# touch 1234.txt
[root@czq git_date]# git add .
[root@czq git_date]# git commit -m "add 1234.txt"
[master e0a7bcb] add 1234.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 1234.txt
[root@czq git_date]# git push -u origin master 
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 229 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: GitLab: You are not allowed to push code to protected branches on this project.
To git@192.168.26.20:test/git_date.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: 无法推送一些引用到 'git@192.168.26.20:test/git_date.git' ##发现无法推送受保护的分支
````

### 5.代码合并流程

5.1在开发的分支上使用`git pull`查看是否最新

````bash
[root@czq git_date]# git branch 
  czq
* master
[root@czq git_date]# git pull 
Already up-to-date.
````

5.2回到主干获取最新

````bash
[root@czq cc]# cd ~/gitdata/
[root@czq gitdata]# git pull 
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
来自 192.168.26.20:test/git_date
   dc522bf..5fd851f  master     -> origin/master
 * [新分支]          czq        -> origin/czq
更新 dc522bf..5fd851f
Fast-forward
 123.txt | 0
 newfile | 0
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 123.txt
 create mode 100644 newfile
````

5.3回到开发分支

````bash
[root@czq git_date]# git branch -d czq  ##把主分支干掉
已删除分支 czq（曾为 725a37d）。
[root@czq git_date]# git checkout -b czq ##创建新分支
切换到一个新分支 'czq'
````

5.4推送代码

````bash
[root@czq git_date]# touch 456.txt
[root@czq git_date]# git add .
[root@czq git_date]# git commit -m "add 456.txt"
[dev f9343cc] add 456.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 456.txt
 [root@czq git_date]# git push -u origin master 
分支 master 设置为跟踪来自 origin 的远程分支 master。
Everything up-to-date
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 229 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: GitLab: You are not allowed to push code to protected branches on this project.
To git@192.168.26.20:test/git_date.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: 无法推送一些引用到 'git@192.168.26.20:test/git_date.git' ##不让推送
[root@czq git_date]# git push -u origin dev ##可以先推送到分支
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 219 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: 
remote: To create a merge request for dev, visit:
remote:   http://192.168.26.20/test/git_date/merge_requests/new?merge_request%5Bsource_branch%5D=dev
remote: 
To git@192.168.26.20:test/git_date.git
 * [new branch]      dev -> dev
分支 dev 设置为跟踪来自 origin 的远程分支 dev。
````

回到开发的gitlab 点击`new merge request`

![image-20200422222846609](/home/czq/.config/Typora/typora-user-images/image-20200422222846609.png) 填好分支和想要合并的分支就点击`compare branches and continue`



![image-20200422223003049](/home/czq/.config/Typora/typora-user-images/image-20200422223003049.png)

填写好标题和提示,其他保持默认值

![image-20200422223235581](/home/czq/.config/Typora/typora-user-images/image-20200422223235581.png) 

往下来,点击![image-20200422223308766](/home/czq/.config/Typora/typora-user-images/image-20200422223308766.png) 

由于设置了权限,开发自己是不能点merge的

![image-20200422223501424](/home/czq/.config/Typora/typora-user-images/image-20200422223501424.png) 

回到管理员的gitlab上点击项目的`merge requests`,在点进请求里面

![image-20200422224021581](/home/czq/.config/Typora/typora-user-images/image-20200422224021581.png)

点击`merge immediately` 

![image-20200422225106948](/home/czq/.config/Typora/typora-user-images/image-20200422225106948.png) 

回到master主干查看

![image-20200422225206639](/home/czq/.config/Typora/typora-user-images/image-20200422225206639.png) 

![image-20200422225214806](/home/czq/.config/Typora/typora-user-images/image-20200422225214806.png)成功合并!

注意:如果写完分支并且和master主干合并过,然后master已经有了新变化,此时不要继续在分支上写代码再提交了,因为此时的主干是比分支新的,会出现推送不了的情况,一定要把分支删除掉,再在新master上创建分支,再写代码再提交,如果主干仓库的代码不是最新的可使用`git pull` 拉取最新的代码,拉下来之后再进行推送.

````bash
[root@czq gitdata]# git pull
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (5/5), done.
来自 192.168.26.20:test/git_date
   cce3901..6a0524a  master     -> origin/master
   f5bd1f6..5663d89  czq        -> origin/czq
更新 cce3901..6a0524a
Fast-forward
 111.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 111.txt
````



# jekins

jenkins是一个开源项目,是基于开发的一种持续集成工具,用于监控持续重复的工作,皆在提供一个可易用的软件平台,使软件的持续集成变成可能

## 安装

````bash
[root@localhost ~]# rpm -ivh jdk-8u191-linux-x64.rpm ##安装java环境
[root@jenkins ~]# rpm -ivh jenkins-2.190.1-1.1.noarch.rpm  ##安装jenkins
````

![image-20200423150551517](/home/czq/.config/Typora/typora-user-images/image-20200423150551517.png) 

## 启动

````bash
[root@jenkins ~]# systemctl start jenkins
````

## 访问

端口8080

![](/home/czq/.config/Typora/typora-user-images/image-20200423150752869.png)

输入密钥

![image-20200423150956350](/home/czq/.config/Typora/typora-user-images/image-20200423150956350.png) 



````bash
[root@jenkins ~]# cat /var/lib/jenkins/secrets/initialAdminPassword 
e8c64a095d804b53b1224711e255241d
````

关闭外网 

![](/home/czq/.config/Typora/typora-user-images/image-20200423151417136.png) 

点击跳过插件安装

![image-20200423151450571](/home/czq/.config/Typora/typora-user-images/image-20200423151450571.png) 

## 更改初始密码

### 1.点击`admin` 

![image-20200423151922629](/home/czq/.config/Typora/typora-user-images/image-20200423151922629.png) 

### 2.点击`Configure` 

![](/home/czq/.config/Typora/typora-user-images/image-20200423152036915.png) 

### 3.修改密码

![image-20200423152143266](/home/czq/.config/Typora/typora-user-images/image-20200423152143266.png) 

### 4.点击`save`

![image-20200423152213176](/home/czq/.config/Typora/typora-user-images/image-20200423152213176.png) 

## 安装插件

使用本地上传的方式

````bash
scp -r jenkins\ plugins/ root@192.168.26.30:~
[root@jenkins ~]# cp jenkins\ plugins/* /var/lib/jenkins/plugins/  ##把插件都移动到/var/lib/jenkins/plugins/下
[root@jenkins plugins]# systemctl restart jenkins ##重启jenkins
````

刷新网页就能看见插件上传成功了

![image-20200423155858922](/home/czq/.config/Typora/typora-user-images/image-20200423155858922.png) 

## 创建项目

### 1.点击`New iteam` 

![image-20200423161053379](/home/czq/.config/Typora/typora-user-images/image-20200423161053379.png) 

### 2.创建一个自由风格的项目,点击`ok` 

![image-20200423161219066](/home/czq/.config/Typora/typora-user-images/image-20200423161219066.png) 

![image-20200423162643556](/home/czq/.config/Typora/typora-user-images/image-20200423162643556.png) 

设置构建命令

![image-20200423163016158](/home/czq/.config/Typora/typora-user-images/image-20200423163016158.png) 

选择shell命令

![image-20200423163218756](/home/czq/.config/Typora/typora-user-images/image-20200423163218756.png) 

查看一下当前路径

![image-20200423163249053](/home/czq/.config/Typora/typora-user-images/image-20200423163249053.png) 

点击`save`

![image-20200423163347169](/home/czq/.config/Typora/typora-user-images/image-20200423163347169.png) 

点击`build now`

![image-20200423163430068](/home/czq/.config/Typora/typora-user-images/image-20200423163430068.png) 

蓝色表示为成功

![image-20200423163503597](/home/czq/.config/Typora/typora-user-images/image-20200423163503597.png) 

点击控制台输出

![image-20200423163616667](/home/czq/.config/Typora/typora-user-images/image-20200423163616667.png) 

在那个那一个项目下执行shell命令,说明当前的路径就在本项目目录下

![image-20200423163649875](/home/czq/.config/Typora/typora-user-images/image-20200423163649875.png)  

测试项目

点击配置

![image-20200423164035478](/home/czq/.config/Typora/typora-user-images/image-20200423164035478.png) 

![image-20200423164123829](/home/czq/.config/Typora/typora-user-images/image-20200423164123829.png) 

设置好命令再保存

出去再点击![image-20200423164218612](/home/czq/.config/Typora/typora-user-images/image-20200423164218612.png) 

![image-20200423164307601](/home/czq/.config/Typora/typora-user-images/image-20200423164307601.png) 

回到shell界面验证

````bash
[root@jenkins freestyle-job]# ls
test.txt
````

## 项目实践

1.gitlab创建一个项目

![image-20200423174201444](/home/czq/.config/Typora/typora-user-images/image-20200423174201444.png) 

点击`create project` 

![image-20200423174421857](/home/czq/.config/Typora/typora-user-images/image-20200423174421857.png) 

复制这段命令

![image-20200423174615738](/home/czq/.config/Typora/typora-user-images/image-20200423174615738.png) 

粘贴到项目目录里,并把项目推送到仓库里

````bahs
[root@czq dzp]# git init
初始化空的 Git 版本库于 /root/dzp/.git/
[root@czq dzp]# git remote add origin git@192.168.26.20:test/dzp.git
[root@czq dzp]# git add .
[root@czq dzp]# git commit -m "Initial commit"
[master（根提交） dcc74d6] Initial commit
 1 file changed, 1 insertion(+)
 create mode 160000 monitor
[root@czq dzp]# git push -u origin master
Counting objects: 2, done.
Writing objects: 100% (2/2), 193 bytes | 0 bytes/s, done.
Total 2 (delta 0), reused 0 (delta 0)
To git@192.168.26.20:test/dzp.git
 * [new branch]      master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
````

