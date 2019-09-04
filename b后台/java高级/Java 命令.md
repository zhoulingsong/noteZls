~~~ 
JDK 自带常用命令。
参考：https://www.jianshu.com/nb/11907385
~~~



+ ## Jstack

  ~~~
  查看进程、线程堆栈信息。
  ~~~

  的



+ ## jps

  ~~~
  jps 类似 linux 的 ps 命令，列出系统中所有的 Java 应用程序。支持远程调用。
  通过 jps 命令可以方便地查看 Java 进程的启动类、传入参数和 Java 虚拟机参数等信息。
  
  查看 java 进程：
  	1. ps -ef | grep java 来获取进程 ID。
    2. jps 命令来直接查看。
    
java程序在启动以后，会在java.io.tmpdir指定的目录下，就是临时文件夹里，生成一个类似于hsperfdata_User的文件夹，这个文件夹里（在Linux中为/tmp/hsperfdata_{userName}/），有几个文件，名字就是java进程的pid，因此列出当前运行的java进程，只是把这个目录里的文件名列一下而已。 至于系统的参数什么，就可以解析这几个文件获得。
  ~~~
  
  -q：只输出进程 ID
  -m：输出传入 main 方法的参数
  -l：输出完全的包名，应用主类名，jar的完全路径名
-v：输出jvm参数
  -V：输出通过flag文件传递到JVM中的参数
  
usage: jps [-help]
         jps [-q] [-mlvV] [<hostid>]
  
  Definitions:
      <hostid>:      <hostname>[:<port>]

```css
[hostid]：远程服务器地址，jps 支持远程调用
[protocol:][[//]hostname][:port][/servername]
```


+ ## jar

+ 

+ jconsole

+ ## jmap

  ~~~
  命令jmap是一个多功能的命令。它可以生成 java 程序的 dump 文件， 也可以查看堆内对象示例的统计信息、查看 ClassLoader 的信息以及 finalizer 队列。
  ~~~

  

+ jstat

