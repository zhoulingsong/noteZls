## Spring

spring 4.3 以后引入 @GetMapping @PostMapping 等注解；

spring 当 bean 为单例时能自动处理循环依赖，但是如果有增强（AOP：如 @Transactional）处理则不行。





### 自定义注解

~~~
实现 HandlerMethodArgumentResolver 接口。
用途：1. 实现 session 共享。
		 2. 权限控制。
~~~

### 拦截器

~~~
实现 HandlerInterceptor 接口，发生在 controller 之前，之后
用途：
	1. 实现 session 共享。
	2. 权限控制。

preHandle：方法执行前(有boolean返回值,返回true则放行)【Hanler执行之前】
postHandle：方法执行后【Handler执行之后,ModelAndView返回之前】
afterCompletion：页面渲染后【ModelAndView执行之后，对异常处理】
~~~



### AOP

~~~
用途：
 1. service 服务监控，耗时，日志等。
 2. controller 接口监控，耗时，入参日志等。(过滤器，只能拿到入参)
~~~



### Controller 参数获取

> 出入参：AOP。

> 入参：过滤器。
>
> 出参：实现 ModelAndViewResolver 接口。

~~~
一般做法：只处理有 @ResponseBody 注解的接口，out.write(data) 然后再打 log。
~~~



### bean 管理

   **Spring 父容器，SpringMVC 子容器，子容器可访问父容器，父容器不能访问子容器。**

1. Spring 管理所有 bean， SpringMVC 不管理。— ok
2. Spring 管理所有 bean， SpringMVC 仅管理 controller。— ok
3. Spring 管理除 controller 的其他 bean， SpringMVC 仅管理 controller。— ok

==啊啊==



### bean 的生命周期

- Bean的建立， 由BeanFactory读取Bean定义文件，并生成各个实例
- Setter注入，执行Bean的属性依赖注入
- BeanNameAware的setBeanName(), 如果实现该接口，则执行其setBeanName方法
- BeanFactoryAware的setBeanFactory()，如果实现该接口，则执行其setBeanFactory方法
- BeanPostProcessor的processBeforeInitialization()，如果有关联的processor，则在Bean初始化之前都会执行这个实例的processBeforeInitialization()方法
- InitializingBean的afterPropertiesSet()，如果实现了该接口，则执行其afterPropertiesSet()方法
- Bean定义文件中定义init-method
- BeanPostProcessors的processAfterInitialization()，如果有关联的processor，则在Bean初始化之前都会执行这个实例的processAfterInitialization()方法
- DisposableBean的destroy()，在容器关闭时，如果Bean类实现了该接口，则执行它的destroy()方法
- Bean定义文件中定义destroy-method，在容器关闭时，可以在Bean定义文件中使用“destory-method”定义的方法

如果使用ApplicationContext来维护一个Bean的生命周期，则基本上与上边的流程相同，只不过在执行BeanNameAware的setBeanName()后，若有Bean类实现了org.springframework.context.ApplicationContextAware接口，则执行其setApplicationContext()方法，然后再进行BeanPostProcessors的processBeforeInitialization()
实际上，ApplicationContext除了向BeanFactory那样维护容器外，还提供了更加丰富的框架功能，如Bean的消息，事件处理机制等。