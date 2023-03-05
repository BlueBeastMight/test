## Dubbo

### 1. 分布式系统中的相关概念

#### 1.1 大型互联网项目架构目标

1. 传统项目和互联网项目

   ![image-20200812091024406](img/image-20200812091024406-16773964563381.png)

2. 用户体验

   美观、功能、速度、稳定性

3. 衡量一个网站速度是否快

    • 打开一个新页面一瞬间完成；页面内跳转，一刹那间完成。

    • 根据佛经《僧祇律》记载：一刹那者为一念，二十念为一瞬，二十瞬为一弹 指，二十弹指为一罗预，二十罗预为一须臾，一日一夜有三十须臾。

    • 经过周密的计算，一瞬间为0.36 秒,一刹那有 0.018 秒
    
    服务端的速度 要求 100ms左右

##### 大型互联网项目架构目标：

1. 互联网项目特点： 

   • 用户多 

   • 流量大，并发高 

   • 海量数据

    • 易受攻击 

   • 功能繁琐 

   • 变更快

   • 高性能：提供快速的访问体验。

2. 衡量网站的性能指标：
   1.  响应时间：指执行一个请求从开始到最后收到响应数据所花费的总体时间。
   2. 并发数：指系统同时能处理的请求数量。
      1. 并发连接数：指的是客户端向服务器发起请求，并建立了TCP连接。每秒钟服务器连接的总TCP数量
         1. 同时连接的数量
      2. 请求数：也称为QPS(Query Per Second) 指每秒多少请求.
         1. 一个请求就是一个QPS
      3. 并发用户数：单位时间内有多少用户
   3. 吞吐量：指单位时间内系统能处理的请求数量。
      1. QPS：Query Per Second 每秒查询数。
      2. TPS：Transactions Per Second 每秒事务数
      3. 一个事务是指一个客户机向服务器发送请求然后服务器做出反应的过程。客户机在发送请求时开始计时，收到服务器响应后结束 计时，以此来计算使用的时间和完成的事务个数。
      4. 一个页面的一次访问，只会形成一个TPS；但一次页面请求，可能产生多次对服务器的请求，就会有多个QPS
      5. QPS >= 并发连接数 >= TPS
   4. 高性能：提供快速的访问体验
   5.  高可用：网站服务一直可以正常访问。
       1.  最重要的,   高并发，高可用
   6.  可伸缩：通过硬件增加/减少，提高/降低处理能力。
   7.  高可扩展：系统间耦合低，方便的通过新增/移除方式，增加/减 少新的功能/模块。
   8.  安全性：提供网站安全访问和数据加密，安全存储等策略。
   9.  敏捷性：随需应变，快速响应。

#### 1.2 集群和分布式

1. 集群：很多“人”一起 ，干一样的事。
2. 分布式：很多“人”一起，干不一样的事。这些不一样的事，合起来是一件大事。

![image-20200812094148633](img/image-20200812094148633.png)

![image-20200812094206597](img/image-20200812094206597.png)

![image-20200812094220669](img/image-20200812094220669.png)

![image-20200812094236989](img/image-20200812094236989.png)

总结：

​	集群：很多“人”一起 ，干一样的事。

​		一个业务模块，部署在多台服务器上。

   分布式：很多“人”一起，干不一样的事。这些不一样的事，合起来是一件大事。

​	 一个大的业务系统，拆分为小的业务模块，分别部署在不同的机器上。

#### 1.3 架构演进

![image-20200812094340547](img/image-20200812094340547.png)

##### 1.3.1 架构演进--单体架构

![image-20200812094418345](img/image-20200812094418345.png)

优点：

​	简单：开发部署都很方便，小型项目首选

缺点：

​	1. 项目启动慢

	2. 可靠性差
	3. 可伸缩性差
	4. 扩展性和可维护性差
	5. 性能低

##### 1.3.2 架构演进--垂直架构

