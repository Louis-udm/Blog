## Pour configurer jupyter comme serveur remote
1. installer Anaconda
2. tester si http://localhost:8888 marche.
> `$jupyter notebook`
3. générer votre password pour entrer jupyter notebook bientôt.
`In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9e60bb8b6:9ffede0.....'`
après avoir eu votre password, ctrl+c arrêter le serveur jupyter.
4. generer un config de jupyter
> `$jupyter notebook --generate-config`
5. modifier le config fichier
> `$vi ~/.jupyter/jupyter_notebook_config.py`
> `c.NotebookApp.ip='*'
c.NotebookApp.password = u'sha1:67c9e60bb8b6:9ffede0.....' #le password qu'on a obtenu.
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888 #ce que on veut `
6. redémarrer le jupyter notebook, maintenant on peut ouvrir http://serverIp:8888 dans un autre ordinateur.