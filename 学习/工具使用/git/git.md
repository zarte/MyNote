[TOC]
##git 强制push
```
如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用–force选项。
$ git push --force origin
上面命令使用–force选项，结果导致在远程主机产生一个”非直进式”的合并(non-fast-forward merge)。除非你很确定要这样做，否则应该尽量避免使用–force选项。
```
##git高级命令
```
1. 使用rebase而非merge来拉取上游修改
使用git rebase将一个feature分支变基到master分支：

$ git checkout feature
$ git rebase master
这么做会将整个feature分支移动到master分支的起点，它会合并master分支上所有新的提交。


2. 在执行git rebase后解决合并冲突
$git rebase –abort来完全取消变基。这么做会取消变基修改，并将分支置回到执行git rebase之前的状态。
$git rebase –skip来完全忽略该提交。这样，有问题的提交所引入的变化就不会被添加到历史中。

3. 临时性保存修改
$ git status
# On branch feature
nothing to commit, working directory clean
这时就可以安全地切换分支做别的事情了。不过不必担心，暂存的提交依旧还在：

$ git stash list
stash@{0}: WIP on feature: 3fc175f fix race condition
稍后，在回到feature分支后，你就可以取回所有暂存的变更了：

$ git stash pop
On branch feature

$ git stash save "describe it"   # give the stash a name
$ git stash clear                # delete a stashed commit
$ git stash save --keep-index    # stash only unstaged files

4. 克隆一个特定的远程分支
如果想要从远程仓库中克隆一个特定的分支该怎么做呢？通常你会使用git clone，不过这么做会将所有其他分支都一并克隆下来。一个便捷的方式是使用git remote add：

$ git init  
$ git remote add -t  -f origin 
$ git checkout

5. 将cherry-pick远程提交合并到自己的分支中
$ git cherry-pick

6. 应用来自于不相关的本地仓库的补丁
将另一个不相关的本地仓库的提交补丁应用到当前仓库该怎么做呢？
$ git --git-dir=/.git format-patch -k -1 --stdout  | git am -3 -k

7. 忽略追踪文件中的变更
通过如下命令永久性地告诉Git不要管某个本地文件：
$ git update-index --assume-unchanged

8. 每隔X秒运行一次git pull
可以在后台通过脚本（或是使用GNU Screen）每隔X秒调用一次git pull：

$ screen
$ for((i=1;i<=10000;i+=1)); do sleep X && git pull; done

9. 将子目录分隔为新的仓库

有时，你可能需要将Git仓库中某个特定的目录转换为一个全新的仓库。这可以通过git filter-branch来实现：

$ git filter-branch --prune-empty --subdirectory-filter  master
# Filter the master branch to your directory and remove empty commits
Rewrite 48dc599c80e20527ed902928085e7861e6b3cbe6 (89/89)
Ref 'refs/heads/master' was rewritten
现在，仓库会包含指定子目录中的所有文件。虽然之前的所有文件都会被删除，但他们依旧存在于Git历史中。现在可以将新的本地仓库推送到远程了。

10. 清理
有时，Git会提示“untracked working tree files”会“overwritten by checkout”。造成这种情况的原因有很多。不过通常来说，我们可以使用如下命令来保持工作树的整洁，从而防止这种情况的发生：

$ git clean -f     # remove untracked files
$ git clean -fd    # remove untracked files/directories
$ git clean -nfd   # list all files/directories that would be removed

11. 将项目文件打成tar包，并且排除.git目录

有时，你需要将项目副本提供给无法访问GitHub仓库的外部成员。最简单的方式就是使用tar或zip来打包所有的项目文件。不过，如果不小心，隐藏的.git目录就会包含到tar文件中，这会导致文件体积变大；同时，如果里面的文件与接收者自己的Git仓库弄混了，那就更加令人头疼了。轻松的做法则是自动从tar文件中排除掉.git目录：

$ tar cJf .tar.xz / --exclude-vcs

12. 查找修改者
最后，如果出现混乱的情况，你一定想要找出是谁造成的。如果生产服务器宕机，那么找到罪魁祸首是比较容易的事情：只需执行git blame。该命令会显示出文件中每一行的作者，提交hash则会找出该行的上一次修改，还能看到提交的时间戳：

$ git blame
```
##查看提交记录
```
1.查看单个文件的提交记录

$ git log --pretty=oneline pom.xml
d8f9ea127a2aa449dcf9a24f34d2d482ac5ecada  update pom.xml
920e18691022bf1b70c5a858e8d8f82cea099a98  update pom.xml
30c6508be66e1e63f4bab3d60afc7aedafd150ab  u
e223c2744a416333497ba85309ee8224a5ba9e95  update files 6fa71dadce1061bd6c99257616d23da13b930249  init files  

2.显示具体的某次的改动的修改

$ git show d8f9ea127a2aa449dcf9a24f34d2d482ac5ecada   

3.查看哪些文件做了修改
提交之前 git diff 。

```
## 19 Tips For Everyday Git Use
```

Parameters for better logging
git log --oneline --graph
Log actual changes in a file
git log -p filename
Only Log changes for some specific lines in file
git log -L 1,1:some-file.txt
Log changes not yet merged to the parent branch
git log --no-merges master..
Extract a file from another branch
git show some-branch:some-file.js
Some notes on rebasing
git pull --rebase
Remember the branch structure after a local merge
git merge --no-ff
Fix your previous commit, instead of making a new commit
git commit --amend
Three stages in git, and how to move between them
git reset --hard HEAD  and  git status -s
Revert a commit, softly
git revert -n
See diff-erence for the entire project (not just one file at a time) in a 3rd party diff tool
git difftool -d
Ignore the white space
git diff -w
Only “add” some changes from a file
git add -p
Discover and zap those old branches
git branch -a
Stash only some files
git stash -p
Good commit messages
Git Auto-completion
Create aliases for your most frequently used commands
Quickly find a commit that broke your feature (EXTRA AWESOME)
git bisect

来源：  http://www.alexkras.com/19-git-tips-for-everyday-use/
```
##git支持中文问题
```
1、ls不能显示中文目录

解决办法：在git/etc/git-completion.bash中增加一行：

  alias ls='ls --show-control-chars --color=auto'

2、git commit不能提交中文注释

解决办法：修改git/etc/inputrc中对应的行：

  set output-meta on

  set convert-meta off 

3、git log无法显示中文注释

解决办法：在git/etc/profile中增加一行：

  export LESSCHARSET=iso8859

到这时有关于git在winxp下安装就介绍完了，希望这个能给一些朋友带来一定的帮助。同时更希望朋友们关注W3CPLUS，因为只有你们的关注才能带来我的成长。
```

