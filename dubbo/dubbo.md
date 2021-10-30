以下项目只是基于maven不是基于springboot，所以需要进行一些基础配置
### dubbo官方推荐使用的项目结构如下
- 1、接口工程
  - 业务接口和实体类
  - Java工程
- 2、服务提供者工程 
  - 实现接口工程中的业务接口
  - web工程
- 3、服务消费者工程
  - 消费服务提供者提供的业务接口
  - web工程

## 项目结构
### 1、接口工程

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2ab323b2507c44dc829a0f153e8fee1a~tplv-k3u1fbpfcp-watermark.image?)
~~~

~~~

### 2、服务提供者工程 

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/57786f32ce224c1399525b4c86ec7560~tplv-k3u1fbpfcp-watermark.image?)
~~~

~~~

### 3、服务消费者工程

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f16dee04f8c4722bddf2cfc71e3e53b~tplv-k3u1fbpfcp-watermark.image?)
~~~

~~~
### 创建项目说明
#### web工程创建

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/080879f17e2244d5ad5d864fa812ff76~tplv-k3u1fbpfcp-watermark.image?)
#### java工程的话把上图中勾选的2的去掉就行了
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/11e9b9e64326469a838a05b572dfa73f~tplv-k3u1fbpfcp-watermark.image?)
这样子就创建成功了。
#### （pom.xml） 三个项目都初始为下图，把没用的都删除了
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.sheng.dubbo</groupId>
    <artifactId>003-interface</artifactId>
    <version>1.0-SNAPSHOT</version>


</project>
```
## 代码实现
### 1、接口工程
#### （pom.xml） 不需要导入依赖
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.sheng.dubbo</groupId>
    <artifactId>003-interface</artifactId>
    <version>1.0-SNAPSHOT</version>
</project>
```
#### (User) dubbo 的对象需要实现序列才能够进行通信
```
package com.sheng.dubbo.model;

import java.io.Serializable;

public class User implements Serializable {
    private Integer id;
    private String username;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```
#### (SomeService)
```
package com.sheng.dubbo.service;

import com.sheng.dubbo.model.User;

public interface SomeService {

    String hello();

    User queryUserById(Integer id);
}
```

### 2、服务提供者工程
#### （pom.xml）
```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.sheng.dubbo</groupId>
  <artifactId>008-multi-zk-provider</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <dependencies>
<!--    dubbo依赖-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>
<!--    spring依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.3.16.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.3.16.RELEASE</version>
    </dependency>
    <!--zookeeper注册中心依赖-->
    <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-framework</artifactId>
      <version>4.1.0</version>
    </dependency>
    <!--接口工程-->
    <dependency>
      <groupId>com.sheng.dubbo</groupId>
      <artifactId>003-interface</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
  </dependencies>
</project>
```
#### 接口的两个实现类
#### (SomeServiceImpl)
```
package com.sheng.dubbo.service.impl;

import com.sheng.dubbo.model.User;
import com.sheng.dubbo.service.SomeService;

public class SomeServiceImpl implements SomeService {
    @Override
    public String hello() {
        return "hello multi dubbo zookeeper";
    }

    @Override
    public User queryUserById(Integer id) {
        User user = new User();
        user.setId(id);
        user.setUsername("zzs-" + id);
        return user;
    }
}
```
#### (NewSomeServiceImpl)
```
package com.sheng.dubbo.service.impl;

import com.sheng.dubbo.model.User;
import com.sheng.dubbo.service.SomeService;

public class NewSomeServiceImpl implements SomeService {
    @Override
    public String hello() {
        return "hello dubbo project 222";
    }

    @Override
    public User queryUserById(Integer id) {
        User user = new User();
        user.setId(id);
        user.setUsername("lisi-" + id);
        return user;
    }
}
```
#### (dubbo-multi-zk-provider.xml) dubbo核心配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!--声明服务提供者的名称，保证它的唯一性，它是dubbo内部使用的唯一标识-->
    <dubbo:application name="008-multi-zk-provider"/>

    <!--
    设置协议和端口号
    name 协议   官方推荐dubbo
    port 端口号  默认端口20880
    -->
    <dubbo:protocol name="dubbo" port="20880"/>
    <!--指定Zookeeper注册中心-->
    <dubbo:registry address="zookeeper://localhost:2181"/>
    <!--
    暴露服务
        interface：接口的全限定类名
        ref 引用接口在spring容器中的唯一标识
        version 版本号 一个接口可能对应多个实现类 以版本号来区分
    -->
    <dubbo:service interface="com.sheng.dubbo.service.SomeService" ref="someServiceImpl" version="1.0"/>

    <dubbo:service interface="com.sheng.dubbo.service.SomeService" ref="newSomeServiceImpl" version="2.0"/>
    <!--
    加载接口实现类
        id 就是接口实现类的唯一标识
    -->
    <bean id="someServiceImpl" class="com.sheng.dubbo.service.impl.SomeServiceImpl"/>
    <bean id="newSomeServiceImpl" class="com.sheng.dubbo.service.impl.NewSomeServiceImpl"/>
