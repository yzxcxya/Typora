### 一、Linux 运行jar包的几种方式

- **方式一： java -jar xxx.jar**

最常用的启动jar包命令，特点：当前ssh窗口被锁定，可按CTRL + C打断程序运行，或直接关闭窗口，程序退出

- **方式二： java -jar xxx.jar &**

&代表在后台运行 ，`ctrl+c` 后程序也会继续运行

- **方式三： nohup java -jar xxx.jar &**

nohup  即 no hang up 不挂断 ，关闭SSH客户端连接，程序不会中止运行

缺省情况下该作业的所有输出被重定向到nohup.out的文件中，如何让输出的内容重定向到指定的文件呢？

- **方式四：nohup java -jar xxx.jar >aaa.log &**

command >out.file 是将commandd 输出重定向到out.flie文件，即输出内容不打印到屏幕上，而是输出到out.file文件中

- **方式五：nohup java -jar spring-boot-demo.jar > springboot.log 2>&1 &**

![363c896c36dec3783bb93a3ebe312a58.png](https://img-blog.csdnimg.cn/img_convert/363c896c36dec3783bb93a3ebe312a58.png)

- **方式六：nohup java -jar spring-boot-demo.jar > /dev/null 2>&1 &**

​			不输出日志

### 二、nohup 和 &

###### 使用`&`后台运行程序：

- 结果会输出到终端
- 使用`Ctrl + C`，程序免疫
- 关闭`session`，程序关闭

###### 使用`nohup`运行程序：

- 结果默认会输出到`nohup.out`
- 使用`Ctrl + C`，程序关闭
- 关闭`session`，程序免疫

平日线上经常使用`nohup`和`&`配合来启动程序

### 三、> /dev/null 2>&1

- ​	`	>` 标准重定向符，允许我们创建一个 0KB 的空文件。它通常用于重定向一个命令的输出到一个新文件中。在没有命令的情况下使用重定向符号时，它会创建一个文件。

- ​	`/dev/null` 可以看作`黑洞`，等价于一个只写文件。所有写入它的内容都会永远丢失，尝试从它那儿读取内容则什么也读不到。也就是将所有产生的日志将被丢弃

- ​	`2>&1` 符号`>&`是一个整体代表将标准错误2重定向到标准输出1,如果是`2>1`的话，代表将标准错误输出到文件1，而不是重定向到标准输出流

​	先了解下1和2在Linux中代表什么

​	当Linux执行一个程序时，会自动打开三个流

​	`0`：标准输入流（默认是键盘）
​	`1`：标准输出流（默认是屏幕）
​	`2`：标准错误流（默认是屏幕）

| 名称                 | 代码 | 操作符            | java中表示 | Linux中文件描述符                            |
| -------------------- | ---- | ----------------- | ---------- | -------------------------------------------- |
| 标准输入(stdin)      | 0    | < 或 <<           | System.in  | /dev/stdin -> /proc/self/fd/0 -> /dev/pts/0  |
| 标准输出(stdout)     | 1    | \>, >>, 1> 或 1>> | System.out | /dev/stdout -> /proc/self/fd/1 -> /dev/pts/0 |
| 标准错误输出(stderr) | 2    | 2> 或 2>>         | System.err | /dev/stderr -> /proc/self/fd/2 -> /dev/pts/0 |

​	从上表看出，平常使用的 `echo 'hello' > a.log` 可以写成 `echo 'hello' 1> a.log`

​	为什么2>&1要放在后面 ? 如下一条shell命令 `nohup java -jar app.jar >log 2>&1 & `  我们不妨把1和2都理解是一个指针,然后来看上面的语句就是这样的：

​		本来1----->屏幕 （1指向屏幕）
​		执行>log后， 1----->log (1指向log)
​		执行2>&1后， 2----->1 (2指向1，而1指向log,因此2也指向了log)

​	再来分析下`nohup java -jar app.jar 2>&1 >log &`
​		本来1----->屏幕 （1指向屏幕）
​		执行2>&1后， 2----->1 (2指向1，而1指向屏幕,因此2也指向了屏幕)
​		执行>log后， 1----->log (1指向log，2还是指向屏幕)
​		所以这就不是我们想要的结果。

​	每次都写">log 2>&1"太麻烦，能简写吗？可以简写成 `&>log` 或 `>&log`

​	`	nohup java -jar app.jar 2>&1 >log &`   简写成：`nohup java -jar app.jar &>log &`

​	



​	

