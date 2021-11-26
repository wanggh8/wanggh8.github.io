---
title: iOS Objective-C 命名规范
date: 2021-05-27
updated: 2021-05-28
tags: 
  - iOS
  - Objective-C
categories: iOS
description: iOS Objective-C 命名规范 
cover: /images/Xcode.png
top_img: /images/Xcode.png
typora-root-url: ../../../source
---

- 项目命名规则

必须统一使用代表项目意义的名字。例如：Xcode中的项目文件统一命名。
可在target中 `Class Prefix` 统一配置类前缀。

- 公共文件

统一命名为 `xx_Constant.h`，`CommonConfig.h`，`Macros.h`。

对于文件的目录要按如下结构创建 （放在Project路径下的ProjectName文件夹下）：

⚠️ 所有文件夹必须是 **实体文件夹**

- Global（全局文件，例如：`xx_Constant.h`，`CommonConfig.h`，`Macros.h`）
- ThirdLibs (第三方类库统一目录)
- Widget (自定义 UI组件)
- Utils（自定义Utils，Helper，类别工具）
- Resource （资源文件夹），其它资源文件放在单独的目录Resource中，并做细分。
- - 图片资源全部放到 Assets.xcassets，另外为了方便图片资源文件管理，可以使用PDF矢量图片。
- Class（模块文件），每个模块建立独立的文件夹进行管理。
- - Models (数据模型)
- - Views（视图文件）
- - Controllers（控制器）。





- 不要缩写事物的名称。将它们说清楚，即使它们很长：
- 尝试在整个项目中使用一致名称。
- 前缀是编程接口中名称的重要组成部分。它们区分软件的功能区域。通常，此软件打包在框架中，或者打包在紧密相关的框架中（例如，Foundation和Application Kit）。前缀可防止第三方开发人员定义的符号与Apple定义的符号之间以及Apple自己的框架中的符号之间发生冲突。前缀具有规定的格式。
  - 它由两个或三个大写字母组成，不使用下划线或“子前缀”。
  - 在命名类，协议，函数，常量和typedef结构时，请使用前缀。命名方法时请勿使用前缀；方法存在于由定义它们的类创建的名称空间中。另外，请勿使用前缀来命名结构的字段。

## 命名API

- 对于由多个单词组成的名称，请勿将标点符号用作名称的一部分或用作分隔符（下划线，破折号等）；取而代之的是，将每个单词的第一个字母大写并一起运行这些单词（例如，runTheWordsTogether），这就是骆驼装箱。但是，请注意以下条件：
  - 对于方法名称，请以小写字母开头，并大写嵌入单词的第一个字母。不要使用前缀。
  - 对于函数和常量的名称，请使用与相关类相同的前缀，并大写嵌入单词的首字母。
- 避免使用下划线字符作为前缀，这意味着方法名称中是私有的（允许使用下划线字符作为实例变量名称的前缀）。苹果保留使用此约定。第三方使用可能会导致名称空间冲突；他们可能会不知不觉地使用自己的方法覆盖现有的私有方法，从而造成灾难性的后果。有关私有API遵循的约定的建议，请参见私有方法。

## Class and Protocol Names

类的名称应包含一个名词，以清楚地指示该类（或该类的对象）表示或做什么。该名称应具有适当的前缀（请参阅前缀）。 Foundation和应用程序框架中充满了示例；一些是NSString，NSDate，NSScanner，NSApplication，UIApplication，NSButton和UIButton。协议应根据它们对行为的分组方式来命名：

- 大多数协议都会将与方法相关的方法归为一组，而这些方法并不与任何类特别相关。应当为此类协议命名，以免将协议与类混淆。常见的约定是使用... ing形式
- 一些协议将许多不相关的方法组合在一起（而不是创建几个单独的小型协议）。这些协议倾向于与作为该协议主要表达的类相关联。在这些情况下，约定是为协议赋予与类相同的名称。
  此类协议的一个示例是NSObject协议。该协议对方法进行了分组，您可以使用这些方法来查询任何对象在类层次结构中的位置，使其调用特定的方法，以及增加或减少其引用计数。由于NSObject类提供了这些方法的主要表达，因此协议以该类命名。

