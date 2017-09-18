# Git & Github
***
> Louis

## git client
git for mac : https://sourceforge.net/projects/git-osx-installer/

git --version
git status

mkdir savoir-jupyter
git init
git add readme.md configurer_jupyter_serveur.md
git commit -m "new file and rename file"
git status

git log
git reflog


rm readme.md #不小心删了
git checkout readme.md

## Github

ssh-keygen -t rsa -C "louis.lv.info@gmail.com"
vi id_rsa.pub 复制整个key去github->setting-> add new key
ssh -T git@github.com
git config --global user.name "Louis-udm"
git config --global user.email "louis.lv.info@gmail.com"

git remote add origin git@github.com:Louis-udm/savoir-jupyter.git
git push -u origin master (-f 强制)
git push origin master