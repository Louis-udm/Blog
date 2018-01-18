# Linux commands frequents

> Louis

> 14 jan. 2017

## 1.文本处理相关命令

### [Raccourci de VI](http://www2.nsysu.edu.tw/csmlab/unix/vi_command.htm)
```
gg返回第一行, nG去第n行，G最后一行, 往下移10行10j, etc
0行首, $行末
A行末插入, I行首插入
o新增下行, O新增上行
H       移至視窗的第一列。
M       移至視窗的中間那列。
L       移至視窗的最後一列。
G       移至該檔案的最後一列。
+       移至下一列的第一個字元處。
-       移至上一列的第一個字元處。
(       移至該句之首。 (!.?)
)       移至該句之末。
{       移至該段落之首。 (以空白行隔開)
}       移至該段落之末。
nG      移至該檔案的第 n 列。
n+      移至游標所在位置之後的第 n 列。
n-      移至游標所在位置之前的第 n 列。
<Ctrl><g>       會顯示該行之行號、檔案名稱、檔案中最末行之行號、游標所在行號佔總行號之百分比。

<Ctrl><f>       視窗往下捲一頁。
<Ctrl><b>       視窗往上捲一頁。
<Ctrl><d>       視窗往下捲半頁。
<Ctrl><u>       視窗往上捲半頁。
<Ctrl><e>       視窗往下捲一行。
<Ctrl><y>       視窗往上捲一行。

.重复上个命令
J句子的連接
dd刪除整行, D删除光标后面到行末，cc修改整行
在指令模式中，可在指令前面加入一數字 n，則此指令動作會重複執行 n次: 10dd, 10yy, 往下移10行10j。
d, y, p, c的范围:
        e       由游標所在位置至該字串的最後一個字元。
        w       由游標所在位置至下一個字串的第一個字元。
        b       由游標所在位置至前一個字串的第一個字元。
        $       由游標所在位置至該行的最後一個字元。
        0       由游標所在位置至該行的第一個字元。
        )       由游標所在位置至下一個句子的第一個字元。
        (       由游標所在位置至該句子的第一個字元。
        {       由游標所在位置至該段落的最後一個字元。
        }       由游標所在位置至該段落的第一個字元。
```

### tr 字符串处理
```
-c或——complerment：取代所有不属于第一字符集的字符；
-d或——delete：删除所有属于第一字符集的字符；
-s或--squeeze-repeats：把连续重复的字符以单独一个字符表示；
-t或--truncate-set1：先删除第一字符集较第二字符集多出的字符。

echo "HELLO WORLD" | tr 'A-Z' 'a-z'
echo "hello 123 world 456" | tr -d '0-9'
echo "thissss is      a text linnnnnnne." | tr -s ' sn' #用tr压缩字符，可以压缩输入中重复的字符
head -n 60 zola1.txt | tr ' ' '\012' | grep -i ven #\012=\n

#删除Windows文件“造成”的'^M'字符(\r\n)
cat file | tr -d "\r" > new_file
cat file | tr -s "\r" "\n" > new_file

cat file | tr -s [a-zA-Z] > new_file #删除“连续着的”重复字母，只保留第一个
cat file | tr -s "\n" > new_file #删除空行
```

### wc 
-l :计算文本文件的行数 </br>
-c :计算字数 </br>
-c或--bytes或——chars：只显示Bytes数；</br>

### [awk的工作原理](http://man.linuxde.net/awk)
awk 'BEGIN{ commands } pattern{ commands } END{ commands }' </br>
第一步：执行BEGIN{ commands }语句块中的语句；可略；</br>
第二步：从文件或标准输入(stdin)读取一行，然后执行pattern{ commands }语句块，**可以有pattern也可以没有。** 它逐行扫描文件，从第一行到最后一行重复这个过程，直到文件全部被读取完毕。</br>
第三步：当读至输入流末尾时，执行END{ commands }语句块；可略 </br>
awk中还可以使用if, for , while, do 等语句；可以定义数组等变量 </br>
**注意: awk中split函数返回的数组从1开始**

