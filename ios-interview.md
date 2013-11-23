
# ios 面试笔记

##内存管理

###非ARC下的setter&accessor

写出的accessor和setter,显然这里是非ARC

```objc
@property (nonatomic,retain) id obj;
```


```objc
- (id) obj{
		return _obj;
}

- (void) setObj:(id)obj{
		[_obj autorelease];
		_obj= [obj retain];
}

```

这道题目考察基本的引用技术模型.
关键点在于setter的实现。

* 首先要对_obj 进行release，然后对obj retain,再然后对`_objc`进行赋值。
* 另外，要防止obj和_obj是同一个对象，如果是同一个对象，那么先release再retain，显然是要crash的。

那么setter的代码就可以这么写
```
- (void) setObj:(id)obj{
		if(_obj != obj){
				[_obj release];
				_obj = [obj retain];
		}
}
```

最开始的写法是google推荐的，显然优雅的对两种考虑都得到了实现。

当然在使用ARC的前提下，就不应该出现这道考题了，因为不再考虑retain release，显然消除了很多的烦恼。代码如下

```objc
- (id)obj{
		return _objc;
}

- (void) setObjc:(id)obj{
		_obj = obj;
}

```

ARC下的生活真是好啊，不过碰见出这种考题的面试官可就要小心点了。

顺便回忆一下没有ARC，cocoa程序员内存管理生存指南。

一句话，谁对引用计数加1，谁就有责任调用release，或者autorelease，使引用计数减1.

具体的操作中：

* 只使用property，一般是retain，delegate显然是assign，NSNumber, NSDate等不可修改对象一律copy
* 一般只在setter中使用显式的使用release, retain
* 如果创建了一个对象，那么直接跟autorelease 类似[[[Class alloc] init] autorelease];

这样基本在非ARC情况下，尽可能少受到内存管理的困扰


