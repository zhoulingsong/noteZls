### ByteBuf读写指针

- 在ByteBuffer中，读写指针都是position，而在ByteBuf中，读写指针分别为readerIndex和writerIndex，直观看上去ByteBuffer仅用了一个指针就实现了两个指针的功能，节省了变量，但是当对于ByteBuffer的读写状态切换的时候必须要调用flip方法，而当下一次写之前，必须要将Buffe中的内容读完，再调用clear方法。每次读之前调用flip，写之前调用clear，这样无疑给开发带来了繁琐的步骤，而且内容没有读完是不能写的，这样非常不灵活。相比之下我们看看ByteBuf，读的时候仅仅依赖readerIndex指针，写的时候仅仅依赖writerIndex指针，不需每次读写之前调用对应的方法，而且没有必须一次读完的限制。



### 零拷贝

> **普通拷贝：**

1. 数据从磁盘读取到内核的read buffer。

2. 数据从内核缓冲区拷贝到用户缓冲区。

3. 数据从用户缓冲区拷贝到内核的socket buffer。

4. 数据从内核的socket buffer拷贝到网卡接口（硬件）的缓冲区。

> **零拷贝：**不需要CPU参与。

1. 调用transferTo,数据从文件由DMA引擎拷贝到内核read buffer。

2. 接着DMA从内核read buffer将数据拷贝到网卡接口buffer。

- Netty的接收和发送ByteBuffer采用DIRECT BUFFERS，使用堆外直接内存进行Socket读写，不需要进行字节缓冲区的二次拷贝。如果使用传统的堆内存（HEAP BUFFERS）进行Socket读写，JVM会将堆内存Buffer拷贝一份到直接内存中，然后才写入Socket中。相比于堆外直接内存，消息在发送过程中多了一次缓冲区的内存拷贝。
- Netty提供了组合Buffer对象，可以聚合多个ByteBuffer对象，用户可以像操作一个Buffer那样方便的对组合Buffer进行操作，避免了传统通过内存拷贝的方式将几个小Buffer合并成一个大的Buffer。
- Netty的文件传输采用了transferTo方法，它可以直接将文件缓冲区的数据发送到目标Channel，避免了传统通过循环write方式导致的内存拷贝问题。



### 引用计数与池化技术

- 在Netty中，每个被申请的Buffer对于Netty来说都可能是很宝贵的资源，因此为了获得对于内存的申请与回收更多的控制权，Netty自己根据引用计数法去实现了内存的管理。Netty对于Buffer的使用都是基于直接内存（DirectBuffer）实现的，大大提高I/O操作的效率，然而DirectBuffer和HeapBuffer相比之下除了I/O操作效率高之外还有一个天生的缺点，即对于DirectBuffer的申请相比HeapBuffer效率更低，因此Netty结合引用计数实现了PolledBuffer，即池化的用法，当引用计数等于0的时候，Netty将Buffer回收致池中，在下一次申请Buffer的没某个时刻会被复用。



## TCP 粘包/拆包

~~~
提供了4种拆包器：将拆包器添加到ChannelPipeline中。
eg：ch.pipeline().addLast(new FixedLengthFrameDecoder(31));
~~~

1. 固定长度：FixedLengthFrameDecoder
2. 行：LineBasedFrameDecoder
3. 分隔符：DelimiterBasedFrameDecoderder

4. 长度域：LengthFieldBasedFrameDecoder