**rm（删除文件或目录）**

``` 
[root@www ~]#rm [-fir] 文件名/目录名

选项：

-f:force的意思，强制删除文件，忽略不存在的文件，不会出现警告询问
-i:互动模式，在删除之时询问使用者是否动作
-r:递归删除,通常用于删除目录  

[root@www ~]#rm -i bashrc
#询问是否删除bashrc  避免错误删除文件
通过通配符*帮忙，以bash开头所有文件通通删除。
[root@www ~]#touch bash{1..5}批量创建文件
[root@www ~]# rm -i bash*
删除目录
[root@www ~]#mkdir -p er/lin-{1..5}//递归批量创建目录
[root@www ~]#rm -rf er  

5.删除一个带-文件
[root@www ~]#touch ./-aaa-
[root@www ~]#rm -aaa- 因为-是选项所以系统误判了
[root@www ~]#rm ./-aaa-


```

---
**mv移动文件或目录或更名**

```
[root@wwww ~]#mv [-ifu] 源文件 目标目录
[root@www ~]# mv [options] s1 s2 s3...directory
选项：
-f:force的意思，强制移动文件
-i:询问是否移动文件
-u:若目标文件已存在且源文件比较新，才会更新

1.移动文件并改名
[root@www ~]# mv bashrc ./ex3/h

2.目录改名
[root@www ~]#mkdir ddir
[root@www ~]# mv ddir dd1

3.移动多个文件到目录
[root@www ~]# touch lin-{1..4}
[root@www ~]# mv lin-1 lin-2 lin-3 et
```

---
**rename更改大量文件的文件名**

>rename 原文件名 目标文件名 原文件名  
>原字符串：将文件名需要替换的字符串； 目标字符串：将文件名中含有的原字符替换成目标字符串； 文件：指定要改变文件名的文件列表。

---
**取得文件名或目录名**  
**basename与dirname**

```
[root@www ~]# basename /home/suroots/t.sh #显示文件名
[root@www ~]# dirname /home/suroots #显示目录
```


---
**最常使用显示文件内容的命令有 cat more less**
* cat由第一行开始显示文件内容
* tac由倒数第一行开始显示，tac是cat倒着写
* nl 显示，随便输出行号
* more 一页一页显示文件内容
* less与more类似，不过less能向前翻
* head只能看头几行
* tail只能看尾几行
* od以二进制显示文件内容

---
**cat(concatenate) 把 （一系列事件、事情等）联系起来**

```
[root@www ~]# cat [-AbEnTv] 文件
选项：
-A:相当于 -vET的整合选项 可以列了出特殊字符而非空白

-b:显示出行号。非空显示行号，空白不显示 

-E ：将结尾的断行字符 $ 显示出来；

-n ：打印出行号，连同空白行也会有行号，与 -b 的选项不同；

-T ：将 [tab] 按键以 ^I 显示出来；

-v:列出一些看不出来的特殊字符


```

---
**nl(添加行号打印)**

```
[root@www ~]# nl [-bwn] 文件名
选项：
-b ：指定行号指定的方式，主要有两种：


      1.-b a:表示不论是否为空，都显示行号
      2. -b t:有空行，那一行不显示行号
-n ：列出行号表示的方法，主要有三种：
-n ln ：行号在屏幕的最左方显示；
-n rn ：行号在自己字段的最右方显示，且不加 0 ；
-n rz ：行号在自己字段的最右方显示，且加 0 ；
-w ：行号字段的占用的位数  

[root@www ~]# nl -b a -n rz -w 3 /etc/issue
001 CentOS release 5.3 (Final)
002 Kernel \r on an \m
003
# 变成仅有 3 位数啰～
```

---
**more(一页一页往下翻) more几个按键**  
* 空格键 (space)：代表向下翻一页；
* Enter ：代表向下翻『一行』
*  /字符串 ：代表在这个显示的内容当中，向下搜寻『字符串』这个关键词；
*  :f ：立刻显示出文件名以及目前显示的行数；
*  q ：代表立刻离开 more ，丌再显示该档案内容
*  b 或 [ctrl]-b：代表往回翻页，不过这动作只对档案有用，对管线无用

---
**less (一页一页翻页)**  
* 空格键 ：向下翻一页；
* [pagedown]：向下翻动一页  

* \[pageup]:向上翻一页
* /字符串 ：向下搜寻『字符串』的功能；
* ？字符串：向上搜寻字符串的功能
* n ：重复前一个搜寻 (与/ 或 ? 有关！)
* N ：反向的重复前一个搜寻 (与 / 或 ? 有关！

---
* head (取出前面几行)
* tail取出后面几行   

> head -n num 文件名 num为更整数 显示出前面num行内容，
  >>tail -n num 文件名 显示出后面num行内容，-f ：表示持续侦测后面所接的档名，要等到按下[ctrl]-c 才会结束 tail 的侦测 ，   默认的情况中，显示最后的十行！若要显示最后的 20 行，就得要这样：tail -n 20  
  
     
     
     
     
```
如果不知道/etc/man.config 有几行，即叧想列出 100 行以后的数据
[root@www ~]# tail -n +100 /etc/man.config   

持续侦测/var/log/messages 癿内容
[root@www ~]# tail -f /var/log/messages
<==要等到输入[crtl]-c 之后才会离开 tail 这个指令的侦测！   

我想要显示 /etc/man.config 的第 11 到第 20 行呢？
答：
想一想，在第 11 到第 20 行，那么我取前 20 行，再取后十行，所以结
果就是：『head -n 20 /etc/man.config |cat -n| tail -n 10 』，这样就可以得到第 11 到第 20
行之间的内容了！并显示行号
```

---
**非纯文本档： od**

```
root@www ~]# od [-t TYPE] 档案
选项或参数：
-t ：后面可以接各种『类型 (TYPE)』的输出，例如：
a ：利用默认的字符来输出；
c ：使用 ASCII 字符来输出
d[size] ：利用十进制(decimal)来输出数据，每个整数占用 size bytes ；

f[size] ：利用浮点数(floating)来输出数据，每个数占用 size bytes ；
o[size] ：利用八进制(octal)来输出数据，每个整数占用 size bytes ；
x[size] ：利用十六进制(hexadecimal)来输出数据，每个整数占用 size bytes  


```



  
  

