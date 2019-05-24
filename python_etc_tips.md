# Python, PyTorch, Numpy, OpenCV, SpaCy, NLTK, etc.

> Louis Lv

> 1 mars. 2019


## Python

#### 调试/debug:
- [in jupyter](https://github.com/Microsoft/vscode-python/issues/4302)
- [pdb,ipdb,pudb,rpdb,ripdb](https://blog.51cto.com/tengxiansheng/1879356)
    - https://www.ibm.com/developerworks/cn/linux/l-cn-pythondebugger/
    - http://python.jobbole.com/82638/
- [pudb](https://www.cnblogs.com/journeyonmyway/p/5892560.html)

#### 如何导入py文件所在目录的相对路径下的其他py文件（特别是当python安装库中有同名的时候）:
```
from .pytorch_pretrained_bert.tokenization import BertTokenizer
from . import offensive_bert_lib as off_lib
```
注意执行的时候，不能用python dir/myfile.py 要用python -m dir.myfile  
[详细解释](https://stackoverflow.com/questions/16981921/relative-imports-in-python-3)

#### python命令行重定向输出,无缓存;nohup可以忽略当前的login的session被关闭:
```
$nohup python -u file.py >> file.log 2>&1 &
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
# transition是列z(t-1)->行z(t)，所以label_ids[:, t]*self.num_labels先定位行
batch_transitions.gather(-1, (label_ids[:, t]*self.num_labels+label_ids[:, t-1]).view(-1,1)) + feats[:, t].gather(-1, label_ids[:, t].view(-1,1)).view(-1,1)
```

#### 一个3d tensor, shape=5,3,8，给定一个序列比如3,2,7,0,5表示这个tensor中的逐个2d Matrix中对应的列，比如第一个数字3表示第一个matrix的第3列，如何不用for循环把序列对应的各列选出来？
```
ind=torch.tensor([3,2,7,0,5]).long() #列号
A.transpose(-1,1).contiguous().view(-1,3)[np.arange(0,40,8)+ind,:]
#或者先变换到2d，再计算地址，然后用gather

# 对一个3d tensor的不同matrix的不同行进行选择：
ind=torch.tensor([3,2,7,0,5]).long() #行号
A.view(-1,A.shape(-1))[( torch.arange(A.shape(0)) *A.shape(1) ).to(device) +ind]
```

#### 2d matrix,如何多次选择不同列，形成一个tensor:
```
A=torch.tensor([[1., 2.], [ 5.,  6.],[ 7.,  8.]])
A[:,[0,2,1,0,0]]
```

#### 2d matrix, ind指示各行的特定列，选出ind指定的这些元素:
```
# method1:
A=torch.randn(5,6)
ind = torch.Tensor([1,0,5,3,4]).long()
A.gather(1,ind.view(-1,1))

# method2:
A.take(torch.arange(0,5)*6+ind)

# method3(可以修改元素值):
A.view(-1)[torch.arange(0,5)*6+ind]=0
```

#### 释放GPU显存
```
del some_object
torch.cuda.empty_cache()
```

#### torch.Tensor(np.array), torch.tensor(np.array,dtype=torch.float64), torch.from_numpy(np.array)
- torch.tensor()会拷贝（而不是直接引用）; torch.tensor(np.array).to(device),会在新建的时候先拷贝到内存，然后移到device后删了内存空间
- torch.Tensor()通常就是torch.FloatTensor(), 共享原数据的内存,但使用float32,所以当同原数据类型不一致时，就会拷贝出新的内存空间
- torch.from_numpy() 共享原数据内存，并使用**同原数据一致的数据类型**, 比如numpy.float64->torch.DoubleTensor; torch.from_numpy(np.array).to(device)，直接把已有数据移到device并删了原数据的内存空间

#### [torch.dtype](https://pytorch.org/docs/stable/tensors.html):
- float=float32
- double=float64
- half=float16
- uint8, int8, short=int16, int=int32, long=int64
不同数据格式占据的内存或显存大小不同,float64占8字节,float32占4字节
一个float32的matrix的内存占用:(shape[0]*shape[1]*4)/(1024**3) G

## Numpy

## OpenCV

