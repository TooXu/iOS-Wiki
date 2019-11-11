## 技术选型

### AVCaptureSession+AVAssetWriter

1. 创建捕捉会话
2. 设置视频的输入 和 输出
3. 设置音频的输入 和 输出
4. 添加视频预览层
5. 开始采集数据，这个时候还没有写入数据，用户点击录制后就可以开始写入数据
6. 初始化AVAssetWriter, 我们会拿到视频和音频的数据流，用AVAssetWriter写入文件，这一步需要我们自己实现。

1、初始化需要的线程队列（这个后面你可以了解到为什么需要这些队列）

​      2、初始化AVCaptureSession录制会话

​      3、需要一个视频流的输入类： 利用AVCaptureDevice  录制设备类，根据 AVMediaType 初始化 AVCaptureDeviceInput  录制输入设备类，是要分音频和视频的，这点和前面的类似。把他们添加到录制会话里面。

​      4、初始化 AVCaptureVideoDataOutput 和 AVCaptureAudioDataOutput ，把它们添加到AVCaptureSession对象，根据你初始化的线程设置setSampleBufferDelegate代理对象

​      5、根据AVCaptureSession得到一个AVCaptureVideoPreviewLayer预览曾对象，用于预览你的拍摄画面

​      6、初始化AVAssetWrite 再给AVSssetWrite通过addInput添加AVAssetWriterInput,AVAssetWriterInput也是根据AVMediaType分为video和audio，这个是重点！！！有许多参数需要设置！

​      7、通过 AVCaptureSession startRunning 开始采集数据，采集到的数据就会走你设置的输出对象AVCaptureAudioDataOutput的代理，代理会遵守AVCaptureVideoDataOutputSampleBufferDelegate协议。你需要在这个协议的方法里面去开始通过 AVAssetWriter 对象 startWriting 开始写入数据

​      8、当写完数据之后就会走AVAssetWriter的finishWritingWithCompletionHandler方法，在这里你就可以拿到你录制的视频去做其他的处理了！

