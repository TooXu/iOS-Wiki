### pod install 

- #### 读取 Podfile 文件

-  #### 版本控制和冲突

- #### 加载源文件

  每个 `.podspec` 文件都包含一个源代码的索引，这些索引一般包裹一个 git 地址和 git tag。它们以 commit SHAs 的方式存储在 `~/Library/Caches/CocoaPods` 中。这个路径中文件的创建是由 Core gem 负责的。

- #### 生成 Pods.xcodeproj

  每次 `pod install` 执行，如果检测到改动时，CocoaPods 会利用 Xcodeproj gem 组件对 `Pods.xcodeproj` 进行更新。如果该文件不存在，则用默认配置生成。否则，会将已有的配置项加载至内存中。

  

[**深入理解 CocoaPods**](https://objccn.io/issue-6-4/)



1. 在项目中第一次使用 CocoaPods , 进行安装的时候使用这个命令.
2. 在`Podfile`中`增加`或`删除`某个pod后, 也是使用这个命令. 而不是`pod update`.
   - 每次运行`pod install`命令, 下载并安装新的pod时, 它会为 `Podfile.lock` 文件中的每个pod写入已安装的版本. 此文件跟踪每个 pod 的已安装版本并锁定这些版本 (.lock命名因此而来).
   - 当运行`pod install`，它只解析 Podfile.lock 中尚未列在其中的 pod 的依赖库.
     - 对于已经在 `Podfile.lock` 中列出的 pod, Podfile.lock不会尝试检查是否有更新的版本.
     - 对于尚未在 `Podfile.lock `中列出的 pod, 会搜索与 Podfile（如中所述pod ‘MyPod’, ‘~>1.2’）匹配的版本或最新的版本.

### pod update

当运行`pod update PODNAME`时, CocoaPods将尝试查找`PODNAME`更新的pod版本, 会忽略掉`Podfile.lock`中已经存在的版本.

如果直接运行`pod update`, 没有指定`PODNAME`, CocoaPods会把Podfile中所有的pod都更新到最新版本.(如果已经是最新版本了, 则不更新)