![image-20200812094527376](img/image-20200812094527376.png)

1. 垂直架构是指将单体架构中的多个模块拆分为多个独立的项目。形成多个独立的单 体架构。
2. 单体架构存在的问题：
   1. 项目启动慢
   2. 可靠性差
    3. 可伸缩性差
    4. 扩展性和可维护性差
    5. 性能低
3. 垂直架构存在的问题：
   1. 重复功能太多



##### 1.3.3 架构演进--分布式架构

![image-20200812094753648](img/image-20200812094753648.png)

1. 分布式架构是指在垂直架构的基础上，将公共业务模块抽取出来，作为独立的服务 ，供其他调用者消费，以实现服务的共享和重用。
2. RPC： Remote Procedure Call 远程过程调用。有非常多的协议和技术来都实现 了RPC的过程。比如：HTTP REST风格，Java RMI规范、WebService SOAP协议 、Hession等等。
3. 垂直架构存在的问题：
   1. 重复功能太多
4. 分布式架构存在的问题：
   1. 服务提供方一旦产生变更，所有消费方都需要变更。

##### 1.3.4 架构演进--SOA架构

![image-20200812094924344](img/image-20200812094924344.png)

1. SOA：（Service-Oriented Architecture，面向服务的架构）是一个组件模型， 它将应用程序的不同功能单元（称为服务）进行拆分，并通过这些服务之间定义良 好的接口和契约联系起来。
2. ESB：(Enterparise Servce Bus) 企业服务总线，服务中介。主要是提供了一个服 务于服务之间的交互。ESB 包含的功能如：负载均衡，流量控制，加密处理，服务 的监控，异常处理，监控告急等等。
3. 分布式架构存在的问题：
   1. 服务提供方一旦产生变更，所有消费方都需要变更

##### 1.3.5 架构演进--微服务架构

1. 微服务架构是在 SOA 上做的升华，微服务架构强调的一个重点是“业务需要彻底的组件化和服务化”，原有的单个 业务系统会拆分为多个可以独立开发、设计、运行的小应用。这些小应用之间通过服务完成交互和集成。
2. 微服务架构 = 80%的SOA服务架构思想 + 100%的组件化架构思想 + 80%的领域建模思想

![image-20200812100612331](img/image-20200812100612331.png)

特点：

	1.	服务实现组件化：开发者可以自由选择开发技术。也 不需要协调其他团队
	2.	服务之间交互一般使用REST API
	3.	去中心化：每个微服务有自己私有的数据库持久化业 务数据
	4.	自动化部署：把应用拆分成为一个一个独立的单个服 务，方便自动化部署、测试、运维



总结：

Dubbo 是 SOA时代的产物，SpringCloud 是微服务时代的产物

![image-20200812100656407](img/image-20200812100656407.png)



### 2.  Dubbo 概述

#### 2.1 Dubbo概念

1. Dubbo是阿里巴巴公司开源的一个高性能、轻量级的 Java RPC 框架
2. 致力于提供高性能和透明化的 RPC 远程服务调用方案，以及 SOA 服务治理方案。
3. 官网：http://dubbo.apache.org

#### 2.2 Dubbo架构   面试题

![image-20200812100758742](img/image-20200812100758742.png)

​		![image-20200609150003530](img/image-20200609150003530.png)

节点角色说明:

 	1.	Provider：暴露服务的服务提供方
 	2.	Container：服务运行容器
 	3.	Consumer：调用远程服务的服务消费方
 	4.	Registry：服务注册与发现的注册中心
 	5.	Monitor：统计服务的调用次数和调用时间的监控中心

### 3. Dubbo 快速入门

#### 3.1 Zookeeper安装

Dubbo官方推荐使用Zookeeper作为注册中心

#### 3.2 Dubbo快速入门-重点

实现步骤：

