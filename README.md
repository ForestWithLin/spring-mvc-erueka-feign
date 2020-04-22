# 简要说明
非Springboot框架集成 eureka 和 feign，此项目作为一个工具JAR包使用，可以通过maven方式引入

# 参考来源
[spring-mvc-starter-feign](https://github.com/r2ys/spring-mvc-starter-feign)，PS：这项目没有说明，调了很久

# 配置步骤
1. 引用项目通过maven引入当前项目，注意需要排除相关冲突的依赖包

```
<dependency>
    <groupId>com.wandaph</groupId>
    <artifactId>spring-mvc-starter-feign</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```
2. 在引用项目添加eureka-client.properties文件
用来配置eureka客户端配置信息，当然也可以参考[spring-mvc-starter-feign](https://github.com/r2ys/spring-mvc-starter-feign)把配置信息写死在代码里通过**ConfigurationManager.loadProperties**加载配置信息
配置文件例子可参考：[sample-eureka](https://github.com/Netflix/eureka/tree/master/eureka-examples/conf)
3. 在引用项目添加自定义配置文件
把项目下的**FeignClientsRegistrar**类注入到Spring bean里，如果有配置Feign远程服务调用，也可以通过scanBasePackage字段配置相应的路径
```
<!-- 调用远程服务 scanBasePackage-扫描feign路径 -->
<bean class="com.wandaph.openfeign.FeignClientsRegistrar">
	<property name="scanBasePackage" value="com.xxx.demo.feign" />
</bean>
```
eureka的注册类**EurekaRegistry**已经配置到**FeignClientsRegistrar**类里，不需要再注入。

# 注意事项

1. 看来SpringCloud集成Eureka的源码，如果想要通过Feign调用当前服务，需要同时配置Eureka的三个参数

```
# 举例，当前服务 http://demo/xxx，那么当前服务对应的三个参数配置如下
eureka.name=demo
eureka.vipAddress=${eureka.name}
eureka.secureVipAddress=${eureka.name}
```

2. 注意Eureka客户端和服务端的JAR包版本要一致