---
title: iOS 事件处理
date: 2020-05-05
updated: 2020-05-05
tags: 
  - iOS
  - Objective-C
  - Swift
categories: iOS
description: iOS 事件传递与处理
cover: /images/Xcode.png
top_img: /images/Xcode.png
typora-root-url: ../../../source
---



事件响应链

```objc
#import "UIView+HitTestDebug.h"
#import <objc/runtime.h>

@implementation UIView (HitTestDebug)

+ (void)load {
    Method origin = class_getInstanceMethod([UIView class], @selector(hitTest:withEvent:));
    Method custom = class_getInstanceMethod([UIView class], @selector(debug_hitTest:withEvent:));
    method_exchangeImplementations(origin, custom);
    
//    origin = class_getInstanceMethod([UIView class], @selector(pointInside:withEvent:));
//    custom = class_getInstanceMethod([UIView class], @selector(debug_pointInside:withEvent:));
//    method_exchangeImplementations(origin, custom);
}

- (UIView *)debug_hitTest:(CGPoint)point withEvent:(UIEvent *)event {
    NSLog(@"%@ hitTest %p", NSStringFromClass([self class]), self);
    UIView *result = [self debug_hitTest:point withEvent:event];
    NSLog(@"%@ hitTest return: %@", NSStringFromClass([self class]), NSStringFromClass([result class]));
    return result;
}

- (BOOL)debug_pointInside:(CGPoint)point withEvent:(UIEvent *)event {
    NSLog(@"%@ pointInside", NSStringFromClass([self class]));
    BOOL result = [self debug_pointInside:point withEvent:event];
    NSLog(@"%@ pointInside return: %@", NSStringFromClass([self class]), result ? @"YES":@"NO");
    return result;
}

@end
```



## 参考

https://segmentfault.com/a/1190000013265845

https://segmentfault.com/a/1190000015060603?utm_source=sf-similar-article

https://juejin.cn/post/6844903493640290311#heading-18