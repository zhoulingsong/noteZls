https://blog.csdn.net/caijunsen/article/details/83002219



## Location

| **标识符** | **描述**                                                     |
| ---------- | ------------------------------------------------------------ |
| =          | **精确匹配**；用于标准uri前，要求请求字符串和uri严格匹配。如果匹配成功，就停止匹配，立即执行该location里面的请求。 |
| ~          | **正则匹配**；用于正则uri前，表示uri里面包含正则，并且区分大小写。 |
| ~*         | **正则匹配**；用于正则uri前，表示uri里面包含正则，不区分大小写。 |
| ^~         | **非正则匹配**；用于标准uri前，nginx服务器匹配到前缀最多的uri后就结束，该模式匹配成功后，不会使用正则匹配。 |
| 无         | **普通匹配（最长字符匹配）**；**与location顺序无关**，是按照匹配的长短来取匹配结果。若完全匹配，就停止匹配。 |

~~~
首先匹配 =，其次匹配^~, 其次是按文件中顺序的正则匹配，最后是交给 / 通用匹配。
当有匹配成功时候，停止匹配，按当前匹配规则处理请求。
~~~



**简单通用配置：**静态动态分离，除了静态资源以外的请求都交给Tomcat处理.

~~~
location / {  
    proxy_pass http://localhost:8080
}  

location ^~ /static/ {  
    root /webroot/static/;  
}  

location ~* \.(html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css)$ {  
    root /webroot/res/;  
} 
~~~









## upstream 负载均衡

~~~
server：后端服务器
~~~

| 参数   | 描述              |
| ------ | ----------------- |
| server | 服务器地址+端口； |
|        |                   |
|        |                   |
|        |                   |
|        |                   |
|        |                   |
|        |                   |



* #### 状态

  * down：当前 server 不提供服务，即使所有server都不能用也不提供。
  * backup：预备服务器，所有可用 server 都不能提供服务时使用该 server。
  * max_fails：允许请求失败的次数。
  * fail_timeout：经过 max_fails 次失败后，服务暂停时间。
  * max_conns：限制最大的接收连接数。

  ~~~
  eg：server 127.0.0.1:8050 weight=1 max_fails=1 fail_timeout=20;
  ~~~

  

* #### 策略

  * 轮询：按顺序逐一分配到后端服务器。
  * 权重：weight 值越大，分配到的几率越高。
  * ip_hash：一致性哈希，根据 ip 的 hash 结果分配，同一个 ip 会被分配到相同 server。
  * url_hash：根据 url 的 hash 结果分配，同一个 url 会被分配到相同的 server。
  * least_conn：最少连接数，分配到连接最少的 server。




##  防盗链配置

什么是图片防盗链？意思是指本站内图片、CSS等资源只有本站点可以访问，不允许其它站点打开! 该功能如果用JAVA如何实现？很单简单，只要判断一下 请求头当中的referer 属性是否为 指定域名即可。 Nginx原理类似。

加入至指定location 即可实现

valid_referers none blocked *.luban.com;

 if ($invalid_referer) {

​	return 403;

}
演示防盗链配置：

说明：如果 valid_referers 条件判断未通过，nginx 则会赋值 invalid_referer 为true

语法: valid_referers none | blocked | server_names | string ...;

参数说明：

none 不允许 “Referer” 来源头部为空的情况

blocked 不允许“Referer”值为空情况，有可能Referer被的值被代理或者防火墙删除

server_names “Referer”来源头部包必须含当前的server_names （当前域名）可以多个 

（关于valid_referers比较详细的讲解：https://blog.csdn.net/ywb201314/article/details/51461053 ）

防盗链配置

location ~* \.(gif|png|jpg|swf|flv)$ {
    root html;
    valid_referers none blocked *.tl.com;
    if ($invalid_referer) {
        #rewrite ^/ http://www.tl.com/image/403.png;
​        return 404;#这里我们直接返回404
​    }
}