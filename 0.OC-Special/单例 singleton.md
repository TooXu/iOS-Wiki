// 单例

```objective-c
shareInstance {
	static id instance = nil
	static dispatch_once_t onceToken;
	dispatch_once(&onceToken, ^{
    instance = [self new];
  });
  return
}
```



