#### NSNotification是同步还是异步

在抛出通知以后，观察者在通知事件处理完成以后，抛出者才会往下继续执行，也就是说这个过程默认是同步的；当发送通知时，通知中心会一直等待所有的 observer 都收到并且处理了通知才会返回到 poster；

#### 改同步为异步

1. 通知事件处理方法在子线程中执行

   ```objc
   // 注册通知
       [[NSNotificationCenter defaultCenter] addObserver:self
                                                selector:@selector(actionNotification:)
                                                    name:kNotificationName
                                                  object:nil];
   - (void) actionNotification: (NSNotification*)notification
   {
       dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
           NSString* message = notification.object;
           NSLog(@"%@",message);
           sleep(3);
           NSLog(@"通知说话结束:%@",[NSThread currentThread]);
       });
   }
   ```

   

2. 可以通过 ` NSNotificationQueue` 的` enqueueNotification: postingStyle:` 和 `enqueueNotification: postingStyle: coalesceMask: forModes: ` 方法将通告放入队列，实现异步发送，在把通告放入队列之后，这些方法会立即将控制权返回给调用对象。

   ``` objc
   - (void)buttonDown {
       NSNotification *notification = [NSNotification notificationWithName:kNotificationName object:@"通知说话开始"];
       [[NSNotificationQueue defaultQueue] enqueueNotification:notification
                                                  postingStyle:NSPostASAP];
   //    [[NSNotificationCenter defaultCenter] postNotificationName:kNotificationName object:@"通知说话开始"];
       NSLog(@"按钮说话");
   }
   ```

   
