---
title: iOS 内存管理
date: 2021-04-19
updated: 2021-04-20
tags: 
  - Objective-C
  - iOS
categories: iOS
description: iOS 内存管理相关
cover: /images/objective-C.png
top_img: /images/objective-C-bg.png
typora-root-url: ../../../source
---

## 简介

## 分区

| 分区                                             | 特点                                                         |
| :----------------------------------------------- | ------------------------------------------------------------ |
| 栈区（stack）                                    | 由编译器自动完成分配合释放，不需要程序员手动管理，主页存储了函数的参数和局部变量值等。 |
| 堆区（heap）                                     | 需要程序员手动开辟并管理内存。                               |
| BSS 段（全局区）（静态区）（未初始化常量区.bss） | 程序运行过程内存的数据一直存在，程序结束后由系统释放         |
| 常量区（数据段）（已初始化常量区.data）          | 专门用于存放常量，程序结束后由系统释放                       |
| 程序代码区（.text）                              | 用于存放程序运行时的代码，代码会被编译成二进制，存进内存的程序代码区 |



## 引用计数

引用计数式内存管理的思考方式：

- 自己生成的对象，自己所持有
- 非自己生成的对象，自己也能持有
- 不再需要自己持有的对象时释放
- 非自己持有的对象无法释放

对象操作与 Objective-C 方法的对应：

| 对象操作       | Objective-C 方法（NSObject）      |
| -------------- | --------------------------------- |
| 生成并持有对象 | alloc/new/copy/mutableCopy 等方法 |
| 持有对象       | retain 方法                       |
| 释放对象       | release 方法                      |
| 废弃对象       | dealloc 方法                      |

### autorelease

autorelease 提供使对象在超出指定的生存范围时，能够自动并正确地释放（调用 release 方法）。autorelease 不立即释放，注册到 autoreleasepool 中，在 pool 结束时自动调用release。

autorelease 的具体使用方法：

1. 生成并持有 NSAutoreleasePool 对象
2. 调用已分配对象的 autorelease 实例方法
3. 废弃 NSAutoreleasePool 对象（对每个对象自动调用 release）

NSAutoreleasePool 对象的生存周期相当于 C 语言变量的作用域。对于所有调用过 autorelease 实例方法的对象，在废弃 NSAutoreleasePool 对象时，都将调用 release 实例方法。源码如下：

```objc
NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
id obj = [[NSObject alloc] init];
[obj autorelease];
[pool drain]; // 此时调用 release 
```

在 Cocoa 框架中，相当于程序主循环的 NSRunLoop 或者在其他程序可运行的地方，对 NSAutoreleasePool 对象进行生成、持有和废弃处理。但是在大量产生 autorelease 的对象时，只要不废弃 NSAutoreleasePool 对象，生成的对象就不能释放，因此可能会发生内存不足的现象。例如大量读入图像并修改生成新的 UIImage 对象。为避免此情况，需要在适当的地方生成、持有或废弃 NSAutoreleasePool 对象。

在 Cocoa 框架中，有很多类方法用于返回 autorelease 的对象，比如：NSMutableArray 的 arrayWithCapacity 类方法。

```objc
id array = [NSMutableArray arrayWithCapacity:1];
// 等同于
id array = [[[NSMutableArray alloc] initWithCapacity:1] autorelease];
```

**注意事项：**

> NSAutoreleasePool 类的autorelease 的实例方法已被该类重载，调用运行时会出错



### alloc/retain/release/dealloc 实现



## 自动引用计数

### 使用方法

设置 ARC 有效的编译方法（Xcode 4.2 以上默认为 ARC）：

- 使用 clang（LLVM 编译器）3.0 或以上版本
- 指定编译器属性为 "-fobjc-arc"

### 所有权修饰符

Objective-C 编程中为了处理对象，可将变量类型定义为 id 类型或各种对象类型。

> 对象类型就是指向 NSObject 这样的 Objective-C 类的指针，例如 "NSObject *"
>
> id 类型用于隐藏对象类型的类名部分，相当于 C 语言中常用的 "void *"

