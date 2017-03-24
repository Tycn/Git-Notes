# GIT-Notes

[TOC]

---

## 本地git bash远程github

创建 SSH Key

> $ ssh-keygen -t rsa -C "这里输入github注册邮箱号"

一路回车，使用默认值即可

登陆GitHub，打开“Settings”->“SSH and GPG Keys”，“New SSH Key”操作

Title任意，在Key文本框里复制id_rsa.pub文件里的内容。注：`id_rsa.pub`文件的内容默认是在本地电脑 `/c/users/用户名/.ssh/id_rsa.pub` 中，用记事本打开。

点“Add SSH Key”，你就应该看到已经添加的Key

验证是否成功，在git bash中输入:

> $ ssh -t git@github.com  // 回车后shell access 这就表示已成功连上github
>

如果出现以下错误解决

> 错误提示一：ssh_exchange_identification: read: Connection reset by peer  // SSH使用HTTP代理来登录服务器
>
> 错误提示二：PTY allocation request failed on channel 0  // 把小写t 改为大写T 即可：$ ssh -T git@github.com   

第一次使用需要设置用户名和email

> $ git config --global user.name "这里输入github用户名"
>
> $ git config --global user.email "这里输入github注册邮箱号"



---



## 本地项目上传github

> $ cd e:/web
>
> $ git init
>
> $ git add --all
>
> $ git commit -m "更新说明"
>
> $ git remote add origin https://github.com/xxx.git   // 可能会提示输入github用户名更密码
>
> $ git push -u origin master



---



## github版本库中添加文件

> $ cd e:/web   // 针对哪个项目库操作就要先进入这个项目的目录
>
> $ git add test.txt  // git暂存库中添加 test.txt文件
>
> $ git commit -m "add test.txt"  // 把暂存区的所有内容提交到当前分支（master分支），冒号里备注说明添加了test.txt 文件
>
> $ git push origin master // 推送数据到github远程库的master分支上

如果想省掉提交之前的 git add 命令，可以直接用

> $ git commit -a -m 'add test.txt'

commit和commit -a的区别, commit -a相当于：

- 第一步：自动地add所有改动的代码，使得所有的开发代码都列于index file中
- 第二步：自动地删除那些在index file中但不在工作树中的文件
- 第三步：执行commit命令来提交


---



## git查看本地master库中更新数据

解释一下：我们把文件file.txt第一次的修改已经git add 添加到缓存区并git commit 到了master分支中，现在我们在文件上又进行了修改，保存后可以用`git diff HEAD -- file`查看修改内容，其中：-- 表示当前分支，不能少

> $ git diff HEAD -- file.txt



还可以使用`cat filt`简要查看

>$ cat filt.txt



---



## git修改撤销命令

我们分几种情况下来说明撤销命令

第一种：当你修改file.txt文件，保存后关电脑出去吃个饭，在吃饭的时候突然想到修改错了，现在回去删除掉，庆幸的是没有`git add `到暂存区，可以自己手动在文件里删除，也可以用`git checkout`命令撤销

> $ git checkout -- file.txt  //说明：--  表示当前分支，必须加上



第二种：还是上面例子，如果你操作`git add`命令放入暂存盘，但是没有`git commit`放入到master分支中，可以使用`git reset HEAD file`命令

> git reset HEAD file.txt

这是后你再`git status`查看状态

> $ git status
> On branch master
> nothing to commit, working tree clean

然后我们再来用命令`cat file`看下是否复原了

> $ cat file.txt

嘿嘿，结果复原了。高兴



第三种：还是上面的例子，如果`git commit `到了master分支了，这个时候就需要用`git reset --hard commit_id` 命令历史版本撤回了

> $ git reset --hard 4c7776c



---



## git历史版本回退（git后悔药）

先查看下历史记录 git log

> $ git log
>
> commit 4c7776c1c869c14d1015ad42de6478d52addxxxx
> Author: unknown <xxx@xx.com>
> Date:   Mon Mar 20 23:06:47 2017 +0800
>
> add html

现在我们开始回退，命令：`git reset --hard commit_id`  ，这里提到的commit id 指的是：commit 4c7776c1c869c14d1015ad42de6478d52addxxxx  这一大串数字

> $ git reset --hard 4c7776c

回退到add html 版本时，再想恢复到更早版本，就必须找到对应版本的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令

> $ git reflog

接下来就根据commit id 回退吧

> $ git reset --hard 4c7776c



---



##本地库手动删除库中某个文件

可以直接右键删除即可，也可在本地项目库web右键打开git bash用命令操作，具体如下

> $ rm test.txt   // 删除库中 test.txt文件
>
> $ git status -s  // 查询状态，库中哪些文件被删除了

当出现以下错误提示解决办法

> $ git status -s
>
> fatal: Not a git repository (or any of the parent directories): .git   // 说明没有进入相应的分支目录



---



## github版本库及本地库中删除某个文件

选择本地项目库web右键打开git bash，在里面输入命令，具体如下：

> $ git rm test.txt
>
> $ git commit -m "remove test.txt"
>
> $ git push



