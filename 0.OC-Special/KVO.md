#### KVO 实现原理

![kvo principle](https://github.com/TooXu/resources/blob/master/Images/kvo principle.png?raw=true)

> 当你观察一个对象时，一个新的类会被动态创建。这个类继承自该对象的原本的类，并重写了被观察属性的 setter 方法。重写的 setter 方法会负责在调用原 setter 方法之前和之后，通知所有观察对象：值的更改。最后通过 `isa 混写（isa-swizzling）` 把这个对象的 isa 指针 ( isa 指针告诉 Runtime 系统这个对象的类是什么 ) 指向这个新创建的子类，对象就神奇的变成了新创建的子类的实例。

第一次对一个对象调用 `addObserver:forKeyPath:options:context:` 时，框架会创建这个类的新的 **KVO** 子类，并将被观察对象转换为新子类的对象。在这个 **KVO** 特殊子类中， Cocoa 创建观察属性的 setter ，大致工作原理如下:

```objc
- (void)setNow:(NSDate *)aDate {
   [self willChangeValueForKey:@"now"];
   [super setValue:aDate forKey:@"now"];
   [self didChangeValueForKey:@"now"];
}
// 这种继承和方法注入是在运行时而不是编译时实现的
```

KVO 在实现中通过 `isa 混写（isa-swizzling）` 把这个对象的 isa 指针 ( isa 指针告诉 Runtime 系统这个对象的类是什么 ) 指向这个新创建的子类，对象就神奇的变成了新创建的子类的实例。

Apple 还重写、覆盖了 `-class` 方法并返回原来的类。 企图欺骗我们：这个类没有变，就是原本那个类。

#### 手动实现 KVO 

`addObserver:forKey:withBlock:`

1. 检查对象的类有没有相应的 setter 方法。如果没有抛出异常；
2. 检查对象 `isa` 指向的类是不是一个 KVO 类。如果不是，新建一个继承原来类的子类，并把 `isa` 指向这个新建的子类；
3. 检查对象的 KVO 类重写过没有这个 setter 方法。如果没有，添加重写的 setter 方法；
4. 添加这个观察者

#### 小结

**KVO**作为`Objective-C`中两个对象间通信机制中的一种，提供了一种非常强大的机制。在经典的MVC架构中，控制器需要确保**视图**与**模型**的同步，当`model`对象改变时，视图应该随之改变以反映模型的变化；当用户和控制器交互的时候，模型也应该做出相应的改变。而KVO便为我们提供了这样一种同步机制：我们让控制器去监听一个`model`对象属性的改变，并根据这种改变来更新我们的视图。所有，有效地使用KVO，对我们应用的开发意义重大。

**一个需要注意的地方是**，KVO 行为是**同步**的，并且发生与所观察的值发生变化的同样的线程上。当我们试图从其他线程改变属性值的时候我们应当十分小心，除非能确定所有的观察者都用线程安全的方法处理 KVO 通知。不推荐把 KVO 和多线程混起来。如果我们要用多个队列和线程，我们不应该在它们互相之间用 KVO。



#### blog:

[南峰子](http://southpeak.github.io/2015/04/23/cocoa-foundation-nskeyvalueobserving/)

[objc中国](https://objccn.io/issue-7-3/)

[手动实现 KVO](https://tech.glowing.com/cn/implement-kvo/)

[实例变量的 KVO 监听](https://yq.aliyun.com/articles/30483)

[面试题中的 KVO](https://github.com/ChenYilong/iOSInterviewQuestions/blob/master/01%E3%80%8A%E6%8B%9B%E8%81%98%E4%B8%80%E4%B8%AA%E9%9D%A0%E8%B0%B1%E7%9A%84iOS%E3%80%8B%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88/%E3%80%8A%E6%8B%9B%E8%81%98%E4%B8%80%E4%B8%AA%E9%9D%A0%E8%B0%B1%E7%9A%84iOS%E3%80%8B%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88%EF%BC%88%E4%B8%8B%EF%BC%89.md#45-addobserverforkeypathoptionscontext%E5%90%84%E4%B8%AA%E5%8F%82%E6%95%B0%E7%9A%84%E4%BD%9C%E7%94%A8%E5%88%86%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88observer%E4%B8%AD%E9%9C%80%E8%A6%81%E5%AE%9E%E7%8E%B0%E5%93%AA%E4%B8%AA%E6%96%B9%E6%B3%95%E6%89%8D%E8%83%BD%E8%8E%B7%E5%BE%97kvo%E5%9B%9E%E8%B0%83)

[优雅的使用 KVO](https://draveness.me/kvocontroller)

[巧妙利用KVO实现精准的VC耗时检测](https://punmy.cn/2018/06/18/15278496835424.html)

[一种基于KVO的页面加载，渲染耗时监控方法](http://satanwoo.github.io/2017/11/27/KVO-Swizzle/)

[KVO进阶](https://www.jianshu.com/p/a8809c1eaecc)

[用代码探讨 KVC/KVO 的实现原理](https://juejin.im/post/5ac5f4b46fb9a028d5675645#heading-2)