```
$ awk 'BEGIN{ i=0 } { i++ } END{ print i }' filename
$ head -n 200 zola1.txt | tr ' ' '\012' | grep -i "∧ven" | sort | uniq -c | sort -k1,1nr | awk 'print $2' #awk只将按频率排好序的输出的第二列打印出来，第一列是计数
$ head -n 1000 zola1.txt | tr ' ' '\012' | grep -i "∧ven" | sort | uniq -c | sort -k1,1nr | awk '$1 > 2 {print $0}' #ici la première colonne doit être supérieure à 2. 打印第一列大于2的，print $0就是打印整行
$ seq 5 | awk 'BEGIN{ sum=0; print "总和：" } { print $1"+"; sum+=$1 } END{ print "等于"; print sum }'
$ seq 5 |awk '$1>2 {print $1; print $1*$1}' #对产生的1~5，只打印>2的

~ ~! 匹配正则表达式和不匹配正则表达式
$ awk 'BEGIN{a="100testa";if(a ~ /^100*/){print "ok";}}'

```


### [sed](http://man.linuxde.net/sed)
sed是一种流编辑器，它是文本处理中非常中的工具，能够完美的配合正则表达式使用，功能不同凡响。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有 改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等。

sed [options] 'command' file(s)

sed [-nefr] ‘动作’
```
选项与参数：
-n:silent 模式，只将 sed 处理过的内容显示 i 出来
-e:设置多个 sed 动作
-f filename: 文件内记录 sed 脚本 scipt
-r:sed 支持的扩展正则表达式语法（默认是基础正则表达式）
-i:直接修改读取文件内容，而不是屏幕输出
动作：n1,n2function
n1,n2不一定存在

sed替换标记
g 表示行内全面替换。  
p 表示打印行。  
w 表示把行写入一个文件。  
x 表示互换模板块中的文本和缓冲区中的文本。  
y 表示把一个字符翻译为另外的字符（但是不用于正则表达式）
\1 子串匹配标记
& 已匹配字符串标记

sed元字符集，正则表达式
^ 匹配行开始，如：/^sed/匹配所有以sed开头的行。
$ 匹配行结束，如：/sed$/匹配所有以sed结尾的行。
. 匹配一个非换行符的任意字符，如：/s.d/匹配s后接一个任意字符，最后是d。
* 匹配0个或多个字符，如：/*sed/匹配所有模板是一个或多个空格后紧跟sed的行。
[] 匹配一个指定范围内的字符，如/[ss]ed/匹配sed和Sed。  
[^] 匹配一个不在指定范围内的字符，如：/[^A-RT-Z]ed/匹配不包含A-R和T-Z的一个字母开头，紧跟ed的行。
\(..\) 匹配子串，保存匹配的字符，如s/\(love\)able/\1rs，loveable被替换成lovers。
& 保存搜索字符用来替换其他字符，如s/love/**&**/，love这成**love**。
\< 匹配单词的开始，如:/\<love/匹配包含以love开头的单词的行。
\> 匹配单词的结束，如/love\>/匹配包含以love结尾的单词的行。
x\{m\} 重复字符x，m次，如：/0\{5\}/匹配包含5个0的行。
x\{m,\} 重复字符x，至少m次，如：/0\{5,\}/匹配至少有5个0的行。
x\{m,n\} 重复字符x，至少m次，不多于n次，如：/0\{5,10\}/匹配5~10个0的行。

function:
a：新增，a 后面接字符串，这些字符在当前的下一行显示
c：替换，c 后面接字符串，这些字符替换 n1-n2之间的行
d：删除，删除 n1-n2之间的行
i：添加，i 后面接字符串，这些字符在当前的上一行显示
p: 打印，打印 n1~n2行之间的数据
s: 替换以关键字形式替换，并不是替换整行. sed ‘s/旧字符串/新字符串/g’’
```

