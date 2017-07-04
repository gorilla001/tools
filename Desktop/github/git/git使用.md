#### git

1. git安装
Window安装
http://git-scm.com/download/win下载Git客户端软件，和普通软件安装方式一样。
Linux安装
CentOS发行版：sudo yum install git
Ubuntu发行版：sudo apt-get install git
Mac安装
打开Terminal直接输入git命令，会自动提示，按提示引导安装即可。

2. git 工作原理
为了更好的学习Git，我们们必须了解Git管理我们文件的4种状态：
未追踪（untracked）
    + 文件未被添加到暂存区，也没有被提交过
未修改（Unmodify）
    + 自上次提交后没有修改过代码
已修改（modified）
    + 自上次提交后，文件有过修改
已暂存（staged）
    + 文件已被添加到暂存区域

由此引入 Git 项目的3个工作区域的概念：

工作目录、暂存区域、Git 仓库

工作目录是对项目的某个版本独立提取出来的内容。这些从Git仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
  
暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在Git仓库目录中。有时候也被称作“索引”（Index），不过一般说法还是叫暂存区域。
  
Git仓库目录是Git用来保存项目的元数据和对象数据库的地方。 这是Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。


基本的Git工作流程如下：
	1. 在工作目录中修改文件。
	2. 暂存文件，将文件的快照放入暂存区域。
	3. 提交文件，找到暂存区域的文件，将快照永久性存储到Git仓库目录。

3. git本地仓库
Git本地仓库指的是开发者计算机中的仓库。

###### git基础
 
 命令行方式：任意目录（建议开发根目录）右键 > Git Bash Here

 1. 配置用户
 配置用户的意义在于记录开发者信息，以便在版本控制记录开发者的操作行为

    git代码
    git config --global user.name "自已的名字"
    git config --global user.email "自已的邮箱地址"
    --global 配置当前用户所有仓库
注：配置用户只需要执行1次，可以重复使用。
    配置的用户信息会被保存到 C:\Users\[用户名]\.gitconfig文件中

2. 初始化仓库
我们如果想要利用git进行版本控制，需要将现有项目初始化为一个仓库，或者将一个已有的使用git进行版本控制的仓库克隆到本地。

    git代码
   Administrator@AAA MINGW64 ~/Desktop/github/git (master)
   $ git init
   Initialized empty Git repository in C:/Users/Administrator/Desktop/github/git/.git/

git init会在当前项目目录中创建一个名为.git的隐藏目录，这个目录包含了暂存区和仓库两个区域，有了这个隐藏目录就可以使用git来管理项目了，通过ls  -al 可以查看。

     git代码
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ ls -al
total 32
drwxr-xr-x 1 Administrator 197121    0 6月  30 17:19  ./
drwxr-xr-x 1 Administrator 197121    0 6月  30 17:06  ../
drwxr-xr-x 1 Administrator 197121    0 6月  30 17:21  .git/
-rw-r--r-- 1 Administrator 197121    0 6月  30 17:09  git.md
-rw-r--r-- 1 Administrator 197121 2332 6月  30 17:09  vcs.md
drwxr-xr-x 1 Administrator 197121    0 6月  30 17:04  images/

 3. 查看文件状态
初始化仓库后便可以进行开发了，进入到刚刚创建好并初始为仓库的目录，添加我们开发需要的文件。

通过git status可以检测当前仓库文件的状态（未追踪untracked）。

     git代码
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        git.md
        vcs.md
        "\346\226\260\345\273\272\346\226\207\344\273\266\345\244\271/"

nothing added to commit but untracked files present (use "git add" to track)
注：git会忽略空的目录

4. 添加文件到暂存区
假设经过一段时间的开发后，需要把已开发的部分暂存起来等待提交，使用git add 添加到暂存区。

git add 文件名/文件路径 “*”或-A代表所有

     git代码
    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ git add *
git add * 添加当前目录的所有文件到暂存区
*注意：每一次添加可以看做是将文件复制了一遍，但实际上每一次添加都只添加了文件内容的修改，也因此仓库中的文件备份远小于实际文件体积*

此时通过git status再次查看文件状态，放到暂存区的文件被标记成了绿色，等待提交。
Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   git.md
        new file:   vcs.md
        new file:   "\346\226\260\345\273\272\346\226\207\344\273\266\345\244\271/1.jpg"
        new file:   "\346\226\260\345\273\272\346\226\207\344\273\266\345\244\271/2..png"
注：颜色是工具给添加的，目的是增加可读性并不是git统一的。

5. 撤销更改
继续我们的开发，再次git status可以再次查看仓库状态

    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   git.md
        new file:   vcs.md
        new file: 
 changes not staged for commit:
    (use "git add<file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
    modified: index.html

    被标记了红色，说明index.html被再次修改了。

又经过一段时间后发现新开发的部分有Bug，想要回到之前状态，可以使用git checkout 文件名，将上次暂存的文件还原到工作区。

     Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ git checkout index.html

5. 提交文件
经过一个相对较长阶段开发或者一个功能开发完成了，就可以提交到本地仓库了，永久保存了。
git commit -m '备注信息'

       Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ git commit -m'没有做什么功能，第一次提交'
    [master (root-commit) d5e08c2] '没有做什么功能，第一次提交'
 6 files changed, 290 insertions(+)
 create mode 100644 git.md
 create mode 100644 vcs.md
 create mode 100644 "\346\226\260\345\273\272\346\226\207\344\273\266\345\244\271/1.jpg"
 create mode 100644 "\346\226\260\345\273\272\346\226\207\344\273\266\345\244\271/2..png"


将暂存区被标记成绿色的文件，全部提交到本地仓库存储。
这时git status查看状态

Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ git status
On branch master
nothing to commit, working tree clean

没有什么可提交的，变的很干净。

6. 查看提交历史
反反复复开发了很多的功能了，通过git log查看一下提交的历史。

Administrator@AAA MINGW64 ~/Desktop/github/git (master)
$ git log
commit d5e08c2d788daaca67bdc9ee466a4d7b2e97dc40 (HEAD -> master)
Author: ‘aaa <33248810@qq.com>
Date:   Fri Jun 30 21:37:17 2017 +0800

    第一次提交

我们可以查看到一次次的提交记录
当提交记录太多的时候，为了不占用太多位置，可以命令' git log --oneline '

7. 再次检测仓库文件状态
隔了好些天后，继续开发
git status 查看状态

如果检测到有新的修改，会有红色的  modified: 文件名 提示，等待重新添加到暂存区

8. 重新添加暂存区然后提交
git add 需要提交的文件名

9. 再次查看历史
git log 可查到所有提交历史

10. 恢复之前提交的版本

默认 HEAD 指向 master,就会把 master 中提交的代码拿到工作区
通过commit id值可以回到之前某一次的提交（时光倒流）
- `git reset --hard  提交的id`
- `git reset --hard 53bd6a3cd5b9ff5782af4837985c1e3023412d23`
*注意：如果是回退到最近一次的提交记录，不需要添加 commit id*
*git reset --head head*即可

git log 再次查看提交历史记录
*git log 只能看到head指向之前的提交记录*
*git reflog 查看所有的操作记录*

**git 使用步骤：**

配置用户：git config --global user.name "自已的名字"
         git config --global user.email "自已的邮箱地址"

初始化仓库：   git init 
查看文件状态： git status (3中状态)
添加到暂存区：  git add 文件名/或者* -A
提交文件：      git commit -m'备注信息必须有'
查看提交历史：   git log
查看所有提交：   git reflog
提交历史在一起显示： git log --oneline
恢复以前提交的版本： git reset --hard commitID


仓库示意图

