# TP0 reponse
> louis

> 18 janv. 2018

0)https://www.jianshu.com/p/a3f9ad8a5715
tr：translate的简写，主要用于压缩重复字符，删除文件中的控制字符以及进行字符转换操作。
grep：更适合单纯的查找或匹配文本
sed：更适合编辑匹配到的文本
cut：适合对文本进行截取操作
awk：更适合格式化文本，对文本进行较复杂格式处理


1) découper un texte en mots
#iconv转换编码
iconv -f Windows-1252 -t UTF-8 zola1.txt > zola1.utf8.txt

#awk: http://www.ruanyifeng.com/blog/2018/11/awk.html
#https://juejin.im/post/6844904148463304718
#https://blog.csdn.net/weiwangchao_/article/details/79730919
#https://www.cnblogs.com/chengmo/archive/2010/10/08/1845913.html
#awk工作流程是这样的：读入有’\n’换行符(可以修改)分割的一条记录，然后将记录按指定的域分隔符划分域，填充域，0则表示所有域,1表示第一个域,...
#{}表示处理动作action
#$0代表当前行,
#每一行分成若干字段，依次用$1、$2、$3代表第一个字段、第二个字段、第三个字段等等。
#变量NF表示当前行有多少个字段，因此$NF就代表最后一个字段,$(NF-1)代表倒数第二个字段
#变量NR表示当前处理的是第几行
#分隔符非空格时，要用-F参数指定分隔符
#查找: /regexp/
cat zola1.txt   | awk 'BEGIN {ok=0} \
                      /DEBUT DU FICHIER/ {ok = 1} \
                      /FIN DU FICHIER/ {ok=0} \
                      {if (ok>10) print $0; else if (ok) ok++}'

等价
cat zola1.txt   | awk 'BEGIN {ok=0}; \
                      /DEBUT DU FICHIER/ {ok = 1}; \
                      /FIN DU FICHIER/ {ok=0}; \
{if (ok>10) {print $0} else if (ok) {ok++}}'

#découper un texte en mots
cat zola1.txt   | awk 'BEGIN {
                        ok=0  # 读入每一行前，设置ok=0
                      } \    
                      /DEBUT DU FICHIER/ {ok = 1} \   # 查到DEBUT DU FICHIER时设置ok=1
                      /FIN DU FICHIER/ {ok=0} \   # 查到FIN DU FICHIER时设置ok=0
                      {
                        if (ok>10) {            # 包含debut...在内的前10行不要
                          i=1;
                          while (i<=NF) {       # 对每一行打印所有段
                            print $i;
                            i++;
                          }
                        } else if (ok) {    # ok=0不action
                          ok++
                        }
                      } \
                      END {
                        ...
                      }
                      '

2) calculer une table de fréquence
#sed中的s表示替换，tr中的-s表示消冗, tr只能处理字符
#第一步:
#保留标点，但分隔开
./scorie.sh zola1.utf8.txt |sed -e "s/'/' /g" -e "s/\./ \. /g" -e "s/\?/ \? /g" -e "s/\!/ \! /g" -e "s/,/ , /g" -e "s/;/ ; /g" -e "s/-/ - /g" -e "s/;/ ; /g" -e "s/\"/ \" /g" -e "s/:/ : /g" |tr 'A-Z' 'a-z' |tr -d '\r' |tr -s '\n' |tr -s ' ' >zola-pretraite.txt
去除标点
./scorie.sh zola1.utf8.txt |sed -e "s/'/ /g" -e "s/\./ /g" -e "s/\?/ /g" -e "s/\!/ /g" -e "s/,/ /g" -e "s/;/ /g" -e "s/-/ /g" -e "s/;/ /g" -e "s/\"/ /g" -e "s/:/ /g" |tr 'A-Z' 'a-z' |tr -d '\r' |tr -s '\n' |tr -s ' ' >zola-pretraite.txt
或者:
./scorie.sh zola1.utf8.txt | ./tokenizer.sh >zola-pretraite.txt
#第二步:
cat zola-pretraite.txt |tr ' ' '\n'|sort|uniq -c|sort -k1,1nr|less

