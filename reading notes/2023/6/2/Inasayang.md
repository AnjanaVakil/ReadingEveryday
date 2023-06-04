内存地址

-   一个内存地址用来定位一段内存
-   一个内存地址用一个操作系统原生字（`native word`）来存储。
-   一个原生字在32位操作系统上占4个字节，在64位操作系统上占8个字节。(所以，32位操作系统上的理论最大支持内存容量为4GB（1GB == 2^30字节），64位操作系统上的理论最大支持内存容量为2^64Byte，即16EB（EB：艾字节，1EB == 1024PB, 1PB ==1024TB, 1TB == 1024GB）)
-   内存地址的字面形式常用整数的十六进制字面量来表示，比如0x1234CDEF



值的地址

-   一个值的地址是指此值的直接部分占据的内存的起始地址
-   在Go中，每个值都包含一个直接部分，但有些值可能还包含一个或多个间接部分



指针类型和值

-   指针是Go中的一种类型分类（`kind`）
-   一个指针可以存储一个内存地址；从地址通常为另外一个值的地址。
-   指针类型的零值的字面量使用预声明的nil来表示。一个nil指针（常称为空指针）中不存储任何地址。
-   如果一个指针类型的基类型为T，则此指针类型的值只能存储类型为T的值的地址。



引用（reference）

-   如果一个指针中存储着另外一个值的地址，则可以说此指针值引用着另外一个值
-   同时另外一个值当前至少有一个引用



如何获取一个指针值？

-   一个可寻址的值是指被放置在内存中某固定位置处的一个值（但放置在某固定位置处的一个值并非一定是可寻址的）
-   所有变量都是可以寻址的
-   但是所有常量、函数返回值和强制转换结果都是不可寻址的
-   当一个变量被声明的时候，Go运行时将为此变量开辟一段内存。此内存的起始地址即为此变量的地址。



Go指针的一些限制

-   不支持算术运算
-   一个指针类型的值不能被随意转换为另一个指针类型
-   一个指针值不能和其它任一指针类型的值进行比较
-   一个指针值不能被赋值给其它任意类型的指针值



打破Go指针的限制

-   `unsafe`标准库包（第25章）中提供的非类型安全指针（`unsafe.Pointer`）机制可以被用来打破上述Go指针的安全限制。



Pp. 118-126