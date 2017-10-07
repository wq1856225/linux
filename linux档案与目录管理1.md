**目录与路径**
* 绝对路径：是由/根目录写起
* 相对路径：不是由/根目录写起  

## 目录相关
变换目录指令是cd,比较特殊的目录：
> <h3>.代表当前目录   
..代表上层目录，  
-代表前一个工作目录   
~ 代表【目前用户身份】所在的家目录  
~ account 代表 account这个用户的家目录
</h3>  

常见目录指令
* cd 变换目录
* pwd:显示当前的目录
* mkdir 建立一个新目录
* rmdir 删除一个空目录
> cd ~suroots 表示进入suroots这个用户的家目录，cd ~表示进入自己的家目录，cd 表示回到自己的家目录，cd ..回到上层目录   

cd （change Directory）改变工作目录的指令 注意目录与cd指令之间存在一个空格
> 我们的 Linux 的默认指令列模式 (bash shell) 具有档案补齐功
能， 你要常常利用 [tab] 按键来达成你的目录完整性啊！这可是个好习惯啊～ 可以
避免你按错键盘输入错字  

* pwd 显示当前目录
<div style="background:black;color:#fff">
<p>[root@www~]# pwd -P</p>  
<p>-P:显示确实路径 而非使用链接link</p>
<p> cd  /var/mail</p>
<p> pwd  </p>
<p>显示出 /var/mail</p>
<p>pwd -P 显示出 /var/spool/mail</p>
<p>ls -ld /var/mail</p>
<p>lrwxrwxrwx 1 root root 10 Sep 4 17:54 /var/mail -> spool/mail
# 看到这里应该知道为啥了吧？因为 /var/mail 是连结档，连结到/var/spool/mail</p>
</div>

pwd(print Working Directory)

* mkdir(建立新目录)
 <div style="background-color:black;color:#fff">
[root@www ~]#mkdir [-mp]目录名称<br>
-m:配置文件权限，可以直接设定<br>
-p:帮你直接将所需要的目录(包含上层目录)递归建立起来！<br>
例如<br>
[root@www ~] # mkdir -m 711 test1 <br> 创建test1并给-rwx--x--x权限 <br>
[root@wwww ~]#mkdir -p test1/test2/test3  <br>
递归创建test1/test2/test3



</div>
<br>  

* rmdir(删除空目录)
<div style="background-color:black;color:#fff">
[root@www ~]# rmdir (-p) 目录名称<br>
-p连同上层空目录删掉<br>
[root@www tmp]#mkdir -p test1/test2/test3<br>
递归创建所需的目录，包含上层目录<br>
[root@wwww tmp]#rmdir -p test1/test2/test3<br>
删除test3空目录以及test2 test1的空目录

</div>   
 <br>  
 
> rmdir删除目录必须一层一层的往下删，而且删除的目录必须为空。如果目录有东西那么用 rm -r 询问下删除目录 rm-rf 强制删除目录   

* 执行文件路径 $PATH  
比如说『ls』好了，系统会依照 PATH 的设定去每个 PATH 定义的目
录下搜寻文件名为 ls 的可执行文件， 如果在 PATH 定义的目录中含有多个文件名为 ls 的可执行文件，
那么先搜寻到的同名指令先被执行！
   
> PATH(一定是大写)变量内容是由一堆目录组成，每个目录中间由冒号:分割，每个目录是顺序之分  

如果我有两个一样指令在的不同目录中，时候，哪个 会被执行？ 
>在PATH中那个目录先被查询，则那个目录下的指令就会被先执行    

* 为什么PATH中的目录不能用当前目录（.）
>如果在PATH中使用当前目录.确实能在指令目录为工作目录下执行指令，由于工作目录并非固定，而且常常使用cd指令变换工作目录，因此指令执行与否是看工作目录和指令目录是否重合，否则警告command not found。所以PATH中目录最好使用绝对路径 。   2.为了安全起见，不建议将『.』加入 PATH 的搜寻目目录中。
   
* 不同身份使用者预设的 PATH 不同，默认能够随意执行的指令也不同；
* PATH可以被修改，一般用户可以通过修改PATH来执行某些位于 /sbin和/usr/sbin的指令
* 使用绝对路径直接指定某个指令的文件名来执行，比PATH搜寻来的更正确
* 指令要放在正确的目录下，执行才能比较方便  




## 有关档案与目录 基础管理
* 档案与目录的检视 ls  

