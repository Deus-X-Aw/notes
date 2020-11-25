# Linux快捷键位（命令行）

Linux uname 命令

Linux uname（英文全拼：unix name）命令用于显示系统信息。

uname 可显示电脑以及操作系统的相关信息。

### 语法

```
uname [-amnrsv][--help][--version]
```

**参数说明**：

- -a或--all 　显示全部的信息。
- -m或--machine 　显示电脑类型。
- -n或-nodename 　显示在网络上的主机名称。
- -r或--release 　显示操作系统的发行编号。
- -s或--sysname 　显示操作系统名称。
- -v 　显示操作系统的版本。
- --help 　显示帮助。
- --version 　显示版本信息。

usr 

> usr是user的缩写，是曾经的HOME目录，然而现在已经被/home取代了，现在usr被称为是Unix System Resource，即Unix系统资源的缩写。

> /usr 是系统核心所在，包含了所有的共享文件。它是 unix 系统中最重要的目录之一，涵盖了二进制文件，各种文档，各种头文件，还有各种库文件；还有诸多程序，例如 ftp，telnet 等等。

> 曾经的 /usr 还是用户的家目录，存放着各种用户文件 —— 现在已经被 /home 取代了（例如 /usr/someone 已经改为 /home/someone）。现代的 /usr 只专门存放各种程序和数据，用户目录已经转移。虽然 /usr 名称未改，不过其含义已经从“用户目录”变成了“unix 系统资源”目录。值得注意的是，在一些 unix 系统上，仍然把 /usr/someone 当做用户家目录，如 Minix

![](E:\ScreenShotFile\linux快捷命令.png)

关机 halt 	init 0	poweroff 	shutdown

重启reboot

tree 目录树

如果只想显示一层目录，需要加参数L，如tree -L 1 /。

![img](https://img2018.cnblogs.com/blog/1486105/201906/1486105-20190607192720369-205433457.png)



rename命令可以修改文件名，可以用来批量修改，语法为rename 修改对象 修改后样子 符合条件的对象，可以参照man rename里面例子进行名字批量修改。

先创建200个文件

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```shell
 1 [root@node02 /home/test/name]]# touch foo{1..200}
 2 [root@node02 /home/test/name]]# ls
 3 foo1    foo117  foo135  foo153  foo171  foo19   foo27  foo45  foo63  foo81
 4 foo10   foo118  foo136  foo154  foo172  foo190  foo28  foo46  foo64  foo82
 5 foo100  foo119  foo137  foo155  foo173  foo191  foo29  foo47  foo65  foo83
 6 foo101  foo12   foo138  foo156  foo174  foo192  foo3   foo48  foo66  foo84
 7 foo102  foo120  foo139  foo157  foo175  foo193  foo30  foo49  foo67  foo85
 8 foo103  foo121  foo14   foo158  foo176  foo194  foo31  foo5   foo68  foo86
 9 foo104  foo122  foo140  foo159  foo177  foo195  foo32  foo50  foo69  foo87
10 foo105  foo123  foo141  foo16   foo178  foo196  foo33  foo51  foo7   foo88
11 foo106  foo124  foo142  foo160  foo179  foo197  foo34  foo52  foo70  foo89
12 foo107  foo125  foo143  foo161  foo18   foo198  foo35  foo53  foo71  foo9
13 foo108  foo126  foo144  foo162  foo180  foo199  foo36  foo54  foo72  foo90
14 foo109  foo127  foo145  foo163  foo181  foo2    foo37  foo55  foo73  foo91
15 foo11   foo128  foo146  foo164  foo182  foo20   foo38  foo56  foo74  foo92
16 foo110  foo129  foo147  foo165  foo183  foo200  foo39  foo57  foo75  foo93
17 foo111  foo13   foo148  foo166  foo184  foo21   foo4   foo58  foo76  foo94
18 foo112  foo130  foo149  foo167  foo185  foo22   foo40  foo59  foo77  foo95
19 foo113  foo131  foo15   foo168  foo186  foo23   foo41  foo6   foo78  foo96
20 foo114  foo132  foo150  foo169  foo187  foo24   foo42  foo60  foo79  foo97
21 foo115  foo133  foo151  foo17   foo188  foo25   foo43  foo61  foo8   foo98
22 foo116  foo134  foo152  foo170  foo189  foo26   foo44  foo62  foo80  foo99
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

然后将所有符合foo?的文件，即foo1~9的文件名字重新命名，将序号变成2位并以0开头

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```shell
 1 [root@node02 /home/test/name]]# rename foo foo0 foo?
 2 [root@node02 /home/test/name]]# ls
 3 foo01   foo11   foo128  foo146  foo164  foo182  foo200  foo40  foo60  foo80
 4 foo02   foo110  foo129  foo147  foo165  foo183  foo21   foo41  foo61  foo81
 5 foo03   foo111  foo13   foo148  foo166  foo184  foo22   foo42  foo62  foo82
 6 foo04   foo112  foo130  foo149  foo167  foo185  foo23   foo43  foo63  foo83
 7 foo05   foo113  foo131  foo15   foo168  foo186  foo24   foo44  foo64  foo84
 8 foo06   foo114  foo132  foo150  foo169  foo187  foo25   foo45  foo65  foo85
 9 foo07   foo115  foo133  foo151  foo17   foo188  foo26   foo46  foo66  foo86
