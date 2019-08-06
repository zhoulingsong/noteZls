## Maven

### 设置 Maven 远程仓库

​	Maven 默认仓库在国外，国内使用会很慢（100倍），可使用阿里云仓库或公司自建私有仓库；

##### 阿里云仓库配置分二步：

1. 修改 maven 根目录下 conf/setting.xml 如下：

   ```xml
     <mirror>
         <id>alimaven</id>
         <mirrorOf>central</mirrorOf>
         <name>aliyun maven</name>
         <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
     </mirror>
   ```

   

2. 修改项目中 pom.xml 文件

   ```xml
   	<repositories>
           <repository>
               <id>alimaven</id>
               <name>aliyun maven</name>
               <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
               <releases>
                   <enabled>true</enabled>
               </releases>
               <snapshots>
                   <enabled>false</enabled>
               </snapshots>
           </repository>
       </repositories>
   ```

   ​	





### 3