<div style="background:black;color:#fff;font-size:20px">
[root@www ~]# ls -[aAdfFhilnrRSt] 目录名称<br><br>
[root@www ~]#ls [--color={never,auto,always}] 目录<br><br>
[root@www ~]# ls [--full-time] 目录<br><br>
选项:<br><br>
-a:全部档案，连同隐藏文件（开头以.开始文件）一起列出来<br><br>
-A:全部文件，连同隐藏文件，但不包括.与..<br><br>
-d:仅列出目录本身，而不是列出目录内文件数据<br><br>
-f ：直接列出结果，而不进行排序<br><br>
-F:根据档案、目录等信息，给予附加数据结构，例如：
*:代表可执行文件； /:代表目录； =:代表 socket 档案； |:代表 FIFO 档案；<br><br>
-h ：将档案容量以人类较易读的方式(例如 GB, KB 等等)列出来；<br><br>
-i：列出inode号码<br><br>
-l ：长数据串行出，包含文件的属性与权限等等数据；(常用)<br><br>
-n ：列出 UID 与 GID 而非使用者不群组的名称 <br><br>
-r：将排序结果反向输出<br>
-R:连同子目录内容一起列出来<br>
-S ：以档案容量大小排序，而不是用档名排序；<br>
-t:以时间排序，而不是用文件名<br>
--color=never：不依据文件特性显示颜色<br>
--color=always:显示颜色<br>
--color=auto:让系统自行 依据设定来判断是否给予颜色<br>
--full-time:以完整时间模式输出<br>
--time={atime,ctime} ：输出 access 时间或改变权限属性时间 (ctime)
而非内容变更时间 (modification time)
</div>  

* cp（复制文件和目录）   


<div style="background:#000;color:#fff;font-size:16px">
#cp [-adrlipsu] 源目录 目标目录<br>
# cp [option] s1....s4  目录 复制s1-s4文件到目录
<br>
选项：<br>
-a ：相当于 -pdr 的意思<br>
-d ：源文件为链接文件的属性(link file)，则复制链接文件属性而非档案本
身；<br>
-f:为force强的制的意思<br>
-i ：若目标文件(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
<br>
-l ：进行硬式连结(hard link)的连结档建立，而非复制档案本身；<br>
-p ：连同档案的属性一起复制过去，而非使用默认属性(备份常用)；<br>
-r：递归持续复制,用于复制目录<br>
-s ：复制成为符号链接文件 (symbolic link)，亦『快捷方式』档案；<br>
-u ：若 destination 比 source 旧才更新 destination ！ <br>
比如说将家目录的.bashrc复制到tmp并改名<br>
#cp /home/suroots/.bashrc /tmp/bashrc;<br>
#cp -i /home/suroot/.bashrc /tmp/bashrc;询问是否覆盖<br>
<p></p>
比如家目录有个var/log/wtmp ，用cp复制它，来比较它们俩的异同<br>
# cp /var/log/wtmp ./<br>
# ls -l /var/log/wtmp wtmp<br>
-rw-rw-r-- 1 root utmp 42240 10月  7 20:03 /var/log/wtmp <br>
-rw-r--r-- 1 root root 42240 10月  7 21:46 wtmp<br>
cp并没有完全复制文件属性要加选项-a保证复制文件的特性<br>
例四：将范例一复制癿 bashrc 建立一个连结档 (symbolic link)<br>
[root@www tmp]# ls -l bajs<br>
-rw-r--r-- 1 root root 176 Sep 24 14:02 bajs <==先观察一下档案情 u况
[root@www tmp]# cp -s bajs bajs_slink<br>
[root@www tmp]# cp -l bajs basj_hlink<br>
[root@www tmp]# ls -l bajs*<br>
-rw-r--r-- 2 root root 176 Sep 24 14:02 bajs <==不源文件丌太一样了！
-rw-r--r-- 2 root root 176 Sep 24 14:02 bajs_hlink
lrwxrwxrwx 1 root root 6 Sep



</div>    

> 使用 -l 及 -s 都会建立所谓的连结档(link file)，但是这两种连结档即有不一样的情
冴。这是怎么一回事啊？ 那个 -l 就是所谓的实体链接(hard link)，至亍 -s 则是符号链接(symbolic
link)， 简单来说，bajs_slink 是一个『忚捷方式』，这个忚捷方式会连结到 bajs 去！所以你会看
到档名右侧会有个指向(->)

你能否使用 suroots 的身份，完整的复制/var/log/wtmp 档案到/tmp 底下，并更名为
suroots_wtmp 呢？
>
[suroots@www ~]$ cp -a /var/log/wtmp /tmp/vs_wtmp<br>
[suroots@www ~]$ ls -l /var/log/wtmp /tmp/vs_wtmp<br>
-rw-rw-r-- 1 suroots suroots 96384 9 月 24 11:54 /tmp/vs_wtmp<br>
-rw-rw-r-- 1 root utmp 96384 9 月 24 11:54 /var/log/wtmp<br>
由亍 suroots 的身份并不能随意修改档案的拥有者与群组，因此虽然能够复制 wtmp 的相关
权限与时间等属性， 但是与拥有者、群组相关的，原本 suroots 身份无法进行的动作，卲使
加上 -a 选项，也是无法达成完整复制权限！   
  
  所以，在复制时，你必须要清楚的了解到：
* 是否需要完整的保留来源档案的信息？
* 来源档案是否为连结档 (symbolic link file)？
* 来源档是否为特殊的档案，例如 FIFO, socket 等？
* 源文件是否为目录？