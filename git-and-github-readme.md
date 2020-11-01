# Git & Github
***
> Louis

> 01 sept. 2017


## 日常操作quotidien:

* git add readme.md configurer_jupyter_serveur.md
* git add -A #stages All, include those deleted; (git v1.x)
* git add . #Stages New & Modified But Without Deleted (git v2.x include those deleted)
* git add -u #Stages Modified & Deleted But Without New
* git commit -m "new file and rename file" 或 git cm "first commit"
* git status
* git log --pretty=oneline
* rm readme.md # 不小心删了
  - git checkout readme.md
* git push # 上传到github
* git pull # 把github的库的内容更新下来
* <b>建项目的通常步骤</b>：
  - 在github上建好一个项目, **本地根目录(要存放项目目录的上级目录)下** 运行: git clone git@github.com:Louis-udm/savoir-jupyter.git
  - 可以改本地的项目目录名称，不影响github的项目名称


### git client
* git for mac : https://sourceforge.net/projects/git-osx-installer/* 
* git for mac: Tower
* git gui for mac :https://desktop.github.com
* git --version

### 建立项目repository

* mkdir savoir-jupyter
* git init

* git log
* git reflog

### 清除repository

* find . -name ".git" | xargs rm -Rf #删除文件夹下的所有 .git 文件, 
* git init 重新初始化新建的git仓库
* 接着add，commit等操作即可


