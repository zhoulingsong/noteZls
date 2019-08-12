## ChannelHandler 的添加顺序和执行顺序

~~~
涉及到编码与解码问题。
~~~

* **从 channel 读取数据**

  需要解码数据



> ChannelHandler
> > ChannelInboundHandler：处理接收数据
> > ChannelOutboundHandler：处理发送数据



~~~
ChannelPipeline 拥有一组 Handler（ChannelInboundHandler和ChannelOutboundHandler）
数据流进入：从 ChannelPipeline 的头部开始传给第一个ChannelInboundHandler，然后依次传递到管道的尾部。即按注册顺序依次执行。
数据被写出：从 ChannelPipeline 的尾部开始传给“最后”一个ChannelOutboundHandler，然后依次传递给前一个ChannelOutboundHandler。即按注册顺序逆序执行。(需要看 write 的位置)
~~~





## ctx.writeAndFlush与ctx.channel().writeAndFlush
https://blog.csdn.net/FishSeeker/article/details/78447684
https://www.cnblogs.com/ruber/p/10186571.html

ctx：ChannelHandlerContext 的缩写。
ctx.writeAndFlush()：直接从当前 handler 往回发出这个消息。
ctx.channel().writeAndFlush()：先把消息发送到整个pipline最后一个handler，再往回发出消息。

注意：当 channel 本来就是最后一个时，通过 ctx.channel().writeAndFlush() 则行程一个死循环，消息永远都无法发送出去。



## ByteBuf



