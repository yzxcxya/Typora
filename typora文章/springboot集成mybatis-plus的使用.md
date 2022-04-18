# springboot集成mybatis-plus的使用

1. IDEA先新建一个springboot工程

2. 在pom.xml中引入以下依赖

   ~~~
   		<!-- lombok -->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.22</version>
               <scope>provided</scope>
           </dependency>
           
           <!-- mysql -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.20</version>
           </dependency>
   
           <!-- mybatis-plus -->
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus-boot-starter</artifactId>
               <version>3.5.1</version>
           </dependency>
   
           <!-- mybatis-plus 代码生成器 -->
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus-generator</artifactId>
               <version>3.5.2</version>
           </dependency>
   
           <!-- freemarker -->
           <dependency>
               <groupId>org.freemarker</groupId>
               <artifactId>freemarker</artifactId>
               <version>2.3.31</version>
           </dependency>
   ~~~

3. 配置application.yml

   ~~~
   # 应用名称
   spring:
     application:
       name: back
     datasource:
       url: jdbc:mysql://127.0.0.1:3306/education?useSSL=false&serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&allowMultiQueries=true
       username: root
       password: 123456
       driver-class-name: com.mysql.cj.jdbc.Driver
   # 应用服务 WEB 访问端口
   server:
     port: 8080
   ~~~
   
4. 创建一个用于代码生成器的类，用于直接生成controller,service,mapper,entity等文件夹，注意，**在此之前，一定要先在数据库中建好各种表，表的每个字段都要有相应的注释**

   官方文档参考[代码生成器配置新 | MyBatis-Plus (baomidou.com)](https://baomidou.com/pages/981406/)

   ~~~
   package com.example.demo;
   
   import com.baomidou.mybatisplus.generator.FastAutoGenerator;
   import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;
   
   public class CodeGenerator {
       public static void main(String[] args) {
           // 动态获取java文件夹目录（D:/IdeaProjects/education-back/src/main/java）
           String pkgPath = System.getProperty("user.dir") + "/src/main/java";
   
           FastAutoGenerator.create("jdbc:mysql://127.0.0.1:3306/education?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&allowMultiQueries=true&useSSL=false&serverTimezone=Asia/Shanghai","root","123456")
                   // 全局配置
                   .globalConfig(builder -> builder
                           .outputDir(pkgPath) // 生成的文件在哪个文件夹下
                           .author("yzxcxya")  // 设置文件作者
                           .disableOpenDir())  // 生成代码和不自动打开资源管理器
                   // 包配置
                   .packageConfig(builder -> builder
                           .parent("com.example.demo")) // 设置父包路径
                   // 模板配置（使用Freemarker引擎模板，默认的是Velocity引擎模板）
                   .templateEngine(new FreemarkerTemplateEngine())
                   // 策略配置
                   .strategyConfig(builder -> builder
                           .controllerBuilder().enableRestStyle()    // 设置controller文件默认注解为@restcontroller
                           .serviceBuilder().formatServiceFileName("%sService")  // 不设置的话,生成的service接口会有 I 前缀
                           .mapperBuilder().enableMapperAnnotation().enableBaseResultMap().enableBaseColumnList() // 开启 @Mapper 注解,启用 BaseResultMap 生成,启用 BaseColumnList
                           .entityBuilder().enableChainModel().enableLombok()    // 开启链式模型和lombok模型
                   )
                   // 执行
                   .execute();
       }
   }
   
   ~~~

   生成的实体类最好加一个@Data注解，默认只有@Getter/@Setter注解，没有重写toString()等方法。

   默认生成的文件夹如下：

   ![image-20220416190032822](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161900875.png)

   controller代码生成示例：

   ![image-20220416190100181](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161901228.png)

   service代码生成示例：

   ![image-20220416190132153](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161901196.png)

   ![image-20220416190144552](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161901604.png)

   entity代码生成示例：

   ![image-20220416190228381](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161902442.png)

   mapper代码生成示例：

   ![image-20220416190312549](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161903589.png)

   ![image-20220416190329987](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161903054.png)

4. 配置mapper里xml扫描路径

   mybatis-plus默认生成的xml文件在java包中，IDEA编译时默认不会扫描xml文件，运行项目后，发现target目录中根本没有mapper.xml文件

   ![image-20220416184048288](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161840352.png)

   需要在配置文件pom.xml文件中配置

   ~~~
   <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
           </resources>
   </build>
   ~~~

   配置完成后，运行后就会扫描到了

   ![image-20220416184152701](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204161841770.png)

   但是还不够，如果有时候自己写sql在xml文件中，运行时，就会报出这个异常

   ~~~
   org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)
   ~~~

   大概意思是mapper和mapper.xml没有关联起来，得在application.yml里加配置

   ~~~
   mybatis-plus:
     mapper-locations: classpath*:/online/yuhang/back/mapper/xml/*.xml
   ~~~

   注意一定得是`mybatis-plus`前缀，不能是`mybatis`

5. 使用指南