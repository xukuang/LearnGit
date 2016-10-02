## git的初始配置

初始配置主要包括配置用户名和密码。

```
git config -- global user.email "kuang_xu@126.com"
git config -- global user.name "xukuang"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

git config -- list 可以查看你设置好的用户名和邮箱。


## 创建仓库（repository）

* 创建空目录

```
mkdir learngit
cd learngit
pwd
```

* 让目录变成git可以管理的仓库

```
git init
```
这时会在当前目录下多了一个.git的目录（默认情况下，在windows系统中文件是隐藏起来的)，这个目录被称作Git版本库，它用来跟踪管理工作区（工作区可以简单理解为当前目录learngit下除.git目录以外的所有文件）文件的版本。没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

## 把一个文件放到Git仓库

* 第一步，用命令git add告诉Git，把文件添加到了Git版本库的暂存区。

```
git add readme.txt
```

如果用git add添加的是文件夹的话，文件夹里面一定要有文件才能添加成功。同样，一个文件夹中任何文件的更改，也只需填写文件夹名就可以了，而不一定非要切换到该文件夹下，再添加。

* 第二步，用命令git commit告诉Git，把文件提交到了Git版本库中的master分支。

```
git commit -m "wrote a readme file"
```

只有提交到Git版本库中的master分支，才算把一个文件放到了Git仓库中。

简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。此外，"git commit --amend"命令可以修改最近一次的commit时的提交说明。

默认情况下，git add一次只能从工作区添加一个文件到Git版本库的暂存区， git commit可以一次把Git版本库的暂存区的所有文件变化的记录提交到master分支。然而, git add \. 一次也能把工作区的多个文件提交到Git版本库的暂存区。

```
git add file1.txt
git add file2.txt file3.txt
git add .
git commit -m "add 3 files."
```

## 时间穿梭机

* git status命令可以让我们时刻掌握仓库当前的状态

```
git status
```

* git log命令可以告诉我们不同版本的历史记录

```
git log
```

git log命令显示从最近到最远的提交日志。如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数。

```
git log --pretty=oneline
```
需要友情提示的是，你看到的一大串类似3628164...882e1e0的是commit id（版本号），和SVN不一样，Git的commit id不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的commit id和我的肯定不一样，以你自己的为准。为什么commit id需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

不同于git log，git reflog 可以查看所有的提交(即 git comit)记录。

* git reset可以重置仓库的版本

首先，Git必须知道当前版本是哪个版本，这个可以借助git log命令实现。在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

```
git reset --hard HEAD^
```

git reset --hard还可以直接加版本号，调到指定版本。此处，版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

事实上，git reset细分分为三个命令：
1. git reset -- soft HEAD^: 移动head的指向，使其指向上一个版本
2. git reset -- mixed HEAD^: 移动head的指向，使其指向上一个版本; 同时将HEAD移动后指向的版本回滚到暂存区
3. git reset -- hard HEAD^: 移动head的指向，使其指向上一个版本; 同时将HEAD移动后指向的版本回滚到暂存区；还要回滚后的暂存区还原到工作区

此外，"git reset 版本号 文件名/路径"还能恢复指定的文件。

## 管理修改

* cat命令可以查看文件

```
cat readme.txt
```

* git diff命令可以比较文件间的差异

默认情况下，该命令用于比较工作区与暂存区的文件间的差异。

```
git diff
```

此外，"git diff  版本号"命令可以比较工作区与指定版本之间的差异；"git diff 版本号1 版本号2"命令可以比较两个版本之间的差异；"git diff --cached"命令可以比较暂存区与当前版本间的差异；"git diff --cached 版本号"命令可以比较暂存区与指定版本之间的差异。

git diff HEAD -- 命令可以查看工作区和版本库里面指定文件的区别。

这里的版本库是指版本库的mast分支。

```
git diff HEAD -- readme.txt
```

* git checkout 命令可以丢弃工作区的修改

工作目录中丢弃指定的文件。

```
git checkout -- readme.txt
```

git checkout 之后，修改撤销了， 回到和版本库一模一样的状态。


git reset HEAD可以把暂存区的修改（即 git add的内容）撤销掉（unstage），重新放回工作区。
```
git reset HEAD readme.txt
```
这之后，你又可以使用git checkout 丢弃工作区的修改。

**小结**

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，你只能参考时光穿梭机部分使用git reset --hard 回到指定版本了，不过前提是没有推送到远程库。

## 删除文件

在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件test.txt到Git并且提交。
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了。这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了。

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉(不要再用资源管理里删除文件了)，并且git commit。

```
git rm test.txt
git commit -m "remove test.txt"
```
注意，这里也可以删除一个文件夹，只需要补充上-m即可，git commit -m 文件夹名。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本。

```
git checkout -- test.txt
```
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

## 重命名文件

同删除文件一样，建议直接使用git mv 命令，而不要再用文件管理器删除文件了。

```
git mv 文件名1 文件名2
```

## 分支管理

* 分支创建 

创建dev分支，然后切换到dev分支

```
git checkout -b dev
```

git checkout命令加上-b参数表示创建并切换，相当于以下两条命令。

```
git branch dev
git checkout dev
```

* 分支查看

用git branch命令查看当前分支。

```
git branch
```

* 分支切换

现在，dev分支的工作完成，我们就可以切换回master分支。


```
git checkout master
```

切换回master分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变。

* 分支合并

git merge命令用于合并指定分支到当前分支。

```
git merge dev
```

git merge命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。

注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。

当然，也不是每次合并都能Fast-forward，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除dev分支了。

```
git branch -d dev
```

此外，在merge前，要删除分支dev，需要强行删除，使用命令。

```
git branch -D dev
```

删除后，查看branch，就只剩下master分支了：
```
git branch
```
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。

bug分支
Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。
```
git stash
```
用git stash list命令看看：
```
 git stash list
