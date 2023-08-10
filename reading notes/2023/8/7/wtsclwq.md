#### 栈到底是什么

栈只是一种抽象概念，是一种虚拟出来的数据存取方法。其实现形式 是不限的，只要满足栈的定义就可以。

- 首先得是线性结构，并且数据的存取在线性结构的一端进行。如果您愿意，可以用链表来实现， 也可以用数组来实现，它们都是线性数据结构。

- 其次需要维护一个指针，用它来指向线性结构的一端，数据存取都通过此指针。

到目前为止，我们说的只是数据结构中的栈，这是逻辑上的，最终我想表达的是内存中的栈，这是物 理上的。把数据结构中的栈的概念用物理硬件来实现，这就是我们要说的栈。它同数据段、代码段一样， 是个内存中的区域，也就是栈段寄存器 SS 和栈指针 SP 所指向的内存区域。我们常听说的栈溢出，指的 就是这个内存区域无法容纳数据了

区域的起始地址作为栈基址， 存入栈基址寄存器 SS 中， 另一端是动态变化的， 用栈指针寄 存器 SP 来指定。 栈在使用过程中是向下扩展的， 所以栈顶地址肯定小于栈底地址

push 指令负责把数 据压入栈，pop 指令功能相反，将其从栈中取出。不过我刚才说的不全面，栈的出口和入口都是栈顶，push 把数据压向哪里，它得知道栈顶在哪里才行。pop 指令也一样，它得知道哪里是栈顶才能从栈中取出正确 的数据。这正是栈指针寄存器 SP 的作用，此寄存器中的值是段内偏移地址，是栈顶相对于栈底的偏移量。