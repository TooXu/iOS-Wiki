![meta class](https://raw.githubusercontent.com/TooXu/resources/master/Images/superclass.jpeg)



### self & super



`self` 是一个隐藏参数, 每个方法的实现的第一个参数即为 `self`

`super` 实际上只是一个"编译器标示符", 它负责告诉编译器, 当调用方法时, 去调用父类的方法, 而不是本类中的方法



在调用[super xxx]时, runtime 会去调用 objc_msgSendSuper 方法