```
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除。

另一种方式是用git stash pop，恢复的同时把stash内容也删了。

你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令。
```
git stash apply stash@{0}
```

##　同步远程仓库

* 关联Github账户

1. 生成密钥

在电脑的git中，输入一下命令生成git密码。

```
ssh-keygen -t rsa -C "kuang_xu@126.com" 
# Generating public/private rsa key pair.
# Enter file in which to save the key (/c/Users/kx/.ssh/id_rsa):
# 此时弹出命令为提醒设置生成文件位置，直接回车(即不设置)
# Created directory '/c/Users/kx/.ssh'.
# 即存放于C:\Users\kx\.ssh
# Enter passphrase (empty for no passphrase):
# 直接回车，既没有密码
# Enter same passphrase again:
# 直接回车，既没有密码
# Enter same passphrase again:
# 再次回车(密码确认，再次输入
```

2. 连接Github

上述命令若执行成功，会在c/Users/kx/.ssh/目录下生成两个文件id_rsa和id_rsa.pub，用文本编辑器打开ssh.pub文件，拷贝其中的内容，在Github网站上个人账户中将其添加到Add SSH Key中。

3. 查看关联成功

```
ssh -T git@github.com
```
回车就会看到：You’ve successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。


* 创建Github远程仓库

在GitHub网站上创建一个新仓库learngit。

* 建立远程链接

在本地的learngit仓库下运行命令。此时，本地的仓库必须是完整的git仓库（也就是说存在版本库）才能上传成功。

```
git remote add origin git@github.com:xukuang/learngit.git
```

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。git@github.com:xukuang/learngit.git可以认为是远程仓库origin的路径。你可以通过"git remote -v"命令查看所有远程仓库的名字。

* 推送本地内容

最后，就可以把本地库的所有内容推送到远程库上。

```
git push -u origin master
```
把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

之后，如果本地作了提交，可以通过命令。
```
git push origin master
```
该命令甚至可以简化为 git push。

当仓库有两个以上分支时，推送完master分支之后，首次其他分支，要使用一下命令。

```
git push --set-upstream origin feature
```
之后，如果在当前分支上，依旧可以使用git push推送。


把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库。

此外，要去除本地仓库与远程仓库的关联，可以使用如下命令。

```
git remote rm origin
```

## 克隆远程仓库

* 克隆自己的远程仓库

使用ssh协议

```
git clone git@github.com:xukuang/xukuang.github.io_hexo.git
```

也可以使用HTTP协议 
```
git clone https://github.com/xukuang/xukuang.github.io_hexo.git
```

* 克隆别人的远程仓库

克隆别人的仓库，你可以在别人的Github上，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，接着再从自己的账号下clone。另外，你也可以先克隆到本地，在在关联到自己的账户。

1. 克隆远程仓库

```
git clone https://github.com/Spatial-R/spatial-r.github.io.git xukuang.github.io
```

该命令的作用是把别人的远程仓库克隆到自己本地的xukuang.github.io目录中。

2. 关联自己的远程仓库

克隆了远程库到自己的电脑上后，可以通过git remote set-url origin将这个库再关联为自己的远程库。即如何修改origin的地址。

```
git remote set-url origin git@github.com:xukuang/R.git
```
这一步的前提是你的远程库R必须存在。

3. 推送本地库

推送本地仓库到自己的远程仓库。

```
git push
```
