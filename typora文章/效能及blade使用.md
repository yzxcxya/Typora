# 效能及blade使用

1.效能平台意义：提供完整的需求管体系，规范开发流程，通过研发效能度量识别改进点，提升组织效率

主要使用人员：普通用户，开发人员，测试人员及其他人员

2.blade平台意义：为测试人员提供一个完整的测试处理平台，规范测试流程

主要使用人员：测试人员

### 需求单

用户对产品有什么新需求，或提出缺陷，就会新增一个需求单

#### 效能需求单处理流程：

![image-20220413135434174](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413135434174.png)

![image-20220413135806580](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413135806580.png)

![image-20220413135837787](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413135837787.png)

![image-20220413135902335](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413135902335.png)

#### blade需求单流程：

当测试人员看到一个需求已经安排处理后，就会对这个需求单进行完整的测试分析。

具体流程为：测试分配-->测试分析-->测分反馈

1.测试分配：安排人员对这个需求单进行分析

![image-20220413141020284](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413141020284.png)

2.测试分析：测试分析人员新建一个测试需求（脑图），对这个需求单进行详细分析

![image-20220413141249870](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413141249870.png)

3.测分反馈：相关人员对分析完成结果进行反馈

![image-20220413141502286](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413141502286.png)



### 任务单

任务单是需求经过处理规划后，基于需求分解得到的一种单子，主要用于分发给开发人员进行开发处理，比如用户提的一个比较大的需求，需求处理人经过分解得到两个任务单，安排给一个或多个人开发处理。

#### 效能任务单流程

开发人员看到有任务分解给自己后，主要流程有：启动任务-->点击开发完成-->提交测试

1.启动任务：启动后，就开始着手开发了。任务单的状态由`待启动`变成`开发中`

![](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413142525642.png)

2.点击开发完成：主要填写工时，修改文件或内容等。任务单的状态由`开发中`变成`待提测`

![image-20220413142620790](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413142620790.png)

3.提交测试：开发完成后，此时应该通知测试人员对修改的单子内容进行测试。任务单的状态由待提测变成待测试。

开发人员的工作基本此时已经完成了。

![image-20220413142953827](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413142953827.png)

#### blade任务单流程

当测试人员看到一个任务单新增后，就会开始对这个任务单就行完成的测试流程操作。

具体流程为：测试分配-->创建(关联)测试用例-->测试反馈-->启动测试-->创建(关联)测试任务-->执行测试任务-->提交执行结果

1.测试分配：安排用例编写人，测试执行人及各阶段要求完成日期等

![image-20220413144016422](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413144016422.png)

2.创建(关联)测试用例：对这个任务单的设计完整的测试案例

![image-20220413144305806](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413144305806.png)

3.测试反馈：用例设计完成后，进行反馈记录

![image-20220413144539016](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413144539016.png)

4.启动测试：任务状态由`待测试`变成`测试中`

![image-20220413144739572](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413144739572.png)

5.创建(关联)测试任务：为这个任务单关联具体的测试任务，以便后面执行

![image-20220413145031543](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413145031543.png)

6.执行测试任务：顾名思义，去执行这个测试任务，包括手工执行和自动化执行等

![image-20220413145415089](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413145415089.png)

7.提交执行结果：测试任务执行完后，就可以进行最后的操作，提交执行结果。把任务单的测试结果反馈给效能。单子由`测试中`变成`已完成`或`测试不通过`

![image-20220413145603932](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413145603932.png)

blade上的任务单启动测试后单子状态流转：

![image-20220413150002184](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/image-20220413150002184.png)