exemples:
```
替换操作：s命令
正则表达式 \w\+ 匹配每一个单词，使用 [&] 替换它，& 对应于之前所匹配到的单词：
echo this is a test line | sed 's/\w\+/[&]/g'
删除空白行
sed '/^$/d' file
```

### bc :bc命令是一种支持任意精度的交互执行的计算器语言
```
-i：强制进入交互式模式；
-l：定义使用的标准数学库；
-w：对POSIX bc的扩展给出警告信息；
-q：不打印正常的GNU bc环境信息；
-v：显示指令版本信息；
-h：显示指令的帮助信息。

exemple:
echo "sqrt(10^10)" | bc
```

### cat(concatenate)
```
语法：cat [-AbEnTv]
选项与参数：
-A：相当于-vET 的整合参数
-b:列出行号，仅针对非空白行做行号显示
-n:输出行号，空白与非空白都会列出
-E:将结尾的断行字符￥显示出来
-v:列出一些看不出的特殊字符
-T:将 Tab 按键以∧I 显示出来
```

### nl [-bnw] 文件: 输出文件的同时给出行号
```
选项与参数：
-b：指定行号指定的方式，主要有两种：
-b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
-b t ：如果有空行，空的那一行不要列出行号(默认值)；
-n：列出行号表示的方法，主要有三种：
-n ln ：行号在萤幕的最左方显示；
-n rn ：行号在自己栏位的最右方显示，且不加 0 ；
-n rz ：行号在自己栏位的最右方显示，且加 0 ；
-w：行号栏位的占用的位数。
```

### less:
```
空白键 ：向下翻动一页；
[pagedown]：向下翻动一页；
[pageup] ：向上翻动一页；
/字串 ：向下搜寻『字串』的功能；
?字串 ：向上搜寻『字串』的功能；
n ：重复前一个搜寻 (与 / 或 ? 有关！)
N ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
q ：离开 less 这个程序；

Less -SN :打开文本文件
```

### head [-nnumber] 文件
```
选项与参数:
-n:后面接数字，代表行数; 
number 默认值是10 当 number 是负数，代表列出前面所有行数但是不包括后面 number 行

head -n :查看文件前n行
```

### tail [-nnumber] 文件
```
选项与参数:
-n:后面接数字，代表行数; 
number 默认值是10 当 number 是正数（+ number），代表该文件从 number 以后才会列出来

tail -n :查看文件后n行
```

### touch[-acdmt] 文件
```
选项与参数：
-a:仅修改访问时间 atime
-c:仅修改文件的时间，若该文件不存在则不创建新文件
-d:后面可接欲修改的日期，也可以使用—date=”时间或日期”
-m:仅修改 mtime
-t:后面可以接欲修改的时间
主要功能：
创建一个空文件
修改文件日期(mtime,atime)
```

### grep 
```
-b 在显示符合范本样式的那一行之外，并显示该行之前的内容。
-c 计算符合范本样式的列数。
-C<显示列数>或-<显示列数>  除了显示符合范本样式的那一列之外，并显示该列之前后的内容。
-E 使用扩展正则表达式。
-v 反转查找
-i 忽略字符大小写
-n 在显示符合范本样式的那一列之前，标示出该列的编号
-A,-B,-C 显示匹配某个结果之后/之前/前后的3行，使用 -A 3 选项
-e 多匹配
-r 搜索目录
...

exemple:
$ grep '^#' git-and-github-readme.md #查找以#开始的那一行
$ head -n 200 zola1.txt | tr ' ' '\012' | grep -i "∧ven" | sort | uniq -c #tr将空格变为\n后, 再grep -i忽略字母大小写并查找ven开头的，然后sort排序，再uniq计数
$ grep -E "[1-9]+"
$ echo this is a text line | grep -e "is" -e "line" -o
$ cat utilitaires.py |grep -in -e "function" -e "test"
$ grep "text" . -r -n #在多级目录中对文本进行递归搜索(.当前目录)
```

