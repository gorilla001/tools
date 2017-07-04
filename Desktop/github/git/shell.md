####  关于 shell

+  在计算机科学中，shell俗称壳，用来区别于Kernel(核)，是指"提供使用者界面"的软件(命令解析器)，它类似于Dos下的command和后来的cmd.exe。<span color:red>它接收用户命令，然后调用相应的应用程序</span>
+ shell很强大，很大程度上得益于对命令做了额外的处理，可以在命令中嵌入其他的命令，在参数中嵌入其他的命令，或者嵌入变量，插入路径通配符，插入表达式，shell都能帮你处理的很好，就是因为shell能做这些，所以shell才能如此的强大
+ 了解shell处理命令的过程，对你学习shell很有帮助

#####shell处理命令的过程
1. 先按<tab> <newline> <space> ( ) < > ; | & 把命令分割成一个个的token
2. 检查第一个token是不是一个开放的关键字，如 for if { 等，如果是，说明这是一个复合命令，    shell会在内部对关键字进行处理，并重复这一步骤
3. 按别名李彪检查每个命令的第一个关键字是不是一个别名，如果是，则用其别名定义替换，然后回退到第一步
4. 执行花括号 { } 扩展
5. 执行 ~ 扩展
6. 执行变量扩展
7. 执行命令替换
8. 执行算术表达式计算
9. 把生成的新命令按IFS分割成token
10. 执行路径扩展
11. 按优先级查找命令，先从内置，再从path
12. 设定好重定向等，执行命令

###### 花括号扩展
+ 简单点说就是给一组字符串加上相同的前缀和后缀，生成一组新的字符串。前缀和后缀都可以为空

    Shell代码
Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ echo a{b,c}d   
abd acd   
Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ echo a{b,c}   
ab ac  

+ 可以使用一个范围，只支持数字和字母

     Shell代码
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ echo a{1..10}
a1 a2 a3 a4 a5 a6 a7 a8 a9 a10

Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ echo a{b..l}
ab ac ad ae af ag ah ai aj ak al

Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ echo 您{好，吃了吗}？
您{好，吃了吗}？

+ 花括号还可以嵌套，逐层有序的进行处理

    Shell代码
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo a{{1,2},{b,c}}   
    a1 a2 ab ac  
**这个过程可以理解为先生成a{1,2,b,c},然后生成 a1 a2 ab ac **

**要注意的地方**
1. 花括号里只有字面量。不支持变量：
     Shell代码
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ a=1  
    $ echo a{a..10}   
    {1..10}

并没有像之前一样生成1,2,3,4..10。**对花括号扩展来说，生成的结果是{$a..10}**，那为什么我们看到的结果是{1..10}呢？这就是shell的命令处理顺序有关系了，我们在看shell的命令处理顺序，花括号扩展是在第4布，到了第6步，才会执行变量扩展，这时候**相当于命令**
   
     Shell代码
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    echo {$a..10} 

    **先执行echo{$a..10},在执行变量扩展 $a被替换成1**，因此输出 {1..10}

2. 花括号里至少要有一个逗号，至少要有两项
3. 两项之间不能有空格，逗号前后不能有空格，否则不进行花括号扩展

###### 波浪号扩展
波浪号扩展就是对 ~ 进行处理。一般情况下，我们认为 ~ 代表了自己的主目录，其实事情并非这么简单。进行波浪号替换的条件是很苛刻的

首先，进行波浪号扩展的前提是波浪号必须位于一个token的开头，简单的说 ~ 前面应该是空格

然后，shell会分析波浪号之后，第一个 / 或 : 之前的未被引号括起来的字符串（如果没有 / 那就取波浪号之后的所有字符）这个字符叫做"波浪号前缀(tilde-prefix)"（注意，所谓波浪号前缀其实是出现在波浪号后面的），波浪号前缀的取值和对应的处理方式是：

1. 如果波浪号前缀是个有效用户名，则波浪号和波浪号前缀一起替换成用户的主目录
2. 如果波浪号前缀为空，则尝试把波浪号替换成HOME,如果HOME没有被设置，则将波浪号替换成当前用户主目录
3. 如果波浪号前缀是 + 则 ~+ 被替换成当前工作目录(pwd)
4. 如果波浪前缀是 - 则 ~- 被替换成上一个工作目录 (OLDPWD)
5. 如果波浪号前缀是个数字 n 则把 ~n 替换成目录堆栈 (用 dirs 命令可以查看目录堆栈)的第n个元素(这个没有什么用)   

      Shell代码 
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo ~
    /c/Users/Administrator/Desktop/github/git   *替换成当前用户主目录*

    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo /~ 
    /~               *波浪号不在token的开头，不进行扩展*                       

    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo ~root 
    /root
 
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo ~root/ 
    /root/

    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo ~+
     /c/Users/Administrator/Desktop/github/git   *被替换成当前工作目录*


    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo ~-
    ~-   *替换成上一个目录,因为没用cd，上一个目录OLDPWD没有设置，替换失败原样输出*
       
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ cd Markdown
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ echo ~-
    /c/Users/Administrator/Desktop/github/git   *现在好了*

###### 变量扩展
这个大家最熟悉了，$foo! $真是个好东西，变量扩展，命令替换，算术扩展都离不了它（当然还能买东西）。一般情况下我们习惯使用$var，其实正规的格式是${var}。前一种形式更简便，后一种更强大，很多时候必须用后一种形式才行。

先说间接引用，这东西很像C语言里的指针

${!var} 就是在花括号后面紧跟一个感叹号。bash会把变量的值作为新的变量再求值

      Shell代码 
      Administrator@AAA MINGW64 ~/Desktop/github/git (master)
      $ a=b
      Administrator@AAA MINGW64 ~/Desktop/github/git (master)
      $ b=1
      Administrator@AAA MINGW64 ~/Desktop/github/git (master)
      $ echo ${!a}
      1

      其他的变量操作，列表看到的更清晰

      形式                  意义
      ${var:-word}	如果变量var已被设置且非空，则代入它的值，否则带入word
      ${var:=word}	如果变量var已被设置且空，就带入它的值，否则将var设为word并带入var，位置参量不能用这种方式赋值。
      ${var:+word}	如果var已被设置且值非空，带入word；否则什么都不带入(带入空)
      ${var:?word}	如果var已被设置且值非空，就带入它的值，否则打印word并退出shell。省略word会输出：parameter null or not set

     ** 注意：上面word可以是一个变量，使用$word的形式引用其值 **
     ${var:offset}	获取var中offset开始的字串
     ${var:offset:length}	获取var中offset开始长为length的字串。
     注意：上面的offset和length可以使变量，使用$offset,$length引用其值
     ${#var}	替换为变量中字符个数，如果var是* ，@或数组，长度则是位置参量的个数。
     ${var%pattern}	把字符串尾部与模式进行最小匹配，并删除匹配到的部分。
     ${var%%pattern}	把字符串尾部与模式进行最大匹配，并删除匹配到的部分。
     ${var#pattern}	把字符串头部与模式进行最小匹配，并删除匹配到的部分。
     ${var##pattern}	把字符串头部与模式进行最大匹配，并删除匹配到的部分
     ${var/pattern/string}	使用string替换pattern的最大匹配部分。如果pattern以/开头则进行全部替换，否则只替换第一个匹配的位置。如果pattern以#开始，则起始部分必须匹配，如果以%开始则结尾部分必须匹配

     ** 注意：

上面的pattern可以使变量，使用$pattern引用其值。

如果var是*、@或数组且以下标为*或@的形式出现，则对其中每一个元素都进行匹配操作。**

###### 命令替换

用命令的输出来替换命令本身。有两种形式$(cmd)和`cmd`，推荐前一种形式，后一种形式是old-style了。

###### 算术扩展

用算术表达式的值替换算术表达式本身。格式$((expr))。expr是个表达式，如4+3。理解起来比较简单。不过关于expression，bash有自己特定的支持，某些运算它是做不了的。

Shell代码
 /c/Users/Administrator/Desktop/github/git
 $ echo $((9+2))   
11  
 /c/Users/Administrator/Desktop/github/git
 $ b=2  
 /c/Users/Administrator/Desktop/github/git
 $ echo $((4+b))   
6  
 
 ##### 路径扩展

shell扫描每个标记看看是否有 * ? [] 这三个就是进行路径扩展的。如果某个标记里出现了三者中的一个或多个，这个标记就被认为是一个模式，shell会对当前目录下的文件列表按文件名排序并逐一与此模式进行比较，如果有匹配这个模式的文件，shell用所有能匹配这个模式的文件名列表替换这个模式。如果没有能匹配这个模式的文件，shell原样保留该模式。
当然，shell提供了很多选项，定制匹配成功和失败的处理，还可以选择使用高级的正则表达式，这里不进行讨论，只说说shell的默认情况
此处的三个特殊字符也都比较简单 * 匹配0到多个字符，?匹配一个字符，[]匹配某个去区间里一个字符
