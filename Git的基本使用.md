# Git 的基本使用

【置顶声明】本文中借鉴廖雪峰老师的文章<https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424>

​		阅读本文不会让你精通 git 命令和 git 操作，只能让你成为一个 git 的使用者，可以基本使用 git 进行版本仓库的管理，如果你想成为更好的 git 精通者，我会为你附上几个链接。

​		我不想复制一些 git 命令然后一笔带过，也不想用高深的语言去描述，我是一个俗人，所以就用我通俗易懂的话去讲明白 git 究竟怎么使用就好了。一些关于 git 的简介我就不在这里啰里啰唆的说了，具体可以去看百科

<https://baike.baidu.com/item/GIT/12647237?fr=aladdin>

那么现在随我开始 git 的学习吧！

## 简述 git

虽然不想啰里啰唆的介绍 git ，但是有一点要说一下，git 的具体作用。

​	最主要的作用就是保存版本，昨天我写了一个版本的代码，电脑关机回家休息，今天我觉得代码写的不好改了改，明天我发现写的代码还不如第一天写的，但是已经找不到了，好懊恼，这个时候你就需要 git 了。 使用 git 保存以后你就可以找回你之前写的代码了。

## 安装 git

首先安装 git ，git 管理代码版本全部仰仗于 git 这个软件

>  你可以在开始菜单中找一下电脑里有没有 Git 或者在桌面上鼠标右键菜单中看一看是否有 Git Bash Here。
>
> - 如果有证明已经安装了 git ，可以跳过安装 git 部分，如果没有请继续向下看。

下载地址 <https://git-scm.com/downloads> 就“傻瓜式”的下一步下一步 默认安装就好了，安装成功的检测方式还是在桌面上鼠标右键菜单中看一看有没有 Git Bash Here。

![1570862420923](D:\博客uEpload\text\1570862420923.png)

如果找到了恭喜你，安装成功！ 	 撒花*★,°*:.☆(￣▽￣)/$:*.°★* 。



## 配置 git

​	恭喜你拥有了目前世界上最先进的版本管理工具 **git**

​	那么下面来进行一些最基本的配置

​	在桌面或者任意文件夹中

 	打开你想要管理的文件夹，在文件夹中鼠标右键空白区域点击 【Git Bash Here】，会弹出一个小黑窗口。

![1570863095888](C:\Users\xxxiechenhao\AppData\Roaming\Typora\typora-user-images\1570863095888.png)

输入命令

**注意那个 $ 是软件自带的，不要输入进入**

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

name 和 e-mail 可以随便起，是为了记录提交者，将来也可以更改。



## 使用 git

> git 管理版本文件有三个区域，工作区、暂存区、版本库

### 初始化 git

首先需要一个工作区或者说仓库，也就是代码想要被管理的文件夹，在那个文件夹中鼠标右键【Git Bash Here】

初始化 git 仓库

```
$ git init
```

输入这行语句后，你的仓库就被创建好了，如果你查看隐藏文件就会发现当前文件夹下生成了一个隐藏的名字叫做 .git 的文件夹，这个文件夹千万**不能删除**，除非你不想用 git 管理文件了。

### 添加暂存区

还记得 git 的三个区么，文件存放、修改的文件夹就是工作区，这是你可以看到的部分。

暂存区是工作区用来提交更改前可以暂存工作区的变化。 这个区貌似也叫 “索引”，不过这不重要，因为这并不影响你的使用。

当你改变工作区中的文件的时候，你可以输入指令查看文件状态

```
$ git status
```

![1570867282611](C:\Users\xxxiechenhao\AppData\Roaming\Typora\typora-user-images\1570867282611.png)



Untracked 表示更改未提交，所以此时输入指令

```
$ git add 1.txt
```

那么文件就提交到暂存区了，重新输入查看状态的指令 $ git status 查看文件状态

![1570867464948](D:\博客upload\text\1570867464948.png)

已经提交到暂存区。

如果同时修改了几个文件怎么办呢？

```
$ git add 1.txt 2.txt 
```

多个文件提交使用空格隔开，当然也可以使用下面的命令

```
$ git add .
```

或者

```
$ git add -A
```

这两条命令都表示提交当前文件夹下所有被更改的文件。

那么此时此刻文件已经被添加到暂存区中了



### 添加版本库

当我们想要保存当前的版本时就需要添加到版本库中，这意味着将来某一天你可以找回这个版本。

```
$ git commit -m '提交描述'
```

这条命令告诉 Git 把刚刚暂存区中文件全部提交到版本库中。 

 -m 后是提交描述，可以书写任何内容，不过我劝你写一些有用的东西，因为你可以在记录里清晰地看到这次提交的原因



### 为什么分两步提交

为什么要先 add 到暂存区，然后再 commit 到版本库呢？

这是因为可以多次 add 不同的文件，然后使用 commit 一次提交到版本库中，比如

