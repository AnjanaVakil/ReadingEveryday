分页机制本质上是将大小不同的大内存段拆分成大小相等的小内存块。

32 位地址表示 4GB 空间，可以将 32 位地址分成高低两部分，低地址部分是内存块大小，高地址部分是 内存块数量，它们是这样一种关系：**内存块数*内存块大小=4GB**

![image-20230819122901640](https://wtsclwq.oss-cn-beijing.aliyuncs.com/image-20230819122901640.png)

地址转换过程原理如下：

一个页表项对应一个页，所以，用线性地址的高 20 位作为页表项的索引，每个页表项要占用 4 字节 大小，所以这高 20 位的索引乘以 4 后才是该页表项相对于页表物理地址的字节偏移量。用 cr3 寄存器中 的页表物理地址加上此偏移量便是该页表项的物理地址，从该页表项中得到映射的物理页地址，然后用线 性地址的低 12 位与该物理页地址相加，所得的地址之和便是最终要访问的物理地址。
