smalltalk最佳实践
===
##模式

##方法

> Methods are important to the system because they are how work gets done in Smalltalk. Just as important, methods are how you communicate to readers how you intended for work to get done. You must write your methods with both of these audiences in mind. Methods must do the work they are supposed to do, but they must also communicate the intent of the work to be done.
> 
> Overall, the goal of breaking your program into methods is to communicate your intent clearly with your reader, provide for future flexibility, and set yourself up for effective performance tuning where necessary.

- Composed Method
- Complete Create Method
- Creation Parameter Method
- Conversation
- Converter Method
- Converter Creation Method
- Constructor Method
- Query Method
- Comparing Method
- Excute Around Method
- Debug Printing Method

### Composed Method

> Divide your program into methods that perform one identifiable task. Keep all of the operations in a method at the same level of abstraction. This will naturally result in programs with many small methods, each a few lines long.

```
- (void) controlActivity{
    [self controlInitialize];
    [self controlLoop];
    [self controlTerminate];
  }

```

> Perhaps most importantly, you can use Composed Method to discover new responsibilities while you are implementing. Any time you are sending two or more messages from one object to another in a single method, you may be able to create a Composed Method in the receiver that combines those messages. Such methods are invariably useful from other parts of your system.

### Complete Create Method

简单灵活的方法：

```
  Point *point = [Point new];
  point.x = 0;
  point.y = 0;
```
缺点： 不能准确的表述怎样才能创建一个有效初始化的实例

```
 + (instancetype) pointWithX:(id)x y:(id) y;
```

```
 + (instancetype) pointWithX:(id)x y:(id) y{
      Point *point = [Point new];
      point.x = x;
      point.y = y;
      return point;
  }
```


### Creation Parameter Method

???

### Conversation

同一信息，不同协议之间的转化

### Converter Method


```
	[@1.f doubleValue]
```

### Converter Creation Method
通过转化得到新对象

如：数字，字符串，时间类型之间的转换

cocoa使用独立的各种formatter类来实现，似乎灵活性更好



### Constructor Method

smalltalk 使用 `1 @ 2` 用来创建点， @是一个方法

```
Number>>@ aNumber ^Point
```

cocoa更多的是使用C函数 NSMakeXxx 

### Query Method

将布尔表达式放进方法内部

objc 的property 可以将accessor 写成 isXxx, 但是YES， NO不是对象，很蛋痛。

### Comparing Method

smalltalk  <, <=, >, >= 之类的都是消息， 

cocoa 似乎只能从`isEqual` 和 `hash` 做文章了

### Execute Around Method
环绕方法， ruby设计模式里面也有，感情是从这里抄来了

### Debug Printing Method
cocoa 里面的 -description(), 模型对象的话，用Mantle默认的就不错


##消息

- Message
- Choosing Message
- Decomposing Message
- Intention Revealing Message
- Doulbe Dispatch
- Send Back
- Super
- Extending Supper
- Delegation
- Simple Delegation
- Self Delegation
- Pluggable Behavior
- Pluggable Selector
- Pluggable Block
- Collecting Parameter

### Message

消息用作控制结构 多态

### Choosing Messsage

条件逻辑：

```
id responsible = if ([anEntry isKindOfClass:[Film class])? [anEntry producer] : [anEntry author];

```

多态做法

```
id responsible = [anEntry responsible]
```


### Decomposing Message

Composed Method

```
- (void) controlActivity{
    [self controlInitialize];
    [self controlLoop];
    [self controlTerminate];
  }
```

###  Intention Revealing Message

简单实现的方法， 选择器名用作文档

合理命名解释目的，而不是实现

@interface NSArray (IntentionRevealingMessage)
- (BOOL) isEmpty;
@end

@implementation NSArray
- (BOOL) isEmpty{
    return self.count == 0;
}
@end

### Doulbe Dispatch
???

### Send Back

???

### Super

引用父类的行为

小心使用，如果没有区别，则向`self`发消息

### Extending Supper
扩展父类的行为

```
- (instancetype) init{
    self = [super init];
    if (self == nil) return nil;
    // do someting extent parent class
    return self;
}
```

### Modifying Supper

改变父类行为

缺点：形成和父类的耦合关系


尼玛，上面的例子可能更合适

### Delegation

不使用继承来分享实现，smalltalk

### Simple Delegate



##状态