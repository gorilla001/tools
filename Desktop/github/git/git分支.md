##### git分支
在我们的现实开发中，需求往往是五花八门的，同时开发多个需求的情况十分常见，当你正在专注开发一个功能时，突然有一个紧急的BUG需要你来修复，这个时候我们当然是希望在能够保存当前任务进度，再去修改这个BUG，等这个BUG修复完成后再继续我们的任务。如何实现呢？

**Git通过创建分支来解决实际开发中类似的问题**

在Git的使用过程中一次提交称为历史记录（版本），并且会生成一个唯一的字符串，如下图
如下图 commit 后会生成一个唯一的字符串

     Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ git log
    commit d5e08c2d788daaca67bdc9ee466a4d7b2e97dc40 (HEAD -> master)

这个串可以代表某一个历史版本（实际使用只取前面几位就可以）
值得注意的是所有的提交（commit）实际上都是在分支（branch）的基础上进行的
    
当我们在初始化仓库的时候（实际上是产生第1次提交时），Git会默认帮我们创建了一个master的分支，并且有指针（HEAD）指到了末端
指针（HEAD）用来标明当前处于哪个分支的哪个版本
默认只有一个master分支
我们也可以创建自已的分支

###### 1. 创建分支
git branch <name>
当接到一个新任务时，先创建一个分支，例如购物车功能git branch cart
新分支会在当前分支原有历史版本的结点上进行创建，我们称其为子分支

    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ git checkout cart

此时 HEAD 指向当前路径 master
**新建的子分支会继承父分支的所有提交历史**
当我们创建分支时，其实相当于把master分支里的内容复制了一份

###### 2. 切换分支
git checkout cart(分支名称)
上一步只是创建了一个新的分支cart，要开发购物车功能需要切换到cart分支

    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ git checkout cart
    Switched to branch 'cart'
 
 **注意 当切换路径后HEAD指向当前 cart 路径** 
 此时是在cart分支内编译代码  
 *执行 git checkout branch的时候git告诉系统之后的代码是在当前分支进行操作了，并且将仓库里当前分支最后一次提交的内容拿出来给到工作区，所以我们能看到之前的提交内容*

    Administrator@AAA MINGW64 ~/Desktop/github/git (cart)

###### 3. 提交操作
持续开发购物车功能
将新添加的功能进行提交
这次的提交历史版本就会记录在cart这个分支上了，**并且HEAD伴随cart在移动**。

###### 4. 修复bug
开发购物车过程中突然需要紧急修复一个bug，这时可以另外建立一个分支来进行修复，先切换到master分支，git checkout master

    Administrator@AAA MINGW64 ~/Desktop/github/git (master)
    $ git checkout master
    Switched to branch 'master'

当我们切换回master后，HEAD指向了master分支的末端，并且我们观察**发现我们工作目录中并没有购物车功能代码。**

###### 5. 创建新分支修复bug
创建一个新分支hotwork,修改并提交会在hotwork分支上创建新版本，并不会影响到cart分支的代码。

##### 6. 继续之前的开发
修复完bug后切换回cart分支，继续开发购物车功能。

注：当我们git checkout branchname时，HEAD会自动指向对应分支的末端，工作目录中的源码也会随之发生改变。
这个时候我们就在hotwork这个分支上修复了这个BUG，而我们原来在cart分支上的操作并未受到影响。
同理可得：cart分支并没有包含hotwork分支上对于bbug的修复

###### 7. 合并（融合）分支
git merge branchname 用于合并指定分支到当前分支上

    $ git checkout cart
    $ git merge hotwork 
这时cart会有两个父结点了，cart便包含了 hotwork 里的修复了
*合并代码时，系统会进行判断，相同的提交内容被跳过，不同的内容会进行合并*

###### 8. 处理冲突
假如两个开发同时改到同一文件的同一段内容会发生什么事情呢？
这时就会就会产生冲突了，当冲突产生后，需要开发者进行协商确认冲突的原因，然后将冲突代码删除重新提交就可以了。


###### 8. 删除分支
git branch -d hotwork
这时用来修复bug创建的hotfix分支已经没有用处了，我们可以将它删除。

删除后，查看branch,就只剩下 master 和 cart 了 

    $ git branch 
    * cart 
    marster

创建、合并和删除分支非常快，所以git鼓励你使用分支完成某个任务，合并后再删除点分支，这和直接在 master 分支上工作效果是一样的，而且过程更安全

**git 鼓励大家使用分支：**

查看分支： git branch
创建分支： git branch <name>
切换分支： git checkout <name>
创建加切换分支： git checkout -b <name>
合并某分支到当前分支： git merge <name>
删除分支： git branch -d <name>


 
