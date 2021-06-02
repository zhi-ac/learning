# Feign（远程调用）与注册中心

声明式远程调用

feign是一个声明式的HTTP客户端，他的目的就是让远程调用更加简单。给远程服务发的是HTTP请求。

会员服务想要远程调用优惠券服务，只需要给会员服务里引入openfeign依赖，他就有了远程调用其他服务的能力。

~~~~
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
~~~

我们之前在member的pom.xml已经引用过了（微服务）。

在coupon中修改如下的内容

~~~
@RequestMapping("coupon/coupon")
public class CouponController {
    @Autowired
    private CouponService couponService;

    @RequestMapping("/member/list")
    public R membercoupons(){    //全系统的所有返回都返回R
        // 应该去数据库查用户对于的优惠券，但这个我们简化了，不去数据库查了，构造了一个优惠券给他返回
        CouponEntity couponEntity = new CouponEntity();
        couponEntity.setCouponName("满100-10");//优惠券的名字
        return R.ok().put("coupons",Arrays.asList(couponEntity));
    }

~~~
这样我们准备好了优惠券的调用内容

在member的配置类上加注解@EnableDiscoveryClient（感觉应该是@EnableFeignClients），告诉member是一个远程调用客户端，member要调用东西的

~~~
/*
* 想要远程调用的步骤：
* 1 引入openfeign
* 2 编写一个接口，接口告诉springcloud这个接口需要调用远程服务
* 	2.1 在接口里声明@FeignClient("gulimall-coupon")他是一个远程调用客户端且要调用coupon服务
* 	2.2 要调用coupon服务的/coupon/coupon/member/list方法
* 3 开启远程调用功能 @EnableFeignClients，要指定远程调用功能放的基础包
* */
@EnableFeignClients(basePackages="com.atguigu.gulimall.member.feign")//扫描接口方法注解
@EnableDiscoveryClient
@SpringBootApplication
public class gulimallMemberApplication {

	public static void main(String[] args) {
		SpringApplication.run(gulimallMemberApplication.class, args);
	}
}

~~~

那么要调用什么东西呢？就是我们刚才写的优惠券的功能，复制函数部分，在member的com.atguigu.gulimall.member.feign包下新建类：

~~~

@FeignClient("gulimall-coupon") //告诉spring cloud这个接口是一个远程客户端，要调用coupon服务(nacos中找到)，具体是调用coupon服务的/coupon/coupon/member/list对应的方法
public interface CouponFeignService {
    // 远程服务的url
    @RequestMapping("/coupon/coupon/member/list")//注意写全优惠券类上还有映射//注意我们这个地方不是控制层，所以这个请求映射请求的不是我们服务器上的东西，而是nacos注册中心的
    public R membercoupons();//得到一个R对象
}
~~~

然后我们在member的控制层写一个测试请求

~~~
@RestController
@RequestMapping("member/member")
public class MemberController {
    @Autowired
    private MemberService memberService;

    @Autowired
    CouponFeignService couponFeignService;

    @RequestMapping("/coupons")
    public R test(){
        MemberEntity memberEntity = new MemberEntity();
        memberEntity.setNickname("会员昵称张三");
        R membercoupons = couponFeignService.membercoupons();//假设张三去数据库查了后返回了张三的优惠券信息

        //打印会员和优惠券信息
        return R.ok().put("member",memberEntity).put("coupons",membercoupons.get("coupons"));
    }

~~~

重新启动服务

http://localhost:8000/member/member/coupons
~~~
{
    "msg":"success",
    "code":0,
    "coupons":[
        {"id":null,"couponType":null,"couponImg":null,"couponName":"满100-10","num":null,"amount":null,"perLimit":null,"minPoint":null,"startTime":null,"endTime":null,"useType":null,"note":null,"publishCount":null,"useCount":null,"receiveCount":null,"enableStartTime":null,"enableEndTime":null,"code":null,"memberLevel":null,"publish":null}
    ],
    "member":{"id":null,"levelId":null,"username":null,"password":null,"nickname":"会员昵称张三","mobile":null,"email":null,"header":null,"gender":null,"birth":null,"city":null,"job":null,"sign":null,"sourceType":null,"integration":null,"growth":null,"status":null,"createTime":null}
}

~~~

上面讲的内容很重要，我们停留5分钟体会一下。

coupon里的R.ok()是什么，就是设置了个msg

~~~
public class R extends HashMap<String, Object> {//R继承了HashMap
    // ok是个静态方法，new了一个R对象，并且
    public static R ok(String msg) {
        R r = new R();
        r.put("msg", msg);//调用了super.put(key, value);，即hashmap的put
        return r;
    }
}

~~~