### [branch](https://www.runoob.com/git/git-branch.html) 分支
* git branch_name #创建分支
* git checkout -b dev
* git branch #查看当前分支
* git branch -r #查看远程分支
* git branch -a #查看所有
* git checkout branch_name/master #切换branch_name/master分支
* git diff <source_branch> <target_branch> #在合并改动之前，你可以使用如下命令预览差异
* git merge dev #合并, 如果有冲突，手工解决，再用git add 解决后文件 & git commit -m
* git log --graph --pretty=oneline --abbrev-commit
* git branch -d feature1
* [git stash #将现在的工作现场“储藏”起来, 去修改bug](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000)
* git checkout origin/branch_name 可以切换到这个分支，然后git pull 可以更新这个分支
* 注意当git clone --depth=1 https://github.com/RetroPie/RetroPie-Setup.git. depth用于指定克隆深度，为1即表示只克隆最近一次commit，但它也只是clone了master. 为了获得其他分支，补救办法：[git clone --depth=1时的一些问题](https://www.jianshu.com/p/1031dd2a6c3a)
```
$git clone --depth 1 https://github.com/dogescript/xxxxxxx.git
$ git remote set-branches origin 'remote_branch_name'
$ git fetch --depth 1 origin remote_branch_name
$ git checkout remote_branch_name
```

### git log
> https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2
git log --pretty=oneline


### 标签
* git checkout master #确定要做哪个分支打标签
* git tag v1.0 #打标签，或 git tag v0.9 6224937 或 git tag -a v0.1 -m "version 0.1 released" 3628164
* git show <tagname>
* git tag

### 替换本地改动
* 假如操作失误，你可以使用如下命令替换掉本地改动：
* git checkout -- <filename>
* 此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。
* 假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
* git fetch origin
* git reset --hard origin/master

### 回退/reset/rollback
* git status, git log, git reset --hard commitID
* Reset the commit branch back before the last 5 commits, then squashes them into a single commit
```
git reset --hard HEAD~5 (Reset the current branch to the commit just before the last 5)
git merge --squash HEAD@{1} (HEAD@{1} is where the branch was just before the previous command. This command sets the state of the index to be as it would just after a merge from that commit)
```
* git reset --soft HEAD^   : Undoes the last commit in the working branch and sets the HEAD back one commit.

* soft / hard / mixed
> https://www.cnblogs.com/kidsitcn/p/4513297.html
--soft参数告诉Git重置HEAD到另外一个commit，但也到此为止。如果你指定--soft参数，Git将停止在那里而什么也不会根本变化。这意味着index,working copy都不会做任何变化，所有的在original HEAD和你重置到的那个commit之间的所有变更集都放在stage(index)区域中。
--hard参数将会blow out everything.它将重置HEAD返回到另外一个commit(取决于~12的参数），重置index以便反映HEAD的变化，并且重置working copy也使得其完全匹配起来。这是一个比较危险的动作，具有破坏性，数据因此可能会丢失！
--mixed是reset的默认参数，也就是当你不指定任何参数时的参数。它将重置HEAD到另外一个commit,并且重置index以便和HEAD相匹配，但是也到此为止。

### rebase
> http://jartto.wang/2018/12/11/git-rebase/
> https://www.jianshu.com/p/5c9c6383aa36

* 用途：1.清理commits，合并多次提交纪录; 2.比merge好的分支合并；3.对一个分支做「变基」操作
* git rebase -i HEAD~10    : To list the last 10 commits and modify them with either the squash or fixup command
* rebase 可以挤压(squash)多次commits到一起, 而不需要使用merge

### merge vs rebase
> https://juejin.im/post/6844903989641756679

### stash
> https://www.jianshu.com/p/14afc9916dcb

* 工作流被打断,需要先做别的需求
```
git stash        //保存开发到一半的代码
# fix emergency bugs ...
git commit -a -m "fixed xxx bug"
git stash pop   //将代码追加到最新的提交之后
```

### 查找哪一次代码提交引入了错误
> http://www.ruanyifeng.com/blog/2018/12/git-bisect.html
```
git bisect 按照两分法不断缩小定位
git log --pretty=oneline
git bisect start [终点/HEAD] [起点]
git bisect good/bad
```

### 查看某次提交的修改列表(list of all files that had been modified or added to a specific commit)
git diff-tree

### git cherry-pick
> https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html

对于多分支的代码库，将代码从一个分支转移到另一个分支是常见需求。
这时分两种情况。一种情况是，你需要另一个分支的所有代码变动，那么就采用合并（git merge）。另一种情况是，你只需要部分代码变动（某几个提交），这时可以采用 Cherry pick。

### .gitignore
* ？：代表任意的一个字符
* ＊：代表任意数目的字符
* {!ab}：必须不是此类型
* {ab,bb,cx}：代表ab,bb,cx中任一类型即可
* [abc]：代表a,b,c中任一字符即可
* [ ^abc]：代表必须不是a,b,c中任一字符
* 一个空的 .gitignore 文件可以当作是一个 placeholder 。比如，当你需要为项目创建一个空的 log 目录时， 在里面放置一个空的 .gitignore 文件。这样当你 clone 这个 repo 的时候 git 会自动的创建好一个空的 log 目录了。
* git rm -r --cached ignore_file : 还有一种情况，就是文件已经commit了，再加入gitignore是无效的，所以需要删除下缓存

### git中fetch和pull的区别
> https://www.jianshu.com/p/d265f7763a3a
* fetch命令是将远程分支的最新内容拉到了本地,本地多了一个FETCH_HEAD的指针,heckout到该指针后可以查看远程分支的最新内容。然后checkout到master分支，执行merge做合并
* pull的作用就相当于fetch和merge，自动合并
* How to force an overwrite of your local files with the master branch
```
git fetch --all
git reset --hard origin/master # origin/master是fetch_head
```

### git-prune
> https://git-scm.com/docs/git-prune
- Prune all unreachable objects from the object database



## Github

* ssh-keygen -t rsa -C "louis.lv.info@gmail.com"
* vi id_rsa.pub 复制整个key去github->setting-> add new key.
* ssh -T git@github.com
* git config --global user.name "Louis-udm"
* git config --global user.email "louis.lv.info@gmail.com"

### 关联项目
* git remote add origin git@github.com:Louis-udm/savoir-jupyter.git
* git push -u origin master # -f 强制
* git push origin master 推送给github (或默认直接git push)
* git remote | git remote -v #查看远程库
* git pull #把github的库的内容更新下来
* <b>通常的clone步骤</b>：在github上建好一个项目, **本地根目录(要存放项目目录的上级目录)下** 运行: git clone git@github.com:Louis-udm/savoir-jupyter.git
* git remote remove origin #取消本地目录下关联的远程库

### 合作项目
1. 项目新建者：新建一个Repository，进入Repository的Settings，然后在Manage Collaborators里管理合作者了，添加其他git账号.
2. 项目新建者：ssh-keygen -C "YourEmail@example.com" （这里的email使用github账号）生成公钥和私钥，在Accounts Settings->SSH keys 将公钥上传上去。
3. 其他合作者: 本地根目录(要存放项目目录的上级目录)下运行: git clone git@github.com:项目新建者账号/项目名.git


### 参与Github的某个开源项目
* 点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone
* git clone git@github.com:Louis-udm/bootstrap.git
* 在GitHub上发起一个pull request，让作者接受我的修改

### 如何从原始作者的项目仓库中更新fork过的项目：

* git remote -v #项目目录下
* git remote add upstream https://github.com/placaille/ift-labo.git   #添加一个将被同步给 fork 远程的上游仓库
* git remote -v #查看添加成功了否
* git fetch upstream #从上游仓库 fetch 分支和提交点，传送到本地，并会被存储在一个本地分支 upstream/master 
* git checkout master 切换到本地主分支(如果不在的话) 
* git merge upstream/master 把 upstream/master 分支合并到本地 master 上，这样就完成了同步，并且不会丢掉本地修改的内容。
* 如果想更新到 GitHub 的 fork 上，直接 git push origin master

### 新建git组织
1. Accounts Settings -> Organizations -> Create new Organizations 新建一个组织, 并添加项目成员。
2. 新建一个Repository ->  进入Repository的Settings -> Collaborators -> 在Teams下面点击刚创建的组织: 比如my-organize/owners 添加或者remove组织成员. 组织的所有者可以针对不同的代码仓库建立不同访问权限的团队。

## 清理.git大文件和历史库
### 常规办法
1. 删除无用的分支
$ git branch -d <branch_name>
2. 删除无用的tag
$ git tag -d <tag_name>
3. 清理本地版本库
$ git gc --prune=now

### 高级办法
> 注意高级办法会导致push冲突，需要强制提交，其他人pull也会遇到冲突，建议重新克隆

1. 完全重建版本库
$ rm -rf .git
$ git init
$ git add .
$ git cm "first commit"
$ git remote add origin <your_github_repo_url>
$ git push -f -u origin master

2. 有选择性的合并历史提交
> https://www.zhihu.com/question/29769130/answer/45546231

$ git rebase -i <first_commit>
将想合并的提交的pick改成s,这样第四个提交就会合并进入第三个提交。等合并完提交之后再运行:
$ git push -f
$ git gc --prune=now

## others
### git 的合并原理（递归三路合并算法）
> https://blog.walterlv.com/post/git-merge-principle.html

### Trunk Based Development
![Trunk Based Development](https://paulhammant.com/images/what_is_trunk.jpg "Trunk Based Development")
> https://paulhammant.com/2013/04/05/what-is-trunk-based-development/
> https://my.oschina.net/u/1993252/blog/2051520
* Trunk Based Development，缩写为TBD，中文就是基于主干的开发: 
同一个产品开发的所有人员共享一个Repository，有一个trunk，单一Developer或是Developer团队可以有自己的private branch，所有修改最后都会回到主干; 
只有在Release时才会有官方的分支，一般Developer不能对Release Branch作动作，只有Release Engineer可以更动Release Branch，当Release Branch完成它的任务，就会被砍掉; 
Bug先在trunk修好，之後把Commit合併到Release Branch，而不是在Release Branch修好再整合到trunk，這樣可以把修改Release Branch的人限制在最小程度。

## reference:
* https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000
* http://www.runoob.com/w3cnote/git-guide.html
* https://www.zhihu.com/question/20070065

* http://xiaocong.githubio/blog/2013/03/20/team-collaboration-with-github/
* http://blog.leezhong.com/tech/2011/02/25/git-workflow-with-blog-demohtml
* http://www.yangzhiping.com/tech/github.html#q1
* https://www.zhihu.com/question/29769130
* https://blog.walterlv.com/post/git-merge-principle.html