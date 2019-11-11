### KVC

+ KVC 是一种可以直接通过字符串的名字 key 来访问类属性的机制，而不是通过调用 setter、getter 方法去访问。
+ 我们可以通过在运行时动态的访问和修改对象的属性。而不是在编译时确定，KVC 是 iOS 开发中的黑魔法之一。

### KVC 实现细节

```objc
- (void)setValue:(id)value forKey:(NSString *)key;
```

![](https://raw.githubusercontent.com/TooXu/resources/master/Images/kvc_set.png)


1. 首先搜索 setter 方法，有就直接赋值。
2. 如果上面的 setter 方法没有找到，再检查类方法 `+ (BOOL)accessInstanceVariablesDirectly`

   > 1. 返回 NO，则执行`setValue：forUNdefinedKey：`
   > 2. 返回 YES，则按`_<key>，_<isKey>，<key>，<isKey>`的顺序搜索成员名。

3. 还没有找到的话，就调用`setValue:forUndefinedKey:`

```objc
- (id)valueForKey:(NSString *)key;
```

![](https://raw.githubusercontent.com/TooXu/resources/master/Images/kvc_get.png)

1. 首先查找 getter 方法，找到直接调用。如果是 bool、int、float 等基本数据类型，会做 NSNumber 的转换。

2. 如果没查到，再检查类方法`+ (BOOL)accessInstanceVariablesDirectly`

   > 1. 返回 NO，则执行`valueForUNdefinedKey:`
   > 2. 返回 YES，则按`_<key>,_is<Key>,<key>,is<Key>`的顺序搜索成员名。
   
3. 还没有找到的话，调用`valueForUndefinedKey:`

### KVC 与点语法比较

1. 用点语法编译器会做预编译检查，访问不存在的属性编译器会报错，但是用 KVC 方式编译器无法做检查，如果有错误只能运行的时候才能发现（crash）。
2. 相比点语法用 KVC 方式 KVC 的效率会稍低一点，但是灵活，可以在程序运行时决定访问哪些属性。
3. 用 KVC 可以访问对象的私有成员变量。

### KVC 应用

**字典转模型**

```objc
- (void)setValuesForKeysWithDictionary:(NSDictionary *)keyedValues;
```

**集合类操作符**

```objc
[products valueForKeyPath:@"@count"]; //返回一个值为集合中对象总数的NSNumber对象。
[products valueForKeyPath:@"@sum.price"]; //首先把集合中的每个对象都转换为double类型，然后计算其总，最后返回一个值为这个总和的NSNumber对象。
[products valueForKeyPath:@"@avg.price"];//首先把集合中的每个对象都转换为double类型，然后计算其平均值，最后返回一个值为该平均值的NSNumber对象。
[products valueForKeyPath:@"@max.price"];//使用compare:方法来确定最大值。所以为了让其正常工作，集合中所有的对象都必须支持和另一个对象的比较。
[products valueForKeyPath:@"@min.launchedOn"]; //返回的是集合中的最小值。
```



[kvc 集合操作](https://www.ibm.com/developerworks/cn/java/j-lo-deadlock/index.html)

**修改私有属性**

```objc
[_textField setValue:[UIColor redColor] forKeyPath:@"_placeholderLabel.textColor"];   
```



通过KVC修改属性会触发KVO么？

会触发KVO，调用了 set 方法