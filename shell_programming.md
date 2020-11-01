## commands
* ''中不解释特殊字符*,$,!,space

* job
ctrl + Z 放入后台,fg到前台，bg jobs_id 后台允许

* source 1.sh和./1.sh的区别

*  If prompted for text at the standard input, you can tell the command you're done entering text with what key combination?
Ctrl + D (Windows) or Command + D (Mac)

* 选取文件中的字段/特定位置和长度的字符： 
$cut -d: -f1,3 /etc/passwd
$cut -c2-50 /etc/passwd

* 合并( by line ) (paste是按列合并，cat是将文件接起来)
paste file1 file2 file3 #默认连接符\t
paste -d+ file1 file2 file3 # 链接符为+
paste -d'$' file1 file2 file3 # 链接符为$
paste -d'\n' file1 file2 file3 # 链接符为\n
paste -d'\0' file1 file2 file3 # 链接符为无
paste -s -d' ' file1 #将文件中所有行拼接为1行,连接符space

* join 根据两个文件的公共字段连接
join file1 file2 (file1/2必须先排好序)

* tools
bc 计算器, sort 排序, grep, wc, tr, sed, awk, uniq, split,  comm/diff
ps aux = ps ef  ,     ps -u $USER
echo "2 ^ 5" |bc
var=$(echo 'scale=2; 10 / 8' | bc)  # var=1.25

* other commands
cal, ls -1, !!, wc file_name, wc -lwc, diff, cat 1.txt 2.txt > all.txt, env, echo *, cd -
chmod a/u/g/o+/-rwx
history, !l/ca/,  !123, !-2
ctrl-R: history search
* 之前打了很长的命令，带很多参数，忘了， !名字或!前几个字符

## vi
2w : move by word (begining), 2words
3e : word end, 3words
4b : move back by word (begining), 4words
$ : move to line end
0/^ : move to line begining
f字符 : move to first 字符
20| : 移动20个字符
:set number / :set nonumber :显示/关闭行号
:20 或者20G : 跳转到行号20
H/M/L : 当前screen的顶/中/底
'' : 返回上一次光标位置
ctrl+F/B/D/U : 前进一页/后退一页/前进半页/后退半页
-/+ : 上/下一行行首
5dd/5x/dw/5dw/D/df字符 : 5line/5chars/word/5words/delete_cursor_to_line_end/del_cursor_to_字符
:5d/4,8d : delte 5th/4th~8th line
:1,$d : empty file
u : undo last action
U : undo all for current line
ctral-r : redo
J : join two lines
cc : 和dd不同，删除当前行但是留着位置，并可直接输入
C : 删除cursor到行尾，但是马上可以输入(D不能马上输入)
:s/original/replace/g g表示全部替换
:19 s/original/replace/g 全部替换19行中的。。。
7yy -> p/P : 复制并粘贴7行
7yw/yf字符 -> p/P
:2 copy/move 4  : 第二行被复制/移动到第4行后面
:2,4 copy 7 : 2~4 copy to 8th
:2,4 copy 0/$ : 2~4 copy to beging/end of the file
:,4 co 7 : current line to 4th line copy to 8th
:2,4 w/write temp1 : 将第2~4行写到temp1
:2,4 w/write >> temp1 : 将第2~4行追加到temp1
:$ r/read temp1 : 将temp1文件内容插入到某行，这里是当前文件末尾
:r/read temp1 : 将temp1文件内容插入到当前光标后面
:! ls/date/cal/cal 2003 :在vi下运行命令
:read ! command :将命令输出内容写到当前光标后面
$vi file1 file2 file3, 然后(:w ->) :n 切换当前编辑到文件, :rew又从文件1开始
:set autoindent/ai/noai 打开/关闭自动缩进, ctal+D配合减小缩进
:set ignorecase/ic 可以让搜索忽略大小写
:set list/nolist 显示/关闭特殊字符
:set showmatch 显示括号匹配
在编辑模式下ctrl-v之后可以输入特殊字符，比如ctrl-v enter


