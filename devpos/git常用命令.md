git常用命令

1.git init 初始化仓库 

````bash
[root@czq gitdata]# git init  ##把当前目录初始化为git仓库
初始化空的 Git 版本库于 /root/gitdata/.git/
````

2.git status 查看当前库的状态

````bash
[root@czq gitdata]# git status ##查看状态
# 位于分支 master
#
# 初始提交
#
无文件要提交（创建/拷贝文件并使用 "git add" 建立跟踪） 
````

3.git add file 添加文件到暂存区

````bash
[root@czq gitdata]# git add a ##提交a文件到暂存区
````

4.git add .  或者git add * 添加当前所有的文件到暂存区

````bash
[root@czq gitdata]# ls
a  b  c
[root@czq gitdata]# git add .
[root@czq gitdata]# git status
# 位于分支 master
#
# 初始提交
#
# 要提交的变更：
#   （使用 "git rm --cached <file>..." 撤出暂存区）
#
#	新文件：    a
#	新文件：    b
#	新文件：    c
#
````

5.git rm --cached 把文件从暂存区撤回到工作区

````bash
[root@czq gitdata]# git rm --cached c
rm 'c'
````

6.git rm -rf  把暂存区和工作区的文件同时删除

````bash
[root@czq gitdata]# git rm -rf b
rm 'b'
````

7.git commit -m 从暂存区提交到本地仓库

````bash
[root@czq gitdata]# git commit -m "add newfile a"  ##引号内的是提示信息
[master（根提交） 8d80079] add newfile a
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a
````

8.git mv oldfile-name newfile-name  直接更改文件名 更改完再commit提交

````bash
[root@czq gitdata]# git mv a.txt a 
[root@czq gitdata]# git commit -m "mv a.txt a"
[master ce467a2] mv a.txt a
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename a.txt => a (100%)
````

9.git diff 比对工作目录和暂存区的不同

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
````

10.git diff  --cached 比对暂存区和本地仓库

````bash
[root@czq gitdata]# git diff --cached ##没输出表示暂存区域和本地仓库是一样的
[root@czq gitdata]# git add a  
[root@czq gitdata]# git diff --cached
diff --git a/a b/a
index e69de29..9015a7a 100644
--- a/a
+++ b/a
@@ -0,0 +1 @@
+index ##暂存区域的a多了一行index
````

11.git commit -am "" 把工作目录的文件直接提交更改到已经存在到仓库的文件

````bash
[root@czq gitdata]# echo 123 >> a
[root@czq gitdata]# git commit -am "add 123" > a
````

12.git log 查看历史提交的信息

-p查看具体的改动

-1查看最近一次

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
[root@czq gitdata]# git log -p  ##显示每一次的变化和内容
[root@czq gitdata]# git log -p --oneline ##一行显示每一次的变化和内容
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

13git reset --hard hash值.回滚数据到某一个提交

````bash
[root@czq gitdata]# git reset --hard 8d80079 
````

14.git relog 查看历史提交

````bash
[root@czq gitdata]# git reflog
2de0e8c HEAD@{0}: reset: moving to 2de0e8c
8d80079 HEAD@{1}: reset: moving to 8d80079
2de0e8c HEAD@{2}: commit: add 123
6a6690c HEAD@{3}: commit: add index
ce467a2 HEAD@{4}: commit: mv a.txt a
20d7119 HEAD@{5}: commit: modifiled a a.txt
8d80079 HEAD@{6}: commit (initial): add newfile a
````

15.git branch 查看分支

````bash
[root@czq gitdata]# git branch
* master
[root@czq gitdata]# git branch testing ##创建分支
[root@czq gitdata]# git branch 
* master
  testing
````

16.git checkout 分支名   切换分支

````bahs
[root@czq gitdata]# git checkout testing  ##切换分支到testing
切换到分支 'testing'
````

17. git checkout -b xxx 创建并切换到xxx分支

````bash
[root@czq gitdata]# git checkout -b testing
切换到一个新分支 'testing'
````

18. git branch -d xxx 删除xxx分支

````bash
[root@czq gitdata]# git branch -d testing
已删除分支 testing（曾为 6a6690c）。
````

19.git merge 分支名 和并分支

````bash
[root@czq gitdata]# git merge testing 
````

20.git tag 打标签

````bash
[root@czq gitdata]# git tag -a "v2.0" -m "xxxx" ##给当前所在的提交打标签 ##-a 指定标签名字 -m 描述信息
[root@czq gitdata]# git tag -d v1.0  ##删除标签
已删除 tag 'v1.0'（曾为 b6fcdeb
````

21. git remote add 添加远程仓库

````bash
[root@czq gitdata]# git remote add origin git@github.com:lvcv-qiangge/git_data.git ##添加远程仓库 并命名为origin
````

22 git remote 查看当前远程仓库的名称

````bash
[root@czq gitdata]# git remote 
origin
````

24,git remote remove 删除远程库

````bash
[root@czq gitdata]# git remote remove origin
````

23.git push 推送代码

````bash
[root@czq gitdata]# git push -u origin master  ##把本地的master分支推送到远程的origin库中
Counting objects: 30, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (22/22), done.
Writing objects: 100% (30/30), 2.32 KiB | 0 bytes/s, done.
Total 30 (delta 9), reused 0 (delta 0)
remote: Resolving deltas: 100% (9/9), done.
To git@github.com:lvcv-qiangge/git_data.git
 * [new branch]      master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
````

24.git clone 拉取代码

````bash
[root@czq tmp]# git clone git@github.com:lvcv-qiangge/git_data.git ##把远程库的代码拉取到本地
````