### uniq 对紧挨着的一样的行进行消冗，并可计数
```
-c或——count：在每列旁边显示该行重复出现的次数；
-d或--repeated：仅显示重复出现的行列；
-f<栏位>或--skip-fields=<栏位>：忽略比较指定的栏位；
-s<字符位置>或--skip-chars=<字符位置>：忽略比较指定的字符；
-u或——unique：仅显示出一次的行列；
-w<字符位置>或--check-chars=<字符位置>：指定要比较的字符。

```

### seq 5
```
产生:
1
2
3
4
5
```

### sort 可以按照不同的数据类型来排序
例如按数字或文字排序，排序结果也受语系编码的影响，例如有的语系字符是这么排序的 AaBbCc….建议语系使用 LANG=C
```
语法：sort [-fbMnrtuk]文件或输入流
选项与参数：
-f:忽略大小写
-b:忽略最前面的空格
-c：检查文件是否已经按照顺序排序；
-i：排序时，除了040至176之间的ASCII字符外，忽略其他的字符；
-m：将几个排序号的文件进行合并；
-o<输出文件>：将排序后的结果存入制定的文件；
-M:以月份(英文)来排序
-r:反向排序
-u:就是 uniq

sort的-n、-r、-k、-t选项的使用,k以那个 field 的进行排序,n作为数字对待，r逆序,t:分隔符与-k 连用:
FStart.CStart Modifie,FEnd.CEnd Modifier
-------Start--------,-------End--------
 FStart.CStart 选项  ,  FEnd.CEnd 选项

$ cat /etc/passwd |sort -t ':' -k 3
$ head -n 200 zola1.txt | tr ' ' '\012' | grep -i "∧ven" | sort | uniq -c | sort -k1,1nr #tr将空格变为\n后, 再grep -i忽略字母大小写并查找ven开头的，然后sort排序，再uniq计数, 最后根据第一列k1倒序r排序
$ sort -t ' ' -k 1.2,1.5 -nrk 3,3 facebook.txt #域分隔符为空格，先安第一个域的第二个字母到第5个字母排序，然后按第三个域以数字方式排序，

```

### iconv转码
$ iconv -f Windows-1252 -t UTF-8 zola1.txt > zola1.utf8.txt

## 2.系统命令

### linux的三个输出描述文件：
```
三个描述文件: 
2 stderr  
1 stdout 
0 stdin

exemple:
$ find / -user bandit7 -group bandit6 2>/dev/null
```

### 去掉换行符“\n”的3种方法
1. cat test.txt | xargs echo -n
2. cat test.txt | tr -d '\n'
3. sed 'N;s/\n//g' test.txt  （最后一行的\n，sed并不处理，原因不明，呵呵）

### [xargs](http://blog.csdn.net/chenzrcd/article/details/50186881): 
（1）将前一个命令的标准输出传递给下一个命令，作为它的参数，xargs的默认命令是echo，空格是默认定界符
（2）将多行输入转换为单行
```
-n： 指定一次处理的参数个数
-d： 自定义参数界定符
-p： 询问是否运行 later command 参数
-t ： 表示先打印命令，然后再执行
-i ： 逐项处理
```
将所有文件重命名，逐项处理每个参数 ls *.txt |xargs -t -i mv {} {}.bak

### exec: 和xargs不同，exec参数是一个一个传递的，并且不需要使用|
```
ls -exec du
find . -name "*.py" -exec grep -i 'function' {} \;  #grep -i忽略字母大小写
find . -name '*.txt' -exec du {} \;
```

### du -sh ./ :查看当前目录所占空间大小 
```
$ du -sh ./ :查看当前目录所占空间大小 
$ du -sh *
$ du -sm 
```

