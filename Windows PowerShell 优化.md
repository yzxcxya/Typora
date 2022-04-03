#### Windows Power Shell 优化

##### 1. 安装 *oh-my-posh*



##### 2.安装 *PSReadLine*

安装最新版本`PSReadLine`，在power shell中执行以下命令:

```shell
Install-Module PSReadLine  -Force
```

再修改配置文件，输入

```she
notepad $profile
```

系统会自动弹出这个配置文件让你编辑

接着在这个文件中添加两行

~~~txt
Import-Module PSReadLine
Set-PSReadLineOption -PredictionSource History
~~~

效果如下：![命令提示](C:\Users\yuhh\Pictures\命令提示.png)

​	

​		



