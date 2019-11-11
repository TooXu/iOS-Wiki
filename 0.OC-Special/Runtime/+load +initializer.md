## load

> + load 调用顺序
>
> + load 是如何被调用的
> + load 为什么会有这种调用顺序

OC 在初始化的时候, 会去加载 map_images, map_images 最终会调用 objc_runtime_new.mm 里面的 _read_images 方法. _read_images 方法里面会去初始化内存中的 map, 这个时候将会 load 所有的类, 协议, category. NSObject 的 load 方法就是这个时候调用的.

 #### 时机

load 方法会在类加载的时候就被调用（APP 启动的时候，会调用每个类的+load方法）

会在类或者分类添加到 oc-runtime 时调用, 该调用发生在 `application:willFinishLaunchingWithOption:` 调用之前调用.

`+load`是我们可以接触到的调用时间最靠前的方法, 在 `main( )`函数之前, `+load`就会被调用.

只会在程序调用期间调用一次, 最重要的是, 如果在类与分类中都实现了 `load` 方法, 他们都会被调用, 不像其它的在分类中实现的方法在分类实现中的方法会被覆盖.

#### 分类的 load 不会覆盖本类的 load 的原因

```c
//分类的 load 不会覆盖本类 load 的原因
// 类与分类的 load 调用原理不一样
do {
  while(loadable_classes_used>0){
    call_class_loads();
  }
  more_categories = call_category_loads();
}while(loadable_classes_used>0 || more_categories);
```

```c
// call_category_loads 
// 判断类是否已经调用了 load 方法
if(cls && cls->isloadable()){
  (*load_method)(cls, SEL _cmd);
  cats[i].cat = nil;
}
```

### 准备 load 方法 消费 load 方法

-----

方法的调用过程也分为两个部分,

-  `准备 load 方法`   将未加载的 类添加到 loadable_classes (loadable_categories) 数组中. 将镜像加载到运行时,对`load`方法的准备就完成了
-  `调用 laod 方法`, 准备完成后 执行 call_load_methods( ), 开始调用 load 方法

### 加载管理  生产 loadable_class 消费 loadable_class

-------

objc 对于加载的管理,主要使用了两个列表, 分别是 laodable_classes 和 loadable_categories 



在 `load`调用之前, 所有的 framework 都已经加载到了运行时中. 所以调用 framework 中的方法是安全的.

> final: load 方法是如何被调用的
>
> 当 oc 运行时初始化的时候, 会通过 dyld_register_image_state_change_handler 在每次有新的镜像加入运行时的时候,进行回调. 执行 load_image 将所有包含 load 方法的文件加入列表 loadable_class, 然后从这个列表中找到 load 方法的实现, 调用 load 方法

[draveness](https://github.com/draveness/analyze/blob/master/contents/objc/%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3%20load%20%E6%96%B9%E6%B3%95%E4%B9%88%EF%BC%9F.md)

父类的 +load 方法 会在他的所有子类的 +load 方法之前执行。

分类的 +load 方法会在它的主类的 +load 方法之后执行。

--------------

----------------

## initialize

+initialize 方法是在类或它的子类收到第一条消息（实例方法 和 类方法）之前被调用的。也就是说 +initialize 是以懒加载的方式调用的。如果程序一直没有给某个类或它的子类发送消息, 那么这个类的 initialize 方法是永远也不会被调用的.

##### 作用: 一般只会在 initialize 方法中进行一些常量的初始化.

1. 当第一次向类发送消息的时候, 会调用 +initialize 方法, 如果像普通函数一样,直接调用 +load 方法, 则会引起 +initialize 的调用,且 initialize 在 load 方法之前调用

2. 在 app 的生命周期内, 某个类的 initialize 方法只会被调用一次. 若父类实现了 initialize 方法, 当子类未实现 initialize 方法. 或者子类在 initialize 方法中显式的调用了 [super initialize]方法, 则父类的 initialize 方法会被调用两次.

3. 若分类实现了 initialize 方法, 则会覆盖掉原类中的 initialize方法.

4. 父类的 initialize 方法 先于子类的 initialize 方法被调用.

5. 在initialize方法收到调用时,运行环境基本健全。 initialize内部也使用了锁，所以是线程安全的。但同时要避免阻塞线程，不要再使用锁

   

   

   #####[initialize会被调用多次](https://kingcos.me/posts/2019/+initialize_in_ios/)
   
   调用 msg_send 时会调用 **lookupImpOrForward** 方法，其中调用了 类的**_class_initialize**

[懒惰的 initialize](https://github.com/draveness/analyze/blob/master/contents/objc/%E6%87%92%E6%83%B0%E7%9A%84%20initialize%20%E6%96%B9%E6%B3%95.md)

[**Category：从底层原理研究到面试题分析**](https://xiaozhuanlan.com/topic/3968017254)