## Java NIO

### NIO与IO的区别

IO是面向流的，NIO是面向缓冲区的。Java IO面向流意味着每次从流中读一个或多个字节，直至读取所有字节，他们没有被缓存在任何地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓存到一个缓冲区。NIO的缓冲导向方法略有不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程的灵活性。但是，还需要检查是否该缓冲区中包含所有需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的数据。

IO的各种流逝阻塞的。当一个线程调用read()或write()时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。该线程在期间不能干任何事。NIO的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取。而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。线程通常将非阻塞IO的空闲时间用于在其他管道上执行IO操作，所以一个单独的线程现在可以管理多饿输入和输出通道。

### Channel

Channel 和 Stream差不多同一等级。Stream是单向的：InputStream, OutputStream。而Channel是双向的，读写都行。

主要实现：

+ FileChannel
+ DatagramChannel
+ SocketChannel
+ ServerSocketChannel

### Buffer

实现：

+ ByteBuffer
+ CharBuffer
+ DoubleBuffer
+ FloatBuffer
+ IntBuffer
+ LongBuffer
+ ShortBuffer

### Selector

Selector运行单线程处理多个Channel，如果应用打开多个通道，但每个连接的流量都很低，使用Selector就很方便。如在一个聊天服务器中，要使用Selector，得想Selector注册Channel，然后调用它的select()方法。这个方法会一直阻塞到某个注册的通道有事件就绪。一旦这个方法返回，线程就可以处理这些事件，事件的例子有如显得连接进来、数据接收等。