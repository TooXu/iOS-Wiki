## 概念

### 1. 什么是RunLoop

简单理解就是⼀个⽆限循环，但操作系统的⽆限循环与算法中的死循环不⼀样，如果你了解过Windows 下的消息循环机制，其实RunLoop与它类似，这个循环不断地运⾏着“等待——>接收事件——>处理事件”，直到这个循环被杀掉。

### 2. 作用

- 保持程序的持续运⾏(⽐如主运⾏循环)
- 处理App中的各种事件（⽐如触摸事件、定时器事件、Selector事件）
- 节省CPU资源，提⾼程序性能：该做事时做事，该休息时休息

RunLoop在OSX/iOS具体表现

- CFRunLoopRef

  CFRunLoopRef 是在 CoreFoundation 框架内的，它提供了纯 C 函数的 API，所有这些 API 都是线程安全的。

- NSRunLoop

  NSRunLoop 是基于 CFRunLoopRef 的封装，提供了⾯向对象的 API， 但是这些 API 不是线程安全的。跨线程调⽤的时候可能会崩溃

  ```objc
  
  void CFRunLoopRun(void) {
      int32_t result; /* DOES CALLOUT */
      do {
          result = CFRunLoopRunSpecific(CFRunLoopGetCurrent(), kCFRunLoopDefaultMode, 1.0e10, false);
          CHECK_FOR_FORK();
      } while (kCFRunLoopRunStopped != result && kCFRunLoopRunFinished != result);
  }
  
  //这种循环和我们普通的循环⼜不⼀样，他并不是⼀种完全占⽤线程的循环。即在需要他⼯作的时候他才会⼯作，其他时候是处于“睡眠”状态以避免资源占⽤
  ```

  

## RunLoop的获取及与线程之间的关系

## RunLoop 的开始、运⾏逻辑与结束

### NSRunloop

