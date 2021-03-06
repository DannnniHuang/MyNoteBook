

# Git教程笔记

https://www.liaoxuefeng.com/wiki/896043488029600

# Mac OS 安装 Git

## 方法一

安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：http://brew.sh/。

## 方法二

直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

# 创建版本库(repository)

1. 选择一个合适的地方，创建一个learngit空目录：(pwd用于显示当前目录)

```terminal
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

2. 把这个目录变成Git可以管理的仓库：

```
$ git init
```

3. 把readme.txt文件添加到仓库，也就是把修改提交到暂存区（stage）

```
$ git add readme.txt
```

4. 把文件提交到仓库，也就是把暂存区所有修改提交到分支（-m 后输入本次提交说明）

```
$ git commit -m "wrote a readme file"
```

# 时光穿梭

查看仓库当前的状态，是否被修改过

```
$ git status
```

查看具体修改内容

```
$ git diff <file>
```

# 版本回退

查看历史提交记录

```
$ git log
```

查看简化历史提交记录

```
$ git log --pretty=oneline
```

Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往上100个版本写成`HEAD~100`。

```
$ git reset --hard HEAD^
```

再次回退到最新版本，需要顺着命令窗口找到commit id

```
$ git reset --hard 1094a
```

查看命令历史可找到commit id

```
$ git reflog
```

# 撤销修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

丢弃工作区的修改,命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令

```
$ git checkout -- <file>
```

***切换到另一个分支

```
$ git checkout <branch>
```

```
$ git checkout -b <branch>
```

***最新版本的Git提供了新的`git switch`命令来切换分支

```
$ git switch -c <branch>
```

```
$ git switch -c <branch>
```

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD `，就回到了场景1，第二步按场景1操作。

```
$ git reset HEAD -- <file>
$ git checkout -- <file>
```

# 删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```
$ rm <file>
```



从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```
$ git rm <file>
$ git commit -m "remove <file>"
```

# 添加远程仓库

先有本地库，后有远程库的时候，如何关联远程库

1. 登陆GitHub，右上角找到“Create a new repo”按钮，创建一个新的仓库

2. 在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库

3. 目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

4. GitHub的提示，在本地的`learngit`仓库下运行命令：（把`michaelliao`替换成你自己的GitHub账户名）

   ```
   $ git remote add origin git@github.com:michaelliao/learngit.git
   ```

5. 把本地库的所有内容推送到远程库上

   ```
   $ git push -u origin master
   ```

6. 从现在起，只要本地作了提交，就可以通过命令

   ```
   $ git push origin master
   ```

# 从远程库克隆

1. 登陆GitHub，创建一个新的仓库，名字叫`gitskills`

2. 勾选`Initialize this repository with a README`，GitHub会自动创建`README.md`文件。

3. 用命令`git clone`克隆一个本地库

   ```
   $ git clone git@github.com:DannnniHuang/gitskills.git
   ```

# 分支管理



## 创建与合并分支

创建并切换分支

```
$ git checkout -b <branch>
或者
$ git switch -c <branch>
```

相当于以下两条命令

```
$ git branch <branch>  //创建分支
$ git checkout <branch>  或 $ git switch <branch> //切换分支
```

查看当前分支，当前分支前面会标一个`*`号

```
$ git branch
```

切换回`master`分支

```
$ git checkout master
```

合并指定分支到当前分支master 分支

```
$ git merge <branch> 
```

删除指定分支

```
$ git branch -d <branch> 
```



## 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。



## 分支管理策略

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```
$ git merge --no-ff -m "merge with no-ff" dev
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```



## Bug分支

Git还提供了一个`stash`功能，可以把当前dev的工作现场“储藏”起来，等以后恢复现场后继续工作：

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```
$ git checkout master

$ git checkout -b issue-101

```

修复bug，然后提交：

```
$ git add readme.txt 
$ git commit -m "fix bug 101"
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```
$ git switch master
$ git merge --no-ff -m "merged bug fix 101" issue-101
```