10 foo08   foo116  foo134  foo152  foo170  foo189  foo27   foo47  foo67  foo87
11 foo09   foo117  foo135  foo153  foo171  foo19   foo28   foo48  foo68  foo88
12 foo10   foo118  foo136  foo154  foo172  foo190  foo29   foo49  foo69  foo89
13 foo100  foo119  foo137  foo155  foo173  foo191  foo30   foo50  foo70  foo90
14 foo101  foo12   foo138  foo156  foo174  foo192  foo31   foo51  foo71  foo91
15 foo102  foo120  foo139  foo157  foo175  foo193  foo32   foo52  foo72  foo92
16 foo103  foo121  foo14   foo158  foo176  foo194  foo33   foo53  foo73  foo93
17 foo104  foo122  foo140  foo159  foo177  foo195  foo34   foo54  foo74  foo94
18 foo105  foo123  foo141  foo16   foo178  foo196  foo35   foo55  foo75  foo95
19 foo106  foo124  foo142  foo160  foo179  foo197  foo36   foo56  foo76  foo96
20 foo107  foo125  foo143  foo161  foo18   foo198  foo37   foo57  foo77  foo97
21 foo108  foo126  foo144  foo162  foo180  foo199  foo38   foo58  foo78  foo98
22 foo109  foo127  foo145  foo163  foo181  foo20   foo39   foo59  foo79  foo99
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

然后将所有foo??的文件重新命名，将序号变成3位并以0开头

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 [root@node02 /home/test/name]]# rename foo foo0 foo??
 2 [root@node02 /home/test/name]]# ls
 3 foo001  foo021  foo041  foo061  foo081  foo101  foo121  foo141  foo161  foo181
 4 foo002  foo022  foo042  foo062  foo082  foo102  foo122  foo142  foo162  foo182
 5 foo003  foo023  foo043  foo063  foo083  foo103  foo123  foo143  foo163  foo183
 6 foo004  foo024  foo044  foo064  foo084  foo104  foo124  foo144  foo164  foo184
 7 foo005  foo025  foo045  foo065  foo085  foo105  foo125  foo145  foo165  foo185
 8 foo006  foo026  foo046  foo066  foo086  foo106  foo126  foo146  foo166  foo186
 9 foo007  foo027  foo047  foo067  foo087  foo107  foo127  foo147  foo167  foo187
10 foo008  foo028  foo048  foo068  foo088  foo108  foo128  foo148  foo168  foo188
11 foo009  foo029  foo049  foo069  foo089  foo109  foo129  foo149  foo169  foo189
12 foo010  foo030  foo050  foo070  foo090  foo110  foo130  foo150  foo170  foo190
13 foo011  foo031  foo051  foo071  foo091  foo111  foo131  foo151  foo171  foo191
14 foo012  foo032  foo052  foo072  foo092  foo112  foo132  foo152  foo172  foo192
15 foo013  foo033  foo053  foo073  foo093  foo113  foo133  foo153  foo173  foo193
16 foo014  foo034  foo054  foo074  foo094  foo114  foo134  foo154  foo174  foo194
17 foo015  foo035  foo055  foo075  foo095  foo115  foo135  foo155  foo175  foo195
18 foo016  foo036  foo056  foo076  foo096  foo116  foo136  foo156  foo176  foo196
19 foo017  foo037  foo057  foo077  foo097  foo117  foo137  foo157  foo177  foo197
20 foo018  foo038  foo058  foo078  foo098  foo118  foo138  foo158  foo178  foo198
21 foo019  foo039  foo059  foo079  foo099  foo119  foo139  foo159  foo179  foo199
22 foo020  foo040  foo060  foo080  foo100  foo120  foo140  foo160  foo180  foo200
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

最后全部修改完成，并完成自然排序。

### 1. Tab

这是你不能没有的 Linux 快捷键。它将节省你 Linux 命令行中的大量时间。

只需要输入一个命令，文件名，目录名甚至是命令选项的开头，并敲击 tab 键。 它将自动完成你输入的内容，或为你显示全部可能的结果。

如果你只记一个快捷键，这将是必选的一个。

### 2. Ctrl + C

这些是为了在终端上中断命令或进程该按的键。它将立刻终止运行的程序。

如果你想要停止使用一个正在后台运行的程序，只需按下这对组合键。

### 3. Ctrl + Z

该快捷键将正在运行的程序送到后台。 通常，你可以在使用 & 选项运行程序前之完成该操作， 但是如果你忘记使用选项运行程序，就使用这对组合键。

### 4. Ctrl + D

