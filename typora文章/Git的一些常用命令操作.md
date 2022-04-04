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
   echo "初始内容" >> README.md
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

| 分支操作 |   本地仓库   |    远程仓库     |                 备注                  |
| :------: | :----------: | :-------------: | :-----------------------------------: |
| 查看分支 | `git branch` | `git branch -v` | `git branch -a`查看本地和远程所有分支 |
|          |              |                 |                                       |



