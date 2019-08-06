版本进化史，3.0后支持集群

**2.2**

1. redis-cli命令非常大的改进: Tab补全，支持help(例如help SET、help BITCOUNT)，原生输出，并用最新的hiredis C库对其进行重写。
2. copy-on-write机制对很多读命令支持更友好，这就意味着，如果大部分是读操作的话，fork出来的子进程只需要很少的内存即可。
3. 新命令WATCH，实现CAS事务。
4. 可以对具有EXPIRE失效属性的KEY进行写操作。
5. 全新的SETBIT、GETBIT、SETRANGE、GETRANGE、STRLEN命令，操作string就像一个数组一样，我们可以进行位操作。
6. 支持syslog(在配置文件中指定：syslog-enabled yes)。
7. List新增了很多命令：LINSERT，LPUSHX，RPUSHX，BRPOPLPUSH 。
8. 更强大的INFO命令。
9. 启动时加载RDB或者AOF是无阻塞的，并且在INFO输出有进度信息。
10. hiredis：强大的C语言Redis客户端库。
11. 支持在配置文件中rename重命名(rename-command KEYS afeikeys)者禁用(rename-command KEYS "")命令。

**syslog说明：**

在Linux的世界里，有两种写日志的方式：

\1. 就是把日志写到文件中，随着时间的推移，我们会写到一个新的文件中。这种方式的问题就是，如果有很多的服务产生很多的日志，那么就比较麻烦了。

2.利用syslog服务记录日志，在几乎所有可用的Unix和Linux服务器的TCP和UDP端口514上运行一个名为syslog的服务，syslog接受任何向其发送的日志消息并将这些消息路由到各种消息的磁盘上的日志文件，同时处理旧日志的滚动和删除。通过配置，它甚至可以将消息转发到其他服务器以进行进一步处理，它是远比直接记录到文件更方便，因为所有的特殊日志文件的滚动和删除已经帮我们做好了。

**2.4**

Redis2.4没有值得大书特书的特性，大部分是一些bug fix或者特性增强，这个版本的新特性一览：

1. VM deprecated，还会支持不过会给出告警，不建议使用。
2. 新增CLIENT命令，支持KILL，LIST等操作，从而能够观察CLIENT的一些信息;
3. 新增OBJECT命令，从而能够观察对象的一些内在信息，比如对象是什么编码;
4. 通过直接通过CONFIG SET在不重启Redis服务的情况下关闭密码授权;
5. 如果没有设置密码，而又通过AUTH发送密码进行鉴权，那么返回ERROR。
6. redis-cli新增实现了--latency模式，以便能监控采样Redis延迟。
7. 当fsync刷新策略是everysec的时候，AOF fsync在后台执行。
8. SADD, HDEL, SREM, ZREM, ZADD, L/RPUSH命令增强，增加对可变参数的支持(老版本只支持一个参数)。

**2.6**

Redis2.6新特性如下：

1. 服务侧的Lua脚本支持，通过EVAL执行，例如：eval "return redis.call('set','foo','bar')" 0。
2. 失效时间支持毫秒精度，并且相应的增加了毫秒级失效的命令：PEXPIRE、PSETEX和PTTL等。
3. 支持只读的Slave。
4. 支持位操作命令：BITCOUNT和BITOP，还有很多其他新增的命令，例如：DUMP, RESTORE, MIGRATE等。
5. RDB文件中新增CRC64校验值。
6. SHUTDOWN支持两个选项：SAVE和NOSAVE。
7. INFO命令输出按段分割。
8. watchdog特性定位延迟问题，只需执行config set watchdog-period 100就能开启该特性，如果值是0就表示关闭该特性。需要说明的是，该特性不能在配置文件中开启。
9. 集成内存测试模式，参考执行：redis-server --test-memory 1024。
10. 所有redis.conf中的配置都可以通过redis-server的命令行选项指定。
11. hash table的seed随机化，以防止碰撞攻击。
12. 去掉虚拟内存你功能(2.4已经deprecated)。
13. 去掉对客户端连接数硬编码限制。

watchdog可以参考：https://redis.io/topics/latency，笔者接下来也会单独撰文进行深入剖析。

**2.8**

Redis2.8虽然没有非常重磅的新特性，但是它非常非常稳定，除了没有分布式特性，我挑不出太大的毛病，高可用可以由sentinel保证。笔者当年经历过的一个项目，用Redis2.8.23这个版本，单机模式，10G+的数据量，居然稳定的运行了500天左右。

即使到现在，如果你的项目不需要几十个G的缓存，我依然认为Redis2.8.x这个版本是不错的选择，结合sentinel模式保证高可用，而且相比Redis3.x的分布式集群模式，它的架构更简单，而且不需要妥协任何功能和命令(Redis cluster模式下，很多命令功能阉割，或者受到限制。当然，Redis3.x以后的高版本，也可以只用sentinel，可以不用redis cluster架构)。

OK，还是来看一下2.8新特性吧：

1. slave支持从master部分同步，这样就能避免主从断开一会儿时间(例如网络抖动)后需要通过RDB全量同步。
2. 支持IPv6。
3. 支持config set maxclients 1024。
4. 支持绑定多个IP地址。
5. CONFIG REWRITE能够将通过CONFIG SET操作的配置输出在redis.conf配置文件中。
6. 新增PUBSUB命令，即为了实现Pub/Sub的自省能力。
7. 通过Pub/Sub实现keyspaces的变化通知，例如实现过期KEY的通知功能。
8. 如果在最大延迟下没有足够数量从，主可以停止写请求。配置参考如下：

