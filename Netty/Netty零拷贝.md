这里所说的零拷贝是广义的零拷贝，只要是在减少数据拷贝次数，包括用户态拷贝，用户态和内核态之间的拷贝，在Java程序中还有Native堆和Java堆之间的拷贝。所以要实现零拷贝需要三个方面入手：

1. 减少内核态到用户态的数据库拷贝，可以使用mmap、shm、sendfile、splice系统调用减少内核和用户态之间的数据拷贝，JDK提供的是内存映射文件和FileChannel的transferTo/transferFrom方法。
2. 减少Java Heap和Native Heap之间的数据拷贝，使用JDK提供的直接内存。
3. 减少用户态的数据拷贝：Netty提供了CompositeByteBuf以及ByteBuf的slice/wrap方法。

#### FileRegion

FileRegion的零拷贝依赖于Java NIO的FileChannel实现文件传输的零拷贝。

#### DirectByteBuf

直接内存又称为堆外内存，通过堆外内存可以避免数据在Java堆和C 堆中进行拷贝。

#### CompositeByteBuf

CompositeByteBuf能将多个ByteBuf合并为一个逻辑上的ByteBuf，减少用户态数据拷贝。

#### wrap/slice

wrap将一个bytes包装为UnpooledHeapByteBuf对象，从而不会产生数据拷贝。

slice将一个ByteBuf切片为多个共享一个存储区域的ByteBuf对象。