1. 创建服务提供者Provider模块
2. 创建服务消费者Consumer模块
3. 在服务提供者模块编写 UserServiceImpl 提供服务
4. 在服务消费者中的 UserController 远程调用 UserServiceImpl 提供的服务
5. 分别启动两个服务，测试

![image-20200812101459209](img/image-20200812101459209.png)

![image-20200812101516092](img/image-20200812101516092.png)

##### 3.2.1创建springmvc工程dubbo-pro 

1. 新建**dubbo-web**模块。在pom.xml 中导入依赖,packaging为war

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>dubbo-web</artifactId>
       <version>1.0-SNAPSHOT</version>
       <packaging>war</packaging>
   
       <properties>
           <maven.compiler.source>8</maven.compiler.source>
           <maven.compiler.target>8</maven.compiler.target>
           <spring.version>5.1.9.RELEASE</spring.version>
           <dubbo.version>2.7.4.1</dubbo.version>
           <zookeeper.version>4.0.0</zookeeper.version>
       </properties>
   
       <dependencies>
           <!-- servlet3.0规范的坐标 -->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>3.1.0</version>
               <scope>provided</scope>
           </dependency>
           <!--spring的坐标-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>${spring.version}</version>
           </dependency>
           <!--springmvc的坐标-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>${spring.version}</version>
           </dependency>
   
           <!--日志-->
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-api</artifactId>
               <version>1.7.21</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
               <version>1.7.21</version>
           </dependency>
   
   
   
           <!--Dubbo的起步依赖，版本2.7之后统一为org.apache.dubbo -->
           <dependency>
               <groupId>org.apache.dubbo</groupId>
               <artifactId>dubbo</artifactId>
               <version>${dubbo.version}</version>
           </dependency>
           <!--ZooKeeper客户端实现 -->
           <dependency>
               <groupId>org.apache.curator</groupId>
               <artifactId>curator-framework</artifactId>
               <version>${zookeeper.version}</version>
           </dependency>
           <!--ZooKeeper客户端实现 -->
           <dependency>
               <groupId>org.apache.curator</groupId>
               <artifactId>curator-recipes</artifactId>
               <version>${zookeeper.version}</version>
           </dependency>
   
   
   
           <!--依赖service模块-->
           <dependency>
               <groupId>com.itheima</groupId>
               <artifactId>dubbo-service</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
   
       </dependencies>
   
   
       <build>
           <plugins>
               <!--tomcat插件-->
               <plugin>
                   <groupId>org.apache.tomcat.maven</groupId>
                   <artifactId>tomcat7-maven-plugin</artifactId>
                   <version>2.1</version>
                   <configuration>
                       <port>8000</port>
                       <path>/</path>
                   </configuration>
               </plugin>
           </plugins>
       </build>
   </project>
   ```

2. 新建**dubbo-service**模块，pom

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>dubbo-service</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <properties>
           <maven.compiler.source>8</maven.compiler.source>
           <maven.compiler.target>8</maven.compiler.target>
           <spring.version>5.1.9.RELEASE</spring.version>
           <dubbo.version>2.7.4.1</dubbo.version>
           <zookeeper.version>4.0.0</zookeeper.version>
       </properties>
   
       <dependencies>
           <!-- servlet3.0规范的坐标 -->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>3.1.0</version>
               <scope>provided</scope>
           </dependency>
           <!--spring的坐标-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>${spring.version}</version>
           </dependency>
           <!--springmvc的坐标-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>${spring.version}</version>
           </dependency>
   
           <!--日志-->
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-api</artifactId>
               <version>1.7.21</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
               <version>1.7.21</version>
           </dependency>
   
   
   
           <!--Dubbo的起步依赖，版本2.7之后统一为rg.apache.dubb -->
           <dependency>
               <groupId>org.apache.dubbo</groupId>
               <artifactId>dubbo</artifactId>
               <version>${dubbo.version}</version>
           </dependency>
           <!--ZooKeeper客户端实现 -->
           <dependency>
               <groupId>org.apache.curator</groupId>
               <artifactId>curator-framework</artifactId>
               <version>${zookeeper.version}</version>
           </dependency>
           <!--ZooKeeper客户端实现 -->
           <dependency>
               <groupId>org.apache.curator</groupId>
               <artifactId>curator-recipes</artifactId>
               <version>${zookeeper.version}</version>
           </dependency>
   
       </dependencies>
   
   </project>
   ```

