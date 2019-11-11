### category

[结合 category 工作原理分析 OC2.0 中的 runtime](https://bestswifter.com/runtime-category/)

是 OC 2.0 之后添加的语言特性

```objc
struct category_t {
    const char *name;
    classref_t cls;
    struct method_list_t *instanceMethods;
    struct method_list_t *classMethods;
    struct protocol_list_t *protocols;
    struct property_list_t *instanceProperties;
    // Fields below this point are not always present on disk.
    struct property_list_t *_classProperties;

    method_list_t *methodsForMeta(bool isMeta) {
        if (isMeta) return classMethods;
        else return instanceMethods;
    }
    property_list_t *propertiesForMeta(bool isMeta, struct header_info *hi);
};
```



##### 作用:

1. 为已存在的类添加方法,
2. 可以把类的实现分在几个不同的文件里.
   + 可以减少单个文件的体积
   + 可以把不同的功能组织到不同的 category 里 (解耦 )
   + 可以由多个开发者共同完成一个类
   + 可以按需加载想要的 category.
3. 声明私有方法
4. 模拟多继承
5. 把 framework 的私有方法公开

##### 通过编译源码分析 category

1. category 的方法没有`完全替换`掉原来类中已有的方法. 

2. category 的方法被放到了方法列表的前面, 原来类的方法被放到了方法列表的后面. 平常所说的 `方法覆盖`,是因为运行时在查找方法的时候是顺着方法列表的顺序查找的, 它只要一找到对应的名字的方法, 就会停止,并返回该方法.

   (插入方发时,用头插法,新的方法在链表的前面,会被优先遍历到)

3. 把 category 的类方法和协议添加到类的 metaclass 上

##### category 中的 load 方法

两个问题:

1. 在类的 +load 方法调用的时候, 可以调用 category 中声明的方法吗?

   可以, 因为 category 的方法加载到类的方法列中的工作会先于 +load 方法的执行

2. category 的 +load 的, 调用顺序

   +load 的调用顺序是 先原类, 后 category , category 的 +load 执行顺序根据编译顺序决定 

   对于 `覆盖`掉的方法, 会找到最后一个编译的 category 里的对应方法.

##### 调用原类中被`覆盖`的方法

只要顺着方法列表找到 `最后一个` 对应名字的方法,就可以调用原类的方法

```objc
Class currentClass = [MyClass class];
MyClass *my = [[MyClass alloc] init];

if (currentClass) {
    unsigned int methodCount;
    Method *methodList = class_copyMethodList(currentClass, &methodCount);
    IMP lastImp = NULL;
    SEL lastSel = NULL;
    for (NSInteger i = 0; i < methodCount; i++) {
        Method method = methodList[i];
        NSString *methodName = [NSString stringWithCString:
                                		sel_getName(method_getName(method)) 
                                        encoding:NSUTF8StringEncoding];
        if ([@"printName" isEqualToString:methodName]) {
            lastImp = method_getImplementation(method);
            lastSel = method_getName(method);
        }
    }
    typedef void (*fn)(id,SEL);

    if (lastImp != NULL) {
        fn f = (fn)lastImp;
        f(my,lastSel);
    }
    free(methodList);
}
```

##### category 添加属性

```objc
#import "MyClass.h"
@interface MyClass (Category1)
@property(nonatomic,copy) NSString *name;
@end
```



```objc
#import "MyClass+Category1.h"
#import <objc/runtime.h>

@implementation MyClass (Category1)

+ (void)load {
    NSLog(@"%@",@"load in Category1");
}

- (void)setName:(NSString *)name {
    objc_setAssociatedObject(self,
                             "name",
                             name,
                             OBJC_ASSOCIATION_COPY);
}

- (NSString*)name {
    NSString *nameObject = objc_getAssociatedObject(self, "name");
    return nameObject;
}

@end
```



##### category 与 extension 的区别

> extension

​		看起来像 匿名的 category 但是和 category 是两个东西. extension 在编译器决议, 它就是类的一部分, 在编译期和.h文件里的 @interface 以及 .m 文件里的 @implementation 一起形成一个完整的类, 它伴随着类的产生而产生, 亦随之消亡. 

​	作用: 一般用来隐藏类的私有信息, 必须有一个类的源码才能添加 extension ,所以无法为系统的类添加 extension

> category

​	category 是在运行期决议的. 就 category 和 extension 的区别来看, extension 可以添加实例变量, category 无法添加实例变量.(原因: 运行期, 对象的内存布局已经确定, 如果添加实例变量就会破坏类的内部布局.)