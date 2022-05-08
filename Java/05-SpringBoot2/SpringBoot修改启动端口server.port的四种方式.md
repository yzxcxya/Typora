### SpringBoot修改启动端口server.port的四种方式

- ##### 方式一：代码内修改`application.properties` 配置文件

​			`server.port = 8080`

- ##### 方式二：在jar包同层目录下有个单独的`application.properties` 配置文件,进行修改

​			`server.port = 8081`

- ##### 方式三：以 jdk 参数方式启动

​			`java -Dserver.port = 8082 -jar aaa.jar`

- ##### 方式四：以应用参数方式启动

​			`java -jar aaa.jar --server.port = 8083`

#### 总结：四种方式都可以修改服务启动的端口，但有着不同的优先级

​			优先级如下：方式四 > 方式三 > 方式二 > 方式一

​			如果代码里配置了8080，jar包外层配置了8081，启动命令是：

​			`java -Dserver.port=8082 -jar aaa.jar --server.port=8083`

​			那么最后程序将会以8083端口运行 ！ 