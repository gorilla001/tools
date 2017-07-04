#### shell  和 vi

##### 1	认识bash这个shell
在window系统下使用bash，需要一个软件，这个软件模拟集成了bash大部分命令。
各个 shell 的功能都差不多， Linux 默认使用 bash ，所以我们主要学习bash的使用。

######  bash命令格式
命令 [-options]  [参数]，如：tar  zxvf  demo.tar.gz
查看帮助：命令 --help

#####  bash常见命令

pwd (Print Working Directory) 查看当前目录
cd (Change Directory) 切换目录，如 cd /etc
ls (List) 查看当前目录下内容，如 ls -al
mkdir (Make Directory) 创建目录，如 mkdir blog
touch 创建文件，如 touch index.html
cat 查看文件全部内容，如 cat index.html
less 查看文件，如more /etc/passwd、less /etc/passwd
rm (remove) 删除文件，如 rm index.html、rm -rf  blog
rmdir (Remove Directory) 删除文件夹，只能删除空文件夹，不常用
mv (move) 移动文件或重命名，如 mv index.html ./demo/index.html
cp (copy) 复制文件，cp index.html ./demo/index.html
tab 自动补全，连按两次会将所有匹配内容显示出来
> 和 >>重定向，如echo hello world! > README.md，>覆盖 >>追加
** | 管道符可以将多个命令连接使用，上一次（命令）的执行结果当成下一次（命令）的参数。
grep 匹配内容，一般结合管道符使用 **

##### 2 vi编辑器

如同Windows下的记事本，vi编辑器是Linux下的标配，通过它我们可以创建、编辑文件。它是一个随系统一起安装的文本编辑**软件**。

1. 三种模式

vi编辑器提供了3种模式，分别是命令模式、插入模式、底行模式，每种模式下用户所能进行的操作是不一样的:

![vi运行模式][id]
[id]:C:\Users\Administrator\Desktop\github\git\images

通过上图我们发现，输入模式是不能直接切换到末行模式的，必须要先切回到命令模式（按ESC键）
2. 使用vi/vim编辑器
	a) 打开/创建文件， vi 文件路径
	b) 底行模式 :w保存，:w filenme另存为
	c) 底行模式 :q退出
	d) 底行模式 :wq保存并退出
	e) 底行模式 :e! 撤销更改，返回到上一次保存的状态
	f) 底行模式 :q! 不保存强制退出
	g) 底行模式 :set nu 设置行号
	h) 命令模式 ZZ（大写）保存并退出
	i) 命令模式 u辙销操作，可多次使用
	j) 命令模式 dd删除当前行
	k) 命令模式 yy复制当前行
	l) 命令模式 p 粘贴内容
	m) 命令模式 ctrl+f向前翻页
	n) 命令模式 ctrl+b向后翻页
	o) 命令模式 i进入编辑模式，当前光标处插入
	p) 命令模式 a进入编辑模式，当前光标后插入
	q) 命令模式 A进入编辑模式，光标移动到行尾
    r) 命令模式 o进入编辑模式，当前行下面插入新行
	s) 命令模式 O进入编辑模式，当前行上面插入新行
当我们处在编辑模式的情况下，和我们在Windows编辑器的使用相似。

3. SSH

SSH是一种网络协议，用于计算机之间的加密登录。
SSH只是一种协议，存在多种实现，既有商业实现，也有开源实现。本文针对的是OpenSSH，它是自由软件，应用非常广泛。
如果要在Windows系统中使用SSH，会用到另一种软件PuTTY，我们后面用到的Git客户也集成了SSH
格式：ssh user@host
user 代表真实存在的用户host代表要登录的远程计算机

常见有两种加密技术，分别是对称性加密和非对称性加密，SSH属于后者。
对称加密算法在加密和解密时使用的是同一个密钥；而非对称加密算法需要两个密钥来进行加密和解密，这两个秘钥分别是公开密钥（public key，简称公钥）和私有密钥（private key，简称私钥）。

工作原理

公钥和私钥是成对出现，可以通过ssh-keygen -t rsa来创建，既可以通过密钥来加密数据，也可以通过私钥来加密数据，如果是以公钥进行的数据加密，只能与之相对应的私钥才可以解密，相反如果以私钥进行的数据加密，则只能与之对应的公钥才可以将数据进行解密，这样就可以提高信息传递的安全性。

免密码登录（待定）

我们可以将本地机器上的公钥保存到特定的远程计算机上，这样当我们再次登录访问这台远程计算机时就可以实现免密码登录了。

1、ssh-keygen -t rsa会创建公钥和密钥（默认在用户目录/.ssh目录下）
2、ssh-copy-id user@host添加到对应远程主机的用户目录/.ssh目录下
3、也可以登录远程主机，进入到用户目录/.ssh目录下手动创建authorized_keys文件，并将自已的公钥粘入该文件。
