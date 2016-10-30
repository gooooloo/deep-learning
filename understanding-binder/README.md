从几个要点来阅读Binder的代码
===

今天看了gityuan博客上的这篇[讲解Binder的文章](http://gityuan.com/2016/09/04/binder-start-service/)。我觉得Binder代码虽然这么多，但是似乎可以从这么几个要点来分层阅读。

1. Binder的作用是把一个Process的指令和数据传到另一个Process。所以一定会有一两个数据结构存放指令和数据。指令靠双方约定的int值，数据则靠void＊。本文会把这两中基本数据称为**RawData**。这个要点可以称为"**RawData**是什么"。

2. unix区分用户态和内核态；Android区分Java和native；Binder是通过把Process的内存映射到`/dev/binder`来共享内存。所以会有很多代码来跨层做指令和数据传递（仅仅做数据传递之用）。这个要点可以称为"把**RawData**传来传去"。

3. 因为涉及到回执／错误处理等等，所以还有一套协议，参见下图。这个要点可以称为"**传递机制**本身是怎么设计的"，这一点和**RawData**是完全无关的。下图引用自gityuan的这篇[讲解Binder的文章](http://gityuan.com/2016/09/04/binder-start-service/): ![](http://gityuan.com/images/binder/binder_start_service/binder_transaction.jpg)

4. 为了做上面的事情，要写一些数据结构，那么就会有内存管理。这个要点可以称为"为实现**传递机制**而导致的额外代码"。