```
# 例如，至少3个延迟在10秒以内的slave，master才会正常写入： 
# min-slaves-to-write 3 
# min-slaves-max-lag 10
```

**3.0**

Redis3.0绝对是一个里程碑版本，耗时4年才开发完成。在Redis3.0之前，Redis是没有分布式模式的(sentinel模式只能保证高可用)。正因为这样，导致民间出现了很多Redis分布式方案，比较有名气的3种是：1. Redis客户端分片，2. twemproxy，3. codis(豌豆荚)。

Redis3.0的重磅功能有：

1. Redis Cluster，即Redis分布式模式，Redis3.0最重磅的特性;
2. WAIT命令阻塞，直到写命令被同步到指定数量的slave上，命令始终返回写命令发送的slaves的数量。用法：WAIT numslaves timeout。例如：WAIT 2 5000，即等待2个slave同步所有写命令并设置超时5秒钟。
3. 新增CLIENT PAUSE命令，例如：CLIENT PAUSE 10000，即停止处理客户端命令10秒钟，这时候如果有新的命令执行，会一直阻塞。
4. CONFIG SET命令增强，支持不同的单位，例如CONFIG SET maxmemory 1gb、CONFIG SET maxmemory 1024mb;
5. MIGRATE命令增强，增加了COPY和REPLACE操作。
6. Redis日志做了小小的调整，日志中会反应当前实例的角色，很明显是为了Redis Cluster。

**3.2**

Redis3.2版本的一些重要特性如下：

1. Lua debugger，官方给出了参考视频，不过是在Youtube上：https://www.youtube.com/watch?v=IMvRfStaoyM，能不能看的了就靠自己了。
2. CONFIG SET/GET实现重写，支持更多的操作项。
3. 新增HSTRLEN命令，返回hash结构指定field的value的字符串长度，如果hash或者field不存在，那么返回0。
4. 通过RESIZEDB避免不必要的hash扩容，实现更快的RDB加载速度。
5. 新增对GEO的支持，并提供若干个操作GEO的命令，例如：GEOADD, GEORADIUS，GEOHASH，GEOPOS等。
6. 实现CLIENT REPLY，并支持三种操作：ON、OFF、SKIP。ON就是正常模式，服务端会回复请求。OFF模式下，服务端不会对任意命令回复。这种情况一般用于发送-忘记的场景下，例如：大量缓存数据加载，利用OFF模式可以关闭响应，节省带宽等资源。
7. SPOP命令支持count参数，使用参考：SPOP key [count]。即删除并返回count个元素。
8. List类型新的编码方式：Quicklists。在RDB文件中能节省很多内存和存储空间。
9. 极力向大家推荐推荐一个Java架构方面的交流学习qq群：834962734，里面有：Spring，MyBatis，Netty源码分析，高并发、高性能、分布式、微服务架构的原理，JVM性能优化这些成为架构师必备的知识体系，进群马上免费领取学习资源！真的不错。

**4.0**

Redis官方给出没有命名为3.4而是4.0的原因是：这个版本添加了很多重要的特性，而且很多核心功能被重写了。让我们看一下Redis4.0主要的功能有哪些：

1. Redis模块系统，这个功能的实现，开发者可以为Redis自定义扩展，实现新的数据结构等任意特性。
2. PSYNC版本升级到v2版本，优化了之前版本中，主从节点切换必然引起全量复制的问题。
3. 缓存驱逐策略增强，新增了LFU策略，即Least Frequently Used。
4. 异步删除(延迟回收)。Redis4.0可以在后台异步删除KEY，而不需要同步阻塞服务端，通过新增的命令UNLINK实现，用法：UNLINK key [key …]。
5. 混合RDB-AOF格式，这种格式被用在重写AOF文件的时候，重写用更紧凑更快速的方式生成RDB文件，AOF流追加到这个文件上，这样的话，AOF持久化重写和重新加载的速度更快，参考配置：aof-use-rdb-preamble yes。
6. 增加一个MEMORY命令，能否做很多不同的内存分析工作，例如内存问题排查、查看一个KEY用了多少内存、提供与INFO命令内容相比更加深入地报告Redis内存使用情况;
7. Redis可以整理内存碎片化了，能在运行的过程中回收内存空间，参考配置：activedefrag yes，还有很多对应的配置参数，可以在Redis4.0以后的版本的redis.conf中查看。
8. Redis集群支持NAT/Docker。

memory命令使用参考--查看一个KEY用了多少内存(单位是字节)，相信很多同学不知道这个神奇的命令吧，那么，现在就GET它吧：

```
> SET foo bar 
OK 
> MEMORY USAGE foo 
(integer) 54
```

**5.0**

这是Redis最新的一个主要版本，笔者之前有篇文章对Redis5.0有深入分析，感兴趣的朋友请戳：。5.0的重要特性主要有：

1. Streams数据结构，Redis一个全新的数据结构，借鉴了很多kafka的设计(这个数据结构笔者之前也有文章介绍，详情请戳：)。
2. Redis集群管理脚本从Ruby(redis-trib.rb)迁移到存C，且集成到redis-cli中(redis-cli --cluster)。
3. Sorted Set新增两个重要的命令：ZPOPMIN和ZPOPMAX。
4. 支持HZ动态化;
5. 不再使用slave这个词，除非为了API向后兼容(种族歧视问题)。
6. Lua脚本可以设置超时。
7. CLIENT支持两个新命令：CLIENT ID和CLIENT UNBLOCK。