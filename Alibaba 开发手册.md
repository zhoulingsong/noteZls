## Alibaba 手册总结

### 并发

~~~
1. 使用 ThreadPoolExecutor 创建线程池，非 Executors。
2. 使用 ScheduledExecutorService 定时任务，非 Timer。
3. 共享 Random 实例使用 ThreadLocalRandom，非 Random、Math.random()。
4. LongAdder 效率高于 AtomicLong。
5. 高并发使用不等号作为中断、退出判断，非 等号，防止条件击穿。

~~~



### 异常

~~~
1. finally 中不适用 return。
2. Optional 防 NPE。
3. 异常抛出来就不用记录日志。
~~~



### 数据库

~~~
1. 单表超过 500W 再考虑分库分表。
2. 超过 3 个表禁止使用 join。关联字段必须有索引。
3. 删除、更新前先查询，避免误操作。
4. 避免使用 in，必要时控制 in 集合在 1000 没。
~~~