##提交java代码到github
```
$ cd gyl /
$ git status
$ git add .
$ git status
On branch master
$ git commit - m "commit by adanac"
$ git remote add origin git@github . com : adanac / gyl
$ git pull origin master
$ git push - u origin master
ok ~
```
##git创建与管理远程分支
```
--远程分支就是本地分支 push 到服务器上的时候产生的。比如 master 就是一个最典型的远程分支（默认）。
$ git push origin master
 
--创建远程分支 dev ，即在本地创建分支后 push 到远程服务器上的。
$ git branch dev --创建分支 dev
$ git checkout dev --切换到分支 dev
--上面的两行相当于： $ git checkout - b dev
$ git push origin dev -- push 到服务器
 
--删除远程分支 dev
$ git push origin : dev
 
--远程分支和本地分支需要区分，所以在从服务器上拉取特定分支的时候，需要指定本地分支名字。
$ git checkout -- track origin / dev
注意该命令由于带有-- track 参数，所以要求 git1 . 6.4 以上！
这样 git 会自动切换到 dev 分支。
 
--提交分支数据到远程服务器
--语法 $ git push origin <local_branch_name> :< remote_branch_name >
$ git push origin dev : dev
--如果当前在 dev 分支下，也可以直接
$ git push
```
##Git fetch和git pull的区别
```
Git 中从远程的分支获取最新的版本到本地有这样 2 个命令：
-- git fetch ：相当于是从远程获取最新版本到本地，不会自动 merge
$ git fetch origin master
$ git log - p master .. origin / master
$ git merge origin / master
以上命令的含义：
首先从远程的 origin 的 master 主分支下载最新的版本到 origin / master 分支上
然后比较本地的 master 分支和 origin / master 分支的差别
最后进行合并,上述过程其实可以用以下更清晰的方式来进行：
$ git fetch origin master : tmp
$ git diff tmp
$ git merge tmp
从远程获取最新的版本到本地的 test 分支上,之后再进行比较合并

-- git pull ：相当于是从远程获取最新版本并 merge 到本地
$ git pull origin master
上述命令其实相当于 git fetch 和 git merge
总结：在实际使用中， git fetch 更安全一些，因为在 merge 前，我们可以查看更新情况，然后再决定是否合并
```
##Git多账户管理
```
很多公司都使用Git作为版本管理工具，今天主要记录下自己如何把工作的Git账号和私人的GitHub账号同时管理，随意切换。

1. 新建SSH-KEY

使用命令 ssh-keygen -t rsa -C "mine@email.com"
为第一个用户新建SSH-Key直接全部回车即可（默认存储文件）,
第二个用户的SSH-Key与第一个用户基本相同，但注意需指定存储文件，如：Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): id_rsa_mine
生成后的文件结构基本如下：
-rw-------   1 eddy  staff  3243  1 14 22:46 id_rsa
-rw-r--r--   1 eddy  staff   747  1 14 22:46 id_rsa.pub
-rw-r--r--   1 eddy  staff   747  1 14 22:23 id_rsa.pub_eddy
-rw-------   1 eddy  staff  3243  1 14 22:23 id_rsa_eddy
-rw-------   1 eddy  staff  1679  1  6 18:53 id_rsa_mine
-rw-r--r--   1 eddy  staff   400  1  6 18:53 id_rsa_mine.pub

2. 将新密钥添加到SSH Agent中

SSH Agent默认只能读取id_rsa私钥
将新生成的第二把私钥添加到SSH Agent中
指令：ssh-add ~/.ssh/id_rsa_mine

3. 配置config文件
vi config
Host github.com
    HostName github.com
    User git
    IdentityFile /Users/eddy/.ssh/id_rsa
Host github3
    HostName github.com
    User git
    IdentityFile /Users/eddy/.ssh/id_rsa_mine

4. 将新建的公钥添加到另一个GitHub中

5. 全局账户切换

配置完上述步骤你已经可以使用不同身份来clone并管理下GitHub中的项目
使用方式1：git clone git@github.com:Justice-love/spi.git
使用方式2：git clone git@github3:Justice-love/spi.git
切换git全局用户标识（user.name/user.email）
```
##unset 仓库名
git config --global --unset user.name
git config --global --unset user.email
然后在不同的仓库下设置：
git config user.name "zhongxin007"
git config user.email "860651416@qq.com"


