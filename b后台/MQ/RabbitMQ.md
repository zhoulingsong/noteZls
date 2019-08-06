## 启动与停止

~~~shell
如果配置好了环境变量：
sudo ./rabbitmq-server
没配置则进入安装目录
cd /usr/local/Cellar/rabbitmq/3.7.12/sbin
./rabbitmq-server

启动成功，浏览器输入 localhost:15672 ,账号密码 guest 即可登录。

3.3.4.启动、停止
service rabbitmq-server start
service rabbitmq-server stop
service rabbitmq-server restart
3.3.5.设置开机启动
chkconfig rabbitmq-server on
3.3.6.设置配置文件
cd /etc/rabbitmq
cp /usr/share/doc/rabbitmq-server-3.4.1/rabbitmq.config.example /etc/rabbitmq/

mv rabbitmq.config.example rabbitmq.config
~~~



## 组件

#### 交换机(Exchanges)

~~~ 
生产者将消息发送给交换机(Exchanges)，交换机再决定将消息发送给哪些消息队列(queue)。
四种类型: direct, topic, headers, fanout.(直接, 主题, 标题, 广播)
1. direct 
2. topic 
	routingKey 由点分割的单词列表。eg: quick.orange.rabbit
	"*"（星号）可以替代一个单词。
	"＃"（hash）可以替换零个或多个单词。
	若仅绑定 "#" 则相当于 fanout。
	若无符号则相当于 direct。
3. headers
4. fanout



~~~

#### 消息队列(Queue)

~~~ 
绑定(Binding): 将消息队列与交换机绑定。
channel.queueBind(queueName, exchangeName, "");
~~~

~~~ 
routing key: 生产者发送消息到交换机时使用。
binding key: 消费者从消息队列接收消息时使用。
仅当两者匹配是才能成功接收消息。
~~~



### 死信队列

通过设置消息的过期时间再配合死信队列，实现延迟任务。