* to permanently remove empty lines from a file called textfile, which command could you use?
sed -i '/^$/d' textfile

3) lister tous les mots d'un texte qui ...
1.ont exactement 4 caractères
#uniq -c 具有计数功能，sort -k 指定按列排序,n按数字排序r降序, -k 1,1nr表示只对列1并且按数字降序排序， 如果是-k 1,3，表示列1,2,3
#grep -iE: 忽略大小写，extend regexp
cat zola-pretraite.txt |tr ' ' '\n'|grep -iE "^....$" |sort|uniq -c|sort -k 1,1nr|more

2.commencent par le préfixe p
cat zola-pretraite.txt |tr ' ' '\n'|grep -iE "^abom" |sort|uniq -c|sort -k 1,1nr|less

3.terminent par le suffixe s
cat zola-pretraite.txt |tr ' ' '\n'|grep -iE "lissait$" |sort|uniq -c|sort -k 1,1nr|more

4.contiennent au milieu (ni au début, ni à la fin) la chaîne a
cat zola-pretraite.txt |tr ' ' '\n'|grep -iE '[^g]glou[^u]' |sort|uniq -c|sort -k 1,1nr|more

5.sont vus au moins n fois dans le texte
cat zola-pretraite.txt |tr ' ' '\n'|sort|uniq -c|sort -k1,1nr|awk '$1>2000 {print $0}'

6.exactement n fois dans le texte
cat zola-pretraite.txt |tr ' ' '\n'|sort|uniq -c|sort -k1,1nr|awk '$1==108 {print $0}'

7.lus à l'envers sont encore des mots
#test
awk '\
BEGIN{\
  while("cat zola-pretraite.txt" | getline out){\
    lens=split(out,array," ");\
    print lens;\
  }\
  close("zola-pretraite.txt");\
}'
#正确
cat zola-pretraite.txt |tr -s ' '|tr ' ' '\n'|sort|uniq |tr '\n' ' '|awk '{
  lens=split($0,array," ");
  for (i=1;i<=lens;i++){
    if (length(array[i])>3){
      cmd="echo \"" array[i] "\" | rev"; # rev 反转顺序
      cmd |getline envstr;
      for (j=1;j<=lens;j++){
        if (array[j]==envstr){
          print array[i] " " envstr "\n";
          break;
        }
      }
      close(cmd);
    }
  }
}'

5) calculer les bigrammes et leur fréquence
#不对
cat zola-pretraite.txt |tr ' ' '\n' | paste -d ' ' - - |sort|uniq -c|sort -k1,1nr|less
#正确
cat zola-pretraite.txt  |tr '\n' ' ' |tr -s ' ' |awk '{
  lens=split($0,array," ");
  for (i=1;i<lens;i++){
    print array[i] " " array[i+1];
  }
}' |sort|uniq -c|sort -k1,1nr|more

6) Écrire un programme capable de lister l'ensemble des trigrammes d'un texte et d'afficher leur fréquence d'occurrence
cat zola-pretraite.txt  |tr '\n' ' ' |tr -s ' ' |awk '{
  lens=split($0,array," ");
  for (i=1;i<lens-1;i++){
    print array[i] " " array[i+1] " " array[i+2];
  }
}' |sort|uniq -c|sort -k1,1nr|more

7) afficher les mots qui suivent un mot donné
cat zola-pretraite.txt  |tr '\n' ' ' |tr -s ' ' |awk '{
  lens=split($0,array," ");
  for (i=1;i<lens;i++){
    print array[i] " " array[i+1];
  }
}' |sort|uniq -c|sort -k1,1nr| grep -iE " le " |more

8) afficher les mots qui suivent un bigramme donné
cat zola-pretraite.txt  |tr '\n' ' ' |tr -s ' ' |awk '{
  lens=split($0,array," ");
  for (i=1;i<lens-1;i++){
    print array[i] " " array[i+1] " " array[i+2];
  }
}' |sort|uniq -c|sort -k1,1nr | grep -iE " la belle " |more

