## GIT的一些常用命令操作

##### 1.配置SSH Key

> Github上的远程仓库有两种使用方式，分别是HTTPS和SSH，区别是：
>
> - HTTPS:零配置，需要重复输入Github的账号和密码才能访问成功
> - SSH：需要进行额外的配置，配置完成后，访问仓库不需要重复输入Github的账号和密码

SSH Key的作用：实现本地仓库和Github间的加密数据传输，它由两部分组成，分别是：

- id_rsa（私钥文件，存放于客户端的电脑中即可)
- id rsa.pub (公钥文件，需要配置到Github 中)

生成SSH Key: 打开Git Bash,输入以下命令

~~~
ssh-keygen -t rsa -C "你的邮箱地址"
~~~

执行命令 生成`ssh key`

- -t = The type of the key to generate 密钥的类型
- -C = comment to identify the key 用于识别这个密钥的注释（好像一般都填的邮箱）

文件默认是在C盘用户目录下，我的是`C:\Users\用户名\.ssh`

文件夹中应该会有两个文件 ：`id_rsa`和`id_rsa.pub`

`id_rsa.pub`就是我们要的key, 一般以`ssh-rsa`开头，以你刚才输的邮箱结尾。

登陆Github–>点击头像–>Settings–>SSH and GPG keys–>选择SSh keys上的New SSH keys–>name 随便写，key就是刚才生成的文件中的所有内容。

**测试验证：**

执行下面命令：

~~~
ssh -T git@github.com
~~~

可能会提示`无法验证主机的真实性`是否要建立连接，输入`yes`就行了。

如果，看到：

> Hi xxx! You’ve successfully authenticated, but GitHub does not # provide shell access.

恭喜你，你的设置已经成功了。

##### 2.本地仓库和远程仓库进行关联操作

- 本地没有现成的Git仓库

1. 使用终端命令创建README.MD文档，并写入初始内容“初始内容”

   ```shell
   echo "# 初始内容" >> README.md
   ```

2. 初始化本地Git仓库，并将文件的修改提交到本地的Git仓库中

   ```shell
   git init
   git add README.md
   git commit -m "first commit"
   ```

3. 将本地仓库和远程仓库进行关联，并把远程仓库命名为origin

   ```
   git remote add origin git@github.com:yzxcxya/xxx.git
   ```

4. 将本地仓库中的内容推送到远程的origin仓库中

   ~~~
   git push -u origin master
   ~~~

- 本地有现成的Git仓库的话直接忽略上面前两步就行

- 最直接的方法就是直接在某个文件夹下

  ~~~
  git clone 你的仓库地址
  ~~~

  运行完后直接关联了

##### 3.分支操作

本地有main  本地建立一个master，推送到远程master, 本地main也修改了push进main,需要将msater合入main,并推送到远程main

远程有main

|  分支操作  |        本地仓库         |             远程仓库             |                  备注                  |
| :--------: | :---------------------: | :------------------------------: | :------------------------------------: |
|  查看分支  |      `git branch`       |         `git branch -r`          | `git branch -a`查看本地和远程所有分支  |
|  创建分支  |   `git branch 分支名`   | 本地切换到新分支并push会自动创建 |                                        |
|  切换分支  |  `git checkout 分支名`  |                                  | `git checkout -b 分支名`创建并切换分支 |
|  删除分支  | `git branch -d  分支名` |                                  |                                        |
| 修改分支名 |                         |                                  |                                        |



##### 4.git常用命令

`git diff [<path>...]`：什么参数都不加，默认比较工作区与暂存区的差异（`git add`后就无差异了）

`git diff --cached [<path>...]`：比较暂存区与最新本地版本库（`git commit`后就无差异了）

`git diff --HEAD [<path>...]`：比较工作区与最新本地版本库。如果HEAD指向的是master分支，那么HEAD还可以换成master

`git remote -v`：查看当前关联的远程仓库地址

`git remote remove origin` ：移除当前远程仓库的连接

`git remote add origin 远程仓库地址`：和远程仓库地址建立连接

git reset

git stash



##### 5.git冲突

一般来说，两个人修改了同一个文件（不管是同一文件不同行还是同一行），你`git pull`时就会有冲突错误报出来，然后在你冲突的文件，寻找用=====隔开的两部分，HEAD是你的内容，另外一部分是repos的内容，然后手工修改保留你想要的部分，在`git add` 和`git commit`然后再`git push`上去就好了。

##### 6.git流程



##### 7.撤销本地修改

- 未使用 git add 缓存代码时：

  放弃某个文件修改（不要忘记中间“--”，不写就是建初分支了）

  ~~~
  git checkout -- filepathname
  ~~~

  放弃所有文件修改

  ~~~
   git checkout . 
  ~~~

  此命令用来放弃掉所有还没有加入到缓存区（就是 `git add` 命令）的修改：包括内容修改与文件删除。但是此命令不会删除掉新建的文件。因为刚新建的文件还没有加入到git的管理系统中，所以对于git是未知的，自己手动删除就可以了。

- 使用了 git add 缓存了代码：

  放弃某个文件的缓存

  ~~~
  git reset HEAD filepathname
  ~~~

  放弃所有文件缓存

  ~~~
  git reset HEAD .
  ~~~

  此命令用来清除 git 对于文件修改的缓存。相当于撤销 `git add` 命令所在的工作。在使用本命令后，本地的修改并不会消失，而是回到了 `git add` 前的状态。继续使用上面add前的操作，就可以放弃本地的修改。

- 已经用 git commit 提交了代码：

  回退到上一次commit的状态

  ~~~
  git reset --hard HEAD^
  ~~~

  回退到任意版本（可以使用 git log命令来查看git的提交历史，commit 后面的一串字符就是commit ID）

  ~~~
  git reset --hard commitId
  ~~~

##### 8.git放弃跟踪某些文件

在工程下新建`.gitignore`文件,并在其中添加要忽略的文件或目录，每行表示一个忽略规则，像下面一样

~~~
target/
*.iml
.idea/
~~~

***.gitignore不生效？***

> .gitignore只能忽略原来没有被跟踪的文件，因此跟踪过的文件是无法被忽略的，解决方法就是先把本地缓存删除（改变成未track状态），然后再提交:

~~~
git rm -r --cached .
git add .
git commit -m ‘xxx’
~~~













