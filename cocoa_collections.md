cocoa 的集合
====
集合用来表示一对多的关系。

回忆一下大学时候的核心基础课程：数据结构。老早就有人说数据结构是计算机科学本科最重要的课程，没有之一。也有国外的编程大牛认为，设计良好的数据结构，算法就是自然而然的东西了。

那么我们学过的数据结构包括什么呢？数组，链表，队列，堆栈，哈希表，二叉树， 有集合么？

- 数组：常数时间访问，size固定
- 链表：线性访问时间，方便伸缩；还有种变形就是双向链表
- 队列：先进先出
- 堆栈：后进先出
- 哈希表：当时的概念是可以使用字符串当作所以的数组～
- 二叉树：blablabla
- 集合：不保证顺序，但保证唯一性

然后的编程工作就是苦逼的用C\C++实现这些概念～

加一句吐槽：当前国内的C++教育方向完全错了，入门教育应该站在更高的抽象层级之上，嗯，传说中的（站在巨人的JB上）；嗯嗯，如果让我来选择，我选择C语言加smalltalk。


##Foundation 提供的类型
然后看看cocoa为我们提供了神马可以利用的东西。

- NSArray & NSMutableArray
- NSDictionary & NSMutableDictionary
- NSSet & NSMutableSet
- NSOrderedSet & NSMutableOrderedSet(5.0)
- NSIndexSet & NSMutableIndexSet
- NSIndexPath
- ~~NSString~~


嗯，应该就是这么多：）

首先，cocoa提供的很多数据类型是有imutable和mutable版本的区分的，像NSString, NSNumber根本就木有mutable的版本。最开始学习使用cocoa的时候，觉得这种东西分明是给自己找别扭。后来代码敲的多了，学习更加深入的时候才觉得，这可是cocoa设计的一大亮点。嗯嗯，水果公司到底还是聚集了众多的天才。使用imutable版本的对象

- 可以更好的表达设计意图，比如常值集合
- 让编译器发现更多的错误
- 更好的适应多线程

实际上完全使用immutable对象进行编程也是可以实现的，例如

```
    NSArray *array = @[@1, @2];
    array = [array arrayByAddingObject:@3];
```

这种就属于cocoa的函数式编程风格，数组对象是不变的，如果需要改变数组，就创建一个新的数组对象。类似于：

```
    NSNumber *sum = @1;
    sum = @( [sum integerValue] + 1);
```

然后在看这些个集合类，NSString就先不说了，cocoa编程还没见过把NSString当作集合操作的情景；indexSet 和 indexPath是和NSArray合起来使用的。orderedSet应该还是有点用的，不过需要在iOS5.0以后使用，嗯嗯，还是淡定吧。

嗯嗯，这样说下来，主要就三个集合类型NSArray, NSDictionary, NSSet;其实说起来，array 和 set的功能都可以由dictionary来实现的，嗯lua其实就是这么干的。

当然了，正常人都不会这么干。抛出去性能方面的考量（大多数时候都不是事），使用特定的类型能更明确的表达设计意图。嗯， 就是说：

- 表达有序序列用 `NSArray`
- 表达映射关系用 `NSDictionary`
- 表达无序的集合类型用 `NSSet`

嗯，平时用的最多的也就是前两者，嗯，大学的数据结构知识基本没用上。。。


##集合上的操作

- 访问集合中的元素
- 检查包含关系，嗯这是所有集合类型的操作，不单单是针对set类型
- 枚举

对于mutable版本的类型，加两条

- 添加元素
- 删除元素

这几种操作里面，我对cocoa api设计不满意的就是枚举接口的设计。苹果的工程师似乎对smalltalk的集合api设计并不感冒。即使很多年以前，c语言还没有block这么个东西，但是ios4.0之后的接口有了一点意思，但是还觉得差上那么一点点。

##集合的枚举操作





