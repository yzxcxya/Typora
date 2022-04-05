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

|  分支操作  |             本地仓库              |                         远程仓库                          |                             备注                             |
| :--------: | :-------------------------------: | :-------------------------------------------------------: | :----------------------------------------------------------: |
|  查看分支  |           `git branch`            |                      `git branch -r`                      |            `git branch -a`查看本地和远程所有分支             |
|  创建分支  |        `git branch 分支名`        |             本地切换到新分支并push会自动创建              |                                                              |
|  切换分支  |       `git checkout 分支名`       |                                                           |            `git checkout -b 分支名`创建并切换分支            |
|  删除分支  |      `git branch -d  分支名`      | `git push origin -d 分支名`或者 `git push origin :分支名` | 删除远程分支时，远程默认分支不能是要删除的分支名；推送一个空分支到远程分支上相当于删除远程分支 |
| 修改分支名 | `git branch -m 旧分支名 新分支名` |     修改本地分支名后推送到远程库，再删除远程库旧分支      |                                                              |

***合并分支场景***：将dev分支合入master分支：

1. 本地切换到dev分支，`git pull origin dev` 拉取远程仓库dev分支最新代码
2. 再切换到master分支，先确保master分支也是最新代码，`git pull` 一下就行
3. 执行`git merge dev`或者`git rebase dev` ,本地的master就把本地的dev分支合并了
4. 最后`git push origin master`，把合并后的master提交到远程master分支

##### 4.git常用命令

`git status [-s]`：查看git文件状态

git commit [-a] -m : 提交文件到仓库，-a表示跳过暂存步骤（git add）,修改后直接提交

`git pull <远程主机名> <远程分支名>:<本地分支名>`

`git push <远程主机名> <本地分支名>:<远程分支名>` : 如果远程分支被省略，如上则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建；如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支，等同于 git push origin --delete master；如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支；如果当前分支只有一个远程分支，那么主机名都可以省略；

`git push -u origin master` : 如果当前分支与多个主机存在追踪关系，则可以使用-u选项关联一个默认远程仓库，这样后面就可以不加任何参数使用git push。

`git diff [<path>...]`：什么参数都不加，默认比较工作区与暂存区的差异（`git add`后就无差异了）

`git diff --cached [<path>...]`：比较暂存区与最新本地版本库（`git commit`后就无差异了）

`git diff --HEAD [<path>...]`：比较工作区与最新本地版本库。如果HEAD指向的是master分支，那么HEAD还可以换成master

`git diff branch1 branch2 --stat`   :显示出所有有差异的文件(不详细,没有对比内容)

`git diff branch1 branch2`   : 显示出所有有差异的文件的详细差异(更详细)

`git diff branch1 branch2 具体文件路径` : 显示指定文件的详细差异(对比内容)

`git remote -v`：查看当前关联的远程仓库地址

`git remote remove origin` ：移除当前远程仓库的连接

`git remote add origin 远程仓库地址`：和远程仓库地址建立连接

`git reset`：撤销本地修改

`git stash [<save 'message'>]` : 把所有未提交的修改（包括暂存的和非暂存的）都保存起来，用于后续恢复当前工作目录，当前的工作目录就干净了

`git stash list`：查看现有stash

`git stash pop [<stash@{0}>]`：重新应用缓存的stash，默认使用第一个存储,即stash@{0}，如果要使用其他个，git stash pop stash@{$num} ， 比如第二个：git stash popstash@{1} 

`git stash apply [<stash@{0}>]` :应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即stash@{0}，如果要使用其他个，git stash apply stash@{$num} ， 比如第二个：git stash apply stash@{1} 

`git stash drop[<stash@{0}>]`：删除stash，不带参数默认最近的

`git stash clear`：删除所有缓存的stash

`git stash show [<stash@{0}>] [-p]`：查看最近stash的diff

`git log -num --graph`: 查看git提交日志，num为显示几条，--graph是否用图形界面显示

`git log --oneline`: 输出 简写commitid,提交说明

`git log --pretty=format:"%h - %an, %ad ,%s" --graph`: 输出 简写commitid,作者名,提交日期,提交说明

