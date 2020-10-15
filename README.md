# lagou-course-downloader
拉勾网课程视频下载工具

本程序仅供学习交流使用，来源：https://github.com/SweetInk/lagou-course-downloader

最后更新时间: 2020年8月7日

# 更新日志

1. 支持最新的拉钩教育视频下载.


# 前置要求

1. **已购买**拉钩上的[视频课程](https://kaiwu.lagou.com/)

2. **成功登陆拉钩网**

# 其他

Lagou课程的视频现托管在阿里云，[相关文档](https://help.aliyun.com/product/29932.html?spm=a2c4g.11186623.3.1.3a082168qYWI6d)

视频元数据API接口文档:`https://help.aliyun.com/document_detail/56124.html?spm=a2c4g.11186623.2.30.14487fbfjBfxAC`

视频的PreAuthCode解密算法逆向自aliplayer-min.js

视频片段使用`AES-CBC-128`加密/解密，通过分析js获取，视频的密钥在视频的m3u8文件中有地址。[相关文档](https://cloud.tencent.com/document/product/266/9638)

~~视频片段通过`ffmpeg`合并~~
现在直接获取视频的mp4地址，跳过了合成（当然也可以）

视频课程信息~~在视频首页html中的`<script>`标签里。~~ 现在通过`https://gate.lagou.com/v1/neirong/kaiwu/getCourseLessons?courseId={0}` 获取

程序默认下载`FHD`全高清视频源

# 如何使用

## 下载源码

打开shell或者cmd，输入`git clone https://github.com/SweetInk/lagou-course-downloader.git`

## 运行(以源码方式)

1. 下载[IDEA](https://www.jetbrains.com/idea/download/#section=windows),并安装，导入之前下载好的源码

2. 成功登陆拉钩网后

3. 浏览器打开调试工具

4. 打开课程首页

![](http://ww1.sinaimg.cn/large/005ViNx8ly1g5mdhltkh8j31yy0mf11q.jpg)

把上图中Cookie值，复制粘贴到`CookieStore.java`中`cookie` 字段中.(方法不用限定为getkey，随意一个请求方法即可。它们的cookie是一致的，在上方还有courseId的编号需要记下下面会用到。)

替换`Downloader.java`代码中的`courseId`,`savePath`中的值为实际的值.

```java
String courseId = "课程ID";
String savePath = "视频保存位置";
Downloader downloader = new Downloader(courseId, savePath);
downloader.start();
```
保存路径需要先行创建好，如：D:\\LagouLessons\\machineLearning\\

在machineLearning文件夹内要先创建一个文件：download.txt   （用于保存解析的视频地址，保存的路径也要随着项目路径变化而变化）

5. 运行`Downloader#main()` 方法.（项目中启动类可能有多个，注意选择Downloader启动。）

6. 最终效果为，能生成课程下的课程目录空壳和一个写着MP4下载地址的dowload.txt，需要再自行通过迅雷下载，可全部拷贝粘贴到迅雷中，即可下载所有。