---



## github版本库中删除某个文件（本地库保留）--cached命令

选择本地项目库web右键打开git bash，在里面输入命令，具体如下：

> $ git rm -r --cached test.txt
>
> $ git commit -m "remove test.txt"
>
> $ git push



---



##从现有远程仓库克隆到本地库

> $ git clone git://github.com/xxx.git



---



##git fetch 从远程获取最新版本到本地

**方法一：** 简单粗暴，可能不安全

从远程获取最新版本到本地

> $ git fetch origin master

git fetch origin master 这句的意思是：从远程的origin仓库的master分支下载代码到本地的origin master。

把远程下载下来的代码合并到本地仓库，远程的和本地的合并

> $ git merge origin/master



**方法二：**好理解，相对方法一安全



> $ git fetch origin master:temp

这句命令的意思是：从远程的origin仓库的master分支下载到本地并新建一个分支temp

可以先比较本地的仓库和远程参考的区别

> $ git diff temp

合并temp分支到master分支

> $ git merge temp

如果不想要temp分支了，可以删除此分支

> $ git branch -d temp



---

##git pull拉取远程分支更新数据合并本地库

`git pull`比`git fetch`简单直接，但是没有`git fetch`更新安全，拉取操作，如下：

> $ git pull



---

##git pull 强制覆盖本地文件

简单粗暴的命令，分三步

第一步

> $ git fetch --all

第二步

> $ git reset --hard origin/master

第三步

> $ git pull



---



## git 创建版本库

版本库又名仓库，在本地电脑里叫做文件夹，这个文件夹可以被git跟踪。操作命名是：`mkdir`

> $ mkdir demo

这样就创建好了demo 仓库，用`cd demo` 进入这个仓库

> $ cd demo

可以用`pwd`来查看具体仓库（文件）路径

> $pwd
>
> /e/git-learni/demo



---



## git中遇到的一些错误解决办法

### 第一种错误：

遇到过的一些错误提示：

   > Git – fatal: Unable to create ‘/web/.git/index.lock’: File exists.
   >
   > If no other git process is currently running, this probably means a
   >
   > git process crashed in this repository earlier. Make sure no other git
   >
   > process is running and remove the file manually to continue.

可以试着删除 index.lock，git bash 命令操作如下：

   > $ rm -f ./.git/index.lock



---



 ###第二种错误提示：

 提示：error: failed to push some refs to 'https://github.com/xxx.git'

当要push代码到git时，出现提示：

> error:failed to push some refs to ...Dealing with “non-fast-forward” errorsFrom time to time you may encounter this error while pushing:$ git push origin master To ../remote/  ! [rejected]        master -> master (non-fast forward) error: failed to push some refs to '../remote/' To prevent you from losing history, non-fast-forward updates were rejectedMerge the remote changes before pushing again.  See the 'non-fast forward'section of 'git push --help' for details.

问题（Non-fast-forward）的出现原因在于：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。于是你有2个选择方式：

第一种：

强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容

> **git push -f origin master**

第二种：

先把git的东西fetch到你本地然后merge后再push

> $ git fetch origin master
>
> $ git merge origin/master

这2句命令等价于

> $ git pull 

可是，这时候又出现了如下的问题：

上面出现的 [branch "master"]是需要明确(.git/config)如下的内容

这等于告诉git2件事:

第一件事：当你处于master branch, 默认的remote就是origin。

第二件事：当你在master branch上使用git pull时，没有指定remote和branch，那么git就会采用默认的remote（也就是origin）来merge在master branch上所有的改变

如果不想或者不会编辑config文件的话，可以在bush上输入如下命令行：

> $ git config branch.master.remote origin 

> $ git config branch.master.merge refs/heads/master 

之后再重新git pull下。最后git push你的代码吧。



---



###第三种错误：

如果是git push ，没有加全 git push origin master 出现以下错误

> $ git push
>
> fatal: You are not currently on a branch.
> To push the history leading to the current (detached HEAD)
> state now, use
>
>
> git push origin HEAD:<name-of-remote-branch>
>

继续 git status 查看状态，出现以下提示情况

> $ git status
>
> rebase in progress; onto dbad060
> You are currently rebasing branch 'master' on 'dbad060'.
>   (all conflicts fixed: run "git rebase --continue")

接下来回滚最近更新 git rebase --continue （**注意：本地库需要备份一下，下一步会跟本地库合并。以免本地更新丢失**）

>  $ git rebase --continue
>  Applying: add web03
>  No changes - did you forget to use 'git add'?
>  If there is nothing left to stage, chances are that something else
>  already introduced the same changes; you might want to skip this patch.
>
>  When you have resolved this problem, run "git rebase --continue".
>  If you prefer to skip this patch, run "git rebase --skip" instead.
>  To check out the original branch and stop rebasing, run "git rebase --abort".

再输入 git rebase --abort   **本地合并回滚数据**

> $ git status
> On branch master
> Your branch is up-to-date with 'origin/master'.

最后把备份的文件拷贝更新回来就可以了，开始push吧，**注：需要写全 git push origin master**