## Header Files

- 声明隔离的类或协议。如果某个类或协议不是组的一部分，则将其声明放在一个单独的文件中，该文件的名称与所声明的类或协议的名称相同。
- 声明相关的类和协议。对于一组相关的声明（类，类别和协议），请将声明放在带有主要类，类别或协议的名称的文件中。
- 包括框架头文件。每个框架都应该有一个以该框架命名的头文件，该头文件包括该框架的所有公共头文件。

## Naming Methods

### General Rules

- 名称以小写字母开头，并大写嵌入单词的第一个字母。不要使用前缀。
- 对于表示对象采取的操作的方法，请以动词开头名称：select、invoke等
- 如果该方法返回接收方的属性，请在该属性后命名该方法。除非间接返回一个或多个值，否则不需要使用“ get”。
- 使参数之前的单词描述参数。
- 当您创建比继承的方法更具体的方法时，请在现有方法的末尾添加新的关键字。
- 不要使用“和” 连接作为接收者属性的关键字。如果该方法描述了两个单独的操作，请使用“和”链接它们。

### Accessor Methods

访问器方法是那些设置和返回对象属性值的方法。根据属性的表达方式，它们具有某些推荐的形式：

如果该属性表示为名称，则格式为：

```objc
- (type)noun;
- (void)setNoun:(type)aNoun;
```

如果该属性表示为形容词，则格式为：

```objc
- (BOOL)isEditable;
- (void)setEditable:(BOOL)flag;
```

如果该属性表示为形容词，则格式为：

```objc
- (BOOL)showsAlpha;
- (void)setShowsAlpha:(BOOL)flag;
```

- 不要通过分词把动词变成形容词，例如 acceptsGlyphInfo 和 glyphInfoAccepted 
- 您可以使用情态动词（以“ can”，“ should”，“ will”等开头的动词）来阐明含义，但不要使用“ do”或“ does”。
- 仅将“ get”用于间接返回对象和值的方法。仅当需要返回多个项目时，才应将此表格用于方法。

### Delegate Methods

委托方法（或委托方法）是对象在某些事件发生时在其委托中调用的方法（如果委托实现了委托方法）。它们具有独特的形式，同样适用于对象数据源中调用的方法：

- 通过标识发送消息的对象的类别来开始命名，类名省略前缀，首字母小写。
- 除非该方法只有一个参数，即发送方，否则在类名后加一个冒号（该参数是对委派对象的引用）。
- 例外情况是由于发布通知而调用的方法。在这种情况下，唯一的参数是通知对象。
- 对调用的方法使用“ did”或“ will”来通知委托人某事已发生或将要发生。
- 尽管可以将“ did”或“ will”用于调用以要求委托人代表另一个对象做某事的方法，但“ should”是优选的。

### Collection Methods

对于管理对象集合（每个称为该集合的元素）的对象，约定应具有以下形式的方法：

```objc
- (void)addElement:(elementType)obj;
- (void)removeElement:(elementType)obj;
- (NSArray *)elements;
- (void)insertElement:(elementType)obj atIndex:(int)index;
- (void)removeElementAtIndex:(int)index;
```

- 如果集合确实是无序的，则返回NSSet对象而不是NSArray对象。

### Method Arguments

- 与方法一样，参数以小写字母开头，连续单词的首字母大写（例如，删除Object：（id）Object）。
- 名称中请勿使用“指针”或“ pointer”。让参数的类型而不是名称声明它是否是指针。
- 避免为参数使用一个字母和两个字母的名称。
- 避免只保存几个字母的缩写。

传统上，以下关键字和参数一起使用：

