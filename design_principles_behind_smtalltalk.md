Smalltalk背后的设计理念（读书笔记）
========

之前 [易学编程](http://worrydream.com/LearnableProgramming/)的作者推荐两篇关于smalltalk的paper，本文是期中的一篇([原文地址](http://www.cs.virginia.edu/~evans/cs655/readings/smalltalk.html))
这段时间连续读了好多遍，觉得甚好；原本是想做全篇翻译的，无奈译文看起来像一坨屎，还是写篇读书笔记好些，免得糟蹋了经典的论文。

## 项目目的
	The purpose of the Smalltalk project is to provide computer support for the creative spirit in everyone
	
好吧，阳春白雪。也许正是这伟大的初衷，造就了伟大的Smalltalk。Bu过当初Unix的目的就是为了玩个小游戏~ 不过后来变成了`worse is better`了，囧rz。。。

##迭代进化

	Our work has followed a two- to four-year cycle that can be seen to parallel the scientific method:

	Build an application program within the current system (make an observation)
	Based on that experience, redesign the language (formulate a theory)
	Build a new system based on the new design (make a prediction that can be tested)

迭代的开发风格，这个我喜欢；先做出个能跑起来的东西，然后在逐步迭代。

##人文理念

	Personal Mastery: If a system is to serve the creative spirit, it must be entirely comprehensible to a single individual.

个人计算机！貌似比乔帮主和盖茨超前啊~ 后来Alan Kay去了apple，也是继续他未完成的事；让Smalltalk计算机的精神在mac系统上得到了传承。

	Good Design: A system should be built with a minimum set of unchangeable parts; those parts should be as general as possible; and all parts of the system should be held in a uniform framework.

微内核加各种扩展，嗯嗯。Smalltalk语言连条件分支，循环之类的都统统由类库实现了；内核似乎只做了一件事：发送消息的对象~

##语言

	Purpose of Language: To provide a framework for communication.
语言是交流的手段，不仅仅是人和计算机，更重要的是人和人之间的交流。嗯嗯，这个也是kent beck反复说教的理念。

	Scope: The design of a language for using computers must deal with internal models, external media, and the interaction between these in both the human and the computer.

这个不是很理解，哪位看明白了给我解释解释~
	

## 相互通信的对象

关于面向对象的干货来了：）

- 对象
- 内存管理
- 消息
- 统一隐喻



		The mind observes a vast universe of experience, both immediate and recorded. One can derive a sense of oneness with the universe simply by letting this experience be, just as it is. However, if one wishes to participate, literally to take a part, in the universe, one must draw distinctions. In so doing one identifies an object in the universe, and simultaneously all the rest becomes not-that-object. Distinction by itself is a start, but the process of distinguishing does not get any easier. Every time you want to talk about "that chair over there", you must repeat the entire processes of distinguishing that chair. This is where the act of reference comes in: we can associate a unique identifier with an object, and, from that time on, only the mention of that identifier is necessary to refer to the original object. 
		We have said that a computer system should provide models that are compatible with those in the mind. Therefore:
		Objects: A computer language should support the concept of "object" and provide a uniform means for referring to the objects in its universe.
		

这一大段说对象概念的存在是为了对思维的模式进行模拟。代表人类思维中的某一个具体的事物。

	The Smalltalk storage manager provides an object-oriented model of memory for the entire system. Uniform reference is achieved simply by associating a unique integer with every object in the system. This uniformity is important because it means that variables in the system can take on widely differing values and yet can be implemented as simple memory cells. Objects are created when expressions are evaluated, and they can be passed around by uniform reference, so that no provision for their storage is necessary in the procedures that manipulate them. When all references to an object have disappeared from the system, the object itself vanishes, and its storage is reclaimed. Such behavior is essential to full support of the object metaphor:
	Storage Management: To be truly "object-oriented", a computer system must provide automatic storage management.

Smalltalk 存储管理提供对整个系统内存的对象化模型。对对象的寻址通过简单的整数即可以实现；这东西和C语言的指针，ruby的对象引用应该是一个概念。这样的存储模型，也就是将所有对象的内存单元都放在堆上，这样程序的各个部分可以创建和传递对象（实际上是对象的引用）。

接下来就涉及到了垃圾回收的问题，Smalltalk，java， ruby提供了自动化的垃圾回收。这种东西对程序员来讲当然是友好，但是却有可能形成性能瓶颈，传说中的死亡时间~。

而Objective-c则提供了另外一种思路，既ARC(automatic reference count), 也算的上是半自动的内存管理了。之所以说是半自动，是因为ARC不能自动处理引用循环，而容易造成内存泄露。在objective-c中最蛋疼的就是涉及到block的内存管理，而对block还真是舍不得弃用。从这方面想，oc还真是比ruby差了些，这时OC程序员只能以运行速度来宽慰自己了~

	A way to find out if a language is working well is to see if programs look like they are doing what they are doing. If they are sprinkled with statements that relate to the management of storage, then their internal model is not well matched to that of humans. Can you imagine having to prepare someone for each thing you tell them or having to inform them when you are through with a given topic and that it can be forgotten? 
    Each object in our universe has a life of its own. Similarly, the brain provides for independent processing along with storage of each mental object. This suggests a third principle of design:
	Messages: Computing should be viewed as an intrinsic capability of objects that can be uniformly invoked by sending messages.

Alan Key 曾经吐槽过：类啊，继承啊神马的都是浮云，发送消息的对象才是Smalltalk的本质所在~
前面的微内核部分，提过：Smalltalk内核似乎只提供了消息机制，所有的计算都是由相对象发送消息来完成的。像最基本的加法操作
```
	5 + 1
```
是这样来实现的，5是一个对象，相5发送“+”消息， 1是参数~ 这就是原文说的：

	Uniform Metaphor: A language should be designed around a powerful metaphor that can be uniformly applied in all areas.

另外一个有着强大统一理念的语言是LISP, 数据和程序都是链表~ Smalltalk的语法据说能写在半页纸上，而lisp根本就木有语法啊。。。

之前看过《java编程思想》尼玛，好厚的一本书啊~ 别的都没记住，就记得刚开始说“万物皆对象”，想在想起来像一句笑话。不过java好歹和Smalltalk神似，尼玛，不要跟我提C++好么~



## 程序组织

- 模块化
- 分类
- 多态
- 分解
- 杠杆效应
- 虚拟机

		
		A uniform metaphor provides a framework in which complex systems can be built. Several related organizational principles contribute to the successful management of complexity.

统一的隐喻为构建复杂系统提供支持。这几个组织原则帮助控制复杂性。

谁说的的来着？计算机科学的本质是控制复杂度~

	Modularity: No component in a complex system should depend on the internal details of any other component. 

首当其冲的原则是模块化。原文有图有真相；
一个有N个模块的系统，系统的复杂度可能达到N^2。减小复杂度的方法就是减少模块之间的相互依赖。消息解耦了消息的意图和方法实现。这个就是封装嘛~ 信息隐藏；

	Classification: A language must provide a means for classifying similar objects, and for adding new classes of objects on equal footing with the kernel classes of the system.
	
好吧，类终于出现了

	Classification is the objectification of nessness. In other words, when a human sees a chair, the experience is taken both literally an "that very thing" and abstractly as "that chair-like thing". Such abstraction results from the marvelous ability of the mind to merge "similar" experience, and this abstraction manifests itself as another object in the mind, the Platonic chair or chairness. 
    Classes are the chief mechanism for extension in Smalltalk. For instance, a music system would be created by adding new classes that describe the representation and interaction protocol of Note, Melody, Score, Timbre, Player, and so on. The "equal footing" clause of the above principle is important because it insures that the system will be used as it was designed. In other words, a melody could be represented as an ad hoc collection of Integers representing pitch, duration, and other parameters, but if the language can handle Notes as easily as Integers, then the user will naturally describe a melody as a collection of Notes. At each stage of design, a human will naturally choose the most effective representation if the system provides for it. The principle of modularity has an interesting implication for the procedural components in a system:

类是扩展smalltalk的主要机制。用类来表示概念，这点倒是C++的ADT概念别无二致，还是说C++的面相对象只有类？

	Polymorphism: A program should specify only the behavior of objects, not their representation.

毛主席教导我们说：多态是个好东西；当年教授们怎么说的？ 面向对象三大特性：继承，封装，多态。其实个人觉得把多态放在最后面太委屈他了；多态应该是面向对象最重要的特性，没有之一。

stackoverflow上有个问题说：OO和函数式编程有啥区别？
有人回答说，OO适用于有很多的数据类型，但是只有较少的几种操作。而函数式编程则恰恰相反：只有少数的几种类型，但是有很多的操作。（个人觉得他是在说Smalltalk和LISP的本质区别）

这个回答突出了多态的重要性。

还是想吐槽一下C++，这个贱人实在是矫情。要实现点多态吧，非要继承相同的鸡肋，还要虚函数神马的，我晕~ 吐槽归吐槽，C++的发明者其实根本就不鸟smalltalk，主要出发点是性能优先，全面兼容C。这都不是主要罪过，主要罪过是坑害好多计算机科班程序员，他们天真的认为面向对象就是C++，C++是最好最强大最灵活的语言，用不好说明功夫还不到，我了个去~

	Factoring: Each independent component in a system would appear in only one place.

factoring的有道解释是：分解；refactoring的标准中文释义是重构。这里强调的是不要有重复的东西，（don't repeat yourself）。个人觉得这应该是程序员的职业道德底线~ 其实真正操作起来并不是那么容易，某种程度上Smalltak不支持多继承就损害factoring的能力。后面展望的部分有讲述。

	Leverage: When a system is well factored, great leverage is available to users and implementers alike.

杠杆原理，嗯嗯。构造良好的系统能够形成杠杆效应。

这个包括两个层面的事情。一个对于系统的编程API，一个不错的例子是ruby的标准库。ruby对集合的操作那叫一个华丽丽啊（Smalltalk也不错的，可能没ruby那么多花样）。

另外一种好处是针对于最终用户的，应用程序之间可以交互，嗯嗯，还是那条基本的隐喻：消息机制嘛。用户可以多现有的应用程序进行合理的组合。主流的系统之中似乎只有mac os有这样的能力，苹果还实现了这一抽象层面上的语言：`applesript`。`quicksilver`也是这种机制的标志性应用；嗯嗯，没有装`quicksilver`的mac我是不会用的。。。


	Virtual Machine: A virtual machine specification establishes a framework for the application of technology.
	The Smalltalk virtual machine establishes an object-oriented model for storage, a message-oriented model for processing, and a bitmap model for visual display of information. Through the use of microcode, and ultimately hardware, system performance can be improved dramatically without any compromise to the other virtues of the system.

虚拟机，理想的计算机也应该是这样构造的吧， 这个想多了~

## 用户接口

- Reactive 原则
- 操作系统


		Reactive Principle: Every component accessible to the user should be able to present itself in a meaningful way for observation and manipulation.

说啥呢？是不是说应用可编程的事情？

		Operating System: An operating system is a collection of things that don't fit into a language. There shouldn't be one.
		Here are some examples of conventional operating system components that have been naturally incorporated into the Smalltalk language:
		Storage management -- Entirely automatic. Objects are created by a message to their class and reclaimed when no further references to them exist. Expansion of the address space through virtual memory is similarly transparent.
		File system -- Included in the normal framework through objects such as Files and Directories with message protocols that support file access.
		Display handling -- The display is simply an instance of class Form, which is continually visible, and the graphical manipulation messages defined in that class are used to change the visible image.
		Keyboard Input -- The user input devices are similarly modeled as objects with appropriate messages for determining their state or reading their history as a sequence of events.
		Access to subsystems -- Subsystems are naturally incorporated as independent objects within Smalltalk: there they can draw on the large existing universe of description, and those that involve interaction with the user can participate as components in the user interface.
		Debugger -- The state of the Smalltalk processor is accessible as an instance of class Process that owns a chain of stack frames. The debugger is just a Smalltalk subsystem that has access to manipulate the state of a suspended process. It should be noted that nearly the only run-time error that can occur in Smalltalk is for a message not to be recognized by its receiver.
		    Smalltalk has no "operation system" as such. The necessary primitive operations, such as reading a page from the disk, are incorporated as primitive methods in response to otherwise normal Smalltalk messages.

Smalltalk 不需要操作系统~
## 展望

- 多继承
- 消息协议
- 性能问题
- 自然选择

		As might be expected, work remains to be done on Smalltalk. The easiest part to describe is the continued application of the principles in this paper. For example, the Smalltalk-80 system falls short in its factoring because it supports only hierarchical inheritance. Future Smalltalk systems will generalize this model to arbitrary (multiple) inheritance. Also, message protocols have not been formalized. The organization provides for protocols, but it is currently only a matter of style for protocols to be consistent from one class to another. This can be remedied easily by providing proper protocol objects that can be consistently shared. This will then allow formal typing of variables by protocol without losing the advantages of polymorphism. 
	    The other remaining work is less easy to articulate. There are clearly other aspects to human thought that have not been addressed in this paper. These must be identified as metaphors that can complement the existing models of the language. 

	Even as the clock ticks, better and better computer support for the creative spirit is evolving. Help is on the way.
	
作者觉得为了更好的factoring，smalltalk似乎应该支持多继承。嗯嗯，ruby也是这么想的，但是多继承也有各种问题，所以ruby实现了mixin机制。Objective-c不支持多继承也不支持mixin，有时候有重复的逻辑也实在是无可奈何。

作者还认为“未来的”smalltalk应该支持一种对象来显示的表示协议。嗯嗯，objective-c有@protocol, java有接口，都是这种想法的实现。ruby木有，ruby支持者可能认为ruby是动态语言芸芸，其实未必见得是优势，如果有这么一种机制，想必能少些不少的单元测试吧~

	    Sometimes the advance of computer systems seems depressingly slow. We forget that steam engines were high-tech to our grandparents. I am optimistic about the situation. Computer systems are, in fact, getting simpler and, as a result, more usable. I would like to close with a general principle which governs this process:
	Natural Selection: Languages and systems that are of sound design will persist, to be supplanted only by better ones.
	
显然，当时smalltalk的运行速度并不好，作者坚信这不是问题；后来人来看，smalltak远远的超越了那个时代，而性能问题也确实阻碍的smalltalk的发展。smalltalk似乎已经死掉了，但是smalltalk的理念却不会死。现在主流语言或多或少都受了smalltalk和lisp的影响。smalltalk的现实妥协版实现：objective-c正伴随着iOS发展得如日中天，mac也继承了smalltalk的理念继续领先于时代。而smalltalk的畸形版本实现：ruby也焕发着勃勃生机。

