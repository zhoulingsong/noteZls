~~~
实现引用计数的接口是 ReferenceCounted，一个实现了ReferenceCounted接口的类就具备了引用计数的能力。
当一个引用计数的对象被初始化时, 引用计数器被初始化为1。
两个主要方法：
	retain()方法对引用计数加1。
	release()方法对引用计数减1。
	引用计数被减到0，则对象将被显式的回收，若再访问则会抛出 IllegalReferenceCountException 异常。
~~~



**场景：**

​	ByteBuf 实现 ReferenceCounted，所以在使用的时候 write、read 可能会抛出异常。

**解决方案：**

* 局部变量：在方法内创建 ByteBuf 对象，这样每次使用都是一个新对象。
* 备份：每次使用时获取 ByteBuf 的一个拷贝，用 byteBuf.copy() 返回一个新的 ByteBuf 对象。