# B+Tree

> 现在正式进入B+树实验的部分, 相关代码都在

## 概览

本次课程实验实现一个B+树索引。B+树是一种自平衡的多路搜索树，广泛应用于数据库和文件系统的索引结构。与其他树结构（如二叉树、前缀树等）相比，B+树具有更低的树高和更好的磁盘I/O性能，特别适用于大量数据的存储和检索。一棵B+树由内部节点和叶子节点组成，内部节点负责进行索引，叶子节点存储真实数据。你的实现需要支持线程安全的搜索、插入、删除以及一个用于有序遍历叶子节点的迭代器。

实验分为两个阶段：

**阶段#1(30分) - 截止日期：5月28号**
- [B+树节点](#任务1---b树结点)
- [B+树插入](#任务2a---b树插入与单点查询)

**阶段#2(70分) - 截止日期：6月18号**
- [B+树删除](#任务2b---b树删除)
- [B+树迭代器](#任务3---b树迭代器)
- [并发索引](#任务4---并发索引)

```admonish warning
你**必须**使用git记录你的开发过程并将记录推送到你的Github仓库!
```

## 阶段#1

### 任务#1 - B+树结点

你需要为你的B+树实现三个类型的节点。
- 节点
- 内部节点
- 叶子节点

```admonish title="节点"

`BPlusTreeNode`是内部节点与叶子节点的基类，只包含两个子类所公有的信息。`BPlusTreeNode`应该包含以下字段：

| 变量名     | 描述                         |
|:-----------|:-----------------------------|
| node_type_ | 节点的类型(internal or leaf) |
| size_      | 该节点当前内部键值对的数量   |
| max_size_  | 该节点最大能容纳的键值对数量 |

你需要在`storage/db20xx/src/b_plus_tree_node.cc`和`storage/db20xx/include/b_plus_tree_node.h`两个文件中实现这个类。
```

```admonish title="内部节点"
一个`InternalNode`最多能存储$d-1$个键和$d$个孩子指针。这些键和指针用一个长度为$d$的数组进行存储即可。由于键和指针的数量不同，数组的第一个元素的键应该被忽略，在查找的时候总是从第二个元素开始。

`InternalNode`始终要保持以下性质：
- size_ = number of children
- $\lceil \frac{d}{2} \rceil \le size_  \le d$
- $\forall i,j \in (0,d), i\lt j\implies key_i \lt key_j$
- $\forall i \in (0, d), \forall key\in child_{i-1} $

你需要在`storage/db20xx/src/b_plus_tree_internal_node.cc`和`storage/db20xx/include/b_plus_tree_internal_node.h`两个文件中实现这个类。
```

```admonish title="叶子节点"
`LeafNode`

`LeafNode`始终要保持以下性质：
- $\lfloor \frac{d}{2} \rfloor\le  size_  \le d$
- $\forall i,j \in \lbrack 0,d), i\lt j \implies key_i \lt key_j$

值得注意的是`InternalNode`和`LeafNode`的$d$可以不同。

你需要在`storage/db20xx/src/b_plus_tree_leaf_node.cc`和`storage/db20xx/include/b_plus_tree_leaf_node.h`两个文件中实现这个类。
```

### 任务#2a - B+树插入与单点查询

在阶段#1，你的B+属索引必须支持插入([`put(key, value)`](https://github.com/FLAYhhh/DB20XX/blob/9aa193c306bf97aa3897ea4086a7a065f81f6566/storage/db20xx/include/bplustree_index.h#L16))以及单点查询([`get(key)`](https://github.com/FLAYhhh/DB20XX/blob/9aa193c306bf97aa3897ea4086a7a065f81f6566/storage/db20xx/include/bplustree_index.h#LL28C3-L28C3))。这个索引不支持重复键；如果试图向树中插入一个已经存在的键，就不进行插入，并且返回插入失败。

如果插入操作会破坏B+树的性质，B+树节点应该分裂或进行键的重新分配。如果插入操作会更改根节点，则必须更新B+树索引中的`header_node_`。


插入的流程如下：
```admonish info
0. 如果树为空，创建一个新的根节点
1. 找到插入位置所在的叶子节点$L$
2. 向$L$中插入键值对
    - 如果$L$还有足够的空间，操作完成
    - 否则，将$L$分裂为$L$和$L'$。将键值对在这两个节点之间平均分配，并且将中间的键复制后插入父亲节点$P$，使得$P$中有指向$L'$的指针。
3. 如果，$P$有充足空间，则操作完成，否则将$P$分裂为$P$和$P'$，键值对平均分配。值得注意的是，与`LeafNode`的分裂不同，`InternalNode`分裂的时候，只需将中间的键插入爷爷节点，无需在$P$或$P'$中保留这个中间键。
```

单点查询的流程则与二叉树的搜索类似，在每次访问`InternalNode`的时候定位到下一层要去哪个孩子节点查询，访问`LeafNode`的时候再去查找`key`对应的`value`即可。

```admonish tip
在进行实现的时候，你会看到一个很奇怪的参数叫`VersionChainHead*`，这是为了让树支持多个版本的值(与MVCC相关)。你可以认为值的版本构成了一个链表，插入操作可以不覆盖旧的值，而是在版本链的头部插入一个新版本的值。当然你也可以不做多版本的实现，直接将VersionChainHead当作值，插入的时候覆盖旧值即可。
```
## 阶段#2

### 任务#2b - B+树删除

本任务你需要实现键的删除([`remove(key)`](https://github.com/FLAYhhh/DB20XX/blob/9aa193c306bf97aa3897ea4086a7a065f81f6566/storage/db20xx/include/bplustree_index.h#L34))。

如果在删除过程中破坏了B+树的性质，B+树节点应该进行合并或进行键的重新分配。与插入类似，如果根节点被删除了，你需要更新B+树的根节点。

删除的流程如下：

```admonish info
0. 如果树是空的，直接返回
2. 找到删除位置所在叶子节点$L$
3. 删除键值对
    - $L$的size_仍在合法范围内，返回
    - size_小于允许的最小值，进行合并或键的重新分配
        - 如果$L$是其父节点$P$最右边的孩子，则与左孩子进行合并
        - 如果$L$是$P$最左边的孩子，则与右孩子进行合并
        - $L$既不是最左也不是最右孩子，随意
        - 你需要自己思考何时进行合并，何时进行键的重新分配
        - 重新分配后需要对$P$做什么操作来保证有序性规定？
4. 如果上一步中发生了合并，在$P$中删除对应的键和指针
5. 如果删除一直传递到了根节点
    - 如果根节点是叶子节点(即树只有根节点一个节点)，该做什么？
    - 如果根节点是内部节点，并且只剩一个唯一的孩子，该做什么？
```

### 任务#3 - B+树迭代器

本任务中，你需要实现一个C++迭代器用于有序便利所有叶子节点的键值对。由于叶子节点存储了它的兄弟节点的指针，迭代器的实现会非常简单。相关的代码在`storage/db20xx/include/index.h`中，你需要关注[`ScanIterator`](https://github.com/FLAYhhh/DB20XX/blob/1048ad20e318e653ee6579c058b77cea85dc81cc/storage/db20xx/include/index.h#LL34C1-L34C1)这个类。

你的迭代器至少要能支持这四个函数：
- [`scan_range_first`](https://github.com/FLAYhhh/DB20XX/blob/1048ad20e318e653ee6579c058b77cea85dc81cc/storage/db20xx/include/index.h#LL55C16-L55C16)
- [`scan_range_next`](https://github.com/FLAYhhh/DB20XX/blob/1048ad20e318e653ee6579c058b77cea85dc81cc/storage/db20xx/include/index.h#LL58C14-L58C15)
- [`rscan_range_first`](https://github.com/FLAYhhh/DB20XX/blob/1048ad20e318e653ee6579c058b77cea85dc81cc/storage/db20xx/include/index.h#L60)
- [`rscan_range_next`](https://github.com/FLAYhhh/DB20XX/blob/1048ad20e318e653ee6579c058b77cea85dc81cc/storage/db20xx/include/index.h#L63)


### 任务#4 - 并发索引

最后，修改你的B+树实现，让他支持安全的并发操作。最简单的思路是用一个互斥锁将整棵B+树锁住，在读/写操作的过程中上锁。但本次实验要求细化锁的粒度，每个节点拥有一个互斥锁，在读写的过程中去决定该锁住哪些节点。同时你可以使用[Lock Crabbing](https://15445.courses.cs.cmu.edu/fall2017/notes/18-notes-indexconcurrency.pdf)技术来释放读写过程中不必要持有的父节点的锁来提升你的实现的并发性能。


## 参考资料

[^Tree Indexes Lecture Note]: [Tree Indexes Lecture Note from CMU 15445](https://15445.courses.cs.cmu.edu/spring2023/notes/08-trees.pdf)
