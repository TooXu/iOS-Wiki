[瘦身 移除无效方法 ](https://mp.weixin.qq.com/s/HWl9lAMq6ILEudHg3HDg4g)

#### 1. 官方 App Thinning

####  2. 无用图片资源

​	LSUnusedResources

​    https://github.com/mitchell-dream/MitImgChecker

#### 3. 图片资源压缩

- WebP 压缩率高，而且肉眼看不出差异

- WebP 在 CPU 消耗和解码时间上会比 PNG 高两倍...

#### 4. 代码瘦身

- LinkMap 结合 Mach-O 找无用代码
- 我们就可以使用 MachOView 来查看 Mach-O 里的信息

#### 5.通过 AppCode 找出无用代码

- 无用类
- 无用方法
- 无用宏
- 无用全局

#### 6. 根据架构打包 Framework 

ijkplayer x86, armv7 arm64