### jobs[-lrs]
```
选项与参数：
-l:列出工作与命令串之外，同时列出 PID
-r:仅列出后台 run 的工作
-s: 仅列出后台 stop 的工作
```

### fg %工作序号
```
fg –
fg +
```

### fuser 查询文件被哪个进程使用(fuser)
```
语法：fuser[-umv] [-ki -signal] file/dir
选项与参数：
-u:列出 user
-m:会查询后面文件所在的文件系统
-v:列出详细命令
-k:找出使用该文件 PID 并试图发送 SIGKILL 这个信号给这个进程
-i:与-k 连用，发送信号之前询问用户
```

### lsof 查询进程正在使用的文件(lsof)
```
语法:lsof [-aUu][+d]
选项与参数：
-a ：多项数据需要同时成立才显示出结果时
-U ：仅列出 Unixlike 系统的 socket 文件类型；
-u ：后面接 username，列出该使用者相关程序所开启的文件；
+d ：后面接目录,找出某个目录底下已经被开启的文件
```

### shutdown [-t 秒][arkhncfF] 时间 [警告信息]
```
-t sec: -t 后面加秒数，也即“过几秒后关机”的意思
-k:不是真的关机，知识发出警告信息
-r:将系统服务停掉后立即重启
-h:将系统服务停掉后立即关机
-n:不经过 init 程序,直接以 shutdown 功能来关机
-f:关机并开机之后，强制略过 fsck 的磁盘检查
-F:系统重启之后，强制进行 fsck 的磁盘检查
-c:取消已经在进行的 shutdown 命令内容
举例：
shutdown -h 10 “I will shutdown after 10 mins”
告诉大家10分钟后服务器重启
shutdown –h now
立刻关机
shutdown -h 20:15
20:15分自动关机
Shutdown -r now
立刻重启启动
Shutdown -r 30 “The System will Reboot”
30分钟后重新启动并通知在线用户
Shutdown -k now “The System will Reboot”
仅发出警告，并不会重启
```

### init 切换执行等级 init
```
linux 操作系统自从开始启动至启动完毕需要经历几个不同的阶段，这几个阶段就叫做 runlevel，通常有8个 runlevel
Runlevel System State
0 Halt the system
1 Single user mode
2 Basic multi user mode
3 Multi user mode
5 Multi user mode with GUI
6 Reboot the system
S, s Single user mode
多数的桌面的 linux 系统缺省的 runlevel 是5，用户登陆时是图形界面，而多数的服务器版本的linux 系统缺省的 runlevel 是3，用户登陆时是字符界面，runlevel 1和2除了调试之外很少使用，runlevel s 和 S 并不是直接给用户使用，而是用来为 Single user mode 作准备。
```

### 修改系统编码
$ locale
$ locale -a |grep ISO
临时改变local语言
$ export LANG=fr_FR.ISO8859-1

## 3.网络命令

### netstat
```
netstat -rn   #显示核心路由信息

netstat -a     #列出所有端口
netstat -at    #列出所有tcp端口
netstat -au    #列出所有udp端口  

netstat -l        #只显示监听端口
netstat -lt       #只列出所有监听 tcp 端口
netstat -lu       #只列出所有监听 udp 端口
netstat -lx       #只列出所有监听 UNIX 端口

netstat -pt        #在netstat输出中显示 PID 和进程名称
```

## 4. iTerm2快捷键配置：
* 先删除profile中的opt+<- 和opt+->
* keys中添加opt+<- : esc,b , opt+-> : esc,f
* keys中添加opt+del : 0x1b 0x08
* keys中修改cmd+<- : esc,[H , cmd+->: esc,[F
* keys中修改cmd+shift<- : prev tab, cmd+shift->: next tab

#### 部分摘自:
* [一步一步学 Linux](http://wiki.jikexueyuan.com/project/learn-linux-step-by-step/file-and-directory-management.html)
* [Linux命令大全](http://man.linuxde.net/)
