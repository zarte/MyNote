[TOC]
```
GCC(GNU Compiler Collection)是Linux下最常用的C语言编译器，是GNU项目中符合ANSI C标准的编译系统,能够编译用C、C++和Object C等语言编写的程序。同时它可以通过不同的前端模块来支持各种语言，如Java、Fortran、Pascal、Modula-3和Ada等。
穿插一个玩笑： GNU意思是GNU’s not Unix而非角马。然而GNU还是一个未拆分的连词，这其实是一个源于hacker的幽默：GNU是一个回文游戏，第一个字母G是凑数的，你当然可以叫他做ANU或者BNU。
下面开始。
一．CC编译程序过程分四个阶段
◆ 预处理（Pre-Processing）
◆ 编译（Compiling）
◆ 汇编（Assembling）
◆ 链接（Linking）
Linux程序员可以根据自己的需要让GCC在编译的任何阶段结束转去检查或使用编译器在该阶段的输出信息，或者对最后生成的二进制文件进行控制，以便通过加入不同数量和种类的调试代码来为今后的调试做好准备。如同其他的编译器，GCC也提供了灵活而强大的代码优化功能，利用它可以生成执行效率更高的代码。
GCC提供了30多条警告信息和三个警告级别，使用它们有助于增强程序的稳定性和可移植性。此外，GCC还对标准的C和C++语言进行了大量的扩展，提高程序的执行效率，有助于编译器进行代码优化，能够减轻编程的工作量。
二．简单编译命令
我们以Hello world程序来开始我们的学习。代码如下：
/* hello.c */
#include <stdio.h>
int main(void)
{
printf ("Hello world!\n");
return 0;
}
1. 执行如下命令：$ gcc -o hello hello.c
运行如下 ： $ ./hello
输出： Hello,world!
2. 我们也可以分步编译如下：
(1) $ gcc –E hello.c -o hello.i 
      //预处理结束
      //这时候你看一下hello.i ，可以看到插进去了很多东西。
(2) $ gcc –S hello.i
      //生成汇编代码后结束
(3) $ gcc –c hello.c
    或者：
    $ gcc -c hello.c –o hello.o
    或者：
    $ gcc -c hello.i -o hello.o
     //编译结束
     //生成 hello.o文件
(4) $ gcc hello.o –o hello.o
    或者：
    $ gcc –o hello hello.c
      //链接完毕，生成可执行代码
3. 我们可以把几个文件一同编译生成同一个可执行文件。
比如：一个工程有main.c foo.c def.c生成foo的可执行文件。
编译命令如下：
$ gcc –c main.c foo.c def.c –o foo
  或者：
$ gcc –o foo main.c foo.c def.c


三．库依赖
函数库是一些头文件（.h）和库文件（.so或者.a）的集合。Linux下的大多数函数都默认将头文件放到/usr/include/目录下，而库文件则放到/usr/lib/目录下，但并非绝对如此。因此GCC设有添加头文件和库文件的编译选项开关。
1. 添加头文件：-I
例如在/home/work/include/目录下有编译foo.c所需头文件def.h，为了让GCC能找到它们，就需要使用-I选项：
$ gcc foo.c -I /home/work/include/def.h -o foo
2. 添加库文件：-L
例如在/home/work/lib/目录下有链接所需库文件libdef.so，为了让GCC能找到它们，就需要使用-L选项：
$ gcc foo.c –L /home/work/lib –ldef.a –o foo
说明：-l选项指示GCC去连接库文件libdef.so。Linux下的库文件命名有一个约定，即库文件以lib三个字母开头，因为所有的库文件都遵循这个约定，故在用-l选项指定链接的库文件名时可以省去lib三个字母。
[题外语] 
Linux下的库文件分为动态链接库（.so文件）和静态链接库（.a文件）。GCC默认为动态库优先，若想在动态库和静态库同时存在的时候链接静态库需要指明为-static选项。比如上例中如还有一个libdef.a而你想链接libdef.a时候命令如下：
$ gcc foo.c –L /home/work/lib –static –ldef.a –o foo


四．代码优化
GCC提供不同程度的代码优化功能。开关选项是：-On，n取值为0到3。默认为1。-O0表示没有优化，而-O3是最高优化。优化级别越高代码运行越快，但并不是所有代码都能够加载最高优化，而应该视具体情况而定。但一般都使用-O2选项，因为它在优化长度、编译时间和代码大小之间，取得了一个比较理想的平衡点。
以下这段是我摘自别人文章的，说的比较详细：
编译时使用选项-O可以告诉GCC同时减小代码的长度和执行时间，其效果等价于-O1。在这一级别上能够进行的优化类型虽然取决于目标处理器，但一般都会包括线程跳转(Thread Jump)和延迟退栈(Deferred Stack Pops)两种优化。选项-O2告诉GCC除了完成所有-O1级别的优化之外，同时还要进行一些额外的调整工作，如处理器指令调度等。选项-O3则除了完成所有-O2级别的优化之外，还包括循环展开和其它一些与处理器特性相关的优化工作。通常来说，数字越大优化的等级越高，同时也就意味着程序的运行速度越快。
下面通过具体实例来感受一下GCC的代码优化功能,所用程序如清单3所示。
/* optimize.c */
#include <stdio.h>
int main(void)
{
double counter;
double result;
double temp;
for (counter = 0; 
counter < 2000.0 * 2000.0 * 2000.0 / 20.0 + 2020; 
counter += (5 - 1) / 4) {
temp = counter / 1979;
result = counter; 
}
printf("Result is %lf\n", result);
return 0;
}
首先不加任何优化选项进行编译：
# gcc -Wall optimize.c -o optimize
借助Linux提供的time命令，可以大致统计出该程序在运行时所需要的时间：
# time ./optimize
Result is 400002019.000000
real 0m14.942s
user 0m14.940s
sys 0m0.000s
接下去使用优化选项来对代码进行优化处理：
# gcc -Wall -O optimize.c -o optimize
在同样的条件下再次测试一下运行时间：
# time ./optimize
Result is 400002019.000000
real 0m3.256s
user 0m3.240s
sys 0m0.000s
对比两次执行的输出结果不难看出，程序的性能的确得到了很大幅度的改善，由原来的14秒缩短到了3秒。
这个例子是专门针对GCC的优化功能而设计的，因此优化前后程序的执行速度发生了很大的改变。尽管GCC的代码优化功能非常强大，但作为一名优秀的Linux
程序员，首先还是要力求能够手工编写出高质量的代码。如果编写的代码简短，并且逻辑性强，编译器就不会做更多的工作，甚至根本用不着优化。


优化虽然能够给程序带来更好的执行性能，但在如下一些场合中应该避免优化代码：
◆ 程序开发的时候优化等级越高，消耗在编译上的时间就越长，因此在开发的时候最好不要使用优化选项，
只有到软件发行或开发结束的时候，才考虑对最终生成的代码进行优化。
◆ 资源受限的时候一些优化选项会增加可执行代码的体积，如果程序在运行时能够申请到的内存资源非常紧张（如
一些实时嵌入式设备），那就不要对代码进行优化，因为由这带来的负面影响可能会产生非常严重的后果。
◆ 跟踪调试的时候在对代码进行优化的时候，某些代码可能会被删除或改写，或者为了取得更佳的性能而进行重组，从而使跟踪和调试变得异常困难
Linux下C程序的编辑，编译和运行以及调试
要使用的工具：
编辑：vim(vi)
编译和运行：gcc
调试：gdb
安装很简单（以下是以在CentOS中安装为例）：
1 yum vim gcc gdb
 
1.使用vim编辑源文件
首先，打开终端练下手：
1 vim hello.c
 
（进入一般模式）
按下"i"，进入编辑模式，在编辑模式下输入：
1 #include <stdio.h>
2 int main(){
3    printf("Hello, World!\n");
4    return 0;
5 }
 
输入完成，按"ESC"键，回到一般模式，然后按下":wq"，即可保存并退出vim。
附注：
在一般模式下，按下":%!xxd"查看hello.c的16进制形式，回到文本格式按下":%!xxd -r"。
查看hello.c的二进制形式，按下":%!xxd -b"，这是hello.c保存在磁盘上的存储状态。
至此，在vim已完成C源文件的编辑。
关于vim的使用，直接上网搜索vim，相关的文章是相当多的；或者参考vim的联机帮助，在命令行上键入"man vim"即可。
2.编译和运行
gcc命令的基本用法：
1 gcc[options] [filenames]
 
其中，filenames为文件名；options为编译选项
当不使用任何编译选项编译hello.c时，gcc将会自动编译产生一个a.out的可执行文件：
1 [root@localhost c]# ls
2 hello.c
3 [root@localhost c]# gcc hello.c
4 [root@localhost c]# ls
5 a.out  hello.c
 
执行：
1 [root@localhost c]# ./a.out
2 Hello, World!
 
使用-o编译选择，可以为编译后的文件指定一个名字：
1 [root@localhost c]# ls
2 a.out  hello.c
3 [root@localhost c]# gcc hello.c -o hello
4 [root@localhost c]# ls
5 a.out  hello  hello.c
 
执行：
1 [root@localhost c]# ./hello
2 Hello, World!
 
注意：使用-o选项时，-o后面必须跟一个文件名，即：-o outfile。
为了便于描述后面的选项，删除hello和a.out可执行文件。
结合介绍gcc的编译选项，分析hello.c的编译和执行过程：
（1）预处理阶段：使用-E选项，对输入文件只做预处理不编译。当使用这个选项时，预处理器的输出被送到标准输出而不是存储到文件。如果想将预处理的输出存储到文件，可结合-o选项使用，使用如下：
1 [root@localhost c]# ls
2 hello.c
3 [root@localhost c]# gcc -E hello.c -o hello.i
4 [root@localhost c]# ls
5 hello.c  hello.i
 
使用less查看下hello.i：
1 [root@localhost c]# less hello.i
 
（2）编译阶段：使用-S选项，将C程序编译为汇编语言文件后停止编译，gcc编译产生汇编文件的默认后缀为.s。
1 [root@localhost c]# ls
2 hello.c  hello.i
3 [root@localhost c]# gcc -S hello.c
4 [root@localhost c]# ls
5 hello.c  hello.i  hello.s
 
在gcc -S hello.c处，使用C源文件编译，也可以用gcc -S hello.i的预处理文件编译，结果一样。
使用-S编译时，也可以和-o结合使用指定编译产生的汇编语言文件的名字：
1 [root@localhost c]# ls
2 hello.c  hello.i  hello.s
3 [root@localhost c]# gcc -S hello.i -o hello_s.s
4 [root@localhost c]# ls
5 hello.c  hello.i  hello.s  hello_s.s
 
可使用less命令查看汇编代码。
（3）汇编阶段：使用-c选项，将C源文件或者汇编语言文件编译成可重定向的目标文件（二进制形式），其默认后缀为.o。
1 [root@localhost c]# ls
2 hello.c  hello.i  hello.s  hello_s.s
3 [root@localhost c]# gcc -c hello.s
4 [root@localhost c]# ls
5 hello.c  hello.i  hello.o  hello.s  hello_s.s
 
也可以和-o结合使用指定编译产生的目标文件的名字：
1 [root@localhost c]# gcc -c hello.s -o hello.o
 
由于hello.o是二进制文件，使用less查看显示为乱码；
然后使用vim hello.o打开也显示为乱码，按下":%!xxd"查看其16进制形式，按下":%!xxd -r"退出 16进制查看模式，回到乱码状态。在退出vim时，若提示已经修改了文件，则使用":q!"强制退出。
（4）链接阶段：链接器将可重定向的目标文件hello.o以及库文件（如printf.o）执行并入操作，形成最终可执行的可执行目标文件。
1 [root@localhost c]# ls
2 hello.c  hello.i  hello.o  hello.s  hello_s.s
3 [root@localhost c]# gcc hello.o
4 [root@localhost c]# ls
5 a.out  hello.c  hello.i  hello.o  hello.s  hello_s.s
 
可使用-o选项，指定输出文件（即可执行目标文件）的名字：
1 [root@localhost c]# gcc hello.o -o hello
2 [root@localhost c]# ls
3 a.out  hello  hello.c  hello.i  hello.o  hello.s  hello_s.s
 
（5）执行阶段：
1 [root@localhost c]# ./a.out
2 Hello, World!
3 [root@localhost c]# ./hello
4 Hello, World!
 
由此，看出前面使用的gcc hello.c -o hello命令，将hello.c直接编译为可执行的目标文件，中间经过于处理器的预处理阶段（源文件到预处理文件），编译器的编译阶段（预处理文件到汇编文件），汇编器的汇编阶段（汇编文件到可重定向的目标文件），链接器的链接阶段（可重定向的目标文件到可执行的目标文件）。
还有其他的选项如下：
-Idir：dir是头文件所在的目录
-Ldir：dir是库文件所在的目录
-Wall：打印所有的警告信息
-Wl,options：options是传递给链接器的选项
编译优化选项：-O和-O2
-O选项告诉GCC 对源代码进行基本优化。这些优化在大多数情况下都会使程序执行的更快。-O2选项告诉GCC产生尽可能小和尽可能快的代码。
-O2选项将使编译的速度比使用-O时慢。但通常产生的代码执行速度会更快。
除了-O和-O2优化选项外，还有一些低级选项用于产生更快的代码。这些选项非常的特殊，而且最好只有当你完全理解这些选项将会对编译后的代码产生什么样的效果时再去使用。这些选项的详细描述，请参考GCC的联机帮助，在命令行上键入"man gcc"即可。
调试选项：-g（使用详情见第3部分）
-g选项告诉GCC产生能被GNU调试器使用的调试信息以便调试你的程序。
即：在生成的目标文件中添加调试信息，所谓调试信息就是源代码和指令之间的对应关系，在gdb调试和objdump反汇编时要用到这些信息。
3.调试
虽然GCC提供了调试选项，但是本身不能用于调试。Linux 提供了一个名为gdb的GNU调试程序。gdb是一个用来调试C和C++程序的调试器。它使你能在程序运行时观察程序的内部结构和内存的使用情况。以下是gdb所提供的一些功能: 
a.它使你能监视你程序中变量的值；
b.它使你能设置断点以使程序在指定的代码行上停止执行；
c.它使你能一行行的执行你的代码。
(1)启动gdb
在命令行上键入"gdb"并按回车键就可以运行gdb了，如下：
1 [root@localhost c]# gdb
2 GNU gdb (GDB) Red Hat Enterprise Linux (7.2-60.el6_4.1)
3 Copyright (C) 2010 Free Software Foundation, Inc.
4 License GPLv3+: GNU GPL version 3 or later This is free software: you are free to change and redistribute it.
5 There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
6 and "show warranty" for details.
7 This GDB was configured as "x86_64-redhat-linux-gnu".
8 For bug reporting instructions, please see:<>.
9 (gdb)
 
当启动gdb之后，即可在命令行上输入命令进行相关的调试操作。
也可以以下面的方式来启动gdb:
1 [root@localhost c]# gdb hello
 
这种方式启动gdb，直接将指定调试的程序文件装载到调试环境中。也就是让gdb装入名称为filename的可执行文件，从而准备调试。
为了能够进行调试，当前调试的程序文件中必须包含调试信息。其中调试信息包含程序中的每个变量的类型和其在可执行文件里的地址映射以及源代码的行号，gdb利用这些信息使源代码和机器码相关联。因此在使用gcc编译源程序的时候必须使用-g选项，以便将调试信息包含在可执行文件中。
例如：
1 [root@localhost c]# gcc -g hello.c -o hello
 
gdb还提供了其他的启动选项，请参考gdb的联机帮助。在命令行上键入"man gdb"并回车即可。
(2)gdb基本命令
<1>单步执行和跟踪函数调用
程序编辑如下：
01 #include <stdio.h>
02 int add_range(int low, int high){
03    int i;
04    int sum;
05    for(i = low; i <= high; i++){
06        sum = sum + i;
07    }
08    return sum;
09 }
10  
11 int main(){
12    int result[100];
13    result[0] = add_range(1, 10);
14    result[1] = add_range(1, 100);
15    printf("result[0] = %d\nresult[1] = %d\n", result[0], result[1]);
16    return 0;
17  
18 }
 
编译和运行如下：
1 [root@localhost gdb_demo]# vim test1.c 
2 [root@localhost gdb_demo]# gcc test1.c -o test1
3 [root@localhost gdb_demo]# ls
4 test1  test1.c
5 [root@localhost gdb_demo]# ./test1
6 result[0] = 55
7 result[1] = 5105
 
以上程序的结果中，显然第二个结果是不正确的，有基础的人会一眼看出问题处在哪里，呵呵，这里只是为了演示使用gdb调试而故意为之。当然在开发人员最好不要太过于依赖gdb才能找到错误。
在编译时加上-g选项，生成的目标文件才能用gdb进行调试：
01 [root@localhost gdb_demo]# gcc test1.c -g -o test1
02 [root@localhost gdb_demo]# gdb test1
03 GNU gdb (GDB) Red Hat Enterprise Linux (7.2-60.el6_4.1)
04 Copyright (C) 2010 Free Software Foundation, Inc.
05 License GPLv3+: GNU GPL version 3 or later This is free software: you are free to change and redistribute it.
06 There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
07 and "show warranty" for details.
08 This GDB was configured as "x86_64-redhat-linux-gnu".
09 For bug reporting instructions, please see:<>...
10 Reading symbols from /root/code/c/gdb_demo/test1...done.
11 (gdb)
 
-g选项的作用是在目标文件中加入源代码的信息，比如目标文件中的第几条机器指令对应源代码的第几行，但并不是把整个源文件嵌入到目标文件中，所以在调试时目标文件必须保证gdb也能找到源文件。
gdb提供一个类是shell的命令行环境，上面的(gdb)就是提示符，在这个提示符下输入help可以查看命令的类别：
01 (gdb) help
02 List of classes of commands:
03 aliases -- Aliases of other commands
04 breakpoints -- Making program stop at certain points
05 data -- Examining data
06 files -- Specifying and examining files
07 internals -- Maintenance commands
08 obscure -- Obscure features
09 running -- Running the program
10 stack -- Examining the stack
11 status -- Status inquiries
12 support -- Support facilities
13 tracepoints -- Tracing of program execution without stopping the program
14 user-defined -- User-defined commands
15 Type "help" followed by a class name for a list of commands in that class.
16 Type "help all" for the list of all commands.
17 Type "help" followed by command name for full documentation.
18 Type "apropos word" to search for commands related to "word".
19 Command name abbreviations are allowed if unambiguous.
 
可以进一步查看某一个类别中有哪些命令，例如查看files类别下有哪些命令可以用：
01 (gdb) help files
02 Specifying and examining files.
03 List of commands:
04 add-symbol-file -- Load symbols from FILE
05 add-symbol-file-from-memory -- Load the symbols out of memory from a dynamically loaded object file
06 cd -- Set working directory to DIR for debugger and program being debugged
07 core-file -- Use FILE as core dump for examining memory and registers
08 directory -- Add directory DIR to beginning of search path for source files
09 edit -- Edit specified file or function
10 exec-file -- Use FILE as program for getting contents of pure memory
11 file -- Use FILE as program to be debugged
12 forward-search -- Search for regular expression (see regex(3)) from last line listed
13 generate-core-file -- Save a core file with the current state of the debugged process
14 list -- List specified function or line
15 load -- Dynamically load FILE into the running program
 
使用list命令从第一行开始列出源代码：
01 (gdb) list 1
02 1 #include <stdio.h>
03 2 
04 3 int add_range(int low, int high){
05 4     int i;
06 5     int sum;
07 6     for(i = low; i <= high; i++){
08 7         sum = sum + i;
09 8     }
10 9     return sum;
11 10 }
12 (gdb)
 
一次只列出10行，如果要从11行开始继续列出源代码可以输入：
1 (gdb) list
 
也可以什么都不输入直接敲回车，gdb提供类一个方便的功能，在提示符下直接敲回车表示用适当的参数重复上一条命令。
1 (gdb) (直接回车)
2 11 
3 12 int main(){
4 13     int result[100];
5 14     result[0] = add_range(1, 10);
6 15     result[1] = add_range(1, 100);
7 16     printf("result[0] = %d\nresult[1] = %d\n", result[0], result[1]);
8 17     return 0;
9 18 }
 
gdb的很多常用命令有简写形式，例如list命令可以写成l，要列出一个函数的源码也可以用函数名做list的参数：
01 (gdb) l add_range
02 1 #include <stdio.h>
03 2 
04 3 int add_range(int low, int high){
05 4     int i;
06 5     int sum;
07 6     for(i = low; i <= high; i++){
08 7         sum = sum + i;
09 8     }
10 9     return sum;
11 10 }
 
现在退出gdb的环境（quit或简写形式q）：
1 (gdb) quit
 
现在把源代码改名或移动到别处，再用gdb调试目标文件，就列不出源代码了：
01 [root@localhost gdb_demo]# ls
02 test1  test1.c
03 [root@localhost gdb_demo]# mv test1.c test.c
04 [root@localhost gdb_demo]# ls
05 test1  test.c
06 [root@localhost gdb_demo]# gdb test1
07 ......
08 (gdb) l
09 5 test1.c: 没有那个文件或目录.
10 in test1.c
11 (gdb)
 
可见gcc的-g选项并不是把源代码嵌入到目标文件中的，在调试目标文件时也需要源文件。
现在把源代码恢复原样，继续调试。首先使用start命令开始执行程序：
01 [root@localhost gdb_demo]# mv test.c test1.c
02 [root@localhost gdb_demo]# gdb test1
03 ......
04 (gdb) start
05 Temporary breakpoint 1 at 0x4004f8: file test1.c, line 14.
06 Starting program: /root/code/c/gdb_demo/test1
07 Temporary breakpoint 1, main () at test1.c:14
08 14     result[0] = add_range(1, 10);
09 Missing separate debuginfos, use: debuginfo-install glibc-2.12-1.132.el6.x86_64
10 (gdb)
 
这表示停在main函数中变量定义之后的第一条语句处等待我们发命令，gdb列出这条语句表示它还没执行，并且马上要执行。我们可以用next命令(简写为n)控制这些语句一条一条地执行：
1 (gdb) n
2 15     result[1] = add_range(1, 100);
3 (gdb) (直接回车)
4 16     printf("result[0] = %d\nresult[1] = %d\n", result[0], result[1]);
5 (gdb) (直接回车)
6 result[0] = 55
7 result[1] = 5105
8 17     return 0;
 
用n命令依次执行两行赋值语句和一行打印语句，在执行打印语句时结果立刻打印出来类，然后停在return语句之前等待我们发命令。
虽然我们完全控制了程序的执行，但仍然看不出哪里错了，因为错误不再main函数中而是在add_range函数中，现在用start命令重新执行，这次用step命令(简写为s)进入函数中去执行：
01 (gdb) start
02 The program being debugged has been started already.
03 Start it from the beginning? (y or n) y
04 Temporary breakpoint 3 at 0x4004f8: file test1.c, line 14.
05 Starting program: /root/code/c/gdb_demo/test1
06 Temporary breakpoint 3, main () at test1.c:14
07 14     result[0] = add_range(1, 10);
08 (gdb) s
09 add_range (low=1, high=10) at test1.c:6
10 6     for(i = low; i <= high; i++){
 
这次进入add_range函数中，停在函数中定义变量之后的第一条语句处。
在函数中有几种查看状态的办法，backtrace(简写为bt)可以查看函数调用的栈帧：
1 (gdb) bt
2 #0  add_range (low=1, high=10) at test1.c:6
3 #1  0x0000000000400507 in main () at test1.c:14
 
可见当前的add_range函数是被main函数调用的，main函数中传给add_range的参数是low=1, high=10。main函数的栈帧编号是1，add_range函数的栈帧编号是0。
现在可以使用info命令(简写为i)查看add_range局部变量的值：
1 (gdb) i locals
2 i = 0
3 sum = 0
 
如果想查看main函数中的当前局部变量的值也可以做到的，先用frame命令(简写为f)选择1号栈帧，然后再查看main中的局部变量：
01 (gdb) f 1
02 #1  0x0000000000400507 in main () at test1.c:14
03 14     result[0] = add_range(1, 10);
04 (gdb) i locals
05 result = {4195073, 0, -1646196904, 50, 4195016, 0, 0, 1, 2041, 1, -1654612390, 
06  50, 0, 0, -1652419360, 50, -7728, 32767, -7688, 32767, -1652420216, 50, 
07  -134227048, 32767, -163754450, 0, -1654612390, 50, 0, 0, -134227048, 32767, 
08  1, 0, 0, 0, 1, 50, -1652420216, 50, -7848, 32767, 750006344, 0, 6, 0, -7826, 
09  32767, 0, 0, -1652419360, 50, -7808, 32767, -1645672825, 50, -7784, 32767, 0, 
10  1, 0, 0, 4195073, 0, 191, 0, -7826, 32767, -7825, 32767, 1, 0, 0, 0, 
11  -1645672168, 50, 0, 0, 4195680, 0, 0, 0, 4195235, 0, -7512, 32767, 4195749, 
12  0, -1646199904, 50, 4195680, 0, 0, 0, 4195296, 0, -7536, 32767, 0, 0}
 
注意到result数组中的很多元素具有杂乱无章的值，因为未经初始化的局部变量具有不确定的值。
到目前为止（即已经进入第一次的函数调用的函数体内），是正常的。
继续用s或n往下走，然后用print(简写为p)打印变量sum的值。
01 (gdb) s
02 7         sum = sum + i;
03 (gdb) 
04 6     for(i = low; i <= high; i++){
05 (gdb) 
06 7         sum = sum + i;
07 (gdb) 
08 6     for(i = low; i <= high; i++){
09 (gdb) p sum
10 $1 = 3
 
注意：这里的$1表示gdb保存着这些中间结果，$后面的编号会自动增长，在命令中可以用$1、$2、$3等编号代替相应的值。
-----------------------------------------------------
以上的执行过程使用下面的方法可能看得更清楚（这里的步骤不是继续跟着上面步骤，是在另一个终端中调试的）：
01 (gdb) f 0
02 #0  add_range (low=1, high=10) at test1.c:7
03 7         sum = sum + i;
04 (gdb) i locals
05 i = 1
06 sum = 0
07 (gdb) s
08 6     for(i = low; i <= high; i++){
09 (gdb) i locals
10 i = 1
11 sum = 1
12 (gdb) s
13 7         sum = sum + i;
14 (gdb) i locals
15 i = 2
16 sum = 1
17 (gdb) s
18 6     for(i = low; i <= high; i++){
19 (gdb) i locals
20 i = 2
21 sum = 3
 
-----------------------------------------------------
由此看出，第一次循环的结果是正确的，再往下单步调试已经没有意义了，可以使用finish命令让程序一直运行到从当前函数返回为止：
1 (gdb) finish
2 Run till exit from #0  add_range (low=1, high=10) at test1.c:6
3 0x0000000000400507 in main () at test1.c:14
4 14     result[0] = add_range(1, 10);
5 Value returned is $1 = 55
 
返回值是55，当前正准备执行赋值操作，用s命令执行赋值操作，然后查看result数组：
01 (gdb) s
02 15     result[1] = add_range(1, 100);
03 (gdb) print result
04 $2 = {55, 0, -1646196904, 50, 4195016, 0, 0, 1, 2041, 1, -1654612390, 50, 0, 0, 
05  -1652419360, 50, -7728, 32767, -7688, 32767, -1652420216, 50, -134227048, 
06  32767, -163754450, 0, -1654612390, 50, 0, 0, -134227048, 32767, 1, 0, 0, 0, 
07  1, 50, -1652420216, 50, -7848, 32767, 750006344, 0, 6, 0, -7826, 32767, 0, 0, 
08  -1652419360, 50, -7808, 32767, -1645672825, 50, -7784, 32767, 0, 1, 0, 0, 
09  4195073, 0, 191, 0, -7826, 32767, -7825, 32767, 1, 0, 0, 0, -1645672168, 50, 
10  0, 0, 4195680, 0, 0, 0, 4195235, 0, -7512, 32767, 4195749, 0, -1646199904, 
11  50, 4195680, 0, 0, 0, 4195296, 0, -7536, 32767, 0, 0}
 
第一个值是55确实赋值给类result数组的第0个元素。
使用s命令进入第二次add_range调用，进入之后首先查看参数和局部变量：
1 (gdb) s
2 add_range (low=1, high=100) at test1.c:6
3 6     for(i = low; i <= high; i++){
4 (gdb) bt
5 #0  add_range (low=1, high=100) at test1.c:6
6 #1  0x000000000040051c in main () at test1.c:15
7 (gdb) i locals
8 i = 11
9 sum = 55
 
到这里，看出了问题：由于局部变量i和sum没有初始化，所以具有不确定的值，又由于连次调用是连续的，i和sum正好取类上次调用时的值。i的初始值不是0不要紧，因为在for循环开始重新赋值了，但如果sum的处置不是0，累加得到的结果就错了。
问题找到了，可以退出gdb修改源代码了。然而我们不想浪费一次调试机会，可以在gdb中马上把sum的初始值改为0，继续运行，看看改了之后有没有其他的bug：
01 (gdb) set var sum=0
02 (gdb) finish
03 Run till exit from #0  add_range (low=1, high=100) at test1.c:6
04 0x000000000040051c in main () at test1.c:15
05 15     result[1] = add_range(1, 100);
06 Value returned is $3 = 5050
07 (gdb) s
08 16     printf("result[0] = %d\nresult[1] = %d\n", result[0], result[1]);
09 (gdb) s
10 result[0] = 55
11 result[1] = 5050
12 17     return 0;
 
这样结果就对了。
修改变量的值除了用set命令之外也可以使用print命令，因为print命令后跟的是表达式，而我们知道赋值和函数调用都是表达式，所以还可以用print来修改变量的值，或者调用函数：
1 (gdb) print result[2]=88
2 $4 = 88
3 (gdb) p printf("result[2]=%d\n", result[2])
4 result[2]=88
5 $5 = 13
 
我们知道：printf函数的返回值表示实际打印的字符数，所以$5的结果是13。
总结一下本节使用过的gdb命令：
01 list(l)：列出源代码，接着上次的位置往下列，每次列10行
02 list 行号：列出产品从第几行开始的源代码
03 list 函数名：列出某个函数的源代码
04 start：开始执行程序，停在main函数第一行语句前面等待命令
05 next(n)：执行下一列语句
06 step(s):执行下一行语句，如果有函数调用则进入到函数中
07 breaktrace(或bt)：查看各级函数调用及参数
08 frame(f) 帧编号：选择栈帧
09 info（i) locals：查看当前栈帧局部变量的值
10 finish：执行到当前函数返回，然后挺下来等待命令
11 print(p)：打印表达式的值，通过表达式可以修改变量的值或者调用函数
12 set var：修改变量的值
13 quit：退出gdb
 
<2>断点
程序编辑如下：
01 # include <stdio.h>
02 int main(){
03    int sum =0;
04    int i = 0;
05    char input[5];
06     
07    while(1){
08        scanf("%s", input);//在输入字符后自动加'\0'形成字符串
09        for(i = 0; input[i] != '\0'; i++){
10            sum = sum * 10 + input[i] - '0';//'1'-'0'=1,'\0'=0
11        }
12        printf("input = %d\n", sum);
13    }
14    return 0;
15 }
 
编译和运行：
01 [root@localhost gdb_demo]# vim test2.c
02 [root@localhost gdb_demo]# gcc test2.c -g -o test2
03 [root@localhost gdb_demo]# ls
04 test1  test1.c  test2  test2.c
05 [root@localhost gdb_demo]# ./test2
06 123
07 input = 123
08 12345
09 input = 12345
10 12345678
11 input = -268647318
12 （Ctrl-C退出程序）
 
从结果看出程序明显是有问题的。
下面来调试：
1 [root@localhost gdb_demo]# gdb test2
2 ......
3 (gdb) start
4 Temporary breakpoint 1 at 0x40053c: file test2.c, line 4.
5 Starting program: /root/code/c/gdb_demo/test2
6 Temporary breakpoint 1, main () at test2.c:4
7 4     int sum =0;
8 Missing separate debuginfos, use: debuginfo-install glibc-2.12-1.132.el6.x86_64
9 (gdb)
 
可见，start不会跳过赋值语句，通过第一次单步调试的例子，sum可列为重点观察对象，可以使用display命令使得每次停下来的时候都显示当前的sum值,继续往下走：
01 (gdb) display sum
02 1: sum = 0
03 (gdb) n
04 5     int i = 0;
05 1: sum = 0
06 (gdb) 
07 9         scanf("%s", input);
08 1: sum = 0
09 (gdb) 
10 123
11 10         for(i = 0; input[i] != '\0'; i++){
12 1: sum = 0
13 (gdb)
 
这个循环应该是没有问题的，因为循环开始sum的值是正确的。可以使用undisplay取消先前设置的那些变量的跟踪。
如果不想一步一步跟踪这个循环，可以使用break(简写为b)在第8行设置一个断点（Breakpoint）：
01 (gdb) l
02 5     int i = 0;
03 6     char input[5];
04 7     
05 8     while(1){
06 9         scanf("%s", input);
07 10         for(i = 0; input[i] != '\0'; i++){
08 11             sum = sum * 10 + input[i] - '0';
09 12         }
10 13         printf("input = %d\n", sum);
11 14     }
12 (gdb) b 8
13 Breakpoint 2 at 0x40054a: file test2.c, line 8.
 
break命令的参数也可以是函数名，表示在某一个函数开头设置断点。现在用continue命令(简写为c)连续运行而非单步运行，程序到达断点会自动停下来，这样就可以停在下一次循环的开头：
1 (gdb) c
2 Continuing.
3 input = 123
4 Breakpoint 2, main () at test2.c:9
5 9         scanf("%s", input);
6 1: sum = 123
 
然后输入新的字符串准备转换：
1 (gdb) n
2 12345
3 10         for(i = 0; input[i] != '\0'; i++){
4 1: sum = 123
 
此时问题已经暴露出来了，新的转换应该是从0开始累加的，而现在sum却是123，原因在于新的循环没有把sum归零。
可见断点有助于快速跳过与问题无关的代码,然后在有问题的代码上慢慢走慢慢分析,“断点加单步”是使用调试器的基本方法。至于应该在哪里设置断点,怎么知道哪些代码可以跳过而哪些代码要慢慢走,也要通过对错误现象的分析和假设来确定。
一次调试可以设置多个断点，用info命令(简写为i)可以查看已经设置的断点：
1 (gdb) b 11
2 Breakpoint 3 at 0x40056c: file test2.c, line 11.
3 (gdb) i breakpoints
4 Num     Type           Disp Enb Address            What
5 2       breakpoint     keep y   0x000000000040054a in main at test2.c:8
6 breakpoint already hit 2 times
7 3       breakpoint     keep y   0x000000000040056c in main at test2.c:11
 
每一个断点都有一个编号，可以用编号指定删除某个断点：
1 (gdb) delete breakpoints 2
2 (gdb) i breakpoints
3 Num     Type           Disp Enb Address            What
4 3       breakpoint     keep y   0x000000000040056c in main at test2.c:11
 
有时候一个断点不想用可以禁用而不必删除，这样以后想用的时候可以直接启用，而不必重新从代码里找应该在哪一行设置断点：
01 (gdb) disable breakpoints 3
02 (gdb) i breakpoints
03 Num     Type           Disp Enb Address            What
04 3       breakpoint     keep n   0x000000000040056c in main at test2.c:11
05 (gdb) enable breakpoints 3
06 (gdb) i breakpoints
07 Num     Type           Disp Enb Address            What
08 3       breakpoint     keep y   0x000000000040056c in main at test2.c:11
09 (gdb) delete breakpoints
10 Delete all breakpoints? (y or n) y
11 (gdb) i breakpoints
12 No breakpoints or watchpoints.
 
gdb设置断点功能非常灵活，还可以设置断点在满足某个条件时才激活，例如我们仍然在循环开头设置断点，但是仅当sum不等于0时才中断，然后用run(简写为r)重新从程序开头连续执行：
01 (gdb) break 10 if sum != 0
02 Breakpoint 5 at 0x400563: file test2.c, line 10.
03 (gdb) i breakpoints
04 Num     Type           Disp Enb Address            What
05 5       breakpoint     keep y   0x0000000000400563 in main at test2.c:10
06 stop only if sum != 0
07 (gdb) r
08 The program being debugged has been started already.
09 Start it from the beginning? (y or n) y
10 Starting program: /root/code/c/gdb_demo/test2 
11 123
12 input = 123
13 123
14 Breakpoint 5, main () at test2.c:10
15 10         for(i = 0; input[i] != '\0'; i++){
16 1: sum = 123
 
结果：第一次执行输入之后没有中断，第二次却中断了，因为第二次循环开始sum的值为123。
总结一下本节使用到的gdb命令：
01 break(b) 行号：在某一行设置断点
02 break 函数名：在某个函数开头设置断点
03 break...if...：设置条件断点
04 continue(或c)：从当前位置开始连续而非单步执行程序
05 delete breakpoints：删除所有断点
06 delete breakpoints n：删除序号为n的断点
07 disable breakpoints：禁用断点
08 enable breakpoints：启用断点
09 info(或i) breakpoints：参看当前设置了哪些断点
10 run(或r)：从开始连续而非单步执行程序
11 display 变量名：跟踪查看一个变量，每次停下来都显示它的值
12 undisplay：取消对先前设置的那些变量的跟踪
 
下面再看一个例子：
程序如下：
01 #include <stdio.h>
02 int main(){
03    int i;
04    char str[6] = "hello";
05    char reverse_str[6] = "";
06    printf("%s\n", str);
07    for(i = 0; i < 5; i ++){
08        reverse_str[5-i] = str[i];
09    }
10    printf("%s\n", reverse_str);
11    return 0;
12 }


运行结果：
1 [root@localhost gdb_demo]# gcc test3.c -g -o test3
2 [root@localhost gdb_demo]# ./test3
3 hello
 
其中，第二次打印空白，结果显然是不正确的。
调试过程如下：
01 [root@localhost gdb_demo]# gdb test3
02 ......
03 (gdb) l
04 1 #include <stdio.h>
05 2 
06 3 int main(){
07 4     int i;
08 5     char str[6] = "hello";
09 6     char reverse_str[6] = "";
10 7 
11 8     printf("%s\n", str);
12 9     for(i = 0; i < 5; i ++){
13 10         reverse_str[5-i] = str[i];
14 (gdb) 
15 11     }
16 12     printf("%s\n", reverse_str);
17 13     return 0;
18 14 }
19 (gdb) i breakpoints
20 No breakpoints or watchpoints.
21 (gdb) b 10
22 Breakpoint 1 at 0x4004fb: file test3.c, line 10.
23 (gdb) i breakpoints
24 Num     Type           Disp Enb Address            What
25 1       breakpoint     keep y   0x00000000004004fb in main at test3.c:10
26 (gdb) r
27 Starting program: /root/code/c/gdb_demo/test3 
28 hello
29 Breakpoint 1, main () at test3.c:10
30 10         reverse_str[5-i] = str[i];
31 Missing separate debuginfos, use: debuginfo-install glibc-2.12-1.132.el6.x86_64
32 (gdb) p reverse_str
33 $1 = "\000\000\000\000\000"
34 (gdb) c
35 Continuing.
36 Breakpoint 1, main () at test3.c:10
37 10         reverse_str[5-i] = str[i];
38 (gdb) p reverse_str
39 $2 = "\000\000\000\000\000h"
40 (gdb) c
41 Continuing.
42 Breakpoint 1, main () at test3.c:10
43 10         reverse_str[5-i] = str[i];
44 (gdb) p reverse_str
45 $3 = "\000\000\000\000eh"
46 (gdb) c
47 Continuing.
48 Breakpoint 1, main () at test3.c:10
49 10         reverse_str[5-i] = str[i];
50 (gdb) p reverse_str
51 $4 = "\000\000\000leh"
52 (gdb) c
53 Continuing.
54 Breakpoint 1, main () at test3.c:10
55 10         reverse_str[5-i] = str[i];
56 (gdb) p reverse_str
57 $5 = "\000\000lleh"
58 (gdb) c
59 Continuing.
60 Program exited normally.
61 (gdb)
 
由上面的观察可知：
在于将str数组中的值赋值到reverse_str的时候，将str的第1个元素赋值给类reverse_str的第6个元素，而该循环只循环了5次（即str数组元素个数），从而导致reverse_str的第一个元素为'\000'，所以输出为空白。
修改如下：
01 #include <stdio.h>
02 #include <string.h>
03 int main(){
04    int i;
05    char str[6] = "hello";
06    char reverse_str[6] = "";
07    printf("%s\n", str);
08    int len = strlen(str);
09    for(i = 0; i <= len-1; i ++){
10        reverse_str[len-1-i] = str[i];
11    }
12    printf("%s\n", reverse_str);
13    return 0;
14 }
 
再次运行就好了：
1 [root@localhost gdb_demo]# gcc test3.c -o test3
2 [root@localhost gdb_demo]# ./test3
3 hello
4 olleh
 
<3>观察点(Watchpoint)
断点是当程序执行到某一代码行时中断,而观察点一般来观察某个表达式（变量也是一种表达式）的值是否有变化了，如果有变化，马上停住程序。
vim test4.c
01 # include <stdio.h>
02 int main(){
03    int sum =0;
04    int i;
05    for(i = 1; i <= 10; i++){
06        sum = sum +i;
07    }
08    printf("sum = %d\n", sum);
09    return 0;
10 }
 
编译运行：
1 [root@localhost gdb_demo]# gcc test4.c -g -o test4
2 [root@localhost gdb_demo]# ./test4
3 sum = 55
 
设置观察点进行调试：
01 [root@localhost gdb_demo]# gdb test4
02 ......
03 (gdb) start
04 Temporary breakpoint 1 at 0x4004cc: file test4.c, line 4.
05 Starting program: /root/code/c/gdb_demo/test4
06 Temporary breakpoint 1, main () at test4.c:4
07 4     int sum =0;
08 Missing separate debuginfos, use: debuginfo-install glibc-2.12-1.132.el6.x86_64
09 (gdb) l
10 1 # include <stdio.h>
11 2 
12 3 int main(){
13 4     int sum =0;
14 5     int i;
15 6     for(i = 1; i <= 10; i++){
16 7         sum = sum +i;
17 8     }
18 9     printf("sum = %d\n", sum);
19 10     return 0;
20 (gdb) 
21 11 }
22 (gdb) watch sum
23 Hardware watchpoint 2: sum
24 (gdb) c
25 Continuing.
26 Hardware watchpoint 2: sum
27 Old value = 0
28 New value = 1
29 main () at test4.c:6
30 6     for(i = 1; i <= 10; i++){
31 (gdb) c
32 Continuing.
33 Hardware watchpoint 2: sum
34 Old value = 1
35 New value = 3
36 main () at test4.c:6
37 6     for(i = 1; i <= 10; i++){
38 (gdb) c
39 Continuing.
40 Hardware watchpoint 2: sum
41 Old value = 3
42 New value = 6
43 main () at test4.c:6
44 6     for(i = 1; i <= 10; i++){
45 (gdb) c
46 Continuing.
47 Hardware watchpoint 2: sum
48 Old value = 6
49 New value = 10
50 main () at test4.c:6
51 6     for(i = 1; i <= 10; i++){
52 (gdb)
 
总结一下本节使用到的gdb命令：
1 watch：设置观察点
2 info(或i) watchpoints：查看当前设置了哪些观察点
 


GDB的补充：
输出格式：
一般来说，GDB会根据变量的类型输出变量的值。但你也可以自定义GDB的输出的格式。
例如，你想输出一个整数的十六进制，或是二进制来查看这个整型变量的中的位的情况。要
做到这样，你可以使用GDB的数据显示格式： 
x 按十六进制格式显示变量。 
d 按十进制格式显示变量。 
u 按十六进制格式显示无符号整型。 
o 按八进制格式显示变量。 
t 按二进制格式显示变量。 
a 按十六进制格式显示变量。 
c 按字符格式显示变量。 
f 按浮点数格式显示变量。
01 (gdb) p sum
02 $1 = 10
03 (gdb) p/a sum
04 $2 = 0xa
05 (gdb) p/x sum
06 $3 = 0xa
07 (gdb) p/o sum
08 $4 = 012
09 (gdb) p/t sum
10 $5 = 1010
11 (gdb) p/f sum
12 $6 = 1.40129846e-44
13 (gdb) p/c sum
14 $7 = 10 '\n'
 
查看内存：
你可以使用examine命令（简写是x）来查看内存地址中的值。x命令的语法如下所示： 
x/ 
n、f、u是可选的参数。 
n 是一个正整数，表示显示内存的长度，也就是说从当前地址向后显示几个地址的内容。 
f 表示显示的格式，参见上面。如果地址所指的是字符串，那么格式可以是s，如果地十是指令地址，那么格式可以是i。
u 表示从当前地址往后请求的字节数，如果不指定的话，GDB默认是4个bytes。u参数可以用下面的字符来代替，b表示单字节，h表示双字节，w表示四字节，g表示八字节。当我们指定了字节长度后，GDB会从指内存定的内存地址开始，读写指定字节，并把其当作
一个值取出来。 
表示一个内存地址。 
n/f/u三个参数可以一起使用。例如： 
命令：x/3uh 0x54320 表示，从内存地址0x54320读取内容，h表示以双字节为一个单位，3表示三个单位，u表示按十六进制显示。
```