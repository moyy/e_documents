- [容器 std::collections](#容器-stdcollections)
  - [1. 序列型](#1-序列型)
  - [2. K-V 型](#2-k-v-型)
  - [3. 其他：BinaryHeap `二叉堆`，用于`优先队列`；](#3-其他binaryheap-二叉堆用于优先队列)
  - [4. 时间复杂度 参考](#4-时间复杂度-参考)

# 容器 std::collections

[std::collections](https://doc.rust-lang.org/std/collections/index.html)

## 1. 序列型

`VecQueue` 说明：双端队列就是可以用O(1)的时间，从头尾插入删除元素的数据结构；栈和队列都是一种抽象的数据结构，具体实现的时候，要么用链表要么用数组，这个模块内部实现用：可增长的环形缓冲区，可以认为是Vec的一种变形；

|模块|作用|附加说明|
|--|--|--|
|[Vec](https://doc.rust-lang.org/std/vec/struct.Vec.html)|`向量`，可变长的数组|可以当 `Stack`（栈）用|
|[VecDeque](https://doc.rust-lang.org/std/collections/struct.VecDeque.html)|用vec实现的`双端队列`|可以当 `Queue`（队列）用|
|[LinkedList](https://doc.rust-lang.org/std/collections/struct.LinkedList.html)|`双向链表`|可以当 （单向）`链表` 用|

## 2. K-V 型

**注：** 用B-Tree实现的功能 和 别的语言用 红黑树，AVL树 实现的功能 差不多，都是 可以提供 `有序` `范围查询` 的数据结构；理论请参考：[数据结构：二叉搜索树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%85%83%E6%90%9C%E5%B0%8B%E6%A8%B9)

**注：** 当你需要用到对key进行 范围查询，包括：同时取 最小，最大 这些需求的时候，考虑用 BTree；否则尽量用Hash；（如果仅仅是取最小值，或者仅仅是取最大值，请考虑用：BinaryHeap）

**注：** 当你仅仅是需要保证一个东西的唯一性的时候，用set；如果你需要一个映射关系，用map

||无序（`Hash`实现）|有序（`B-树`实现）|
|--|--|--|
|`Map`（映射表）|[HashMap](https://doc.rust-lang.org/std/collections/struct.HashMap.html)|[BTreeMap](https://doc.rust-lang.org/std/collections/struct.BTreeMap.html)|
|`Set`（集合）|[HashSet](https://doc.rust-lang.org/std/collections/struct.HashSet.html)|[BTreeSet](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html)|

## 3. 其他：[BinaryHeap](https://doc.rust-lang.org/std/collections/struct.BinaryHeap.html) `二叉堆`，用于`优先队列`；

注：collections::BinaryHeap 不支持 删除内部节点 和 更新值 操作；

## 4. 时间复杂度 参考

下表，从上往下，复杂度 依次 增加：

|表示|描述|
|--|--|
|O(1)|常量|
|O( log2(n) )|对数|
|O(n)|线性|
|O( n * log2(n) )|n-lgn|
|O(n^2)|平方|
|O(n^3)|立方|
|O(n^a)|多项式，a是大于3的常量|
|O(e^n)|指数，e是自然对数的底|
|O(n!)|阶乘|