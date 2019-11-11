## 宏

> ​	 特点: 预编译 (编译前就会被出来)  

> ​	 编译检查: 不会检查 不会报编译错误 只是替换

> ​	优点: 能定义一些函数 方法

> ​	坏处: 大量使用时, 会是编译时间变长 需要做大量替换



## Const

​	特点: 

> 编译阶段处理

> 会做编译检查 会报编译错误

> 不能修饰方法 或函数

作用: 

1. 用来修饰右边的变量 ( `基本数据变量`  `指针变量 *p` ) 
2. 被 const 修饰的变量是只读的. 强调其不可修改.

```c
// 这两种写法是一样的, const 只修饰右边的基本变量 b
const int b = 20;
int const b = 20;
```



```c
// const 修饰指针变量访问的内存空间, 修饰的是右边*p1
// 两种方式一样
const int *p1;
int const *p1
// const 修饰 指针变量 p1
int *const p1

```

```c	
// 第二个 const 修饰 p1
const int * const p1;
int const * const p1;	
```


​	

堆和栈都是在动态存储区

## static

- 修饰局部变量

1. 延长局部变量的生命周期, 程序结束时才会销毁.

> 局部变量 是存储在栈区的, 生命周期是整个代码块, 作用域也是整个代码块. 出了这个代码块, 存储局部变量的内存就会被回收, 局部变量也就被销毁了 

2. 局部变量只会生成一份内存, 只会初始化一次.

> 当用 static 修饰局部变量时, 变量变成 `静态局部变量`, 储存在静态储存区. 这块内存直到程序结束才会销毁. 也就是说 `静态局部变量` 的生命周期是整个源程序. 但是只在声明他的代码块可见, 也就数说他的`作用域`是声明他的代码块. 

- 修饰全局变量

当全局变量`没有`使用 static 修饰符时, 其存储在静态存储区域, 直到程序结束时才销毁. 作用域是整个源程序. 

`static` 修饰全局变量, 会改变其`作用域`为当前文件内部.

可以使用 `extern` 关键字来引用这个全局变量.

## extern

`作用` 声明外部全局变量 (声明要调用外部变量了)

`特点` 只能用于声明 不能用于实现, 定义 和 分配内存都在原来类中.

```objc
// ViewController.m
#import "ViewController.h"
extern NSString *testExtern;
@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@" testExtern == %@",testExtern);
}
@end
```

```objc
// ExternTest.m
NSString *testExtern = @" extern Global Variable";
@implementation ExternTest
@end
```

```c
	// log
"testExtern ==  extern Global Variable"
```