ARC有效时，id 类型和对象类型必须附加所有权修饰符。所有权修饰符一共 4 种：

- __strong 修饰符
- __weak 修饰符
- __unsafe_unretained 修饰符
- __autoreleasing 修饰符

strong 修饰符、weak 修饰符、autoreleasing 修饰符可以保证附有这些修饰符的自动变量初始化为 nil

```objc
id __strong obj0;
id __weak obj1;
id __autoreleasing obj2;
// 等同于
id __strong obj0 = nil;
id __weak obj1 = nil;
id __autoreleasing obj2 = nil;
```



#### __strong 修饰符

__strong 修饰符是 id 类型和对象类型默认的所有权修饰符，在 id 和对象类型没有明确指定所有权修饰符时，默认为此修饰符。

```objc
{
	id obj = [[NSObject alloc] init];
    // 等同于
	id __strong obj = [[NSObject alloc] init];
}
```

附有 __strong 修饰符的变量在超出其作用域时，即该变量被废弃时，会释放其被赋予的对象。等同于 ARC 无效时，增加调用 release 方法，源码：

```objc
{
    id __strong obj = [[NSObject alloc] init];
}
// ARC 无效
{
	id obj = [[NSObject alloc] init];
	[obj release];
}
```

__strong修饰符表示对对象的**强引用**，持有强引用的变量在超出其作用域时被废弃，随着强引用的失效，引用的对象也会随之释放。

##### 自己生成并持有对象分析

```objc
{
    // 自己生成并持有对象
    id __strong obj = [[NSObject alloc] init];
    // 因为变量 obj 为强引用，所以自己持有对象
}
	// 因为变量 obj 超出其作用域，强引用失效
	// 所以自动地释放自己持有的对象
	// 对象的所有者不存在，因此废弃该对象
```

##### 持有非自己生成的对象分析

```objc
{
    // 取得非自己生成并持有的对象
    id __strong obj = [NSMutableArray array];
    // 因为变量 obj 为强引用，所以自己持有对象
}
	// 因为变量 obj 超出其作用域，强引用失效
	// 所以自动地释放自己持有的对象
```

##### 总结

**自己生成的对象自己持有**和**非自己生成的对象也能持有**只需通过对待 strong 修饰符的变量赋值即可。通过废弃待 strong修饰符的变量（变量作用域结束或成员变量所属对象废弃）或者对变量赋值，都可做到**不再需要自己持有的对象时释放**。最后一项**非自己持有的对象无法释放**，由于不必再次键入 release ，所以根本不会执行。

#### __weak 修饰符

#### __unsafe_unretained 修饰符

#### __autoreleasing 修饰符



### __bridge 转换

- **__bridge 转换**：安全性与赋值和 __unsafe_unretained 修饰符相近，易因悬垂指针而崩溃（原变量 +0，新变量 +0）
- **__bridge_retained 转换**：转换赋值的变量也持有所赋值的对象（原变量 +0，新变量 +1）
- **__bridge_transfer 转换**：被转换的变量所持有的对象在该变量被赋值给转换目标变量后随之释放（原变量 -1，新变量 +1）

### 属性



属性声明的属性与所有权修饰符的对应关系如下：

| 属性声明的属性    | 所有权修饰符               |
| ----------------- | -------------------------- |
| assign            | __unsafe_unretained 修饰符 |
| copy              | __strong 修饰符            |
| retain            | __strong 修饰符            |
| strong            | __strong 修饰符            |
| unsafe_unretained | __unsafe_unretained 修饰符 |
| weak              | __weak 修饰符              |

以上各种属性赋值给指定的属性中就相当于赋值给附件各属性对应的所有权修饰符的变量中。只有 copy 不是简单的赋值，而是通过 NSCopying 接口的 copyWithZone: 方法复制赋值源所生成的对象。

**注意事项：**

> 在声明类成员变量时，如果同属性声明中的属性不一致则会引起编译错误