# 初识git
## Git简介
### 安装Git
本学习笔记来自[廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
### 创建版本库
版本库又名仓库，英文名repository，可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
1. 选择一个合适的地方，创建一个空文件夹，右键文件夹打开Git Bash。
2. 通过git init命令把这个目录变成Git可以管理的仓库。
#### 把文件添加导版本库
建议使用Visual Studio Code或notepad++代替记事本。

把一个文件放到Git仓库只需要两步
1. 用命令git add把文件添加到仓库
> $ git add readme.txt

2. 用命令git commit告把文件提交到仓库
> $ git commit -m "提交的说明"

简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。
git commit命令执行成功后会告诉你，1 file changed：1个文件被改动（我们新添加的readme.txt文件）；2 insertions：插入了两行内容（readme.txt有两行内容）。
> $ git add file1.txt
> 
> $ git add file2.txt file3.txt
> 
> $ git commit -m "add 3 files."

### 时光穿梭机
git status命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

虽然Git告诉我们readme.txt被修改了，但如果能看看具体修改了什么内容，自然是很好的。比如你休假两周从国外回来，第一天上班时，已经记不清上次怎么修改的readme.txt，所以，需要用git diff这个命令看看。
> $ git diff readme.txt 

git diff顾名思义就是查看difference
#### 版本回退
一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用git log命令查看

在Git中，用HEAD表示当前版本,上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

要把当前版本append GPL回退到上一个版本，就可以使用git reset命令。
> $ git reset --hard HEAD^

可以通过前几位版本号去恢复版本

在Git中，总是有后悔药可以吃的，就必须找到append GPL的commit id。Git提供了一个命令git reflog用来记录你的每一次命令。
> $ git reflog

#### 工作区和暂存区
##### 工作区
就是你在电脑里能看到的目录。
##### 版本库
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。

#### 管理修改
Git管理的是修改，当你用git add命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
#### 撤销修改
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

Git同样告诉我们，用命令git reset HEAD \<file\> 可以把暂存区的修改撤销掉（unstage），重新放回工作区
> $ git reset HEAD readme.txt

#### 删除文件
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了
> $ rm test.txt

一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
> $ git rm test.txt
> 
> $ git commit -m "remove test.txt"

现在，文件就从版本库中被删除了。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本
> $ git checkout -- test.txt
### 远程仓库
#### 添加远程库
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改。
#### 从远程库克隆
用命令git clone克隆一个本地库