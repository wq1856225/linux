**touch新建文件或修改文件时间**  

每个文件在liux下都会记录许多时间参数，其实有三个主要的变动时间，三个时间的意义有多少：  
* modification time（mtmie)：  
当文件内容数据改变时，就会更新这个时间，内容数据是指文件内容，而不是文件属性或权限

* status time(ctime):  
该文件状态改变时，就会更新这个时间，像权限或属性改变，就会更新时间

* access time(atime):  
当文件内容被读取时，就会更新这个读取时间（access)

<pre style="font-size:20px">
[root@www ~]# touch [-acdmt]文件  

选项：

-a:修改access time
-c:仅修改文件的时间
-d:后面接欲修改的日期不是当前的日期
-m:修改mtime
-t:后面接要修改的时间
 将 ~/.bashrc 复制成为 bashrc，假设复制完全的属性，检查其日期
[root@www test]# cp -a ~/.bashrc ./bashrc
[root@www test]# ll bashrc;ll --time=atime bashrc; ll --time=ctime bashrc
-rw-r--r--. 1 suroots suroots 231 8月   3 2016 bashrc =>mtime
-rw-r--r--. 1 suroots suroots 231 10月  6 20:33 bashrc=>atime
-rw-r--r--. 1 suroots suroots 231 10月  9 20:42 bashrc=>ctime
修改案例二的 bashrc 档案，将日期调整为两天前
[root@www test]# touch -d "2 days ago" bashrc

[suroots@localhost test]$ touch -d 20170509 bashrc
[suroots@localhost test]$ ll
总用量 4
-rw-r--r--. 1 suroots suroots 231 5月   9 00:00 bashrc
-rwxrwxr-x. 1 suroots suroots   0 10月  9 20:28 

</pre>

#### ;则是代表连续指令的下达
touch最常用的是情况
* 新建空白文件
* 修改某个文件的日期

---
**文件与目录的默认权限与隐藏权限**  

除了基本 r, w, x 权限外，在 Linux 的 Ext2/Ext3 文件系统下，我们还可以

 设定其他的系统隐藏属性，
 
这部份可使用 chattr 来设定，而以 lsattr 来查看，最重要的属性就是可以设定其不可修改的特性！让
连档案的拥有者都不能进行修改！

这个属性可是相当重要的，尤其是在安全机制上面 (security)！   
 
 <pre style="font-size:20px">
 
 在 /tmp 底下建立一个目目录，这个目弽名称为 tu ，并且这个目录拥有者为
dmtsai， 群组为 users ，此外，任何人都可以进入该目录浏览档案，不过除了 dmtsai 之外，其他人都不能修改该目目录下的档案。
答：
因为除了 dmtsai 之外，其他人不能修改该目录下的档案，所以整个目录的权限应该是
drwxr-xr-x 才对！ 因此你应该这样做：
建立目录： mkdir /tmp/tu
修改属性： chown -R dmtsai:users /tmp/tu
修改权限： chmod -R 755 /tmp/tu 递归给权限
 </pre>
 

---
**umask 目前用户在建立文件或目录时候的权限默认值**  


<pre style="font-size:25px">
# umask
0022 =>与一般权限有关的是后面三个数字！
# umask -S
u=wrx,g=rx,o=rx

</pre>
在默认权限的属性上，目录与档案是不一样的。 但
是一般档案的建立则不应该有执行的权限，因为一般档案通常是用在于数据的记录嘛！当然不需要执行
的权限了。 因此，预设的情况如下：  

* 若使用者建立为『档案』则预设『没有可执行( x )权限』，亦即只有 rw 这两个项目，也就是最
大为 666 分，预设权限如下：
-rw-rw-rw 

* 若用户建立为『目录』，则由于 x 与是否可以进入此目录有关，因此默认为所有权限均开放，亦即为 777 分，预讴权限如下：
drwxrwxrwx