> git log 更多用法可以看网上这篇文章：[git log 命令详解](https://www.cnblogs.com/ay2021/p/15160893.html)，讲得特别特别详细

##### 5.git冲突

一般来说，两个人修改了同一个文件（不管是同一文件不同行还是同一行），你`git pull`时就会有冲突错误报出来，然后在你冲突的文件，寻找用=====隔开的两部分，HEAD是你的内容，另外一部分是repos的内容，然后手工修改保留你想要的部分，在`git add` 和`git commit`然后再`git push`上去就好了。

##### 6.git流程

![git](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/git.jpg)

##### 7.撤销修改

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

- 使用了 git add 缓存了代码（取消暂存文件）：

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
  git reset HEAD^
  ~~~

  向前回退到第三个版本

  ~~~
  git reset HEAD~3 
  ~~~
  
  回退1.txt这个文件到上一版本
  
  ~~~
  git reset HEAD^ 1.txt
  ~~~
  
  回退到任意版本（可以使用 git log命令来查看git的提交历史，commit 后面的一串字符就是commit ID）
  
  ~~~
  git reset commitId
  ~~~
  
  回退到和远程库一样
  
  ~~~
  git reset origin/master 
  ~~~

- 撤销远程提交

  ~~~
  git reset [--hard] 上个commitid
  git push -f  // 强制提交
  ~~~

- 回退的几个参数

  ~~~
  1. git reset --mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
  2. git reset --soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
  3. git reset --hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容
  ~~~

##### 8.git放弃跟踪某些文件

一般我们总会有些文件无需纳入Git的管理，也不希望它们总出现在未跟踪文件列表。在这种情况下，我们可以在工程下新建`.gitignore`文件,并在其中添加要忽略的文件或目录，每行表示一个忽略规则，像下面一样

~~~
target/
*.iml
.idea/
~~~

.gitignore格式规范如下：

>- 以#开头的是注释
>- 以/结尾的是目录
>- 以/开头防止递归,只作用当前目录
>- 以!开头表示取反

可以使用alob模式进行文件和文件夹的匹配（alob指简化了的正则表达式)

![image-20220405175742986](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220405175742986.png)

***.gitignore不生效？***

> .gitignore只能忽略原来没有被跟踪的文件，因此跟踪过的文件是无法被忽略的，解决方法就是先把本地缓存删除（改变成未track状态），然后再提交:

~~~
git rm -r --cached .
git add .
git commit -m ‘xxx’
~~~

##### 9.git分支与工作区、暂存区关系

文件不管新增修改，在`git add` 前后在任何一个分支都是全局的，在工作区都可见，切换分支不会影响它。

文件在`git commit`后，就会归属于某个分支了，此时你切换到另外一个分支，将不会显示上面分支修改的文件了。

##### 10.Tag操作（打标签）

Git 中的tag指向一次commit的id，通常用来给开发分支做一个标记，如标记一个版本号,如 v1.0.0。

- 创建 Tag

  ```shell
  git tag tag名字
  ```

- 创建带注释的 Tag

  ```
  git tag -a Tag名字 -m 注释文字
  ```

- 为历史某个 commit 添加tag

  ~~~
  git log --oneline //先获取commitid
  git tag tag名词 commitid
  ~~~

- 查看 Tag 列表

  ~~~
  git tag               //列出所有tag列表
  git tag -l "*测试*"    //列出所有带“测试”的tag
  ~~~

- 查看 tag 的详细信息

  ~~~
  git show tag名字
  ~~~

- 删除 tag

  ~~~
  git tag -d tag名
  ~~~

- 推送 tag 到远程

  ~~~
  git push origin Tag名字 // 推送单个Tag
  git push origin --tags  // 推送所有本地Tag
  ~~~

- 删除远程 tag

  ~~~
  git push origin :refs/tags/Tag名字
  ~~~

- 切换到标签（此时会处于一个空的分支上）

  ~~~
  git checkout tag名
  ~~~

- 修复历史tag中的bug

  ~~~ 
  git show v1.0               // 查看v1.0tag下的commitid
  git checkout -b bugfix      // 新建分支用于修复bug
  git reset --hard *****      // 回滚到tag下的那次commit
  git add .                   // 添加代码到暂存区
  git commit -m "修复bug"      // 提交到本地仓库
  git tag v1.0.1              // 打标签
  git checkout master         // 切回master
  git merge bugfix            // 合并bugfix分支，一般会有冲突，需手动解决
  git push origin master      // 推送合并后的分支到远程
  git push origin v1.0.1      // 推送标签到远程
  ~~~

  



