# Git & Github
***
> Louis
## reference:
> https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000
> http://www.runoob.com/w3cnote/git-guide.html
> https://www.zhihu.com/question/20070065
## git client
* git for mac : https://sourceforge.net/projects/git-osx-installer/* 

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

## Github

* ssh-keygen -t rsa -C "louis.lv.info@gmail.com"
* vi id_rsa.pub 复制整个key去github->setting-> add new key
* ssh -T git@github.com
* git config --global user.name "Louis-udm"
* git config --global user.email "louis.lv.info@gmail.com"

* git remote add origin git@github.com:Louis-udm/savoir-jupyter.git
* git push -u origin master # -f 强制
* git push origin master
