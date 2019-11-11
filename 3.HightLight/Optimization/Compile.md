## APP 编译速度优化

####  Injection for Xcode

Injection 会监听源代码文件的变化，如果文件被改动了，Injection Server 就会执行 rebuildClass 重新进行编译、打包成动态库，也就是 .dylib 文件。编译、打包成动态库后使用writeSting 方法通过 Socket 通知运行的 App。