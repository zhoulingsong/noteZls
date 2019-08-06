## [mac下tomcat的安装与配置](https://www.cnblogs.com/freeyiyi1993/p/3436368.html)

##### 1、到 [apache官方主页](http://tomcat.apache.org/) 下载 Mac 版本的完整 tar.gz文件包。解压拷贝到 /Library目录下,并命名为Tomcat，其他目录也可。

2、修改目录权限 

​	到终端输入 sudo chmod 755 /Library/Tomcat/bin/*.sh 

​	(方便其他用户使用 tomcat)

3、启动Tomcat

按回车键之后会提示输入密码，请输入管理员密码。之后输入并回车： 
sudo sh startup.sh 
若出现如下提示则表示安装并运行成功： 
Using CATALINA_BASE: /Library/Tomcat 
Using CATALINA_HOME: /Library/Tomcat 
Using CATALINA_TMPDIR: /Library/Tomcat/temp 
Using JRE_HOME: /System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home 

4、 打开浏览器，输入 http://localhost:8080/ 
回车之后如果看到Apache Tomcat，表示已经成功运行Tomcat 

5、配置**Tomcat**启动脚本：

使用文本编辑器添加以下代码：

\#!/bin/bash

case $1 in
start)
sh /Library/Tomcat/bin/startup.sh
;;
stop)
sh /Library/Tomcat/bin/shutdown.sh
;;
restart)
sh /Library/Tomcat/bin/shutdown.sh
sh /Library/Tomcat/bin/startup.sh
;;
*)
echo “Usage: start|stop|restart”
;;
esac

exit 0

将文件保存为tomcat，小写并不带后缀。赋予文件执行权限：

chmod 777 tomcat

。将这个文件放置到终端包含的路径中，例如/usr/bin，而后便可以在终端中简单地输入tomcat start和tomcat stop启用tomcat了。

快捷命令如下：

1）tomcat start 

2)  tomcat stop

3)  tomcat restart 





## 支持 SSL

```xml
1、生成KeyStore
	1)运行-->CMD-->"keytool -genkey -alias Jadyer_SSL_20120508 -keyalg RSA -validity 1024 -keystore D:\Jadyer_SSL_20120508.keystore"
	参数说明:
	-genkey  表示生成密钥
	-alias    指定别名,这里是Jadyer_SSL_20120508
	-keyalg   指定算法,这里是RSA
	-validity 指定证书有效期,这里是1024天
	-keystore 指定存储位置,这里是D:\\Jadyer_SSL_20120508.keystore
	
	2)CMD输出----->输入keystore密码：hongyu75
 	再次输入新密码：hongyu75
	您的名字与姓氏是什么？[Unknown]：127.0.0.1（这里要根据实际情况填写网站域名或者IP,否则会出现证书上的名称无效）
	您的组织单位名称是什么？[Unknown]：http://blog.csdn.net/jadyer
 	您的组织名称是什么？[Unknown]：JavaLover_jadyer
	您所在的城市或区域名称是什么？[Unknown]：BJ
	您所在的州或省份名称是什么？[Unknown]：BJ_NanTian
 	该单位的两字母国家代码是什么[Unknown]：CN
 	CN=127.0.0.1, OU=http://blog.csdn.net/jadyer, O=JavaLover_jadyer, L=BJ, ST=BJ_NanTian, C=CN 正确吗？[否]：Y
	输入<Jadyer_SSL_20120508>的主密码（如果和 keystore 密码相同，按回车）：这里按回车键
	这里的主密码一定要与keystore密码相同,否则启动Tomcat时就会告诉你java.io.IOException: Cannot recover key）

        3)接下来就会按照-keystore参数值在指定位置生成指定的KeyStore文件了
===================================================================================================================================
 50  * @see 2、让Tomcat支持SSL
 51  * @see    1)将生成的Jadyer_SSL_20120508.keystore拷贝到\\%TOMCAT_HOME%\\conf\\目录中(其它目录也可以)
 52  * @see    2)修改\\%TOMCAT_HOME%\\conf\\server.xml文件(大约在85行的位置),新增内容如下
 53  * @see      <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
 54  * @see                 maxThreads="150" scheme="https" secure="true"
 55  * @see                 clientAuth="false" sslProtocol="TLS" URIEncoding="UTF-8"
 56  * @see                 keystoreFile="conf/Jadyer_SSL_20120508.keystore" keystorePass="hongyu75"/>
 57  * @see    3)这样,我们的Tomcat就支持HTTPS访问了(关于<Connector/>标签中的属性说明,参拜Google大神)
 58  * @see ===================================================================================================================================
 59  * @see 3、用浏览器访问我们的应用
 60  * @see   1)输入https://127.0.0.1:8443/blog会发现你的应用已经处于SSL安全通道中了
 61  * @see     此时,如果我们在浏览器里访问http://127.0.0.1:8443/blog会发现,竟然能访问
 62  * @see     也就是说,我们虽然启用了HTTPS,但现在还可以绕开HTTPS直接访问HTTP还能,这样HTTPS也就起不到作用了
 63  * @see   2)我们可以配置一下\\%TOMCAT_HOME%\\conf\\web.xml文件,使得HTTP的访问能够重定向到HTTPS的连接
 64  * @see     修改位置大约为web.xml的1224行,即在</welcome-file-list>标签后面加入下面的内容,即可
 65  * @see     <security-constraint>
 66  * @see         <!-- Authorization setting for SSL -->
 67  * @see         <web-resource-collection>
 68  * @see             <web-resource-name>SSL_App</web-resource-name>
 69  * @see             <!-- 指明需要SSL的url -->
 70  * @see             <url-pattern>/*</url-pattern>
 71  * @see             <http-method>GET</http-method>
 72  * @see             <http-method>POST</http-method>
 73  * @see         </web-resource-collection>
 74  * @see         <user-data-constraint>
 75  * @see             <!-- 指明需要SSL -->
 76  * @see             <transport-guarantee>CONFIDENTIAL</transport-guarantee>
 77  * @see         </user-data-constraint>
 78  * @see     </security-constraint>
 79  * @see ===================================================================================================================================
 80  * @author http://blog.csdn.net/jadyer
```