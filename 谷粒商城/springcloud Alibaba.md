## 依赖

~~~
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.0.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

~~~
上面是依赖管理，相当于以后再dependencies里引spring cloud alibaba就不用写版本号， 全用dependencyManagement进行管理。注意他和普通依赖的区别，他只是备注一下，并没有加入依赖