</beans>
```
#### (web.xml) 定义一个监听器 加载dubbo的核心配置文件（dubbo-multi-zk-provider.xml）
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="4.0" xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
   http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">

  <!--配置监听器 服务启动时自动加载配置文件-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:dubbo-multi-zk-provider.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
</web-app>
```
### 3、服务消费者工程
#### (pom.xml) 导入的依赖和服务提供者的一样
```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.sheng.dubbo</groupId>
  <artifactId>009-multi-zk-consumer</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <dependencies>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.3.16.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.3.16.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-framework</artifactId>
      <version>4.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.sheng.dubbo</groupId>
      <artifactId>003-interface</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
  </dependencies>
</project>
```


#### (dubbo-multi-zk-provider.xml) dubbo核心配置文件
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!--声明服务提供者的名称，保证它的唯一性，它是dubbo内部使用的唯一标识 一般使用项目名称-->
    <dubbo:application name="009-multi-zk-consumer"/>

    <dubbo:registry address="zookeeper://localhost:2181"/>

    <!--id: 远程接口服务的代理对象名称-->
    <dubbo:reference id="someService" interface="com.sheng.dubbo.service.SomeService" version="1.0"/>
    <dubbo:reference id="newSomeService" interface="com.sheng.dubbo.service.SomeService" version="2.0"/>
</beans>
```
#### (SomeController) 。上面的（dubbo-multi-zk-provider.xml）配置文件中通过版本号区分不同实现类。
```
package com.sheng.dubbo.web;

import com.sheng.dubbo.service.SomeService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class SomeController {

    /*
     * 注入两次，注入的实体类和（dubbo-multi-zk-provider.xml）配置文件中配置的顺序相同
     */
    @Autowired
    SomeService someService;

    @Autowired
    SomeService newSomeService;

    @RequestMapping("/hello")
    public String hello(Model model) {
        String hello = someService.hello();
        model.addAttribute("hello", hello);
        String hello1 = newSomeService.hello();
        model.addAttribute("hello1", hello1);
        return "hello";
    }

}
```

#### (springmvc.xml) 配置视图解析器
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <dubbo:application name="009-multi-zk-consumer"/>

    <dubbo:registry address="zookeeper://localhost:2181"/>

    <dubbo:reference id="someService" interface="com.sheng.dubbo.service.SomeService" version="2.0"/>
    <dubbo:reference id="newSomeService" interface="com.sheng.dubbo.service.SomeService" version="1.0"/>
</beans>
```
#### (web.xml) 定义一个监听器 加载dubbo的核心配置文件（dubbo-multi-zk-provider.xml）和视图解析配置文件
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="4.0" xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
   http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
  
  <!--中央调度器 本质是一个servlet-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:dubbo-multi-zk-consumer.xml, classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <!--拦截所有请求-->
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```
#### (hello.js) 
```
<%--
  Created by IntelliJ IDEA.
  User: 战神
  Date: 2021/10/27
  Time: 11:44
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>${hello}</h1>
<h1>${hello1}</h1>
</body>
</html>
```
### web项目启动 java项目不需要启动


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6fda468ecd564deb8ba190284b03adf2~tplv-k3u1fbpfcp-watermark.image?)


![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/89613d5a5d4d4bdf958f0d6af3444367~tplv-k3u1fbpfcp-watermark.image?)


![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9689f02227ac4a9da9c1d4501e64bbc5~tplv-k3u1fbpfcp-watermark.image?)

两个web项目的端口不能一样，消费者的端口按默认的8080就行了 JMX端口也是按默认的
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/613aeca34f9b41949078c4d195bb333a~tplv-k3u1fbpfcp-watermark.image?)
点击OK，Tomcat就配置完了，启动项目,启动项目的时候还需要吧Zookeeper启动起来，这个我就不贴图了，不会的可以上网找以下教学

#### 最终效果

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/981e5ccd2ae447aaa70fa5bfd23ca43b~tplv-k3u1fbpfcp-watermark.image?)