3. dubbo-service模块中，新建一个UserService,以及impl

   ```java
   package com.itheima.service;
   
   public interface UserService {
       public String sayHello();
   }
   ```

   ```java
   package com.itheima.service.impl;
   
   import com.itheima.service.UserService;
   
   @Service
   public class UserServiceImpl implements UserService {
       @Override
       public String sayHello() {
           return "hello dubbo";
       }
   }
   ```

4. dubbo-service模块中,放入资料中的spring配置文件和日志配置文件

   ![image-20210319165542585](img/image-20210319165542585.png)

   并且在xml中，进行扫包

   ```
   <context:component-scan base-package="com.itheima.service" />
   ```

5. dubbo-web中，新建webapp->WEB-INF->web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns="http://java.sun.com/xml/ns/javaee"
            xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
            version="2.5">
   
   
       <!-- spring -->
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath*:spring/applicationContext*.xml</param-value>
       </context-param>
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   
       <!-- Springmvc -->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!-- 指定加载的配置文件 ，通过参数contextConfigLocation加载-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:spring/springmvc.xml</param-value>
           </init-param>
       </servlet>
   
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>*.do</url-pattern>
       </servlet-mapping>
   
   
   </web-app>
   ```

6. dubbo-web中,复制资料中的springmvc配置文件和日志配置文件

   ![image-20210319170343365](img/image-20210319170343365.png)

   并且加上

   ```xml
       <mvc:annotation-driven/>
       <context:component-scan base-package="com.itheima.controller"/>
   ```

7. dubbo-web中,创建controller

   ```java
   package com.itheima.controller;
   
   import com.itheima.service.UserService;
   import org.apache.zookeeper.server.admin.Commands;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @RestController
   @RequestMapping("/user")
   public class UserController {
   
       @Autowired
       private UserService userService;
   
       @RequestMapping("/sayHello")
       public String sayHello(){
           return userService.sayHello();
       }
   
   }
   ```

8. service工程imstall,启动web工程

   ![image-20210319170934584](img/image-20210319170934584.png)

   访问：http://localhost:8000/user/sayHello.do

##### 3.2.2改造service

1. 已经导入dubbo包

   ![image-20210319171420424](img/image-20210319171420424.png)

2. 打包方式改为war

   ```xml
       <packaging>war</packaging>
       
       <build>
           <plugins>
               <!--tomcat插件-->
               <plugin>
                   <groupId>org.apache.tomcat.maven</groupId>
                   <artifactId>tomcat7-maven-plugin</artifactId>
                   <version>2.1</version>
                   <configuration>
                       <port>9000</port>
                       <path>/</path>
                   </configuration>
               </plugin>
           </plugins>
       </build>
   ```

3. UserServiceImpl中 修改注解

   ```
   @Service //dubbo包  发布到dubbo中
   ```

4. 配置 applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:dubbo="http://dubbo.apache.org/schema/dubbo" xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
           http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
   
   	<!--<context:component-scan base-package="com.itheima.service" />-->
   
   	<!--dubbo的配置-->
   	<!--1.配置项目的名称,唯一-->
   	<dubbo:application name="dubbo-service"/>
   	<!--2.配置注册中心的地址-->
   	<dubbo:registry address="zookeeper://192.168.200.130:2181"/>
   	<!--3.配置dubbo包扫描-->
   	<dubbo:annotation package="com.itheima.service.impl" />
   
   </beans>
   ```

