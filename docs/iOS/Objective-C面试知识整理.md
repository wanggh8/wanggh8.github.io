---
title: iOS Objective-C 面试知识整理
date: 2019-03-21
updated: 2021-03-05 
tags: 
  - Objective-C
  - iOS
categories: iOS
description: iOS Objective-C Manual
cover: /images/objective-C.png
top_img: /images/objective-C-bg.png
typora-root-url: ../../../source
---

## Objective-C 基础

### 对象方法和类方法的区别?

- 对象方法：以减号开头,只可以被对象调用，可以访问成员变量
- 类方法：以加号开头只能用类名调用，对象不可以调用，类方法不能访问成员变量

### 什么是分类?

- 分类: 在不修改原有类代码的情况下,可以给类添加方法
  Categroy 给类扩展方法,或者关联属性, Categroy底层结构也是一个结构体:内部存储这结构体的名字,那个类的分类,以及对象和类方法列表,协议,属性信息
- 通过Runtime加载某个类的所有Category数据
- 把所有Category的方法、属性、协议数据，合并到一个大数组中后面参与编译的Category数据，会在数组的前面
- 将合并后的分类数据（方法、属性、协议），插入到类原来数据的前面

### 什么是协议?

- 协议：协议是一套标准，这个标准中声明了很多方法，但是不关心具体这些方法是怎么实现的，具体实现是由遵循这个协议的类去完成的。
- 在OC中，一个类可以实现多个协议，通过协议可以弥补单继承的缺陷但是协议跟继承不一样，协议只是一个方法列表，方法的实现得靠遵循这个协议的类去实现。

### 为什么说OC是一门动态语言？

- 动态语言:是指程序在运行时可以改变其结构，新的函数可以被引进,已有的函数可以被删除等在结构上的变化
- 动态类型语言: 就是类型的检查是在运行时做的。

OC的动态特性可从三方面:

- 动态类型（Dynamic typing）：最终判定该类的实例类型是在运行期间
- 动态绑定（Dynamic binding）：在运行时确定调用的方法
- 动态加载（Dynamic loading）：在运行期间加载需要的资源或可执行代码

### 动态绑定

- 动态绑定 将调用方法的确定也推迟到运行时。OC可以先跳过编译，到运行的时候才动态地添加函数调用，在运行时才决定要调用什么方法，需要传什么参数进去，这就是动态绑定。
- 在编译时，方法的调用并不和代码绑定在一起，只有在消息发送出来之后，才确定被调用的代码。

### cocoa touch底层技术架构?

cocoa touch底层技术架构 主要分为4层:

- 可触摸层 Cocoa Touch : UI组件,触摸事件和事件驱动,系统接口
- 媒体层 Media: 音视频播放,动画,2D和3D图形
- Core Server: 核心服务层,底层特性,文件,网络,位置服务区等
- Core OS: 内存管理,底层网络,硬盘管理

### 什么是谓词?

谓词(`NSPredicate`)是OC针对数据集合的一种逻辑帅选条件,类似一个过滤器,简单实实用代码如下:

```objc
Person * p1 = [Person personWithName:@"alex" Age:20];
Person * p2 = [Person personWithName:@"alex1" Age:30];
Person * p3 = [Person personWithName:@"alex2" Age:10];
Person * p4 = [Person personWithName:@"alex3" Age:40];
Person * p5 = [Person personWithName:@"alex4" Age:80];
    
NSArray * persons = @[p1, p2, p3, p4, p5];
//定义谓词对象,谓词对象中包含了过滤条件
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"age < 30"];
//使用谓词条件过滤数组中的元素,过滤之后返回查询的结果
NSArray *array = [persons filteredArrayUsingPredicate:predicate];
```

### 什么是类工厂方法?

类工厂方法就是用来快速创建对象的类方法, 他可以直接返回一个初始化好的对象,具备以下特征:

1. 一定是类方法
2. 返回值需要是 id/instancetype 类型
3. 规范的方法名说说明类工厂方法返回的是一个什么对象,一般以类名首字母小写开始;

比如系统 UIButton 的buttonWithType 就是一个类工厂方法:

```objc
// 类工厂方法
+ (instancetype)buttonWithType:(UIButtonType)buttonType;
// 使用
+ UIButton * button = [UIButton buttonWithType:UIButtonTypeCustom];
```

### Svn 和 Git 区别

- svn 和 git 都是用来对项目进行版本控制以及代码管理的.可以监测代码及资源的更改变化.有利于实现高效的团队合作;
- svn 是集中式的,集中式是指只有一个远程版本库,git 是分布式的,分布式有本地和远程版本库,本地仓库都保留了整个项目的完整备份;
  如果存储远程版本库的服务器挂了，所有人的代码都无法提交，甚至丢失版本库, git则因为有本地版本库而不会有这个问题。
- 由于两者的架构不同,git 和 svn 的分支也是不同的, svn 的分支是一个完整的目录,包含所有的实际文件,和中心仓库是保持同步的,如果某个团队成员创建新的分支,那么会同步到所有的版本成员中,所有人都会收到影响. 而 git下创建的分支合并前是不会影响到任何人的.创建分支可以在本地脱机进行任何操作.测试无误后在合并到主分支,然后其他成员才可以看得到.

### CocoaPods理解

CocoaPods 是一个 objc 的依赖管理工具，而其本身是利用 ruby 的依赖管理 gem 进行构建的

- 想深入了解这个命令执行的详细内容，可以在这个命令后面加上 --verbose。现在运行这个命令 pod install --verbose
- CocoaPod三方库,会优先编译

### --verbose 和 --no-repo-update有什么用?

