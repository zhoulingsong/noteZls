## Spring MVC 

### @ResponseBody 响应中文乱码

**问题：**通过 get 请求服务端返回 String 类型的服务时，会出现中文乱码问题??

**原因：**
@ResponseBody 注解的返回值，是由一系列 HttpMessageConverter 类来处理的， 默认对 String 类型采用的是 StringHttpMessageConverter 类来处理。这个类的默认字符编码为 iso-8859-1 源码如下。

public static final Charset DEFAULT_CHARSET = Charset.forName("ISO-8859-1");

增强：如果是非 String 则不会有乱码，如 list 则使用 MappingJacksonHttpMessageConverter；

**解决办法：**

方案一：单次有效（不推荐）
对于需要返回字符串的方法添加注解。

~~~ java
@ResponseBody
@RequestMapping(value = "/getUsers", produces = "application/json; charset=utf-8")
public String getAllUser() {
　　return "张三";
}
~~~



方案二：

在spring-servlet.xml中加入：

```xml
<mvc:annotation-driven>
	<mvc:message-converters register-defaults="true">
		<bean class="org.springframework.http.converter.StringHttpMessageConverter">
			<constructor-arg value="UTF-8" />
		</bean>
	</mvc:message-converters>
</mvc:annotation-driven>
```





### @JsonProperty

~~~
jackson的注解
能处理属性首字母大写问题。
需要引入jar
~~~





## 权限控制

1. 拦截器

   ~~~ 
   拦截器是在执行 controller 之前执行。
   ~~~

   

2. 自定义注解，实现登录态校验和权限校验。

