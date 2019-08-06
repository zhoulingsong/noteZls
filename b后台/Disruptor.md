## Disruptor-并发框架

### 第一章：为什么这么快

~~~
1. 使用 CAS 无锁模式工作，摒弃锁的缺点。
2. 补齐缓存行，解决伪共享。(解决主流的 CPU： 64B = 8 * long，一般为 32-256B)
3. 内存屏障，使用 volatile 来保证。
~~~

### RingBuffer

~~~
数组实现的环形数据结构。
1. 读数据：ConsumerBarrier
2. 写数据：ProducerBarrier
	nextEntry() 申请下一个节点，
	
两阶段提交
size 一般设置为实际使用的 2 倍。必须是 2 的 N 次方。
~~~

### 组件说明







TODO







- `Sequence`: Disruptor 使用 Sequence 来表示一个特殊组件处理的序号。和 Disruptor 一样，每个消费者(EventProcessor)都维持着一个 Sequence。大部分的并发代码依赖这些 Sequence 值的运转，因此 Sequence 支持多种当前为 AtomicLong 类的特性。
- `Producer`：由用户实现，它调用 RingBuffer 来插入事件(Event)。在 Disruptor 中没有相应的实现代码，由用户实现。
- `Event`：从生产者到消费者过程中所处理的数据单元。在 Disruptor 中没有相应的实现代码，由用户定义的。
- `EventHandler`：由用户实现并且代表了 Disruptor 中的一个消费者的接口。

从 Disruptor 框架如何处理 Event 的细节：

- `Sequencer`: 这是 Disruptor 真正的核心。实现了这个接口的两种生产者（单生产者和多生产者）均实现了所有的并发算法，为了在生产者和消费者之间进行准确快速的数据传递。通过复杂的算法去协调生存者和消费者之间的关系。
- `SequenceBarrier`: Sequecer具体的实施者，由 Sequencer 生成。它包含了决定是否有供消费者来消费的 Event 的逻辑。（生产者发布事件快于消费，生产者等待。消费速度大于生产者发布事件速度，消费者监听）
- `EventProcessor`：主要事件循环，处理 Disruptor 中的 Event，并且拥有消费者的 Sequence。它有一个实现类是 BatchEventProcessor，包含了event loop有效的实现，并且将回调到一个 EventHandler 接口的实现对象。
- `WorkProcessor`：确保每个 sequence 只被一个 processor 消费，在同一个 WorkPool 中的处理多个 WorkProcessor 不会消费同样的 sequence。
- `LifecycleAware`：当 BatchEventProcessor 启动和停止时，实现这个接口用于接收通知。
- 
- `WaitStrategy`：等待策略，当消费者等待在 SequenceBarrier上时得策略，不同的策略在于`响应延迟`与 `CPU 资源占用`上的取舍。
  - BusySpinWaitStrategy：自旋等待。低延迟、CPU 资源的占用高。
  - BlockingWaitStrategy：使用锁和条件变量。高延迟、CPU资源的占用低。
  - SleepingWaitStrategy：在多次循环尝试不成功后，让出 CPU，等待下次调度，多次调度仍不成功，睡眠一个纳秒级别的时间再尝试。这种策略平衡了延迟和CPU资源占用，但延迟不均匀。
  - YieldingWaitStrategy ： 在多次循环尝试不成功后，让出 CPU，等待下次调度。平衡了延迟和CPU资源占用，延迟也比较均匀。
  - PhasedBackoffWaitStrategy ： 上面多种策略的综合，延迟大、CPU资源的占用低。

 





### 使用步奏

1. **定义事件**：事件(Event)就是通过 Disruptor 进行交换的数据类型。
2. **定义事件工厂**：事件工厂(Event Factory)定义了如何实例化前面第1步中定义的事件(Event)，需要实现接口 com.lmax.disruptor.EventFactory<T>。Disruptor 通过 EventFactory 在 RingBuffer 中预创建 Event 的实例。一个 Event 实例实际上被用作一个“数据槽”，发布者发布前，先从 RingBuffer 获得一个 Event 的实例，然后往 Event 实例中填充数据，之后再发布到 RingBuffer 中，之后由 Consumer 获得该 Event 实例并从中读取数据。
3. **定义事件处理方式**：需实现接口 com.lmax.disruptor.EventHandler<T> 事件处理的具体实现。
4. **定义线程池**：用来处理事件，通过 java.util.concurrent.ExecutorService 提供的线程来触发 Consumer 的事件处理。Executors 工具来创建。
5. **指定等待策略**： 实现了 com.lmax.disruptor.WaitStrategy 接口的类，用于抽象 Consumer 如何等待新事件。
6. **启动 Disruptor**：start()。
7. **发布事件**：Disruptor 的事件发布过程是一个两阶段提交的过程：
    　　第一步：先从 RingBuffer 获取下一个可以写入的事件的序号；
        　　第二步：获取对应的事件对象，将数据写入事件对象；
        　　第三部：将事件提交到 RingBuffer;
    事件只有在提交之后才会通知 EventProcessor 进行处理。
8. **关闭 Disruptor**：shutdown()。

