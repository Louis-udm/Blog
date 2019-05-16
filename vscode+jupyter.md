# Visual Studio Code + jupyter plugin

> Louis

> 14 jan. 2018

## raccourci
[shortcuts for mac os](https://www.jianshu.com/p/99ae6f886da4) </br>
[shortcuts frequent](https://www.zhihu.com/question/37623310) </br>
[shortcuts pdf for mac os](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf) </br>

1. convert to upper: ctrl+shift+U
1. convert to lower: ctrl+shift+L
1. cmd+shift+p, >python: select interpreter #choisir version de python
2. cmd+shift+O #tout les symbols
3. ctrl+cliquer function/variable/symboles
4. rename symbole: sur le symbole, F2
5. debug: cliquer la gauche du numero de ligne.
6. cacher le menu: view->changer le menu, pour re afficher: alt
4. column selection: opt+鼠标点选; opt+shift+鼠标点选或拖动; cmd+opt+上下键，然后shift+左右
8. opt+上下: 交换行
4. opt+左右: 光标按单词移动, cmd+左右:光标到行首行末
4. cmd+f: 查找 opt+cmd+f: 替换
5. opt+cmd+左右: 在tab中切换
6. ctl+-,ctl+shift+-: navigate back, navigate forward
7. shift+alt+f: format the code


## installer des plugins:
1. Python (microsoft)
2. Jupyter (Don Jayamanne) -> python
3. Code Outline (Patryk Zawadzki)
4. One Dark Pro
5. vscode-fileheader, utilise: ctrl+opt+i. altinative: topper
4. Bracket Pair Colorizer
4. markdownlint
4. markdown pdf
4. markdown preview, utilise: cmd+k v
4. git history
4. LaTex Workshop, cmd+shift+p -> latex workshop view latex in new tab
4. Path Intellisense, utilise: quand on tape "./../"
4. output colorizer
5. sftp: cmd+shift+p -> sftp
4. Settings Sync, Upload Key : Shift + Alt + U , Download Key : Shift + Alt + D
5. bracket-padder
6. sftp

## commands(shift+cmd+p):
Trim Trailing Whitespace

## flake8 rules:
[flake8](https://lintlyci.github.io/Flake8Rules/)
```
"python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  // pep8Enabled or flake8Enabled
  "python.linting.pep8Enabled": false,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Path": "/Users/louis/anaconda3/envs/py37/bin/flake8",
  "python.linting.flake8Args": [
    "--max-line-length=160",
    "--ignore=F401,  E265,E402,E302,E305,F403,W291,E128,E303,E116,E126,E301,E228,E262,E201,E202,F811,E261,E127,E241,  E225,E226,E231",
    "--max-complexity=12"
  ],
```

## jupyter dans VS Code:
1. installer la plugin Jupyter de  Don Jayamanne
2. If you want to run cell with Ctrl+Enter, add those code in keybindings.json.
menu->code->preference->raccoucis, add:
```
{ "key": "ctrl+enter",      "command": "jupyter.execCurrentCell",
                                  "when": "editorTextFocus"
}
```

3. demarrer jupyter notebook dans terminal
4. connecter jupyter server:
cmd+shift+p, >Jupyter: Enter the url of local/remote Jupyter Notebook, enter the token.

5. new py file, enter:
```
#%%
import matplotlib.pyplot as plt
import matplotlib as mpl
import numpy as np

x = np.linspace(0, 20, 100)
plt.plot(x, np.sin(x))
plt.show()
```

6. cliquer "Run Cell"

## topper
menu->code->preference->raccoucis, add:
    { "key": "ctrl+opt+shift+h",      "command": "topper.addTopHeader.personalProfile",
        "when": "editorTextFocus"}

## settings.json for others:
```
  "files.autoSave": "off",
  "explorer.confirmDelete": false,
  "editor.fontSize": 14,
  "window.zoomLevel": 0,
  "editor.minimap.enabled": false,
  "workbench.colorTheme": "One Dark Pro",
  "python.condaPath": "/Users/louis/anaconda3/condabin/conda",
  "python.venvPath": "/Users/louis/anaconda3/envs/",
  "python.pythonPath": "/Users/louis/anaconda3/envs/py37/bin/python",

  "files.exclude": {
    "**/.git": true,
    "**/.svn": true,
    "**/.hg": true,
    "**/CVS": true,
    "**/.DS_Store": true,
    "**/tmp": true,
    "**/node_modules": true,
    "**/bower_components": true,
    "**/dist": true
  },
"files.watcherExclude": {
    "**/.git/objects/**": true,
    "**/.git/subtree-cache/**": true,
    "**/node_modules/**": true,
    "**/tmp/**": true,
    "**/bower_components/**": true,
    "**/dist/**": true
  }
```

## reference:
1. [vscode](https://code.visualstudio.com/docs?start=true)
2. [python on vs code](https://code.visualstudio.com/docs/languages/python)
3. [Jupyter for VS code](https://github.com/DonJayamanne/vscodeJupyter)
4. [Jupyter: Getting Started](https://github.com/DonJayamanne/pythonVSCode/wiki/Jupyter:-Getting-Started)

test LaTex in markdown:
$$f(z)=\int_{i=1}^{100} \frac{x^2}{y} $$