```objc
...action:(SEL)aSelector
...alignment:(int)mode
...atIndex:(int)index
...content:(NSRect)aRect
...doubleValue:(double)aDouble
...floatValue:(float)aFloat
...font:(NSFont *)fontObj
...frame:(NSRect)frameRect
...intValue:(int)anInt
...keyEquivalent:(NSString *)charCode
...length:(int)numBytes
...point:(NSPoint)aPoint
...stringValue:(NSString *)aString
...tag:(int)anInt
...target:(id)anObject
...title:(NSString *)aString
```

### Private Methods

在大多数情况下，私有方法名称通常遵循与公共方法名称相同的规则。但是，通常的约定是为私有方法提供前缀，因此很容易将它们与公共方法区分开。即使采用此约定，给私有方法指定的名称也可能导致特殊类型的问题。当设计Cocoa框架类的子类时，您将无法知道您的私有方法是否无意中覆盖了同名的私有框架方法。

- Cocoa框架中大多数私有方法的名称都有下划线前缀（例如_fooData），以将其标记为私有。基于这个事实，遵循两个建议。
- 如果您要继承大型Cocoa框架类（例如NSView或UIView），并且要绝对确保私有方法的名称与超类中的名称不同，则可以在私有方法中添加自己的前缀。前缀应尽可能唯一，也许是基于您公司或项目的前缀，形式为“ XX_”。因此，如果您的项目名为Byte Flogger，则前缀可能是BF_addObject：
- 尽管给私有方法名称加前缀的建议似乎与先前的主张称其方法存在于其类的命名空间中相矛盾，但此处的意图有所不同：防止意外覆盖超类私有方法。

## Naming Functions

Objective-C允许您通过函数和方法来表达行为。当基础对象总是单例或您正在处理明显的功能子系统时，您应该使用函数而不是类方法。

函数有一些一般的命名规则，你应该遵循：

- 函数名与方法名一样形成，但有几个例外：
  - 它们以您用于类和常量的相同前缀开头。
  - 前缀后的第一个字母大写。
- 大多数函数名以描述函数效果的动词开头：



查询属性的函数有另外一组命名规则：

- 如果函数返回其第一个参数的属性，请省略动词。

- 如果引用返回值，请使用“获取”。

  `const char *NSGetSizeAndAlignment（const char *typePtr，unsigned int *sizep，unsigned int *alignp）`

## Naming Properties and Data Types

本节描述了声明的属性，实例变量，常量，通知和异常的命名约定。

### Declared Properties and Instance Variables

声明的属性有效地声明了属性的访问器方法，因此命名已声明属性的约定与命名访问器方法的约定大致相同（请参阅访问器方法）。但是，如果声明的属性的名称表示为形容词，则属性名称会省略“ is”前缀，但可以指定get访问器的常规名称。

```objc
@property (strong) NSString *title;
@property (assign) BOOL showsAlpha;
@property (assign, getter=isEditable) BOOL editable;
```

确保实例变量的名称简洁地描述了存储的属性。通常，您不应该直接访问实例变量。相反，您应该使用访问器方法（您可以直接在init和dealloc方法中访问实例变量）。为了说明这一点，请在实例变量名称前加上下划线（_），例如：

```objc
@implementation MyClass {
    BOOL _showsTitle;
}
```

如果使用声明的属性合成实例变量，请在@synthesize语句中指定实例变量的名称。

```objc
@implementation MyClass
@synthesize showsTitle=_showsTitle;
```

- 避免显式声明公共实例变量。
  开发人员应该关注对象的界面，而不要关注对象如何存储数据的细节。您可以通过使用声明的属性并合成相应的实例变量来避免显式声明实例变量。
- 如果需要声明实例变量，请使用@private或@protected明确声明它。
  如果您希望您的类将被子类化，并且这些子类将需要直接访问数据，请使用@protected指令
- 如果实例变量将作为类实例的可访问属性，请确保为其编写访问器方法（如果可能，请使用声明的属性）。

### Constants

常量的规则根据常量的创建方式而有所不同

#### Enumerated constants

