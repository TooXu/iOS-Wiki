##什么是 SEL， IMP，Method, _cmd

### SEL

> SEL 又叫选择器，是方法的 `selector` 的指针
>
 ```objc
 typedef struct objc_selector *SEL;
 ```

> objc_selector 是一个映射到方法的 C 字符串. 需要注意的是 @selector( ) 选择子 只与函数名有关. 不同类终端相同名字的方法所对应的方法选择器是相同的, 即使方法名字相同 而变量类型不同也会导致它们具有相同的方法选择器. 由于这点特性, oc 不支持函数重载.

> 本质上，`SEL`只是一个指向方法的指针（准确的说，只是一个根据方法名`hash`化了的`KEY`值，能唯一代表一个方法），它的存在只是为了加快方法的查询速度。

我们可以在运行时添加新的 `selector` , 也可以在运行时获取已存在的 `selector`, 我们可以通过三种方法来获取 `SEL`
1. `sel_registerName` 函数
 	2. OC 编译器提供的 `@selector()`
 	3. `NSSelectorFromString()` 方法

### **_cmd**

> **_cmd**在Objective-C的方法中表示当前方法的**selector**，正如同**self**表示当前方法调用的对象实例。

### IMP

> 实际上是一个函数指针，指向方法的实现的首地址。代表了方法的最终实现

```objc
id (*IMP)(id, SEL, ...)
```

第一个参数: 指向 `self` 的指针 

> 如果是 `实例方法`, 则是 `类实例` 的内存地址; 
>
> 如果是 `类方法`,  则是指向`元类`的指针 ), 

第二个参数: 方法选择器 ( `selector` ).

通过取得 `IMP` , 我们可以跳过 `Runtime` 的消息传递机制, 直接执行 `IMP` 指向的函数实现, 这样省去了 `Runtime` 消息传递过程中所做的一系列查找操作, 会比直接向对象发送消息高效一些.

> 在runtime中消息传递和转发的目的就是为了找到IMP，并执行函数。

# Method

```objc
struct method_t {
    SEL name;
    const char *types;
    MethodListIMP imp;

    struct SortBySELAddress :
        public std::binary_function<const method_t&,
                                    const method_t&, bool>
    {
        bool operator() (const method_t& lhs,
                         const method_t& rhs)
        { return lhs.name < rhs.name; }
    };
};	
```

 `-[XXObject hello]` 方法的结构体是这样的：

```objc
name = "hello"                            // 方法名字
types = 0x0000000100000fa4 "v16@0:8"      // 类型编码
imp = 0x0000000100000e90 (method`-[XXObject hello] at XXObject.m:13
```