- verbose意思为 冗长的、啰嗦的，一般在程序中表示详细信息。此参数可以显示命令执行过程中都发生了什么。
- pod install或pod update可能会卡在Analyzing dependencies步骤，因为这两个命令会升级 CocoaPods 的 spec 仓库，追加该参数可以省略此步骤，命令执行速度会提升。

### 简要说明const,宏,static,extern区分以及使用?

**const**

```objc
const常量修饰符,经常使用的字符串常量，一般是抽成宏，但是苹果不推荐我们抽成宏，推荐我们使用const常量。

- const 作用：限制类型
- 使用const修饰基本变量, 两种写法效果一致 , b都是只读变量
  const int b = 5; 
  int const b = 5;   
- 使用const修饰指针变量的变量 
  第一种: const int *p = &a 和 int const *q = &a; 效果一致,*p 的值不能改,p 的指向可以改; 
  第二种: int * const p = &a;  表示 p 的指向不能改,*p 的值可以改
  第三种: 
  const int * const p = &a; *p 值和 p 的指向都不能改
  
  const 在*左边, 指向可变, 值不可变
  const 在*的右边, 指向不可变, 值可变
  const 在*的两边, 都不可变
```

**宏**

```objc
* 基本概念：宏是一种批量处理的称谓。一般说来，宏是一种规则或模式，或称语法替换 ，用于说明某一特定输入（通常是字符串）如何根据预定义的规则转换成对应的输出（通常也是字符串)。这种替换在预编译时进行，称作宏展开。编译器会在编译前扫描代码，如果遇到我们已经定义好的宏那么就会进行代码替换，宏只会在内存中copy一份，然后全局替换，宏一般分为对象宏和函数宏。 宏的弊端：如果代码中大量的使用宏会使预编译时间变长。

const与宏的区别？

* 编译检查 宏没有编译检查，const有编译检查；
* 宏的好处 定义函数，方法 const不可以；
* 宏的坏处 大量使用宏，会导致预编译时间过长
```

**static**

```objc
* 修饰局部变量: 被static修饰局部变量，延长生命周期，跟整个应用程序有关，程序结束才会销毁,被 static 修饰局部变量，只会分配一次内存
* 修饰全局变量: 被static修饰全局变量，作用域会修改，也就是只能在当前文件下使用
```

**extern**

```objc
声明外部全局变量(只能用于声明，不能用于定义)

常用用法（.h结合extern联合使用）
如果在.h文件中声明了extern全局变量，那么在同一个类中的.m文件对全局变量的赋值必须是：数据类型+变量名（与声明一致）=XXXX结构。并且在调用的时候，必须导入.h文件。代码如下：

.h
@interface ExternModel : NSObject
extern NSString *lhString;
@end 
.m     
@implementation ExternModel
NSString *lhString=@"hello";
@end

调用的时候：例如：在viewController.m中调用，则可以引入：ExternModel.h，否则无法识别全局变量。当然也可以通过不导入头文件的方式进行调用（通过extern调用）。
```

### 编译型和解释型的区别?

- 编译型语言: 首先是将源代码编译生成机器指令，再由机器运行机器码 (二进制)。
- 解释型语言: 源代码不是直接翻译成机器指令，而是先翻译成中间代码，再由解释器对中间代码进行解释运行。

### 动态语言和静态语言?

- 动态类型语言: 是指数据类型的检查是在运行时做的。用动态类型语言编程时，不用给变量指定数据类型，该语言会在你第一次赋值给变量时，在内部记录数据类型。
- 静态类型语言: 是指数据类型的检查是在运行前（如编译阶段）做的。

### 什么是指针常量和常量指针？

#### 指针常量——指针类型的常量（int *const p）

本质上一个常量，指针用来说明常量的类型，表示该常量是一个指针类型的常量。**在指针常量中，指针自身的值是一个常量，不可改变，始终指向同一个地址。在定义的同时必须初始化。**用法如下：

```objc
int a = 10, b = 20;
int * const p = &a;
*p = 30; // p指向的地址是一定的，但其内容可以修改
```

#### 常量指针——指向“常量”的指针（const int *p， int const *p）

常量指针本质上是一个指针，常量表示指针指向的内容，说明该指针指向一个“常量”。**在常量指针中，指针指向的内容是不可改变的，指针看起来好像指向了一个常量。**用法如下：

```objc
int a = 10, b = 20;
const int *p = &a;
p = &b; // 指针可以指向其他地址，但是内容不可以改变
```

### 指针函数和函数指针

**指针函数**

- 指针函数： 顾名思义，它的本质是一个函数，不过它的返回值是一个指针。

```objc
// 指针函数
int *sum(int a, int b){
    int result = a + b;
    int *c = &result;
    return c;
}
int *p = sum(10, 20);
printf("sum:%d\n", *p);
```

**函数指针**

- 与指针函数不同，函数指针 的本质是一个指针，该指针的地址指向了一个函数，所以它是指向函数的指针。

```objc
// 函数指针
int max(int a, int b){
    return (a > b)?a:b;
}
int (*p)(int, int) = max;
int result = p(10, 20);
printf("result:%d\n", result);
```

### 自定义宏 #define MIN(A,B) A<B?A:B 代码运行结果?

```objc
float a = 1;
float b = MIN(a++,1.5);
问 a= ? b = ?
答案: a = 3; b = 2
a++ 会后执行, a++在表达式出现了2次,得3,  a++<1.5,返回a++,得2

// 扩展
float a = 1;
float b = [self getMax:a++ b:1.5];
- (CGFloat)getMax:(CGFloat ) a b:(CGFloat)b{
   return a>b?a:b;
}
运行 a = 2; b =1.5;
```