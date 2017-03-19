#### 本地git bash远程github

1. 创建 SSH Key

   > $ ssh-keygen -t rsa -C "这里输入github注册邮箱号"

2. 一路回车，使用默认值即可

3. 登陆GitHub，打开“Settings”->“SSH and GPG Keys”，“New SSH Key”操作

4. Title任意，在Key文本框里复制id_rsa.pub文件内容

5. 点“Add SSH Key”，你就应该看到已经添加的Key

6. 验证是否成功，在git bash中输入:

   > $ ssh -t git@github.com  // 回车后shell access 这就表示已成功连上github
   >

7. 如果出现以下错误解决

   > 错误提示一：ssh_exchange_identification: read: Connection reset by peer  // SSH使用HTTP代理来登录服务器
   >
   > 错误提示二：PTY allocation request failed on channel 0  // 把小写t 改为大写T 即可：$ ssh -T git@github.com   

8. 第一次使用需要设置用户名和email

   > $ git config --global user.name "这里输入github用户名"
   >
   > $ git config --global user.email "这里输入github注册邮箱号"



#### 本地项目上传github

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

#### github版本库中添加文件

> $ cd e:/web   // 针对哪个项目库操作就要先进入这个项目的目录
>
> $ git add test.txt  // 库中添加 test.txt文件
>
> $ git commit -m "add test.txt"  // 备注说明添加了test.txt 文件
>
> $ git push  // 推送数据到github库中

如果想省掉提交之前的 git add 命令，可以直接用

> $ git commit -a -m 'add test.txt'

commit和commit -a的区别, commit -a相当于：

- 第一步：自动地add所有改动的代码，使得所有的开发代码都列于index file中
- 第二步：自动地删除那些在index file中但不在工作树中的文件
- 第三步：执行commit命令来提交

#### 本地手动删除库中某个文件

可以直接右键删除即可，也可在本地项目库web右键打开git bash用命令操作，具体如下

> $ rm test.txt   // 删除库中 test.txt文件
>
> $ git status -s  // 查询状态，库中哪些文件被删除了

当出现以下错误提示解决办法

> $ git status -s
>
> fatal: Not a git repository (or any of the parent directories): .git   // 说明没有进入相应的分支目录



#### github版本库及本地库中删除某个文件

选择本地项目库web右键打开git bash，在里面输入命令，具体如下：

> $ git rm test.txt
>
> $ git commit -m "remove test.txt"
>
> $ git push

#### github版本库中删除某个文件（本地库保留）

选择本地项目库web右键打开git bash，在里面输入命令，具体如下：

> $ git rm -r --cached test.txt
>
> $ git commit -m "remove test.txt"
>
> $ git push



#### git中遇到的一些错误解决办法

##### 第一种错误：

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

#### 从现有仓库克隆

> $ git clone git://github.com/Tycn/web.git



第二种错误提示： error: failed to push some refs to 'https://github.com/xxx.git'

当要push代码到git时，出现提示：

> error:failed to push some refs to ...Dealing with “non-fast-forward” errorsFrom time to time you may encounter this error while pushing:$ git push origin master To ../remote/  ! [rejected]        master -> master (non-fast forward) error: failed to push some refs to '../remote/' To prevent you from losing history, non-fast-forward updates were rejectedMerge the remote changes before pushing again.  See the 'non-fast forward'section of 'git push --help' for details.

问题（Non-fast-forward）的出现原因在于：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。于是你有2个选择方式：

1. 强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容

   > **git push -f origin master**



2. 先把git的东西fetch到你本地然后merge后再push

   > $ git fetch
   >
   > $ git merge

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