``` 
$ git add 1.txt
$ git add 2.txt 3.txt
$ git commit -m '提交了三个文件'
```

​	

如果你再使用 $ git status 查看文件状态就会发现小黑框提醒你

“nothing to commit, working tree clean”

表示刚刚的版本已经上传到版本库中，当前没有任何修改的文件了。



## 回溯版本

下面就是 git 穿梭时空的魔法了。

如果你 commit 多次后，想要回退到原来的某一次版本时，使用命令

```
$ git log
```

查看版本库中的详细信息

里面有一行

commit xxxxxxxxxxxxxxxxxxxxxxxx  (HEAD -> master)

其中 xxxxxxxxxxxxx 是你的版本号， 下面还有你的描述，知道描述的重要性了吧！

是不是看起来非常繁琐，你也可以使用下面的命令查看简短信息

```
$ git log --oneline
```



如果你想退回到某一次版本 可以使用命令

``` 
$ git reset --hard 版本号
```

版本号不用都输入进去，可以使用tab自动补全，小心有很版本号非常类似的补全出错哦，一定要细心。如果回退错了别担心。

```
$ git reflog
```

这条命令可以查看你的每一次操作，找到更改前的版本号，使用 $ git reset 就可以回来了。



记得查看版本时括号中的 HEAD 么，这表示你当前的版本，HEAD 就像是个标记，标记着现在工作区里的内容。

![1570868853301](D:\博客upload\text\1570868853301.png)

### 总结

如果你想找到之前的版本 

```
$ git log  或  $ git log --oneline
$ git reset --hard 版本号
```

如果你后悔了

```
$ git reflog 
$ git reset --haed 版本号
```

就可以再各个版本中穿梭了。

下面是最重要的一点

千万不要删除 .git 文件夹，否则所有的版本记录就都没有了。



## git 分支管理

【特别声明】 下面的图片【仿照】了**廖雪峰**老师的文章，特附上链接<https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424>

### 分支介绍

我们的每一次 commit 都是一个版本，这些版本组成了一条**主支** ，有时我们需要对项目做一些更改，但又不想影响原来项目的正常运行情况，于是我们就需要创建一个**分支**。

>  好像做一张 “试卷” ，你总不会把所有的过程都写到试卷上吧，是不是需要一张 “草稿纸” ，在草稿纸上写你的运算，确认无误后书写到试卷上。

有没有点大概理解了，我们**主支**上只存放没有任何问题的版本，如果我们要进行测试更改，则需要新建一个**分支**，在**分支**上进行想进行的操作，当确定没有问题后再将**分支**合并到**主支**上，这样就可以保证 **主支** 时时刻刻都是正常的。

你也可以把**分支**想象成一个平行空间，不会对**主支**造成任何影响。



### 分支操作

#### 1、创建分支

```
$ git branch dev
```

dev 是分支名，起其他的名字也行，

![1570870275043](D:\博客upload\text\创建分支.png)

##### 查看分支

当工作中如果忘记了创建的分支，可以使用命令查看分支

```
$ git branch
```

小黑框中的【  *  】号表示当前在那一个分支上



#### 2、切换分支

```
$ git checkout dev
```

切换到主支

```
$ git checkout master
```



单单创建了分支还不够，分支虽然创建了，但是还需要切换到分支上。

![1570872093261](D:\博客upload\text\切换分支.png)



已经切换到分支上时工作区的内容没有任何变化，还是原来的样子，现在就可以放心大胆的对文件进行更改

#### 3、修改分支

当你切换到**分支**后，进行一大堆操作之后也是要像正常一样提交到暂存区，再提交到版本库，但每一次提交都是在**分支**上提交的，所以才不会影响到**主支**，比如我们使用 $ git checkout master 时发现工作区会变回更改前的文件。

![1570870704210](C:\Users\xxxiechenhao\AppData\Roaming\Typora\typora-user-images\1570870704210.png)

#### 3、合并分支

当我们确保分支没有任何问题以后，我们就可以把分支合并了,

首先我们要先切回主分支，然后再进行合并，合并分支命令是将**其他**分支合并到当前分支上

```
$ git checkout master
$ git merge dev
```

表示将 dev 合并到 master 分支上

![1570872419230](D:\博客upload\text\合并分支)

### 4、删除分支

当我们合并分支后，**主支**和**分支**内容就一致了，所以我们甚至可以删除掉分支。

```
$ git branch -d dev
```

![1570872809193](D:\博客upload\text\删除分支)

##### 可以使用命令查看分支

```
$ git branch
```

删除后就只剩下 master分支了



### 有关分支

 主分支上一定是稳定、正常的版本，不能在主支上做修改。多人合作时，都在 dev 分支上工作，同时又都有自己的分支，合并时都先合并到 dev 分支上，测试无误后才会合并到主支上，这也是确保主支没有任何问题的关键