多线程模型 :
不是指连接即 accept的时候阻塞, 
而是面对n个客户端时是要每个客户端维护一个保存这个连接socket的线程.(为什么一定要起线程呢?以为socket.read()是阻塞的,是必须用多线程的)

nio模型:(多路复用,非阻塞io,缓存,channel)
而nio的非阻塞,也与select时的阻塞不矛盾.主要是io的操作(read和write)是非阻塞的.
并且因为有了缓存的存在,read和write的效率高,不需阻塞.
不阻塞的好处就是不必须每个连接(channel)单独维护一个线程,只需一个线程就可以实现所有连接的维护,(当然也可以起进程进行读写业务).

尽管这样,nio虽是非阻塞io,但消息机制上还是属于同步的.(主动处理消息);

关于selectionKey
SelectionKey
A token representing the registration of a SelectableChannel with a Selector.

A selection key is created each time a channel is registered with a selector. A key remains valid until it is cancelled by invoking its cancel method, by closing its channel, or by closing its selector . Cancelling a key does not immediately remove it from its selector; it is instead added to the selector's cancelled-key set for removal during the next selection operation. The validity of a key may be tested by invoking its isValid method.

A selection key contains two operation sets represented as integer values. Each bit of an operation set denotes a category of selectable operations that are supported by the key 's channel .

The interest set determines which operation categories will be tested for readiness the next time one of the selector's selection methods is invoked. The interest set is initialized with the value given when the key is created; it may later be changed via the interestOps(int) method.

The ready set identifies the operation categories for which the key's channel has been detected to be ready by the key's selector. The ready set is initialized to zero when the key is created; it may later be updated by the selector during a selection operation, but it cannot be updated directly.

That a selection key's ready set indicates that its channel is ready for some operation category is a hint, but not a guarantee, that an operation in such a category may be performed by a thread without causing the thread to block. A ready set is most likely to be accurate immediately after the completion of a selection operation. It is likely to be made inaccurate by external events and by I/O operations that are invoked upon the corresponding channel.

This class defines all known operation-set bits, but precisely which bits are supported by a given channel depends upon the type of the channel. Each subclass of SelectableChannel defines an validOps() method which returns a set identifying just those operations that are supported by the channel. An attempt to set or test an operation-set bit that is not supported by a key's channel will result in an appropriate run-time exception.

It is often necessary to associate some application-specific data with a selection key, for example an object that represents the state of a higher-level protocol and handles readiness notifications in order to implement that protocol. Selection keys therefore support the attachment of a single arbitrary object to a key. An object can be attached via the attach method and then later retrieved via the attachment method.

Selection keys are safe for use by multiple concurrent threads. The operations of reading and writing the interest set will, in general, be synchronized with certain operations of the selector. Exactly how this synchronization is performed is implementation-dependent: In a naive implementation, reading or writing the interest set may block indefinitely if a selection operation is already in progress; in a high-performance implementation, reading or writing the interest set may block briefly, if at all. In any case, a selection operation will always use the interest-set value that was current at the moment that the operation began.



在 JDK 1.4 中原来的 I/O 包和 NIO 已经很好地集成了。 java.io.* 已经以 NIO 为基础重新实现了，所以现在它可以利用 NIO 的一些特性。例如， java.io.* 包中的一些类包含以块的形式读写数据的方法，这使得即使在更面向流的系统中，处理速度也会更快。

可以看到，1.4后的IO经过了集成。所以NIO的好处，集中在其他特性上，而非速度了： 

1、分散与聚集读取 
2、文件锁定功能 
3、网络异步IO
