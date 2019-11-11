## id 和 instanceType

### 关联返回类型和非关联返回类型。

#### 1. 关联返回类型

根据Cocoa的命名规则，满足下述规则的方法：
（1）类方法中，以alloc或new开头
（2）实例方法中，以autorelease，init，retain或self开头
会返回一个方法所在类类型的对象，这些方法就被称为是关联返回类型的方法。换句话说，这些方法的返回结果以方法所在的类为类型。

#### 2. 非关联返回类型

```
@interface NSArray  
+ (id)constructAnArray;  
@end
```

当我们使用如下方式初始化NSArray时：

```
[NSArray constructAnArray];
```

根据Cocoa的方法命名规范，得到的返回类型就和方法声明的返回类型一样，是id。

但是如果使用instancetype作为返回类型，如下：

```
@interface NSArray  
+ (instancetype)constructAnArray;  
@end
```

当使用相同方式初始化 NSArray 时：

```
[NSArray constructAnArray];
```

得到的返回类型和方法所在类的类型相同，是 NSArray* !

总结一下，instancetype的作用，就是使那些非关联返回类型的方法返回所在类的类型！

#### 3 **instancetype和id区别**

1. instancetype在编译的时候可以判断对象的真实类型

2. 如果init方法的返回值是instancetype,那么将返回值赋值给一个其它的对象会报一个警告

   如果是在以前, init的返回值是id,那么将init返回的对象地址赋值给其它对象是不会报错的。

3. id可以用来定义变量, 可以作为返回值, 可以作为形参，instancetype只能用于作为返回值