9)afficher les bigrammes et trigrammes caractéristiques d'un texte (un mot assez long)
cat zola-pretraite.txt  |tr '\n' ' ' |tr -s ' ' |awk '{
  lens=split($0,array," ");
  for (i=1;i<lens-1;i++){
    if (length(array[i])>6 || length(array[i+1])>6 || length(array[i+2])>6){
      print array[i] " " array[i+1] " " array[i+2];
    }
  }
}' |sort|uniq -c|sort -k1,1nr|more

10) récupérer une liste de mots outils du français
iconv -f ISO-8859-1 -t UTF-8 stop.txt > stop.utf8.txt

cat stop.utf8.txt |sed 's/\ \{1,\}\|.*//g' |sed  '/^$/d' | wc -w

11) retirer les mots outils dans un texte
cat zola-pretraite.txt  |tr '\n' ' ' |tr -s ' ' |awk 'BEGIN{
  cmd="cat french.stop.louis.txt | xargs echo -n"; # echo不接受管道传来的标准输入作为参数,xargs命令的作用，是将标准输入转为命令行参数, echo -n 转变为1行输出
  cmd |getline stopstr; # 这几句的意思就是，将很多行的txt变成一行读入到awk，并split成一个数组(stops)
  close(cmd);
  split(stopstr,stops," ");
}
{
  lens=split($0,array," ");
  for (i=1;i<=length(array);i++){
    for (j=1;j<=length(stops);j++){
      if (array[i]==stops[j]) {
        array[i]="STOP";
      }
    }
  }
}
END{for (i=1;i<=length(array);i++){print array[i];}}' |more

12) retrouver les mots manquants
1.Remplacez chaque mot GUESS par le mot le plus fréquent.
cat zola1.guess.utf8.txt  |tr '\n' ' ' |tr -s ' ' |awk '
{
  lens=split($0,array," ");
  for (i=1;i<=length(array);i++){
    if (array[i]=="GUESS"){
      array[i]="de";
    }
    print array[i];
  }
}'  | sed '/^$/d' >zola1.cand.louis.txt

./evaluation.sh zola1.guess.utf8.txt zola1.cand.louis.txt zola1.toks.utf8.txt
#good: 301 bad: 6990 err: 95,87

2.Remplacez chaque mot GUESS par le mot qui a le plus de chance de suivre le mot qui précède le mot GUESS

cat zola-pretraite.txt  |tr '\n' ' ' |tr -s ' ' |awk '{
  lens=split($0,array," ");
  for (i=1;i<lens;i++){
    print array[i] " " array[i+1];
  }
}' |sort|uniq -c|sort -k1,1nr|awk '{print $2" "$3","}' > zola1.bigrammes2.louis.txt

cat zola1.guess.utf8.txt  |tr '\n' ' ' |tr -s ' ' |awk 'BEGIN{
  cmd="cat zola1.bigrammes2.louis.txt | xargs echo -n";   # echo不接受管道传来的标准输入作为参数,xargs命令的作用，是将标准输入转为命令行参数, echo -n 转变为1行输出
  cmd |getline bigrammes;
  close(cmd);
  split(bigrammes,stops,", ");  # stops是返回的数组
}
{
  lens=split($0,array," "); # 对zola做spit，返回array
  print array[1];
  for (i=2;i<=length(array);i++){
    if (array[i]=="GUESS"){
      for (j=1;j<=length(stops);j++){
        split(stops[j],onebigramme," ");
        if (array[i-1]==onebigramme[1]){
          array[i]=onebigramme[2];
          break;
        }
      }
    }
    print array[i];
  }
}' | sed '/^$/d' >zola1.cand.louis2.txt

./evaluation.sh zola1.guess.utf8.txt zola1.cand.louis2.txt zola1.toks.utf8.txt

#good: 1377 bad: 5914 err: 81,11