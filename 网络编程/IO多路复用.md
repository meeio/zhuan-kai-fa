# 定义

{% hint style="info" %}
参考内容：https://zhuanlan.zhihu.com/p/64138532
{% endhint %}


「IO 多路复用」就是很多网络连接(多路)，共(复)用少数几个(甚至是一个)线程。

在WIKI上，「I/O multiplexing can also be used to refer to the concept of processing multiple input/output events from a single event loop」.

连接很多的时候，不能每个连接一个线程，会耗尽系统内存的。
线程也不能阻塞在任何一个连接上，这样就不能及时响应其他连接发来的数据了；
也不能用非阻塞方式，轮询所有的连接，这会浪费掉大量CPU时间；
只能告诉系统，我对哪些连接感兴趣，有消息来的时候，通知我处理。

# IO多路复用API

{% hint style="info" %}
参考内容：https://juejin.im/post/6844903688780120078

参考内容：https://mp.weixin.qq.com/s/tXEfsLqdePjcPS6FKa-qzg
{% endhint %}

## Select

select 通过文件描述符 `fd` 列表来储存要监听的 socket，并且把主线程添加到这些 socket 的「等待队列」中。
`fd` 列表会被拷贝至内核，当其中的socket接收到数据时，会触发中断程序，内核接过CPU使用，将主线程从每个socket的「等待队列」中删除，并加入到「工作列表」中。
主线程被激活后，开始遍历 `fd` 列表寻找就绪的 socket。

select 的缺点在于需要大量的拷贝，而且需要 O(n) 的时间遍历 `fd` 列表进行「等待队列」的相关操作，以及线程激活后的就绪 socket 查询。

## Poll
poll和select是非常相似的，poll相对于select的优化仅仅在于解决了文件描述符不能超过1024个的限制，select和poll都会随着监控的文件描述数量增加而性能下降，因此不适合高并发场景。

## Epoll

由于 Select 是直接与主线程进行交互的，所以这种设计也是不可避免，可以引入中间方来解决 Select 存在的问题，这就是 Epoll的主要思想。

Epoll 本身就是一个系统管理的文件，其句柄由 `epoll_create` 创建，此时主线程会被添加到 Epoll 的等待序列。
Epool 调用 `epoll_ctl` 指定需要监听的 Socket，Epoll 会遍历这些 Sokcet为其添加回调函数，当 Socket 接收到数据触发中断程序时，会通过回调函数将自身的 `fd` 添加到 Epoll 维护的就「绪队列中」。
`epoll_wait` 的工作实际上就是在这个就绪链表中查看有没有就绪的 `fd`，如果有的话就通知 Epoll 的等待进程。

Epoll 克服了 Select 的缺点， 对于主线程来说只需要 O(1) 的时间就可以定位到激活的 Socket。 




