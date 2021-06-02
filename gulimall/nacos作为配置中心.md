# nacos作为配置中心

我们还可以用nacos作为配置中心。配置中心的意思是不在application.properties等文件中配置了，而是放到nacos配置中心公用，这样无需每台机器都改。

common中添加依赖 nacos配置中心

~~~
<dependency>
     <groupId>com.alibaba.cloud</groupId>
     <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
 </dependency>

~~~

在coupons项目中创建/src/main/resources/bootstrap.properties ，这个文件是springboot里规定的，他优先级别application.properties高

~~~
# 改名字，对应nacos里的配置文件名
spring.application.name=gulimall-coupon
spring.cloud.nacos.config.server-addr=127.0.0.1:8848

~~~

配合@Value

~~~
@RestController
@RequestMapping("coupon/coupon")
public class CouponController {
    @Autowired
    private CouponService couponService;

    @Value("${coupon.user.name}")//从application.properties中获取//不要写user.name，他是环境里的变量
    private String name;
    @Value("${coupon.user.age}")
    private Integer age;
    @RequestMapping("/test")
    public R test(){

        return R.ok().put("name",name).put("age",age);
    }

~~~

浏览器去nacos里的配置列表，点击＋号，data ID：`gulimall-coupon.properties`，配置

~~~
# gulimall-coupon.properties
coupon.user.name="配置中心"      
coupon.user.age=12

~~~

然后点击发布。重启coupon（生产中加入@RefreshScope即可），http://localhost:7000/coupon/coupon/test

~~~
{"msg":"success","code":0,"name":"配置中心","age":12}

~~~

在coupon的控制层上加`@RefreshScope`实现动态刷新

~~~
@RefreshScope
@RestController
@RequestMapping("coupon/coupon")
public class CouponController {
    @Autowired
    private CouponService couponService;

    @Value("${coupon.user.name}")//从application.properties中获取//不要写user.name，他是环境里的变量
    private String name;
    @Value("${coupon.user.age}")
    private Integer age;
    @RequestMapping("/test")
    public R test(){

        return R.ok().put("name",name).put("age",age);
    }

~~~

nacos的配置内容优先于项目本地的配置内容。

# 配置中心进阶

## 命名空间：用作配置隔离。（一般每个微服务一个命名空间）
* 默认public。默认新增的配置都在public空间下
~~~
# 可以选择对应的命名空间 # 写上对应环境的命名空间ID
spring.cloud.nacos.config.namespace=b176a68a-6800-4648-833b-be10be8bab00
~~~

## 配置集：一组相关或不相关配置项的集合

## 配置集ID：类似于配置文件名，即Data ID

## 配置分组：默认所有的配置集都属于DEFAULT_GROUP。双十一，618的优惠策略改分组即可

~~~
# 更改配置分组
spring.cloud.nacos.config.group=DEFAULT_GROUP
~~~ 
最终方案：每个微服务创建自己的命名空间，然后使用配置分组区分环境（dev/test/prod）

# 加载多配置集

在其中用数组spring.cloud.nacos.config.extension-configs[]写明每个配置集

~~~
spring.application.name=gulimall-coupon

spring.cloud.nacos.config.server-addr=127.0.0.1:8848
# 可以选择对应的命名空间 # 写上对应环境的命名空间ID
spring.cloud.nacos.config.namespace=b176a68a-6800-4648-833b-be10be8bab00
# 更改配置分组
spring.cloud.nacos.config.group=dev

#新版本不建议用下面的了
#spring.cloud.nacos.config.ext-config[0].data-id=datasource.yml
#spring.cloud.nacos.config.ext-config[0].group=dev
#spring.cloud.nacos.config.ext-config[0].refresh=true
#spring.cloud.nacos.config.ext-config[1].data-id=mybatis.yml
#spring.cloud.nacos.config.ext-config[1].group=dev
#spring.cloud.nacos.config.ext-config[1].refresh=true
#spring.cloud.nacos.config.ext-config[2].data-id=other.yml
#spring.cloud.nacos.config.ext-config[2].group=dev
#spring.cloud.nacos.config.ext-config[2].refresh=true

spring.cloud.nacos.config.extension-configs[0].data-id=datasource.yml
spring.cloud.nacos.config.extension-configs[0].group=dev
spring.cloud.nacos.config.extension-configs[0].refresh=true

spring.cloud.nacos.config.extension-configs[1].data-id=mybatis.yml
spring.cloud.nacos.config.extension-configs[1].group=dev
spring.cloud.nacos.config.extension-configs[1].refresh=true

spring.cloud.nacos.config.extension-configs[2].data-id=other.yml
spring.cloud.nacos.config.extension-configs[2].group=dev
spring.cloud.nacos.config.extension-configs[2].refresh=true

~~~