是时候接着回到`dev`分支干活了！

```
$ git switch dev
$ git status
On branch dev
nothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

```
$ git stash apply 恢复
$ git stash drop 删除stash的内容
```

```
$ git stash pop 恢复+删除stash内容
```

再用`git stash list`查看，就看不到任何stash内容了：

```
$ git stash list
```

同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git自动给dev分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于master的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从dev分支切换到master分支。

## 强项删除为合并过的分支

```
$ git branch -D <branch>
```



## 多人协作

查看远程库的信息

```
$ git remote
origin
或
$ git remote -v //显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```
$ git push origin master
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再pull：

```
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后，提交，再push：

```
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev

```



因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin `推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin `推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to  origin/`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单



## Rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；

- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

  ```
  $ git rebase
  $ git log
  $ git log --graph --pretty=oneline --abbrev-commit
  * 7e61ed4 (HEAD -> master) add author
  * 3611cfe add comment
  * f005ed4 (origin/master) set exit=1
  * d1be385 init hello
  ...
  ```

  

## 创建标签

切换到需要打标签的分支上

```
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```

打一个新标签

```
$ git tag <name>
```

查看所有标签：

```
$ git tag
```

如果忘了打标签,方法是找到历史提交的commit id，然后打上就可以了：

```
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

```
$ git tag v0.9 f52c633
```

再用命令`git tag`查看标签：

```
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show `查看标签信息：

```
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```

创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```



## 操作标签

- 命令`git push origin `可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d `可以删除一个本地标签；
- 命令`git push origin :refs/tags/`可以删除一个远程标签。

如果标签打错了，也可以删除：

```
$ git tag -d v0.1
```

如果要推送某个标签到远程，使用命令`git push origin `：

```
$ git push origin v1.0
```

一次性推送全部尚未推送到远程的本地标签：

```
$ git push origin --tags
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```
$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
```

然后，从远程删除。删除命令也是push，但是格式如下：

```
$ git push origin :refs/tags/v0.9
To github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```



# Git简介

## 集中式与分布式

Git 属于分布式版本控制系统，而 SVN 属于集中式。

集中式版本控制只有中心服务器拥有一份代码，而分布式版本控制每个人的电脑上就有一份完整的代码。

集中式版本控制有安全性问题，当中心服务器挂了所有人都没办法工作了。

集中式版本控制需要连网才能工作，如果网速过慢，那么提交一个文件的会慢的无法让人忍受。而分布式版本控制不需要连网就能工作。

分布式版本控制新建分支、合并分支操作速度非常快，而集中式版本控制新建一个分支相当于复制一份完整代码。



## 中心服务器

中心服务器用来交换每个用户的修改，没有中心服务器也能工作，但是中心服务器能够 24 小时保持开机状态，这样就能更方便的交换修改。

Github 就是一个中心服务器。

## 工作流

<div align="center"> <img src="pics/a1198642-9159-4d88-8aec-c3b04e7a2563.jpg"/> </div><br>新建一个仓库之后，当前目录就成为了工作区，工作区下有一个隐藏目录 .git，它属于 Git 的版本库。

Git 版本库有一个称为 stage 的暂存区，还有自动创建的 master 分支以及指向分支的 HEAD 指针。

<div align="center"> <img src="pics/46f66e88-e65a-4ad0-a060-3c63fe22947c.png"/> </div><br>

- git add files 把文件的修改添加到暂存区
- git commit 把暂存区的修改提交到当前分支，提交之后暂存区就被清空了
- git reset -- files 使用当前分支上的修改覆盖暂存区，用来撤销最后一次 git add files
- git checkout -- files 使用暂存区的修改覆盖工作目录，用来撤销本地修改

<div align="center"> <img src="pics/17976404-95f5-480e-9cb4-250e6aa1d55f.png"/> </div><br>

可以跳过暂存区域直接从分支中取出修改，或者直接提交修改到分支中。

