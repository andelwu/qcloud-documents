## 1. 功能说明
大眼、瘦脸、动效贴纸、绿幕等特效功能，是基于优图实验室的人脸识别技术和天天P图的美妆技术为基础开发的特权功能，腾讯云小直播团队通过跟优图和P图团队合作，将这些特效深度整合到 RTMP SDK 的图像处理流程中，以实现更好的视频特效。

## 2. 版本下载
可以到 [RTMP SDK 开发包](https://www.qcloud.com/document/product/454/7873) 页面下方下载特权版 SDK 压缩包，压缩包有加密（解压密码 & license 可以跟我们的商务同学获取）, 成功解压后得到一个 `txrtmpsdk.jar` 和 `libtxrtmpsdk.so` 等几个 so，替换工程中的非特权版 jar 和 so 即可。

> **说明：**
> 区分特权版与非特权版，可以查看 SDK 的 bundler id。bundler id 为 com.tencent.TXRTMPSDK 表示非特权版，com.tencent.TXRTMPSDK.pitu 表示特权版。

## 3. Xcode 工程设置

### 3.1 添加 framework

特权版需要额外链接一些系统 framework：
> 1. AssetsLibrary.framwork
> 2. CoreMedia.framework
> 3. Accelerate.framework

### 3.2 添加链接参数

在工程 Build Setting -> Other Link Flags 里，增加 `-ObjC` 选项。

### 3.3 添加资源 bundle

将 zip 包中下列文件添加到工程中。

> 1. FilterEngine.bundle
> 2. PE.dat
> 3. ufa.bundle
> 4. youtubeauty.bundle

### 3.4 添加动效资源

将 zip 包中 Resource 目录以 folder refrence 形式添加到工程中。这些资源非常重要，如果没有添加切换到换脸类素材时会发生 crash。
![](https://mc.qcloudimg.com/static/img/b7fac6b5e08b0ff245b17d29f7296b18/AAE85661-7601-4473-A338-747FB9A6981C.png)

### 3.5 导入 licence 文件
特权版需要 licence 验证通过后，相应功能才能生效。您可以向我们的商务同学申请一个免费 30 天的调试用 license。
得到 licence 后，将其命名为 **YTFaceSDK.licence**，并添加到工程的 assets 目录下。

> **说明：**
> 1. 每个 licence 都有绑定具体的 package name，修改 app 的 package name 会导致验证失败。
> 2. YTFaceSDK.license 的文件名固定，不可修改、且必须放在 assets 目录下。
> 3. iOS 和 Android 不需要重复申请 license，一个 license 可以同时授权一个 iOS 的 bundleid 和一个 Android 的 packageName。

## 4. 功能调用

### 4.1 动效贴纸

一个动效模版是一个目录，里面包含很多资源文件。每个动效因为复杂度不同，目录个数以和文件大小也不尽相同。
小直播中的示例代码是从后台下载动效资源，再统一解压到 Resource 目录。您可以在小直播代码中找到动效资源和动效缩略图的下载地址，格式如下：
> https://st1.xiangji.qq.com/yunmaterials/{动效名}.zip
> https://st1.xiangji.qq.com/yunmaterials/{动效名}.png
>


强烈建议客户将动效资源放在自己的服务器上，以防小直播变动造成不必要的影响。
当解压完成后，即可通过以下接口开启动效效果。

```objective-c
/**
 * 选择动效
 *
 * @param tmplName: 动效名称
 * @param tmplDir: 动效所在目录
 */
- (void)selectMotionTmpl:(NSString *)tmplName inDir:(NSString *)tmplDir;
```


### 4.2 绿幕功能

使用绿幕需要先准备一个用于播放的 mp4 文件，通过调用以下接口即可开启绿幕效果：

```objective-c
/**
 * 设置绿幕文件
 * 
 * @param file: 绿幕文件路径。支持mp4; nil 关闭绿幕
 */
-(void)setGreenScreenFile:(NSURL *)file;
```

### 4.3 大眼瘦脸

大眼和瘦脸通过以下方法设置：

```objective-c
/**
 * 设置大眼级别
 * 
 *  @param eyeScaleLevel: 大眼级别取值范围 0 ~ 9； 0 表示关闭 1 ~ 9值越大 效果越明显。
 */
-(void)setEyeScaleLevel:(float)eyeScaleLevel;

/**
 * 设置瘦脸级别
 *
 *  @param faceScaleLevel: 瘦脸级别取值范围 0 ~ 9； 0 表示关闭 1 ~ 9值越大 效果越明显。
 */
-(void)setFaceScaleLevel:(float)faceScaleLevel;
```