- 对具有整数值的相关常数组使用枚举。
- 枚举常量和对其进行分组的typedef遵循函数的命名约定（请参阅命名函数）。以下示例来自NSMatrix.h：

```objc
typedef enum _NSMatrixMode {
    NSRadioModeMatrix           = 0,
    NSHighlightModeMatrix       = 1,
    NSListModeMatrix            = 2,
    NSTrackModeMatrix           = 3
} NSMatrixMode;
```

您可以创建位运算的枚举，例如：

```objc
enum {
    NSBorderlessWindowMask      = 0,
    NSTitledWindowMask          = 1 << 0,
    NSClosableWindowMask        = 1 << 1,
    NSMiniaturizableWindowMask  = 1 << 2,
    NSResizableWindowMask       = 1 << 3
};
```

### Constants created with const

使用const为浮点值创建常量。如果该常量与其他常量无关，则可以使用const创建一个整数常量。否则，请使用枚举。

```objc
const float NSLightGray;
```

### Other types of constants

- 通常，请勿使用#define预处理程序命令创建常量。如上所述，对于整数常量，请使用枚举；对于浮点常量，请使用const限定符。
- 使用大写字母表示预处理器在确定是否将处理代码块时评估的符号。例如：`#ifdef DEBUG`
- 请注意，编译器定义的宏具有前后双下划线字符。例如：`__MACH__`
- 为用于通知目的名称和字典键之类的字符串定义常量。通过使用字符串常量，可以确保编译器验证指定的正确值（即，它执行拼写检查）。 Cocoa框架提供了许多字符串常量示例，例如：`APPKIT_EXTERN NSString *NSPrintCopies;` 。实际的NSString值分配给实现文件中的常量。 （请注意，APPKIT_EXTERN宏对Objective-C的值为extern。）

**常量命名**

- 若常量作用域局限于某编译单元（实现文件）内，则在常量名前面添加字母 `k`，如 `static const NSTimeInterval kAnimationDuration = 0.3;` 。为什么会是 `k` 呢？这其实是历史原因，在匈牙利命名法中，`k` 的意思是 constants，用以表示常量前缀。
- 若常量作用域超出编译单元，在类外可见时，使用类名前缀，如 `extern NSString *const EOCStringConstant;`。

## Notifications and Exceptions

通知和例外的名称遵循类似的规则。但是两者都有自己的推荐用法模式。

### Notifications

如果类具有委托，则委托的大多数通知可能会通过定义的委托方法由委托接收。这些通知的名称应反映相应的委托方法。例如，每当应用程序 `NSApplicationDidBecomeActiveNotification` 时，全局 `NSApplication` 对象的委托都会自动注册以接收 `applicationDidBecomeActive：`消息。

通知由名称由以下方式组成的全局NSString对象标识：

```objc
[Name of associated class] + [Did | Will] + [UniquePartOfName] + Notification
```

例如：

```objc
NSApplicationDidBecomeActiveNotification
NSWindowDidMiniaturizeNotification
NSTextViewDidChangeSelectionNotification
NSColorPanelColorDidChangeNotification
```

### Exceptions

尽管您可以随意选择将异常（即NSException类和相关函数提供的机制）用于任何选择的目的，但是Cocoa保留了用于编程错误的异常，例如数组索引超出范围。可可不使用异常来处理常规的预期错误情况。对于这些情况，请使用返回的值，例如nil，NULL，NO或错误代码。有关更多详细信息，请参见《错误处理编程指南》。

异常由全局NSString对象标识，其名称以这种方式组成：

```objc
[Prefix] + [UniquePartOfName] + Exception
```

名称的唯一部分应将组成词放在一起，并大写每个词的第一个字母。这里有些例子：

```objc
NSColorListIOException
NSColorListNotEditableException
NSDraggingException
NSFontUnavailableException
NSIllegalSelectorException
```



## 参考

https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingFunctions.html#//apple_ref/doc/uid/20001283-BAJGGCAD

https://www.jianshu.com/p/06cc78e8b8b6

https://www.jianshu.com/p/f4bebecf4100