- git commit -a 直接把所有文件的修改添加到暂存区然后执行提交
- git checkout HEAD -- files 取出最后一次修改，可以用来进行回滚操作

## 分支实现

使用指针将每个提交连接成一条时间线，HEAD 指针指向当前分支指针。

<div align="center"> <img src="pics/fb546e12-e1fb-4b72-a1fb-8a7f5000dce6.jpg"/> </div><br>

新建分支是新建一个指针指向时间线的最后一个节点，并让 HEAD 指针指向新分支表示新分支成为当前分支。

<div align="center"> <img src="pics/bc775758-89ab-4805-9f9c-78b8739cf780.jpg"/> </div><br>

每次提交只会让当前分支指针向前移动，而其它分支指针不会移动。

<div align="center"> <img src="pics/5292faa6-0141-4638-bf0f-bb95b081dcba.jpg"/> </div><br>

合并分支也只需要改变指针即可。

<div align="center"> <img src="pics/1164a71f-413d-494a-9cc8-679fb6a2613d.jpg"/> </div><br>

## 冲突

当两个分支都对同一个文件的同一行进行了修改，在分支合并时就会产生冲突。

<div align="center"> <img src="pics/58e57a21-6b6b-40b6-af85-956dd4e0f55a.jpg"/> </div><br>

Git 会使用 <<<<<<< ，======= ，>>>>>>> 标记出不同分支的内容，只需要把不同分支中冲突部分修改成一样就能解决冲突。

```
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

## Fast forward

"快进式合并"（fast-farward merge），会直接将 master 分支指向合并的分支，这种模式下进行分支合并会丢失分支信息，也就不能在分支历史上看出分支信息。

可以在合并时加上 --no-ff 参数来禁用 Fast forward 模式，并且加上 -m 参数让合并时产生一个新的 commit。

```
$ git merge --no-ff -m "merge with no-ff" dev
```

<div align="center"> <img src="pics/dd78a1fe-1ff3-4bcf-a56f-8c003995beb6.jpg"/> </div><br>

## 分支管理策略

master 分支应该是非常稳定的，只用来发布新版本；

日常开发在开发分支 dev 上进行。

<div align="center"> <img src="pics/245fd2fb-209c-4ad5-bc5e-eb5664966a0e.jpg"/> </div><br>

## 储藏（Stashing）

在一个分支上操作之后，如果还没有将修改提交到分支上，此时进行切换分支，那么另一个分支上也能看到新的修改。这是因为所有分支都共用一个工作区的缘故。

可以使用 git stash 将当前分支的修改储藏起来，此时当前工作区的所有修改都会被存到栈上，也就是说当前工作区是干净的，没有任何未提交的修改。此时就可以安全的切换到其它分支上了。

```
$ git stash
Saved working directory and index state \ "WIP on master: 049d078 added the index file"
HEAD is now at 049d078 added the index file (To restore them type "git stash apply")
```

该功能可以用于 bug 分支的实现。如果当前正在 dev 分支上进行开发，但是此时 master 上有个 bug 需要修复，但是 dev 分支上的开发还未完成，不想立即提交。在新建 bug 分支并切换到 bug 分支之前就需要使用 git stash 将 dev 分支的未提交修改储藏起来。

## SSH 传输设置

Git 仓库和 Github 中心仓库之间的传输是通过 SSH 加密。

如果工作区下没有 .ssh 目录，或者该目录下没有 id_rsa 和 id_rsa.pub 这两个文件，可以通过以下命令来创建 SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

然后把公钥 id_rsa.pub 的内容复制到 Github "Account settings" 的 SSH Keys 中。

## .gitignore 文件

忽略以下文件：

- 操作系统自动生成的文件，比如缩略图；
- 编译生成的中间文件，比如 Java 编译产生的 .class 文件；
- 自己的敏感信息，比如存放口令的配置文件。

不需要全部自己编写，可以到 [https://github.com/github/gitignore](https://github.com/github/gitignore) 中进行查询。