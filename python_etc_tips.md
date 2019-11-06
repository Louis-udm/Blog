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

#### 大规模Identity 用pytorch的dropout会出现这个错:
```
Intel MKL ERROR: Parameter 3 was incorrect on entry to viRngBernoulli.
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

#### [torch.dtype和内存](https://pytorch.org/docs/stable/tensors.html):
- float=float32
- double=float64
- half=float16
- uint8, int8, short=int16, int=int32, long=int64
不同数据格式占据的内存或显存大小不同,float64占8字节,float32占4字节
一个float32的matrix的内存占用:(shape[0]*shape[1]*4)/(1024**3) G

#### torch.sparse
- torch_sparse_tensor.mm/matmul(dense_tensor).is_sparse=False

- squeeze sparse:
```
# adj是一个大的sparse，X和Ha是batch x row x dim, 并为0的行号相同
def squeeze_sparse(adj,X,Ha,device):
    N=adj.shape[0]
    batch_size=X.shape[0]
    edge=adj._indices()
    values=adj._values()
    seq=torch.arange(N).to(device)
    valid_col=torch.zeros(edge.shape[1],dtype=torch.uint8).to(device)
    # seq=seq.repeat(batch_size).view(batch_size,-1)
    # new_edges=[]
    sp_list=[]
    restore_ids_list=[]
    x_list=[]
    edge_h_list=[]
    Ha_list=[]
    for i in range(batch_size):
        x_list.append(X[i][(X[i]!=0).sum(-1)!=0,:])
        Ha_list.append(Ha[i][(X[i]!=0).sum(-1)!=0,:])
        valid_ids=seq[(X[i]!=0).sum(-1)!=0]
        empty_ids=seq[(X[i]!=0).sum(-1)==0]
        valid_col*=0
        # 根据adj.X，为0的x会导致对应的adj的列失效，所以将adj中的失效列去除
        # adj的失效列去除后，部分行也会变为失效。但失效的行会少于失效的列
        valid_col = edge[1][np.where(np.isin(edge[1],valid_ids))]
        # valid_col*=0
        # for vid in valid_ids:
        #     valid_col += edge[1]==vid
        new_edge=edge[:,valid_col].clone()
        edge_h_list.append(torch.cat((Ha[i][new_edge[0],:], Ha[i][new_edge[1],:]), dim=-1))
        new_values=values[valid_col].clone()
        # 用于压缩的adj.X后，恢复回vocab大小的串，记录下有效的行的号码
        restore_ids=new_edge[0].unique(sorted=True)
        restore_ids_list.append(restore_ids)
        # 压缩有效列的号码
        for i in range(new_edge.shape[1]):
            new_edge[1][i]=new_edge[1][i]-(empty_ids<new_edge[1][i]).sum()
        #压缩有效行的号码。失效的行会少于失效的列
        row_empty_ids=torch.LongTensor(list(set(empty_ids.tolist())-set(restore_ids.tolist()))).to(device)
        for i in range(new_edge.shape[1]):
            new_edge[0][i]=new_edge[0][i]-(row_empty_ids<new_edge[0][i]).sum()
        sp_list.append(torch.sparse.FloatTensor(new_edge,new_values).to(device))
    return sp_list,x_list,Ha_list,edge_h_list,restore_ids_list
```

## Numpy

#### 选出数组中特定的几个值
```
a=torch.LongTensor([0, 0, 0, 1, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6])
b=torch.LongTensor([0, 2, 4, 5]) 一个set
# 要根据b，从a中获得c=tensor([0,0,0,2,2,4,4,5,5])
a[np.where(np.isin(a,b))]
```

#### [scipy.sparse](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.csr_matrix.html)
```
# lil的sparse:
lil.rows: 有多少行就有多少个list,每个list代表该行非零的列
lil.rows=array([list([0, 2]), list([2]), list([0, 1, 2])], dtype=object)

# coo方式最简单:
csr_matrix((data, (row, col)), shape=(3, 3)).toarray()
array([[1, 0, 2],

# scr方式
indptr = np.array([0, 2, 3, 6]) 值对应indices的下标，指示每行在indices的区域, 0行[0:2],1行[2:3],2行[3:6]
indices = np.array([0, 2, 2, 0, 1, 2]) 值代表着在indptre划分下，每行非零的列
data = np.array([1, 2, 3, 4, 5, 6])
csr_matrix((data, indices, indptr), shape=(3, 3)).toarray()
array([[1, 0, 2],
       [0, 0, 3],
       [4, 5, 6]])
```

## pandas

#### How to shuffle a dataframe:
```
from sklearn.utils import shuffle
df = shuffle(df)
```

## OpenCV

