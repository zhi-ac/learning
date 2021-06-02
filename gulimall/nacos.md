## nacos作为我们的注册中心和配置中心。

安装启动nacos：下载–解压–双击bin/startup.cmd。http://127.0.0.1:8848/nacos/ 账号密码nacos
也可以把nacos的源码下载下来，跑起来，这样子配置的信息能够保留下来

## 使用nacos：
* 在某个项目里properties里写spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848（yaml同理，指定nacos的地址）。再指定applicatin.name告诉注册到nacos中以什么命名
* 依赖：放到common里，不写版本是因为里面有了版本管理
微服务想要注册到注册中心，需要引入以下依赖
~~~
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
~~~
* 使用 @EnableDiscoveryClient 注解开启服务注册与发现功能
~~~
@SpringBootApplication
@EnableDiscoveryClient
public class ProviderApplication {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @RestController
    class EchoController {
        @GetMapping(value = "/echo/{string}")
        public String echo(@PathVariable String string) {
            return string;
        }
    }
}

~~~
* 最后application.yml内容，配置了服务中心名和当前模块名字
~~~
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://192.168.56.10:3306/gulimall-sms?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai
    driver-class-name: com.mysql.cj.jdbc.Driver
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
  application:
    name: gulimall-coupon


mybatis-plus:
  mapper-locations: classpath:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto
      logic-delete-value: 1
      logic-not-delete-value: 0

server:
  port: 7000
~~~