## shell programming
> https://www.runoob.com/linux/linux-shell-array.html
#!/bin/sh
### 特殊符号
* 在 ',",\ 里面时，都不被解释
* < & | > ; space  在引号里面，不被解释
* $变量在"里面会被解释，但在'里面不被解释
* 不解释引号: "'" or \'   ,   '"' or \"
* 反斜杠, 不解释\\, '\' ,  解释"\"
* 在"中被解释的有 $,`,!,\

### 参数
* 获得脚步参数 $1, $2, $*一起的全部参数, $@分开的全部参数, $0脚本, $#参数数量, $$当前脚本进程pid
* echo $? 刚运行完成的程序的返回值,0正常结束
* $@ treats each quoted argument as a separate entity. $* treats the entire argument string as one entity

### debug
* -x
解释每一行，显示语句(+)，然后执行
可以在脚本中用set -x  , 也可以用$bash -x script.sh

### 感叹号
#https://blog.csdn.net/hepeng597/article/details/8057692
#感叹号是可以引用间接变量的值
aaa=123
bbb=aaa
echo $bbb
echo ${!bbb}
输出:aaa和123

### eval的使用
aaa=123
bbb=aaa
echo $bbb
eval ccc=\${$bbb}
输出结果：
aaa
123

### 数组
array_name=(value1 value2 ... valuen)
my_array=(A B "C" D)
array_name[0]=value0
* 读取数组元素
${array_name[index]}
* 使用@ 或 * 可以获取数组中的所有元素
echo "数组的元素为: ${my_array[*]}"
echo "数组的元素为: ${my_array[@]}"
* 数组长度
echo "数组元素个数为: ${#my_array[*]}"
echo "数组元素个数为: ${#my_array[@]}"
* 选取多个
${my_array[@]:从0开始算的idx:个数}
${my_array[@]:0:1}
${my_array[@]::2}
${my_array[@]:2}

### string 
* length of str
length of str: ${#str}
* 查找替换string中的substring
VAR="This old man came rolling"
echo "${VAR//man/rolling}"
-> This old rolling came rolling

### 基本结构

#### 赋值
* let or (())
aa=20
let aa=20
let aa=$aa+4 or ((aa=$aa+4))
echo $aa
* 一条语句: echo $((aa+4)) #这里面的aa不用$
* 注意执行语句是: var=$(date)

#### export 传递变量
export 要在当前shell执行，子shell中才能使用对应变量。并且除非使用source执行脚本，export是逐级往下传，不能传回

#### if
* 两条语句在同一行用';', 可以不用缩进。
* 可以在一行内，用;写上所有句子
if date ; then  # date执行完返回0, 如果程序不是返回0，则[]结果是false
    echo it worked, u r $USER
elif [ ... ] ; then
  ...
else
    echo it failed
fi

* 判断, eq,ne,lt,gt,ge,le / =, !=, <, >, >=, <=
* '['其实是/bin/test, 而']'是一个参数
[ 6 -eq 7 ] # 全部用space分开
echo $?     # 1
[ 6 -ne 7 ]
echo $?     # 0
* 如果$a和$b一样, [ $a == $b ] 同 (($a ==$b)) 都是返回0
* ==和=一样,比如[ $a == $b ] 和[ $a = $b ]

* [] 同[[]] 的区别
> https://www.cnblogs.com/zeweiwu/p/5485711.html
在[[中使用&&和||表示逻辑与和逻辑或。[ 中使用 -a 和 -o 表示逻辑与和逻辑或
使用[[ ... ]]条件判断结构，而不是[ ... ]，能够防止脚本中的许多逻辑错误。比如，&&、||、<和> 操作符能够正常存在于[[ ]]条件判断结构中，
但是如果出现在[ ]结构中的话，会报错。比如可以直接使用if [[ $a != 1 && $a != 2 ]]
如果不使用双括号, 则为if [ $a -ne 1] && [ $a != 2 ]或者if [ $a -ne 1 -a $a != 2 ]
bash把双中括号中的表达式看作一个单独的元素，并返回一个退出状态码。
[[支持字符串的模式匹配，使用=~操作符时甚至支持shell的正则表达式。字符串比较时可以把右边的作为一个模式，而不仅仅是一个字符串，比如[[ hello == hell? ]]，结果为真。[[ ]] 中匹配字符串或通配符，不需要引号。

* [任何非空字符串] =True
[ hello ]
echo $?     #0
[ $hello ]
echo $?     #1
[ $USER ]
echo $?     #0
[ -f /etc/passwd ]  #文件存在？
echo $?     #0
[ ! -d $date_dir ]  #不存在目录xxx?
[ -r $fname ]   #fname是个可读文件?

#### loop
* for
for ff in file1 file2 file3
do
    head -3 $ff
    ls -l $ff
    wc $ff
done

* for arg in $*

* while...do...
aa=0
clear
while [ $aa -le 10 ] ; do
    echo $aa
    sleep 1
    ((aa=$aa+1))
done

#### switch
aa='B' # or aa=B
echo $aa
case $aa in 
    A) date ;;     # ;;表示该case分支结束，跳到esac
    B) cal ;;
    C) ls ;;
    D) who | wc -l ;;
esac

### examples
#### menu
clear
echo "1) grep $USER /etc/passwd"      # 这5个echo可以该用cat<<++ \n text \n ++ 来实现
echo "2) ypcat passwd |grep $USER"
echo "3) who | sort | awk '{print $1} |uniq -c"
echo "What command do you want to run?"
echo Select 1  2  or  3
read choice
echo you selected number $choice
case $choice in
    1) grep $USER /etc/passwd ;;
    2) ypcat passwd |grep $USER ;;
    3) who | sort | awk '{print $1}' | uniq -c ;;
    x|q|X|Q) exit 0
    *) echo "invalid choice!" ;;
