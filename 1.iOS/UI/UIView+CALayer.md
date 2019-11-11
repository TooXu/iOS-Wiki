##UIView CALayer

#### UIView 与 CALayer 关系



1. UIView 负责响应事件, CALayer 负责绘制 UI

   `UIView : UIResponer` 

   `CALayer : NSObjec`

> UIView 内部有一个 CALayer 提供内容绘制和显示, UIView 的尺寸都是由内部的 Layer 提供的. 
>
> > layer 有 subLayers 
> >
> > view 有 subViews

1. UIView 对 CALayer 封装属性

2. UIView 是 CALayer 的代理

   > UIView 中持有一个 layer 对象, 同时是这个 layer 对象的 delegate , UIView 和 CALayer 协同工作. `CALayerDelegate`
   >
   > 平时我们设置 UIView 的 frame ,bounds, center ,  其实都是 UIView 对 CALayer 进行一层封装, 使我们可以方便的设置控件的位置.但是像圆角, 阴影 等属性没有进一步的封装, 所以需要去设置 layer 的属性来实现. 

   

3. 动画

   >  layer 中的很多属性都是 animatable 的, 修改这些属性会产生隐式动画.
   >
   > > 无论何时一个可动画的 layer 属性改变时, layer 都会寻找并运行何时的 `action` 来实现这个改变. 		

   > 当修改 UIView 的 `主layer` ( `RootLayer` ) ,隐式动画会失效. 因为 UIView 默认情况下, 禁止了 layer 动画, 但是  在 `animation block`中 又重启了他们(`CABasicAnimation`).
   >
   > > 属性改变的时候 layer 会向 view 请求一个动作, 一般情况下返回 `NSNull`,
   > >
   > > 只有发生再 animation block 中时, view 才会返回实际动作.

   *AsyncDisplayKit*

   *Texture*

## CALayer 中 AnchorPoint 和 Position 的关系



#### AnchorPoint 锚点  

anchorPoint 所在的坐标系为,当前 layer  从左上角 到 右下角 为 ( 0,0 ) 到 (1,1)

右上角( 1,0 ) 左下角 ( 0,1 )

#### Position 

1. position 是 layer 中 anchorPoint 相对于 superLayer 的坐标

2. Postion 与 AnchorPoint 互补影响 

3. 计算公式 : 

> view.frame.origin.x = position.x - anchorPoint.x * bounds.size.width;
>
> view.frame.origin.y = positon.y - anchorPoint.y * bounds.size.height;





   

   

   







