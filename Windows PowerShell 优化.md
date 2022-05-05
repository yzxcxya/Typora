#### Windows Power Shell 优化

##### 1. 安装 *oh-my-posh*



修改git中文文件名乱码：

乱码前![image-20220418115841397](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220418115841397.png)

运行

~~~
git config --global core.quotepath false
~~~

解决后

![image-20220418115938594](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220418115938594.png)

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

效果如下：![QQ截图20220404162700](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/QQ%E6%88%AA%E5%9B%BE20220404162700.png)

##### 3.修改默认背景图片

打开power shell 的设置，选择打开JSON 文件

![image-20220403163222810](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220403163222810.png)

编辑json 文件，输入下面图片路径就行，透明度随自己调

![image-20220403163416797](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220403163416797.png)

我的图片是下面这个，想用的话可以直接右键保存

![black-desk](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/black-desk.webp)

![white-desk](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/white-desk.webp)