umask 的分数指的是『该默认值需要减掉的权限！』因为 r、 w、 x 分别是 4、 2、 1 分，
所以也就是说，当要拿掉能写的权限，就是输入 2 分，而如果要拿掉能读的权限，也就是 4 分，
那么要拿掉读不写的权限，也就是 6 分，而要拿掉执行不写入的权限，也就是 3 分，这样了解吗？

请问你， 5 分是什么？呵呵！ 就是读不执行癿权限啦！
比如说，因为 umask 为 022 ，所以 user 并没有被拿掉任何权限，不过 group
与others 的权限被拿掉了 2 (也就是 w 这个权限)，那么当使用者：  

* 建立档案时：  
(-rw-rw-rw-) - (-----w--w-) ==> -rw-r--r--
* 建立目目录时：  
(drwxrwxrwx) - (d----w--w-) ==> drwxr-xr-x



<pre style="font-size:20px">
#umask 002=>改变预设权限
- --w
#touch t
# mkdir tt
#ls -l
-rw-rw-r--. 1 suroots suroots  0 10月  9 22:05 t

drwxrwxr-x. 2 suroots suroots   6 10月  9 22:07 tt

</pre>

假设你的 umask 为 003 ，请问该 umask 情况下，建立文件和目录的权限


>   #umask 003   
>\- -wx  
> #touch ty  
> \# ls -l   
>  -rw-rw-r--. 1 suroots suroots 0 10月  9 22:19 ty  
> \# mkdir yu  
>\# ls -l  
> drwxrwxr--. 2 suroots suroots   6 10月  9 22:25  yu     
>
>档案： (-rw-rw-rw-) - (--------wx) = -rw-rw-r--  
目录： (drwxrwxrwx) - (--------wx) = drwxrwxr--  

---
**档案隐藏属性：**  
底下的 chattr 命令叧能在 Ext2/Ext3 的

---

文件系统上面生效， 其他的文件系统可能就无法支持这个命令了。  
* chattr (配置文件案隐藏属性)
  

<pre style="font-size:24px">
[root@www ~]# chattr [+-=][ASacdistu] 文件或目录名称
选项与参数：
+ ：增加某一个特殊参数，其他原本存在参数则不动。
- ：移除某一个特殊参数，其他原本存在参数则不动。
= ：设定一定，且仅有后面接的参数    

A ：当设定了 A 这个属性时，若你有存取此档案(或目录)时，他的访问时间
atime，将不会被修改，可避免 I/O 较慢的机器过度的存取磁盘。这对速度较慢的计
算机有帮助  

S ：一般档案是异步写入磁盘的，如果加上 S
这个属性时，当你进行任何档案的修改，该更会『同步』写入磁盘中。  

a ：当设定 a 之后，这个档案将叧只能增加数据，而不能删除也不能修改数据，叧
有 root才能设定这个属性

<span style="color:red">d:</span>当dump程序被执行，设定d属性将可使该文件或目录不被dump备份

i ：他可以让一个文件『不能被删除、改名、设定连结也
无法写入或新增资料！』对于系统安全性有相当大癿帮助！叧有 root 能讴定此属
性

s ：当文件主设定了 s 属性时，如果这个文件被删除，他将会被完全的移除出这个
硬盘
u:与 s 相反的，当使用 u 来配置文件时，如果该文件被删除了，则数据内容
其实还存在磁盘中，可以救援该文件

</pre>

---
* lsattr (显示文件隐藏属性)

> [root@www test]# lsattr [-adR] 文件或目录  
>选项不参数：  
> -a ：将隐藏文件的属性也秀出来；  
> \-d:如果接的是目录，仅列出目录本身的属性而非目录内的文件名；    
>-R ：连同子目录的数据也一并列出来；
>
>[root@www test]#mkdir mp  
>[root@www test]#chattr +aiSA mp  
>[root@www test]#lsattr -d mp  
> --S-ia-A-------- mp

