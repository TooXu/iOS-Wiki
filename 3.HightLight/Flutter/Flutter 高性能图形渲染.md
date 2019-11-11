# 深入 Flutter 的高性能图形渲染



#### 1. 图形性能为何能够媲美原生应用
![skia](https://raw.githubusercontent.com/TooXu/resources/master/Images/Graphics%20performance%20.png)

##### Skia 开原图形引擎

用于 Chrome , Android , FireFox , Sublime , Adobe

#### 2. 我的 Flutter App 有绘图性能问题怎么办?

16 毫秒 / 60 帧

##### 	GPU thread

- ###### 分析 FLutter 应用的 skia 调用

  *flutter run --profile --trace-skia*  Dart VM obeserver   去除冗余函数调用

- ###### 捕捉 SkPicture 分析每一条绘图指令

  flutter screenshot --type=skia --observatory-port=<port>  去除冗余渲染

- ###### 常见的 Skia 函数调用性能瓶颈

  saveLayer

  clipPath  

  尽量减少这两个函数调用 将性能在老旧的设备上提升了 2 倍
  
  **saveLayer 和 clipPath 都会在哪出现?**
  
  - Opacity , ShaderMask
  - 使用 clip.antiAlias  clip.hardEdge 

##### 	UI

#### 3. 如何处理疑难杂症

1. 捕捉 timeline (flutter --profile --trace-skia)
2. 捕捉 skip(flutter screenshot --type=skia)
3. 截取可重现的的样例代码
4. 发送问题至 github.com/flutter/flutter/issues