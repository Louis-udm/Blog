# Python, PyTorch, Numpy, OpenCV, SpaCy, NLTK, etc.

> Louis Lv

> 1 mars. 2019


## Python

#### python命令行重定向输出,无缓存:
```
$python -u file.py > file.log 2>&1 &
```

#### vitrual env安装anaconda 和 python, pytorch
```
$conda update conda
$conda update anaconda
$conda install python=3.7.2 anaconda=custom
$conda create --name py37 python=3.7.2
$source ~/anaconda3/bin/activate py36
$source ~/anaconda3/bin/activate py37
$pip install hmmlearn

$conda list |grep python

$conda install --name py37 -U spacy[cuda90]
$python3 -m spacy download en
$python3 -m spacy download fr

$conda install -n py37 pytorch torchvision -c pytorch
```



## PyTorch

#### 如何从3维Tensor中针对每个2维matix选择不同的element? 扔出一张二向箔!
```
batch_transitions = self.transitions.expand(batch_size,self.num_labels,self.num_labels)
# 先对3维Tensor进行降维打击
batch_transitions = batch_transitions.flatten(1)
# 换算indeces:
batch_transitions.gather(-1, (label_ids[:, t]*self.num_labels+label_ids[:, t-1]).view(-1,1)) + feats[:, t].gather(-1, label_ids[:, t].view(-1,1)).view(-1,1)
```


## Numpy

## OpenCV

