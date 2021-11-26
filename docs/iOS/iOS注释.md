---
title: iOS 注释
date: 2021-04-23
updated: 2019-04-23
tags: 
  - Objective-C
  - iOS
categories: iOS
description: iOS 注释
cover: /images/Xcode.png
top_img: /images/Xcode.png
typora-root-url: ../../../source
---

Xcode8提供了快速生成格式化注释的快捷键：option+command+/。如果方法有参数，会自动添加@param关键字，用于描述对应的参数。
Apple提供了官方的headDoc语法，但是很多都已经在Xcode中失效了，而且有些关键字也和appleDoc不兼容。下面几种列举出了在Xcode中仍然有效的一些关键字：


```objc
/**
 演示苹果headDoc的语法。这里可以写方法简介
 
 @brief 方法的简介(appleDoc不支持此关键字)
 @discussion 方法的详细说明
 @param 方法参数
 
 @code //示例代码(这个在Xcode里常用，但是appleDoc不支持此关键字)
 UIView *view;
 @endcode
 
 @bug       存在的bug的说明
 @note      需要注意的提示
 @warning   警告
 @since     iOS7.0
 @exception 方法会抛出的异常的说明
 
 @attention 注意，从这里开始往下的关键字，appleDoc都不支持
 @author    编写者
 @copyright 版权
 @date      日期
 @invariant 不变量
 @post      后置条件
 @pre       前置条件
 @remarks   备注
 @todo      todo text
 @version   版本
 */
- (void)sampleMethod;
```

## 参考

https://www.jianshu.com/p/91c603061943