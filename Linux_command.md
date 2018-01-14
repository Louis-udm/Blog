# Linux command

### [xargs](http://blog.csdn.net/chenzrcd/article/details/50186881): （1）将前一个命令的标准输出传递给下一个命令，作为它的参数，xargs的默认命令是echo，空格是默认定界符（2）将多行输入转换为单行

-n： 指定一次处理的参数个数
-d： 自定义参数界定符
-p： 询问是否运行 later command 参数
-t ： 表示先打印命令，然后再执行
-i ： 逐项处理

ls -exec du

将所有文件重命名，逐项处理每个参数 ls *.txt |xargs -t -i mv {} {}.bak

### 和xargs不同，exec参数是一个一个传递的，并且不需要使用|

find . -name "*.py" -exec grep -i 'function' {} \;  #grep -i忽略字母大小写

find . -name '*.txt' -exec du {} \;

### Less -SN :打开文本文件

### head -n :查看文件前n行

### tail -n :查看文件后n行

### wc -l :计算文本文件的行数

### [awk](http://wiki.jikexueyuan.com/project/learn-linux-step-by-step/regular-expression-and-its-application.html)

### [sed](http://wiki.jikexueyuan.com/project/learn-linux-step-by-step/regular-expression-and-its-application.html)

### [sort](http://wiki.jikexueyuan.com/project/learn-linux-step-by-step/pipe-command.html)

### du -sh ./ :查看当前目录所占空间大小 #du -sm

### bc -l :计算器

### [VI](http://wiki.jikexueyuan.com/project/learn-linux-step-by-step/vim-and-vi-common-commands.html)

### find / -user bandit7 -group bandit6 2>/dev/null  #三个描述文件: 2 stderr  1 stdout 0 stdin

### cat(concatenate)

语法：cat [-AbEnTv]
选项与参数：
-A：相当于-vET 的整合参数
-b:列出行号，仅针对非空白行做行号显示
-n:输出行号，空白与非空白都会列出
-E:将结尾的断行字符￥显示出来
-v:列出一些看不出的特殊字符
-T:将 Tab 按键以∧I 显示出来

### nl [-bnw] 文件

选项与参数：
-b：指定行号指定的方式，主要有两种：
-b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
-b t ：如果有空行，空的那一行不要列出行号(默认值)；
-n：列出行号表示的方法，主要有三种：
-n ln ：行号在萤幕的最左方显示；
-n rn ：行号在自己栏位的最右方显示，且不加 0 ；
-n rz ：行号在自己栏位的最右方显示，且加 0 ；
-w：行号栏位的占用的位数。

### less:

空白键 ：向下翻动一页；
[pagedown]：向下翻动一页；
[pageup] ：向上翻动一页；
/字串 ：向下搜寻『字串』的功能；
?字串 ：向上搜寻『字串』的功能；
n ：重复前一个搜寻 (与 / 或 ? 有关！)
N ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
q ：离开 less 这个程序；

### head [-nnumber] 文件

选项与参数:
-n:后面接数字，代表行数
number 默认值是10 当 number 是负数，代表列出前面所有行数但是不包括后面 number 行

### tail [-nnumber] 文件

选项与参数:
-n:后面接数字，代表行数
number 默认值是10 当 number 是正数（+ number），代表该文件从 number 以后才会列出来

### touch[-acdmt] 文件

选项与参数：
-a:仅修改访问时间 atime
-c:仅修改文件的时间，若该文件不存在则不创建新文件
-d:后面可接欲修改的日期，也可以使用—date=”时间或日期”
-m:仅修改 mtime
-t:后面可以接欲修改的时间
主要功能：
创建一个空文件
修改文件日期(mtime,atime)

### jobs[-lrs]

选项与参数：
-l:列出工作与命令串之外，同时列出 PID
-r:仅列出后台 run 的工作
-s: 仅列出后台 stop 的工作

### fg %工作序号

fg –
fg +


### 查询文件被哪个进程使用(fuser)
语法：fuser[-umv] [-ki -signal] file/dir
选项与参数：
-u:列出 user
-m:会查询后面文件所在的文件系统
-v:列出详细命令
-k:找出使用该文件 PID 并试图发送 SIGKILL 这个信号给这个进程
-i:与-k 连用，发送信号之前询问用户

### 查询进程正在使用的文件(lsof)
语法:lsof [-aUu][+d]
选项与参数：
-a ：多项数据需要同时成立才显示出结果时
-U ：仅列出 Unixlike 系统的 socket 文件类型；
-u ：后面接 username，列出该使用者相关程序所开启的文件；
+d ：后面接目录,找出某个目录底下已经被开启的文件


### grep '^#' git-and-github-readme.md #查找以#开始的那一行

### sed [-nefr] ‘动作’
选项与参数：
-n:silent 模式，只将 sed 处理过的内容显示 i 出来
-e:设置多个 sed 动作
-f filename: 文件内记录 sed 脚本 scipt
-r:sed 支持的扩展正则表达式语法（默认是基础正则表达式）
-i:直接修改读取文件内容，而不是屏幕输出
动作：n1,n2function
n1,n2不一定存在
function:
a：新增，a 后面接字符串，这些字符在当前的下一行显示
c：替换，c 后面接字符串，这些字符替换 n1-n2之间的行
d：删除，删除 n1-n2之间的行
i：添加，i 后面接字符串，这些字符在当前的上一行显示
p: 打印，打印 n1~n2行之间的数据
s: 替换以关键字形式替换，并不是替换整行. sed ‘s/旧字符串/新字符串/g’’

### cat /etc/passwd |sort -t ':' -k 3

### sort可以按照不同的数据类型来排序，例如按数字或文字排序，排序结果也受语系编码的影响，例如有的语系字符是这么排序的 AaBbCc….建议语系使用 LANG=C
语法：sort [-fbMnrtuk]文件或输入流
选项与参数：
-f:忽略大小写
-b:忽略最前面的空格
-M:以月份(英文)来排序
-r:反向排序
-t:分隔符与-k 连用
-u:就是 uniq
-k:以那个 field 的进行排序

### shutdown [-t 秒][arkhncfF] 时间 [警告信息]
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

### 切换执行等级 init
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


部分摘自[一步一步学 Linux](http://wiki.jikexueyuan.com/project/learn-linux-step-by-step/file-and-directory-management.html)