5. 新建webapp->WEB-INF->web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
            version="3.1">
   
       <!-- spring -->
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath*:spring/applicationContext*.xml</param-value>
       </context-param>
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   
   </web-app>
   
   ```

6. 启动service工程

##### 3.2.3改造dubbo-web

1. 依赖中去掉dubbo-service，创建UserService

   ```java
   package com.itheima.service;
   
   public interface UserService {
       public String sayHello();
   }
   ```

2. pom.xml 已经导入依赖

   ![image-20210319172938211](img/image-20210319172938211.png)

3. web.xml 去掉spring加载

   ```xml
   <!--    <context-param>-->
   <!--        <param-name>contextConfigLocation</param-name>-->
   <!--        <param-value>classpath*:spring/applicationContext*.xml</param-value>-->
   <!--    </context-param>-->
   <!--    <listener>-->
   <!--        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>-->
   <!--    </listener>-->
   ```

4. 改造controller

   ```java
   package com.itheima.controller;
   
   import com.itheima.service.UserService;
   import org.apache.dubbo.config.annotation.Reference;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @RestController
   @RequestMapping("/user")
   public class UserController {
   
       //注入Service
       //@Autowired//本地注入
   
       /*
           1. 从zookeeper注册中心获取userService的访问url
           2. 进行远程调用RPC
           3. 将结果封装为一个代理对象。给变量赋值
        */
   
       @Reference//远程注入
       private UserService userService;
   
   
       @RequestMapping("/sayHello")
       public String sayHello(){
           return userService.sayHello();
       }
   
   }
   ```

5. 改造springmvc.xml 加入

   ```
       <!--dubbo的配置-->
       <!--1.配置项目的名称,唯一-->
       <dubbo:application name="dubbo-web"/>
       <!--2.配置注册中心的地址-->
       <dubbo:registry address="zookeeper://192.168.200.130:2181"/>
       <!--3.配置dubbo包扫描-->
       <dubbo:annotation package="com.itheima.controller" />
   ```

6. 启动web，访问http://localhost:8000/user/sayHello.do

7. 问题解决，springmvc.xml修改监控端口

   ```xml
       <dubbo:application name="dubbo-web">
           <dubbo:parameter key="qos.port" value="33333"></dubbo:parameter>
       </dubbo:application>
   ```

##### 3.2.4深入改造

目前问题，service和web都定义一次接口，麻烦。所以我们提取接口。

1. 新建工程dubbo-interface

2. 复制UserService到interface工程

   ```java
   package com.itheima.service;
   
   public interface UserService {
       public String sayHello();
   }
   ```

3. 删除web和service工程中的接口

4. web和service工程中导入interface工程

   ```xml
           <dependency>
               <groupId>com.itheima</groupId>
               <artifactId>dubbo-interface</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
   ```

5. interface工程install,重启web、service工程，测试

   

默认调用：单一的长连接形式，NIO实现的，序列化与反序列化，hession协议实现的， hession协议本质上是一个http协议，输入和输出做了优化

### 4. Dubbo 高级特性

#### 4.1 dubbo-admin管理平台

1安装nodejs

2ui  安装

```
npm build
```

3admin 

```
mvn clean package
```

4启动ui

```
npm run dev
```

5启动admin后端

distribution中

```
java -jar xxxx.jar
```





1. dubbo-admin 管理平台，是图形化的服务管理页面
2. 从注册中心中获取到所有的提供者 / 消费者进行配置管理
3. 路由规则、动态配置、服务降级、访问控制、权重调整、 负载均衡等管理功能
4. dubbo-admin 是一个前后端分离的项目。前端使用vue ，后端使用springboot
5. 安装 dubbo-admin 其实就是部署该项目

#### 4.2 dubbo 常用高级配置

##### 4.2.1 序列化

![image-20200812101629625](img/image-20200812101629625.png)

hession协议，

两个机器传输数据，如何传输Java对象？

1. dubbo 内部已经将序列化和反序列化的过程内部封装了
2. 我们只需要在定义pojo类时实现Serializable接口即可
3. 一般会定义一个公共的pojo模块，让生产者和消费者都依赖 该模块。

代码实现

1. 创建模块dubbo-pojo

2. 创建实体类

   ```java
   package com.itheima.pojo;
   
   import java.io.Serializable;
   
   /**
    * 注意！！！
    *  将来所有的pojo类都需要实现Serializable接口
    */
   public class User implements Serializable {
       private int id;
       private String username;
       private String password;
   
       public User() {
       }
   
       public User(int id, String username, String password) {
           this.id = id;
           this.username = username;
           this.password = password;
       }
   
       public int getId() {
           return id;
       }
   
       public void setId(int id) {
           this.id = id;
       }
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   
       public String getPassword() {
           return password;
       }
   
       public void setPassword(String password) {
           this.password = password;
       }
   }
   
   ```

3. interface service web三个工程导包pojo

   ```java
           <dependency>
               <groupId>com.itheima</groupId>
               <artifactId>dubbo-pojo</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
   ```

   

4. intterface工程接口添加方法

   ```java
       /**
        * 查询用户
        */
       public User findUserById(int id);
   ```

5. serviceimpl实现方法

   ```java
       @Override
       public User findUserById(int id) {
           //查询User对象
           User user = new User(1,"zhangsan","123");
   
           return user;
       }
   ```

6. web中controler新增方法

   ```java
       @RequestMapping("/find")
       public User find(int id){
           return userService.findUserById(id);
       }
   ```

7. pojo、interface安装install。启动service、web测试http://localhost:8000/user/find.do?id=1

##### 4.2.2 地址缓存

注册中心挂了，服务是否可以正常访问？  面试题

1. 可以，因为dubbo服务消费者在第一次调用时， 会将服务提供方地址缓存到本地，以后在调用则 不会访问注册中心。
2. 当服务提供者地址发生变化时，注册中心会通知 服务消费者。



测试：关闭zk，还能调用成功。

##### 4.2.3 超时与重试

![image-20200812101740858](img/image-20200812101740858.png)

1. 服务消费者在调用服务提供者的时候发生了阻塞、等待的情形，这个时候，服务消费者会一直等待下去。
2. 在某个峰值时刻，大量的请求都在同时请求服务消费者，会造成线程的大量堆积，势必会造成雪崩。
3. dubbo 利用超时机制来解决这个问题，设置一个超时时间，在这个时间段内，无法完成服务访问，则自动断开连接。
   1.  使用timeout属性配置超时时间，默认值1000，单位毫秒。

![image-20200812101839863](img/image-20200812101839863.png)

![image-20200812101944486](img/image-20200812101944486.png)

1. 设置了超时时间，在这个时间段内，无法完成服务访问，则自动断开连接。
2. 如果出现网络抖动，则这一次请求就会失败。
3. Dubbo 提供重试机制来避免类似问题的发生。
4.  通过 retries 属性来设置重试次数。默认为 2 次。

**代码**：

​	1提供方超时：

​    1.1提供方

```java
package com.itheima.service.impl;

