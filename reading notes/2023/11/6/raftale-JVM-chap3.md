## 并发的可达性分析

当前主流的垃圾收集器都是依靠可达性分析算法来判定对象是否存活的，可达性分析算法理论上要求 全过程都基于一个能保住一致性的快照中，这意味着必须要冻结用户线程的运行。根节点枚举时，在OopMap等的优化下，停顿额可以做到非常短暂，但从GC Roots向下遍历图标记对象，这一步骤的时间跟随Java堆容量的大小，也就是标记时间是不可控的。所以为了STW的时间短，遍历这一步不能进行停顿，不停顿就意味着垃圾回收线程和用户线程是同时执行的，这样的并发就可能产生安全问题。

为了解决并发带来的问题，首先要搞清楚并发产生了什么问题，然后是如何解决这个问题。在遍历的过程中，不是遇到一个节点就标记为已标记的过程，这样对解决并发问题没有帮助，而是使用了三色标记来区分不同的场景。

三色标记：

1. 白色：未被标记，表示不可达
2. 黑色：已被访问 且其引用 都已经被扫描过，标记为黑色，表示可达，安全存活的对象
3. 灰色：已被访问，但其引用还没有被扫描



并发条件下，即垃圾回收线程在访问标记的过程中，用户线程在修改引用关系。这可能导致：

1. 浮动垃圾：在已经标记为黑色的节点上断开与GC Roots的连接，导致本应该被回收的对象是黑色的，未被标记

1. 1. 可以容忍，下一次垃圾回收时再重新标记回收

1. 漏标问题：将原本存活的对象错误标记为已消亡，这是非常致命的bug。

1. 1. 在已经被黑色标记过的对象上，用户线程新增一个引用，由于为黑色的对象不会再次扫描，所以挂在黑色标记对象上的对象仍然是白色，那么它就会被回收掉。


Wilson在1994年理论上证明了，当且仅当以下两个条件同时满足时，会产生对象消失的问题，即原本为黑色的对象误标记为白色：

1. 赋值器插入了一条或多条从黑色对象到白色对象的新引用
2. 赋值器删除了全部从灰色对象到该白色对象的直接或间接引用

因此，我们要解决并发扫描的对象消失问题，只需要破坏这两个条件的任意一个即可。

1. 增量更新（Incremental Update）：破坏第一个条件

1. 1. 当黑色对象插入新的指向白色对象的引用时，就将这个新插入的引用记录下来，等并发扫描结束后，再将这些记录过的引用关系中的黑色对象为根，重新扫描一次。

1. 原始快照（Snapshot At the beginning, SATB）：破坏第二个条件

1. 1. 当灰色对象要删除指向白色对象的引用关系时，就将这个要删除的引用记录下来，在并发扫描结束之后可，再将这些记录过的引用关系中的灰色对象为根，重新扫描一次，那被用户线程修改的删除关系将会被再次打上标记。
2. 相当于是记录了一个扫描那一刻的对象快照图，真正的变动放到下一次垃圾回收时

总结解决思路就是：关系变更时做记录，事后再根据记录重新扫描。



增量更新：CMS

原始快照：G1、Shenandoah