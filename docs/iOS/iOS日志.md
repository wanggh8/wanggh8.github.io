---
title: iOS 日志
date: 2021-04-23
updated: 2019-04-23
tags: 
  - Objective-C
  - iOS
categories: iOS
description: iOS 日志
cover: /images/objective-C.png
top_img: /images/objective-C-bg.png
typora-root-url: ../../../source
---

## 使用 freopen 重定向控制台输出

使用 `freopen` 重定向控制台打印语句时，`C` 语言中的print 函数使用 `stdout` 流，`OC` 中的 `NSLog` 使用 `stderr` 流。

```objc
+ (void)saveLogToDocumentFolder {
    NSString *fileName = [NSString stringWithFormat:@"log.text"];
    NSString *logfilePath = [NSHomeDirectory() stringByAppendingFormat:@"/Documents/%@",fileName];
    NSLog(@"logfilePath = %@",logfilePath);
        
    NSFileManager *fileManage = [NSFileManager defaultManager];
    // 判断是否存在（/Documents/log.text）文件路径，存在就清空文件
    BOOL islogfilePath = [fileManage fileExistsAtPath:logfilePath];
    if (islogfilePath) {
        [fileManage removeItemAtPath:logfilePath error:nil];
    }
    [fileManage createFileAtPath:logfilePath contents:nil attributes:nil];
    
    freopen([logfilePath cStringUsingEncoding:NSASCIIStringEncoding],"a+",stdout);
    freopen([logfilePath cStringUsingEncoding:NSASCIIStringEncoding],"a+",stderr);
}
```

## 文件可查看 Documents

在 info.plist 文件中，配置如下：

- Application supports iTunes file sharing  即  UIFileSharingEnabled
- Supports opening documents in place  即  LSSupportsOpeningDocumentsInPlace

第一个是 UIFileSharingEnabled，这个可以使 iTunes 分享你文件夹内的内容；第二个是 LSSupportsOpeningDocumentsInPlace ，它保证了你文件夹内本地文件的获取权限，你需要将这两个键值对的值设置为 YES ，结果如下：

![image-20210426104812144](/assets/image-20210426104812144.png)