这对键盘快捷键将使你退出当前终端。如果你使用 SSH 连接，它将会关闭。 如果你直接使用一个终端，该应用将会立刻关闭。

把它当成“退出”命令。

### 5. Ctrl + L

你怎么清空你的终端屏幕？我猜是用 clear 命令。

你可以使用 Ctrl+L 清空终端，代替输入 C-L-E-A-R。得心应手，不是吗？

### 6. Ctrl + A

该快捷键将移动光标到所在行首。

假设你在终端输入了一个很长的命令或路径，并且你想要回到它的开头， 使用方向键移动光标将花费大量时间。注意你无法使用鼠标移动光标到行首。

这是 Ctrl+A 节省时间的地方。

### 7. Ctrl + E

这对快捷键与 Ctrl+A 相反。 Ctrl+A 送光标到行首，反之 Ctrl+E 移动光标到行尾。

### 8. Ctrl + U

输入了错误的命令？ 代替用退格键来丢弃当前命令，使用 Linux 终端中的 Ctrl+U 快捷键。 该快捷键会擦除从当前光标位置到行首的全部内容。

### 9. Ctrl + K

这对和 Ctrl+U 快捷键有点像。 唯一的不同在于不是行首，它擦除的是从当前光标位置到行尾的全部内容。

### 10. Ctrl + W

你刚才了解了擦除到行首和行尾的文本。 但如果你只需要删除一个单词呢？使用 Ctrl+W 快捷键。

使用 Ctrl+W 快捷键，你可以擦除光标位置前的单词。 如果光标在一个单词本身上，它将擦除从光标位置到词首的全部字母。

最好的方法是用它移动光标到要删除单词后的一个空格上， 然后使用 Ctrl+W 键盘快捷键。

### 11. Ctrl + Y

这将粘贴使用 Ctrl+W，Ctrl+U 和 Ctrl+K 快捷键擦除的文本。 如果你删除了错误的文本或需要在某处使用已擦除的文本，这将派上用场。

### 12. Ctrl + P

你可以使用该快捷键来查看上一个命令。 你可以反复按该键来返回到历史命令。 在很多终端里，使用 PgUp 键来实现相同的功能。

### 13. Ctrl + N

你可以结合 Ctrl+P 使用该快捷键。Ctrl+N 显示下一个命令。 如果使用 Ctrl+P 查看上一条命令，你可以使用 Ctrl+N 来回导航。 许多终端都把此快捷键映射到 PgDn 键。

### 14. Ctrl + R

你可以使用该快捷键来搜索历史命令。

![img](https://img-blog.csdnimg.cn/20191230143137801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nX3l1X2xpbmc=,size_16,color_FFFFFF,t_70)

1、tab //命令或路径等的补全键，linux用的最多的一个快捷键 ⭐️

2、ctrl+a //光标迅速回到行首 ⭐️

3、ctrl+e //光标迅速回到行尾 ⭐️

4、ctrl+f //光标向右移动一个字符

5、ctrl+b //光标向左移动一个字符

6、ctrl+insert //复制命令行内容（mac系统不能使用）

7、shift+insert //粘贴命令行内容（mac系统不能使用）

8、ctrl+k //剪切（删除）光标处到行尾的所有字符 ⭐️

9、ctrl+u //剪切（删除）光标处到行首的所有字符 ⭐️

10、ctrl+w //剪切（删除）光标前的一个字符

11、ctrl+y //粘贴 ctrl+k、ctrl+u、ctrl+w删除的字符 ⭐️

12、ctrl+c //中断终端正在执行的任务并开启一个新的一行 ⭐️

13、ctrl+h //删除光标前的一个字符（相当于退格键）

14、ctrl+d //退出当前shell命令行，如果是切换过来的用户，则执行这个命令回退到原用户 ⭐️

15、ctrl+r //搜索命令行使用过的历史命令记录 ⭐️

16、ctrl+g //从ctrl+r的搜索历史命令模式中退出

17、ctrl+l //清楚屏幕所有的内容，并开启一个新的一行 ⭐️

18、ctrl+s //锁定终端，使之任何人无法输入

19、ctrl+q //解锁ctrl+s的锁定状态

20、ctrl+z //暂停在终端运行的任务,使用"fg"命令可以使暂停恢复 ⭐️

21、!! //执行上一条命令 ⭐️

22、!pw //这是一个例子，是执行以pw开头的命令，这里的pw可以换成任何已经执行过的字符 ⭐️

23、!pw:p //这是一个例子，是仅打印以pw开头的命令，但不执行，最后的那个“p”是命令固定字符 ⭐️

24、!num //执行历史命令列表的第num条命令，num代指任何数字（前提是历史命令里必须存在）⭐️

25、!$ //代指上一条命令的最后一个参数，该命令常用于shell脚本中 ⭐️

26、esc+. //注意那个".“ 意思是获取上一条命令的(以空格为分隔符)最后的部分 ⭐️

27、esc+b //移动到当前单词的开头

28、esc+f //移动到当前单词的结尾