#1、什么是Netty？在什么场景使用？
### 答：Netty是由JBOSS提供的一个java开源框架。Netty是一个高性能、异步事件驱动的NIO框架，它提供了对TCP、UDP和文件传输的支持。作为当前最流行的NIO框架，Netty在互联网领域、大数据分布式计算领域、游戏行业、通信行业等获得了广泛的应用。
#2、Netty的线程模型是怎样的？
### Netty的线程模型是Reactor模型的变种，那就是去掉线程池的第三种形式的变种，这也是Netty NIO的默认模式。
#3、Netty是怎么运行的?

#4、NIO的组成有哪些？
Channels
Buffers
Selectors
虽然Java NIO 中除此之外还有很多类和组件，但在我看来，Channel，Buffer 和 Selector 构成了核心的API。其它组件，如Pipe和FileLock，只不过是与三个核心组件共同使用的工具类。因此，在概述中我将集中在这三个组件上。其它组件会在单独的章节中讲到。

Channel 和 Buffer
基本上，所有的 IO 在NIO 中都从一个Channel 开始。Channel 有点象流。 数据可以从Channel读到Buffer中，也可以从Buffer 写到Channel中。这里有个图示：

技术分享

Channel和Buffer有好几种类型。下面是JAVA NIO中的一些主要Channel的实现：

FileChannel
DatagramChannel
SocketChannel
ServerSocketChannel
正如你所看到的，这些通道涵盖了UDP 和 TCP 网络IO，以及文件IO。

与这些类一起的有一些有趣的接口，但为简单起见，我尽量在概述中不提到它们。本教程其它章节与它们相关的地方我会进行解释。

以下是Java NIO里关键的Buffer实现：

ByteBuffer
CharBuffer
DoubleBuffer
FloatBuffer
IntBuffer
LongBuffer
ShortBuffer
#5、BIO、NIO和AIO的区别？
BIO：同步并阻塞，服务器的实现模式是一个连接一个线程，这样的模式很明显的一个缺陷是：由于客户端连接数与服务器线程数成正比关系，可能造成不必要的线程开销，严重的还将导致服务器内存溢出。当然，这种情况可以通过线程池机制改善，但并不能从本质上消除这个弊端。
NIO：在JDK1.4以前，Java的IO模型一直是BIO，但从JDK1.4开始，JDK引入的新的IO模型NIO，它是同步非阻塞的。而服务器的实现模式是多个请求一个线程，即请求会注册到多路复用器Selector上，多路复用器轮询到连接有IO请求时才启动一个线程处理。
.AIO：JDK1.7发布了NIO2.0，这就是真正意义上的异步非阻塞，服务器的实现模式为多个有效请求一个线程，客户端的IO请求都是由OS先完成再通知服务器应用去启动线程处理（回调）。
应用场景：并发连接数不多时采用BIO，因为它编程和调试都非常简单，但如果涉及到高并发的情况，应选择NIO或AIO，更好的建议是采用成熟的网络通信框架Netty。
#6、Netty的优势在于什么？
 ### API使用简单，开发门槛低，
 功能强大，预置了多种编解码功能，支持多种主流协议；
 定制能力强，可以通过ChannelHandler对通信框架进行灵活地扩展；
 性能高，通过与其他业界主流的NIO框架对比，Netty的综合性能最优；
 成熟、稳定，Netty修复了已经发现的所有JDK NIO BUG，业务开发人员不需要再为NIO的BUG而烦恼；
 社区活跃，版本迭代周期短，发现的BUG可以被及时修复，同时，更多的新功能会加入；
 经历了大规模的商业应用考验，质量得到验证。在互联网、大数据、网络游戏、企业应用、电信软件等众多行业得到成功商用，证明了它已经完全能够满足不同行业的商业应用了
#7、了解哪几种序列化协议？
1、XML
（1）定义：

XML（Extensible Markup Language）是一种常用的序列化和反序列化协议， 它历史悠久，从1998年的1.0版本被广泛使用至今。

（2）优点

人机可读性好

可指定元素或特性的名称

（3）缺点

序列化数据只包含数据本身以及类的结构，不包括类型标识和程序集信息。

类必须有一个将由 XmlSerializer 序列化的默认构造函数。

只能序列化公共属性和字段

不能序列化方法

文件庞大，文件格式复杂，传输占带宽
（4）使用场景

当做配置文件存储数据

实时数据转换

2、JSON
（1）定义：

JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式。它基于 ECMAScript (w3c制定的js规范)的一个子集， JSON采用与编程语言无关的文本格式，但是也使用了类C语言（包括C， C++， C#， Java， JavaScript， Perl， Python等）的习惯，简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。

（2）优点

前后兼容性高

数据格式比较简单，易于读写

序列化后数据较小，可扩展性好，兼容性好

与XML相比，其协议比较简单，解析速度比较快

（3）缺点

数据的描述性比XML差

不适合性能要求为ms级别的情况

额外空间开销比较大

（4）适用场景（可替代ＸＭＬ）

跨防火墙访问

可调式性要求高的情况

基于Web browser的Ajax请求

传输数据量相对小，实时性要求相对低（例如秒级别）的服务

3、Fastjson
（1）定义

Fastjson是一个Java语言编写的高性能功能完善的JSON库。它采用一种“假定有序快速匹配”的算法，把JSON Parse的性能提升到极致。

（2）优点

接口简单易用

目前java语言中最快的json库

（3）缺点

过于注重快，而偏离了“标准”及功能性

代码质量不高，文档不全

（4）适用场景

协议交互

Web输出

Android客户端

4、Thrift
（1）定义：

