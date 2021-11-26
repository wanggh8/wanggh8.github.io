---
title: iOS Tips and Techniques for Framework Developers
date: 2021-05-28
updated: 2021-05-28
tags: 
  - iOS
  - Objective-C
categories: iOS
description: iOS Tips and Techniques for Framework Developers 
cover: /images/Xcode.png
top_img: /images/Xcode.png
typora-root-url: ../../../source
---

## Initialization

### Class Initialization

initialize 类方法是一个一次性，懒散的执行一些代码的地方，它会在类的其他方法被调用之前调用。最典型的应用是设置类的版本号。 动态系统给继承链上每一个类发送initialize方法，即使方法没有实现。因此它可能会被调用多次（例如，一个子类没有实现它。）。通常我们只想初始化代码被执行一次。一种实现的方法是使用dispatch_once();

```objc
+ (void)initialize {
    static dispatch_once_t onceToken = 0;
    dispatch_once(&onceToken, ^{
        // the initializing code
    }
}
```

因为动态系统发送initialize给每一个类，所以它很可能会在子类的上下文中调用—如果子类没有实现initialize，会调用父类的实现。如果在相关类的上下文中有特殊的初始化需求，我们可以进行如下操作而不仅仅是使用dispatch_once();

```objc
if (self == [NSFoo class]) {
    // the initializing code
}
```

您绝不应明确调用 `initialize` 方法。如果您需要触发初始化，则调用一些无害的方法，例如：``

```objc
[NSImage self];
```

### 在初始化过程中检测错误

好的初始化方法应该完全遵循以下步骤去保证正确的错误检测和传递： 调用父类的designated initializer给self重新赋值。 验证返回值是否为nil，它表明父类的初始化发生了一些错误。 如果现在当前类的初始化发生了错误，释放对象并返回nil。

```objc
- (id)init {
    self = [super init];  // Call a designated initializer here.
    if (self != nil) {
        // Initialize object  ...
        if (someError) {
            [self release];
            self = nil;
        }
    }
    return self;
}
```

