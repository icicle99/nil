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

为了继续下面的笔记，虚构一个需求先：假设有一个数组

```
    NSArray *array = @[@1, @2, @3, @4, @5];

```

要求依次打印每一个数值~


```
//C程序员做法：
    for (NSInteger i = 0; i < [array count]; i++) {
        NSLog(@"%@", array[i]);
    }

//2B程序员做法：
    NSEnumerator *enumerator = [array objectEnumerator];
    NSNumber *obj = nil;
    while (obj == [enumerator nextObject]) {
        NSLog(@"%@", obj);
    }

//普通程序员：
    for (NSNumber *obj in array) {
        NSLog(@"%@", obj);
    }

//文艺程序员：
    [array enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        NSLog(@"%@", obj);
    }];
```

C程序员的做法是C编程里面基本的“模式”，笔者在做C程序员的时候写这东西基本上已经内化为本能了，不过做iOS开发之后就很少这么玩了。其实在这个“模式”里面还是有很多讲究的，比如在for语句内声明变量i, 从0开始计数，半开半闭合区间等等。但是在这里面有一个本质的不同，cocoa的数组不再是C数组，这种形式显然没有C语言中的那么高效，水果公司的文档也不建议采用这种方式。另外这种方式似乎没法遍历NSDictionary和NSSet.

第二种做法显然更面向对象，标准的外部迭代器用法；之所以说是2B写法，是因为实现同样的功能，多了一倍的代码，多了两个局部变量。。。类似于拒绝采用ARC的cocoa程序员，多写了好多的autorelease, 还有种莫名的优越感。。。

第三种是apple推荐的用法，据说是最快的枚举方法。这种东西本质上应该是第二种做法的语法糖。有人对块糖表示过异议，认为这只是实现了一种很有局限性的操作，却增加了语言复杂度。理论上说水果公司应该提供更好的对block的支持，然后用库来实现更好的遍历方式。

第四种就是上面所说的做法了，看起来功能上跟第三种快速枚举没差别。不过如果需求要求打印序数，文艺程序员就有优越感了，明显不需要多声明一个变量~

下面需求变了，恩，需求又变了。。。

要求对数组求和，下面只给出快速枚举和基于block的枚举方式

```

//快速枚举
    NSInteger sum = 0;
    for (NSNumber *obj in array) {
        sum += [obj integerValue];
    }
    
//文艺枚举
    __block NSInteger sum = 0;
    [array enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        sum += [obj integerValue];
    }];
    
```
这下普通程序员乐了，很简单的一个累加求和，文艺用法偏偏还得加个`__block`,要多丑有多丑，文艺变2B了。。。

毛主席教导我们说：当文艺显着有些2B，就说明文艺的还不够，应该沿着文艺的道路继续走下去~

首先实现一个基于`NSArray`的扩展：

```
typedef id(^AccumulationBlock)(id sum, id obj);

@interface NSArray (BlockKit)
- (id)reduce:(id)initial withBlock:(AccumulationBlock)block;
@end

@implementation NSArray (BlockKit)
- (id)reduce:(id)initial withBlock:(AccumulationBlock)block {
    
	__block id result = initial;
    
	[self enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
		result = block(result, obj);
	}];
    
	return result;
}

@end
```
实现的细节并不是很重要，恩，丑陋的细节被封装起来了。。。

于是文艺版本的求和变成了这个样子：

```
    NSInteger sum = [[array reduce:@0 withBlock:^id(id sum, id obj) {
        return @( [sum integerValue] + [obj integerValue] );
    }] integerValue];
```

这种做法的优势是什么？计算过程不需要局部变量参与，把那个丑陋的`__block NSInteger sum` 封装到数组的reduce方法里面去了。方法的调用者只需要关注于各个元素怎么“加和”到一起就行了。

类似的还可以这么用：

```
    NSArray *mappedArray = [array reduce:@[] withBlock:^id(id sum, id obj) {
        return [sum arrayByAddingObject:@( [obj integerValue]  * 2)];
    }];
```
这个reduce方法实现的是把原数组的每个整数加倍，然后形成一个新数组。为了更好的抽象和更好的表达设计意图，一般都是再加多一个扩展，然后可以这样写：

```
    NSArray *douleArray = [array map:^id(id obj) {
        return @( [obj integerValue] * 2 );
    }];
```

这样有更明确的表意，更加的简单，编写代码的时只需要指名，怎样映射就好了。就是说通用逻辑由库实现，应用程序员可以只关注业务逻辑。很遗憾，苹果那些家伙并没有实现这种方法，不过没关系，我们可以用`catagory`和`block`构造出我们想要的东西。嗯嗯

如果可以像苹果的工程师提意见，我最想要的foundation结合支持的方法是`reduce，map， select， reject, match`。。。
当然了，如果他们能参考一下Ruby和smalltalk的集合实现，并且还能给objective-c加上mixin的官方支持。。。好吧，我想多了。

##函数style




