# Git & Github
***
> Louis
## reference:
> https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000
> http://www.runoob.com/w3cnote/git-guide.html
> https://www.zhihu.com/question/20070065


## git client
* git for mac : https://sourceforge.net/projects/git-osx-installer/* 
* git gui for mac :https://desktop.github.com

* git --version
* git status* 

* mkdir savoir-jupyter
* git init
* git add readme.md configurer_jupyter_serveur.md
* git commit -m "new file and rename file"
* git status

* git log
* git reflog

* rm readme.md # 不小心删了
* git checkout readme.md

> 创建分支
* git checkout -b dev
* git branch #查看当前分支
* git checkout master #切换回master分支
* git diff <source_branch> <target_branch> #在合并改动之前，你可以使用如下命令预览差异
* git merge dev #合并, 如果有冲突，手工解决，再用git add 解决后文件 & git commit -m
* git log --graph --pretty=oneline --abbrev-commit
* git branch -d feature1
* 
* git stash #工作现场“储藏”起来, 去修改bug: https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000

> 标签
* git checkout master #确定要做哪个分支打标签
* git tag v1.0 #打标签，或 git tag v0.9 6224937 或 git tag -a v0.1 -m "version 0.1 released" 3628164
* git show <tagname>
* git tag

## 替换本地改动
* 假如操作失误，你可以使用如下命令替换掉本地改动：
* git checkout -- <filename>
* 此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。
* 假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
* git fetch origin
* git reset --hard origin/master

## Github

* ssh-keygen -t rsa -C "louis.lv.info@gmail.com"
* vi id_rsa.pub 复制整个key去github->setting-> add new key.
* ssh -T git@github.com
* git config --global user.name "Louis-udm"
* git config --global user.email "louis.lv.info@gmail.com"
> 关联项目
* git remote add origin git@github.com:Louis-udm/savoir-jupyter.git
* git push -u origin master # -f 强制
* git push origin master
* git remote | git remote -v #查看远程库

> 参与Github的某个开源项目
* 点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone
* git clone git@github.com:Louis-udm/bootstrap.git
* 在GitHub上发起一个pull request，让作者接受我的修改
