# 私有 pods 使用教程

#### 1.建立两个 Git 仓

- 第一个 Git 仓

  > 用来存放私有库的版本信息： `DYPods`

  

- 第二个 Git 仓

  > 用来存放私有仓库的代码：`DYFoundation`
  
  ​	
  
  ```json
  在 GitHub 上创建一个公开项目，项目中必须包含这几个文件
  
  LICENSE:开源许可证
  README.md:仓库说明
  DYFoundation
  DYFoundation.podspec: CocoaPods 的描述文件，这个文件非
  ```
  
  
  
  

#### 2.编写.podspec 文件



```ruby
Pod::Spec.new do |s|
  s.name         = "DYFoundation" # 项目名称
  s.version      = "1.0.0"        # 版本号 与 你仓库的 标签号 对应
  s.license      = "MIT"          # 开源证书
  s.summary      = "A delightful TextField of PhoneNumber" # 项目简介

  s.homepage     = "https://github.com/qiubaiying/BYPhoneNumTF" # 你的主页
  s.source       = { :git => "https://github.com/qiubaiying/BYPhoneNumTF.git", :tag => "#{s.version}" }#你的仓库地址，不能用SSH地址
  s.source_files = "BYPhoneNumTF/*.{h,m}" # 你代码的位置， BYPhoneNumTF/*.{h,m} 表示 BYPhoneNumTF 文件夹下所有的.h和.m文件
  s.requires_arc = true # 是否启用ARC
  s.platform     = :ios, "7.0" #平台及支持的最低版本
  s.frameworks   = "UIKit", "Foundation" #支持的框架
  # s.dependency = "AFNetworking" # 依赖库
  
  # User
  s.author             = { "BY" => "qiubaiyingios@163.com" } # 作者信息
  s.social_media_url   = "http://qiubaiying.github.io" # 个人主页

end
```



	> 每次内容更新时更新版本号

```shell
$ pod lib lint  #验证 podspec 文件
```

> The `DYFoundation.podspec` specification does not validate.

#### 3.代码提交

#####  3.1 将代码提交到 Git 仓库。

  ``` shell
$ git add .
$ git commit 
$ git push
  ```

#####  3.2 打 tag 标记

```shell
创建标签
$ git tag -a 1.0.0 -m '标签说明' 
推送到远程
$ git push origin --tags
```



#### 4.发布`.podspec`

```shell
pod repo push DYPods DYFoundation.podspec
```

#### 5. 模块化设计

```ruby
spec.source_files = 'SGExtension/SGExtension.h'

spec.subspec 'Category' do |ss|
ss.source_files = 'SGExtension/Category/{*.h,*.m}';
ss.library = 'z'
end

spec.subspec 'Tools' do |ss|
ss.source_files = 'SGExtension/Tools/{*.h,*.m}';
end
```

```ruby
 s.subspec 'DYLogCatagory' do |ss|
    ss.source_files = "DYFoundation/DYLogCatagory/*.{h,m}"
```



#### 6.参考链接

[CocoaPods私有仓库的创建](http://liqingzhen.top/2017/03/10/CocoaPods%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93%E7%9A%84%E5%88%9B%E5%BB%BA/)

[Cocoapods系列教程---模块化设计](https://www.jianshu.com/p/1c986ba7af41)