![](https://raw.githubusercontent.com/TooXu/resources/master/Images/runloop.png)



## Runloop 应用

#### mode模式的应⽤：利⽤mode互斥关系

- Image加载时机，GIF动画滑动时是否继续播放：如果UITablevIewCell的UIImageView需要加载尺⼨较⼤的本地图⽚，setImage时也会对图⽚进⾏解码，如果设置图⽚的过 程对列表滑动降低到最⼩可将设置图⽚事件放在NSDefaultRunLoopMode下，UITableView滑动时，RunLoop的Mode在UITrackingRunLoopMode，这样可以简单优化⽤户体验 。对应的主要⽅法为：

  ```objc
  [UIImageView performSelector:@selector(setImage:) withObject:[UIImage imageNamed:@"XXX"] afterDelay:0.0 inModes:@[NSDefaultRunloopMode]];
  ```

  > sdwebimage 等三⽅图⽚加载库对图⽚的下载、解码等操作均已在⼦线程完成，对滑动影响不⼤

  

- **Timer**加⼊哪个mode在滑动时**是否需要继续计时**，如我们如果使⽤scheduledTimerWithTimeInterval: 初始化Timer，则Timer只会在NSDefaultRunLoopMode下执⾏，如 果timer需要执⾏的任务⽐较耗时，则可避免影响滑动体验。如果想要Timer在滑动时也能执⾏，可以使⽤timerWithTimeInterval:⽅法，并将其加⼊ NSRunLoopCommonModes当中

#### RunLoop本身的应⽤

- 线程保活（AFNetworking2.0的常驻线程、使⽤EarlGrey延时或保活）
  - 保证线程的长时间存活 (如果程序中，需要经常在子线程中执行任务，频繁的创建和销毁线程，会造成资源的浪费。这时候我们就可以使用RunLoop来让该线程长时间存活而不被销毁。)

#### RunLoop时机的利⽤：

- 卡顿监控（fps监控、CPU 占⽤率很⾼、主线程 RunLoop执⾏时间过⻓）
- 

## Blog

[掘金Runloop 系列](https://juejin.im/post/5d1ab117f265da1b80205ed1)

[RunLoop总结与面试](https://juejin.im/post/5c9e28ddf265da307261efff)

[RunLoop实战：实时卡顿监控](https://juejin.im/post/5cacb2baf265da03904bf93b)

[深入理解 Runloop](https://blog.ibireme.com/2015/05/18/runloop/)

[sunnyxx 大神视频](https://www.jianshu.com/p/929d855c5a5a)

[sunnyxx guo 总结](https://blog.gocy.tech/2016/09/03/runloop-source-reading/)

[解密 runloop](http://mrpeak.cn/blog/ios-runloop/)



## Timer

[iOS Target-Action模式下内存泄露问题深入探究](https://www.jianshu.com/p/b1bb87d68de5)

堆栈信息中, 对于`_target`成员变量，在 `UIControlTargetAction ` 的初始化方法中调用了`objc_storeWeak`，即这个成员变量对外部传进来的 `target` 对象是以 `weak` 的方式引用的。

其实在 `UIControl `的文档中，`addTarget:action:forControlEvents:` 方法的说明还有这么一句：

> When you call this method, target is not retained.

另外，如果我们以同一组 target-action 和 event 多次调用 `addTarget:action:forControlEvents:` 方法，在 `_targetActions` 中并不会重复添加 `UIControlTargetAction` 对象。

NSTimer的 scheduledTimerWithTimeInterval：target 方法内部不会判断修饰target的关键字，所以这里传self 和 weakSelf是没区别的，其内部会对target进行强引用，还是会产生循环引用。 

我们可以看到在UIControl下的target的持有方式确实是weakRetained弱持有的方式解开了引用循环，所以我们在使用时不会出现引用循环的问题。但是在NSTimer下，我看到的堆栈信息中看到这行代码的时候，开始明白机制的原理了，在NSTimer机制下对Target持有的方式使用的是autorelease的方式，也就是说target会在runloop下一次执行的时候查看这块区域是否进行释放，这也就能解释为什么我们如果将repeats属性设置成NO内存可以释放的原因，以及为什么将self设置成nil后内存依然不释放的原因。接下来我对invalidate方法打印堆栈信息，但是我发现没有对应方法的堆栈信息，反而会再次调用addtarget方法，这是我联想到NSTimer的官方文档中有说明，一旦调用了invalidate方法之后，这个timer就不能再使用，我认为底层这个时候就是个当前的timer进行了一个target的重定向，正好执行一次runloop的timerobserver监听，将之前的内存释放掉了，然后解开了引用的循环，现在我们已经明白了原理，那么我们就从原理出发，看看现有的解决方案是否合理。

```objc
// 中间代理弱引用

@interface DYWeakTimerTarget : NSObject

@property(nonatomic, weak) id target;
@property(nonatomic, assign) SEL selector;
@property(nonatomic, weak) NSTimer *timer;

@end

@implementation DYWeakTimerTarget

- (void)fire:(NSTimer *)timer {
    if (self.target) {
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Warc-performSelector-leaks"
        [self.target performSelector:self.selector withObject:timer.userInfo afterDelay:0.0f];
#pragma clang diagnostic pop
    } else {
        [self.timer invalidate];
    }
}

@end

@implementation DYWeakTimer

+ (NSTimer *) scheduledTimerWithTimeInterval:(NSTimeInterval)interval
                                      target:(id)aTarget
                                    selector:(SEL)aSelector
                                    userInfo:(id)userInfo
                                     repeats:(BOOL)repeats {
    DYWeakTimerTarget *timerTarget = [DYWeakTimerTarget new];
    timerTarget.target = aTarget;
    timerTarget.selector = aSelector;
    timerTarget.timer = [NSTimer scheduledTimerWithTimeInterval:interval
                                                         target:timerTarget
                                                       selector:@selector(fire:)
                                                       userInfo:userInfo
                                                        repeats:repeats];
    return timerTarget.timer;
}
@end
```

```objc
// 为 timer 创建分类 解决循环引用
//调用者是NSTImer自己，只是NSTimer捕获了参数block。这样我们在使用timer时，由于target的改变，就不再有循环引用了。
#import "NSTimer+TimerBlock.h"

@implementation NSTimer (TimerBlock)
+ (NSTimer *)block_TimerWithTimeInterval:(NSTimeInterval)interval 
  																 block:(void (^)())block 
                                 repeats:(BOOL)reqeats{
    return [self timerWithTimeInterval:interval 
            										target:self 
            									selector:@selector(blockSelector:) 
            									userInfo:[block copy] 
            									repeats:reqeats];
}

+ (void) blockSelector:(NSTimer *)timer{
    void (^block)() = timer.userInfo;
    if (block) {
        block();
    }
}
@end
```

