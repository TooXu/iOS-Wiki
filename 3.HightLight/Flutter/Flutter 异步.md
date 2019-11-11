# Flutter 异步



### dart 是单线程的

持续执行，不允许抢占

支持异步 (单线程也可以异步？？)



### Event Loop



dart 事件循环中有两个队列： 

1. 微任务队列 (MicroTask queue)
   - 包含有 Dart 内部的微任务，通过 scheduleMicrotask 来调度
2. 事件队列(Event queue)
   - 包含外部事件 例如 I/O  timer  绘制事件

事件循环的运行规则：

1. 首先处理所有微任务队列里的微任务
2. 处理完微任务后。从事件队列里取 1 个事件进行处理
3. 回到微任务队列继续循环







### 