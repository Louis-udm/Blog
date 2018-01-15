# VS Code + jupyter

> Louis

> 01 jan. 2018

## raccourci
[shortcuts for mac os](https://www.jianshu.com/p/99ae6f886da4)
[shortcuts frequent](https://www.zhihu.com/question/37623310)
[vs code shortcuts for mac os](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)

## installer des plugins:
1. Python (microsoft)
2. Jupyter (Don Jayamanne)
3. Code Outline (Patryk Zawadzki)
4. One Dark Pro
5. vscode-fileheader, utilise: ctrl+opt+i. altinative: topper
7. Bracket Pair Colorizer
6. markdownlint
8. markdown pdf
9. markdown preview, utilise: cmd+k v
10. git history
11. latex Workshop, cmd+shift+p -> latex workshop view latex in new tab

## command:
1. cmd+shift+p, >python: select interpreter #choisir version de python
2. cmd+shift+O #tout les symbols
3. ctrl+cliquer function/variable/symboles
4. rename symbole: sur le symbole, F2
5. debug: cliquer la gauche du numero de ligne.
6. cacher le menu: view->changer le menu, pour re afficher: alt
7. column selection: opt+鼠标点选; opt+shift+鼠标点选或拖动; cmd+opt+上下键，然后shift+左右
8. opt+上下: 交换行

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
    { "key": "ctrl+alt+i",      "command": "extension.addTopHeader",
        "when": "editorTextFocus"}

## reference:
1. [vscode](https://code.visualstudio.com/docs?start=true)
2. [python on vs code](https://code.visualstudio.com/docs/languages/python)
3. [Jupyter for VS code](https://github.com/DonJayamanne/vscodeJupyter)
4. [Jupyter: Getting Started](https://github.com/DonJayamanne/pythonVSCode/wiki/Jupyter:-Getting-Started)

test LaTex in mac:
$$f(z)=\int_{i=1}^{100} \frac{x^2}{y} $$
