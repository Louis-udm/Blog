# VS Code + jupyter

> Louis
> 01 jan. 2018

## installer des plugin:
1. Python (microsoft)

2. Jupyter (Don Jayamanne)

3. Code Outline (Patryk Zawadzki)

## command:
1. cmd+shift+p, >python: select interpreter #choisir version de python
2. cmd+shift+O #tout les symbols
3. ctrl+cliquer function/variable/symboles
4. rename symbole: sur le symbole, F2
5. debug: cliquer la gauche du numero de ligne.
6. cacher le menu: view->changer le menu, pour re afficher: alt

## jupyter dans VS Code:
1. installer la plugin Jupyter de  Don Jayamanne
2. If you want to run cell with Ctrl+Enter, add those code in keybindings.json.
menu->code->preference->raccoucis
{ "key": "ctrl+enter",      "command": "jupyter.execCurrentCell",
                                  "when": "editorTextFocus"
}
3. demarrer jupyter notebook dans terminal
4. connecter jupyter server:
ctrl+shift+p, >Jupyter: Enter the url of local/remote Jupyter Notebook, enter the token.

5. new py file, enter:
#%%
import matplotlib.pyplot as plt
import matplotlib as mpl
import numpy as np

x = np.linspace(0, 20, 100)
plt.plot(x, np.sin(x))
plt.show()

6. cliquer "Run Cell"

## reference:
1. [vscode](https://code.visualstudio.com/docs?start=true)
2. [python on vs code](https://code.visualstudio.com/docs/languages/python)