import com.itheima.pojo.User;
import com.itheima.service.UserService;
import org.apache.dubbo.config.annotation.Service;

//@Service
@Service(timeout = 3000,retries = 0) //dubbo包  发布到dubbo中
public class UserServiceImpl implements UserService {
    @Override
    public String sayHello() {
        return "hello dubbo interface";
    }

    @Override
    public User findUserById(int id) {
        //查询User对象
        User user = new User(1,"zhangsan","123");
        
        //查询数据库很慢 5秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return user;
    }
}
```

​	1.2web调用方

```java
/**
     * 根据id查询用户信息
     * @param id
     * @return
     */
    int i = 1;
    @RequestMapping("/find")
    public User find(int id){

        new Thread(new Runnable() {
            public void run() {
                while (true){
                    System.out.println(i++);
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();


        return userService.findUserById(id);
    }
```

测试，爆出错

![image-20210320002245424](img/image-20210320002245424.png)



![image-20210320002221609](img/image-20210320002221609.png)

 

​	2调用方超时

```
@Reference(timeout = 1000)
```

​	**思考：到底几秒超时？**

​    **结论**：谁短谁超时！



**真实生产环境，建议只在提供方配置即可！**



重试：

 提供方：

```
package com.itheima.service.impl;

import com.itheima.pojo.User;
import com.itheima.service.UserService;
import org.apache.dubbo.config.annotation.Service;

//@Service
@Service(timeout = 3000,retries = 2) //dubbo包  发布到dubbo中
public class UserServiceImpl implements UserService {
    @Override
    public String sayHello() {
        return "hello dubbo interface";
    }

    int i=1;
    @Override
    public User findUserById(int id) {
        System.out.println("服务被调用了："+i++);
        //查询User对象
        User user = new User(1,"zhangsan","123");

        //查询数据库很慢 5秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return user;
    }
}
```

测试：

![image-20210320003153929](img/image-20210320003153929.png)

##### 4.2.4 多版本

![image-20200812102026056](img/image-20200812102026056.png)

1. **灰度发布**：当出现新功能时，会让一部 分用户先使用新功能，用户反馈没问题 时，再将所有用户迁移到新功能
2. dubbo 中使用version 属性来设置和调 用同一个接口的不同版本

代码：

**提供者**

1实现类：

```
@Service(version = "v1.0")
```

2新增实现类

```java
package com.itheima.service.impl;

import com.itheima.pojo.User;
import com.itheima.service.UserService;
import org.apache.dubbo.config.annotation.Service;


//@Service//将该类的对象创建出来，放到Spring的IOC容器中  bean定义
//将这个类提供的方法（服务）对外发布。将访问的地址 ip，端口，路径注册到注册中心中
@Service(version = "v2.0")
public class UserServiceImpl2 implements UserService {

    public String sayHello() {
        return "hello dubbo hello!~";
    }


    public User findUserById(int id) {
        System.out.println("new....");
        //查询User对象
        User user = new User(1,"zhangsan","123");

        return user;
    }
}
```

 3调用方

```
@Reference(version = "v2.0")
```

测试：

![image-20210320003831420](img/image-20210320003831420.png)

##### 4.2.5 负载均衡

![](img/image-20200812102055804.png)

![image-20200812102144740](img/image-20200812102144740.png)

![image-20200812102204376](img/image-20200812102204376.png)

负载均衡策略（4种） ：

1. Random ：按权重随机，默认值。按权重设置随 机概率。
2. RoundRobin ：按权重轮询
3. LeastActive：最少活跃调用数，相同活跃数的随 机。
4. ConsistentHash：一致性 Hash，相同参数的请求 总是发到同一提供者。
   1. hash(1)=12312311%3=1
   2. hash(2)=123123132%3=2



测试： 

1提供方 起三份

UserserviceImpl  权重分别为100 200 100

```
@Service(weight = 100) 
public class UserServiceImpl implements UserService {

    public String sayHello() {
        return "1......";
    }
```

pom.xml tomcat端口分别为9000 9001 9002

![image-20210320004412253](img/image-20210320004412253.png)

applicationContext.xml  提供端口20880 20881 20882 监控端口22222 33333 44444

![image-20210320004500654](img/image-20210320004500654.png)

  结果查看

![image-20210320005643799](img/image-20210320005643799.png)

2调用方： 值为 random、roundrobin、leastactive、consistenthash （全局搜索AbstractLoadBalance）

```
@Reference(loadbalance = "random")
```

测试：http://localhost:8000/user/sayHello.do

其余算法大家自己测试



##### 4.2.6 集群容错

![image-20200812102328620](img/image-20200812102328620.png)

集群容错模式：

	1.  Failover Cluster：失败重试。默认值。当出现失败，重试其它 服务器 ，默认重试2次，使用 retries 配置。一般用于读操作
	2.	Failfast Cluster ：快速失败，只发起一次调用，失败立即报错。 通常用于写操作。
	3.	Failsafe Cluster ：失败安全，出现异常时，直接忽略。返回一 个空结果。
	4.	Failback Cluster ：失败自动恢复，后台记录失败请求，定时 重发。通常用于消息通知操作。
	5.	Forking Cluster ：并行调用多个服务器，只要一个成功即返回
	6.	Broadcast Cluster ：广播调用所有提供者，逐个调用，任意 一台报错则报错。



代码：

1提供方：

```
    int i=1;
    @Override
    public User findUserById(int id) {
        System.out.println("1 服务被调用了："+i++);
        //查询User对象
        User user = new User(1,"zhangsan","123");

        //查询数据库很慢 5秒
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return user;
    }
```

起三份 修改相应代码

第三份，去掉睡眠。

2调用方： 值搜索Cluster

```
  @Reference(version = "v1.0",cluster = "failover")
```



测试：http://localhost:8000/user/find.do?id=1

![image-20210320011028692](img/image-20210320011028692.png)

![image-20210320011047803](img/image-20210320011047803.png)

![image-20210320011058615](img/image-20210320011058615.png)



##### 4.2.7 服务降级

![image-20210320011937236](img/image-20210320011937236.png)

Random LoadBalance

服务降级的概念，将一些服务临时停掉，保证资源最大化的利用，优先保证核心服务的运行。

- `mock="force:return null"` 表示消费方对该服务的方法调用都直接返回 null 值，不发起远程调用。用来屏蔽不重要服务不可用时对调用方的影响。
- 还可以改为 `mock="fail:return null" 表示消费方对该服务的方法调用在失败后，再返回 null 值，不抛异常。用来容忍不重要服务不稳定时对调用方的影响。



代码：

**情况一**：

提供方：打开睡眠

调用方：

```
    @Reference(version = "v1.0",cluster = "failover",mock = "force:return null")
```

测试：http://localhost:8000/user/find.do?id=1 

结果：毫无反应



**情况二**：

提供方：打开睡眠

调用方：

```
    @Reference(version = "v1.0",cluster = "failover",mock = "fail:return null")
```

测试：http://localhost:8000/user/find.do?id=1 

结果：调用方失败三次返回空



总结：

1互联网项目特点

高并发 海量数据 

tps qps

伸缩性

2集群分布式

集群：同一个模块，部署多份，干同一份事儿。

分布式：多个模块做不同的事，总体完成一件事儿。

3架构演进

单体

![image-20210325173343020](img/image-20210325173343020.png)

垂直拆分

![image-20210325173414501](img/image-20210325173414501.png)

分布式

![image-20210325173443164](img/image-20210325173443164.png)

soa架构

![image-20210325173511827](img/image-20210325173511827.png)

微服务架构

![image-20210325173643910](img/image-20210325173643910.png)



4dubbo

阿里巴巴 Apache soa

![image-20210325174008478](img/image-20210325174008478.png)

用法：提供方 @service注解

​			调用方@Refrence

5高级特性

dubbo-admin

序列化： implements Serializable

地址缓存：zk挂了 还能调用

超时：

​	提供方：timeout=1000

​	调用方：timeout

重试：提供方 retries=2

多版本： 灰度上线

​			 提供：version="v2"

​			调用：version="v2

负载均衡：

​	调用方：loadbalance = "random"

集群容错：

​	调用方：cluster = "failover"

服务降级：砍掉功能

​	调用方：mock = "fail:return null" 

​				   mock = "force:return null"



反馈：