Thrift并不仅仅是序列化协议，而是一个RPC框架。它可以让你选择客户端与服务端之间传输通信协议的类别，即文本(text)和二进制(binary)传输协议, 为节约带宽，提供传输效率，一般情况下使用二进制类型的传输协议。

（2）优点

序列化后的体积小, 速度快

支持多种语言和丰富的数据类型

对于数据字段的增删具有较强的兼容性

支持二进制压缩编码

（3）缺点

使用者较少

跨防火墙访问时，不安全

不具有可读性，调试代码时相对困难

不能与其他传输层协议共同使用（例如HTTP）

无法支持向持久层直接读写数据，即不适合做数据持久化序列化协议

（4）适用场景

分布式系统的RPC解决方案
5、Avro
（1）定义：

Avro属于Apache Hadoop的一个子项目。 Avro提供两种序列化格式：JSON格式或者Binary格式。Binary格式在空间开销和解析性能方面可以和Protobuf媲美，Avro的产生解决了JSON的冗长和没有IDL的问题

（2）优点

支持丰富的数据类型

简单的动态语言结合功能

具有自我描述属性

提高了数据解析速度

快速可压缩的二进制数据形式

可以实现远程过程调用RPC

支持跨编程语言实现

（3）缺点

对于习惯于静态类型语言的用户不直观
（4）适用场景

在Hadoop中做Hive、Pig和MapReduce的持久化数据格式
6、Protobuf
（1）定义

protocol buffers 由谷歌开源而来，在谷歌内部久经考验。它将数据结构以.proto文件进行描述，通过代码生成工具可以生成对应数据结构的POJO对象和Protobuf相关的方法和属性。

（2）优点

序列化后码流小，性能高

结构化数据存储格式（XML JSON等）

通过标识字段的顺序，可以实现协议的前向兼容

结构化的文档更容易管理和维护

（3）缺点

需要依赖于工具生成代码

支持的语言相对较少，官方只支持Java 、C++ 、Python

（4）适用场景

对性能要求高的RPC调用

具有良好的跨防火墙的访问属性

适合应用层对象的持久化

7、其它
protostuff 基于protobuf协议，但不需要配置proto文件，直接导包即

Jboss marshaling 可以直接序列化java类， 无须实java.io.Serializable接口

Message pack 一个高效的二进制序列化格式

Hessian 采用二进制协议的轻量级remoting onhttp工具

kryo 基于protobuf协议，只支持java语言,需要注册（Registration），然后序列化（Output），反序列化（Input）
#8、TCP 粘包/拆包的原因?
我们知道tcp是以流动的方式传输数据，传输的最小单位为一个报文段（segment）。tcp Header中有个Options标识位，常见的标识为mss(Maximum Segment Size)指的是，连接层每次传输的数据有个最大限制MTU(Maximum Transmission Unit)，一般是1500比特，超过这个量要分成多个报文段，mss则是这个最大限制减去TCP的header，光是要传输的数据的大小，一般为1460比特。换算成字节，也就是180多字节。

tcp为提高性能，发送端会将需要发送的数据发送到缓冲区，等待缓冲区满了之后，再将缓冲中的数据发送到接收方。同理，接收方也有缓冲区这样的机制，来接收数据。

发生TCP粘包、拆包主要是由于下面一些原因：

应用程序写入的数据大于套接字缓冲区大小，这将会发生拆包。

应用程序写入数据小于套接字缓冲区大小，网卡将应用多次写入的数据发送到网络上，这将会发生粘包。

进行mss（最大报文长度）大小的TCP分段，当TCP报文长度-TCP头部长度>mss的时候将发生拆包。

接收方法不及时读取套接字缓冲区数据，这将发生粘包。

……
#9、Netty的零拷贝实现？因及解决方法？

#10、Netty的高性能表现在哪些方面？
从大的方面看，netty性能高效主要体现在：
1.io线程模型
使用reactor模式，同步非阻塞。这决定了可以用最少的资源做更多的事。
2.内存零拷贝
使用直接缓存
3.内存池设计
申请的内存可以重用，主要指直接内存。
内部实现是用一颗二叉查找树管理内存分配情况。
4.串形化处理socket读写，避免锁，即一个指定socket的消息是串形化处理的。这样性能比多个线程同时 处理一个socket对应消息要好，因为多线程处理会有锁。
5.提供对protobuf等高性能序列化协议支持

应用netty调优：
1.若只希望该handler处理完后，交给下一个handler处理，则调用:
ChannelHandlerContext的fireRead之类的方法。
直接通过channel.read或write则会触发完整的handler处理链。

2.SimpleChannelInboundHandler读取完并且全部handler处理完后，会自动释放消息。
ChannelInboundHandlerAdapter不会自动释放消息。

3.ServerSocketChannel一次loop默认最多处理16次客户端连接，这个主要是因为服务端会对应很多客户端，保证较高的吞吐量。
对应参数：MAX_MESSAGES_PER_READ

4.WRITE_BUFFER_LOW_WATER_MARK可用于做流控
需要应用调用isWritable方法，如果返回false暂时写失败。

5.直接内存使用，零拷贝。直接内存块缓存池设计。

6.避免多线程在从内存块缓存池中获取内存块发生阻塞竞争，小块内存大小从线程独有的cache中获取。

7.reactor模式核心是用最少的资源处理最多的事情，一个reactor线程负责查看注册的关心事件

8.软中断
linux 2.6.35以上版本可以设置rps，主要目的是将数据包的处理均匀打散在每个cpu上。提升网络处理能力。
9.设置tcp参数
主要是接收缓存区大小：SO_RCVBUF，及发送缓存区大小：SO_SNDBUF。
关闭SO_TCPNODELAY，对于要求延迟小的应用。