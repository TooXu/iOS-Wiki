[**深入理解 CocoaPods**](https://objccn.io/issue-6-4/)

[基于 CocoaPods 进行 iOS 开发](https://medium.com/@DianQK/%E5%9F%BA%E4%BA%8E-cocoapods-%E8%BF%9B%E8%A1%8C-ios-%E5%BC%80%E5%8F%91-fd02ecc19842)

##### CocoaPods 集成 Pod 的基本处理步骤:

1. 更新 Podfile 指定 source 中本地 repo 中的 spec, 如果没有指定 source 则搜索所有 repo
2. 分析 Podfile
3. 根据 Podfile 在 CocoaPods 的 repo 里查找对应 Pod 版本的 podspec
4. 分析所有依赖 Pod 之间的依赖关系
5. 和 Podfile.lock 文件对比, 看哪些 Pod 有更新
6. 下载更新了的 Pod
7. 生成工程 Pods.xcodeproj
8. 生成 xconfig 和 dummy 文件
9. 将配置信息及 Podfile.lock 等写入磁盘
10. 根据配置信息生成工程文件



##### pod - 指定项目的依赖项

- `> 0.1`高于0.1版本（不包含0.1版本）的任意一个版本
- `>= 0.1`高于0.1版本（包含0.1版本）的任意一个版本
- `< 0.1`低于0.1版本（不包含0.1版本）的任意一个
- `<= 0.1`低于0.1版本（包含0.1版本）的任意一个
- `~> 0.1.2`版本 0.1.2的版本到0.2 ，不包括0.2。这个基于你指定的版本号的最后一个部分。这个例子等效于>= 0.1.2并且 <0.2.0，并且始终是你指定范围内的最新版本。