esac

#### getopts 获得命令行选项
while getopts dc:lw option; do   # dclw是选项, c: 表示l选项有参数(-c ttt) ,  option是赋值变量
    case $option in
        d) ;;
        c) cal $OPTARG ;;   # 这个就是-c ttt中的ttt
        l) ;;
        w) ;;
    esac
done

#### 要求只输入一个参数
if [ "$2" ] ; then
  cat<<EOH-A
You entered more than one argument. ...
...
...
EOH-A
  exit 1
elif [ ... ] ; then
  cat<<EOH-B
You entered more than one argument. ...
...
...
EOH-B
  exit 1
else
  ...
fi

#### search sub string
> http://www.tldp.org/LDP/abs/html/parameter-substitution.html#PSUB2
* ${var#Pattern}  # Remove from $var the shortest part of $Pattern that matches the front end of $var.
* ${var##Pattern}  # Remove from $var the longest part of $Pattern that matches the front end of $var.
* ${var%Pattern}/${var%%Pattern}  # to remove pattern that matches the back end of $var
* examples:
text="//ABC/REC/TLC/SC-prod/1f9/20/00000000957481f9-08d035805a5c94bf"
echo ${text##*/}
echo ${text#*/}
echo ${text%/*}
echo ${text%%/*}

#### 不要用 for file in $(ls *.txt) 用 for file in *
因为如果有文件名带有空格，会导致失败
for name in *; do echo $name; done

#### bash函数
#!/bin/bash
#!/usr/bin/env bash
#bash可以定义函数
square() {
    echo "square of $1 is \c"
    echo "$1*$1" | bc
}
power() {
    echo "2^$1"|bc
}
#调用: 
power 5
#程序主体
while getopts s:c:p: option ; do
    case $option in
        s) square $OPTARG ;;
        c) cube $OPTARG ;;
        p) power $OPTARG ;;
    esac
done

#运行sh:
xxx.sh -p 5

#### bash, c style loop and map
#!/usr/bin/env bash
declare -A ARRAY=([user1]=bob [user2]=ted [user3]=sally)
echo ${ARRAY[user2]}
echo ${ARRAY['user2']}
KEYS=(${!ARRAY[@]})  # get all keys: user1,user2,user3
for (( i=0; $i < ${#ARRAY[@]}; i+=1 ));do
        echo ${KEYS[$i]} - ${ARRAY[${KEYS[$i]}]}
done

#### [[]] and =~ regexp
#!/usr/bin/env bash
read -p "Enter text " var
if [[ "$var" =~ "^[0-9]+$" ]];then
    echo "Is numeric"
else
    echo "Is not numeric"
fi

#### loop echo files
for i in $(ls); do echo ${i}; done
for i in * 更好