---
title: iOS面经
date: 2021-03-08
updated: 2021-03-08 
tags: 
  - Swift
  - iOS
categories: iOS
description: iOS面经
cover: /images/swift.jpg
top_img: /images/swift.jpg
typora-root-url: ../../../source
hidden: true
---

## Internet

### 操作系统

- 进程和线程的区别
- 进程的通信方式
- 线程的通信方式
- 多线程安全（包括锁、信号量，互斥锁、自旋锁，iOS相关的处理方法等）
- 线程的状态
- 线程管理（包括多级反馈队列、FCFS、LCFS等）
- 线程死锁的条件
- 虚存是用来做什么的？
- 缺页中断
- 页管理方式（包括LRU算法、FIFO等）
- 程序的分布（包括iOS程序的分布等）
- 函数调用栈
- 堆栈和栈的区别

### 计算机网络

- TCP和UDP的区别
- TCP流量控制、拥塞控制
- HTTP包头字段
- HTTP和HTTPS的区别
- 七层网络模型
- 五层网络模型
- 浏览器输入URL，回车之后发生了什么？（从硬件和网络的角度上）
- Cookie和Session区别
- 浏览器内核（包括WebKit/Blink等）

### 数据结构

- 数据结构（数组、链表、二叉树、堆、栈、队列等）
- 算法理论（排序、二分查找、partition、buildMaxHeap、单调栈、动态规划、回溯法、贪心法等）

### iOS

- iOS必备考点：多线程（包括GCD、NSOperationQueue等）、Block（包括循环引用等）、内存管理（包括ARC、属性等）、分类（包括关联对象等）、UI相关
- iOS底层原理：Runtime（包括消息机制、Class方法、方法交换等）和RunLoop
- iOS项目（工作或者实习所做的事）：亮点和贡献

### 编译原理

- 编译器前端
- 编译器后端
- LLVM
- LLDB
- 语法树
- 正则表达式

## Tencent

- [ ] 视频从采集到传输到播放的过程

- [ ] 朋友圈列表滚动时加载图片引起卡顿（卡顿的原因、如何优化、图片下载提速）

- [ ] 每个线程是否都有 RunLoop，NSThread 创建的是否也有，C函数创建的是否也有

  > 线程和RunLoop并不是包含的关系，而是对应的关系。线程创建时，对应的RunLoop如果不调用暂时不创建（如图中子线程2，它可以对应RunLoop，但是默认并不提前创建出来，除非你需要在子线程中使用RunLoop，再手动调用下面方法获取：[NSRunLoop currentRunLoop]; 获取方法内部进行了判断，如果此时没有runloop，会自动帮你创建一个。主线程对应的runloop程序启动时默认创建好了。线程销毁时，对应的RunLoop也跟着销毁。
  >
  > RunLoop是iOS事件响应与任务处理最核心的机制
  >
  > 主线程的RunLoop在应用启动的时候就会自动创建
  >
  > 其他线程则需要在该线程下自己启动
  > 不能自己创建RunLoop
  > RunLoop并不是线程安全的，所以需要避免在其他线程上调用当前线程的RunLoop
  > RunLoop负责管理autorelease pools（RunLoop负责管理autorelease poolsRunLoop通常在2次sleep之间释放autorelease的对象，也就是在RunLoop执行一圈完成的时候。）
  > RunLoop负责处理消息事件，即输入源事件和计时器事件

- [ ] 你项目中印象深刻的技术难点

- [ ] RunLoop 是什么

  >  
  >
  >  5个默认Model
  >
  >  ```objc
  >  kCFRunLoopDefaultMode //App的默认Mode，通常主线程是在这个Mode下运行 
  >  UITrackingRunLoopMode //界面跟踪 Mode，用于 ScrollView 追踪触摸滑动，保证界面滑动时不受其他 Mode 影响 
  >  UIInitializationRunLoopMode // 在刚启动 App 时第进入的第一个 Mode，启动完成后就不再使用
  >  GSEventReceiveRunLoopMode // 接受系统事件的内部 Mode，通常用不到 
  >  kCFRunLoopCommonModes //这是一个占位用的Mode，不是一种真正的Mode，而是一种模式组合，一个操作 Common 标记的字符串,你可以用这个字符串来操作 Common Items
  >  }
  >  ```
  >




- [ ] 如何控制线程的并发数（NSOperationQueue、 GCD 如何控制，如果让你去实现如何做）

  > https://www.jianshu.com/p/5d51a367ed62
  >
  > NSOperationQueue中，已经考虑到了最大并发数的问题，并提供了**maxConcurrentOperationCount**属性**设置最大并发数**(该属性需要在任务添加到队列中之前进行设置)。maxConcurrentOperationCount默认值是-1；如果值设为0，那么不会执行任何任务；如果值设为1，那么该队列是串行的；如果大于1，那么是并行的。
  >
  > GCD的信号量机制(dispatch_semaphore）
  >
  > **信号量**是一个整型值，有初始计数值；可以接收**通知信号**和**等待信号**。当信号量收到通知信号时，计数+1；当信号量收到等待信号时，计数-1；如果信号量为0，线程会被阻塞，直到信号量大于0，才会继续下去。
  >
  > 使用信号量机制可以实现线程的同步，也可以控制最大并发数。以下是如何控制最大并发数的代码。
  >
  > ```objc
  > dispatch_queue_t workConcurrentQueue = dispatch_queue_create("cccccccc", DISPATCH_QUEUE_CONCURRENT);
  > dispatch_queue_t serialQueue = dispatch_queue_create("sssssssss",DISPATCH_QUEUE_SERIAL);
  > dispatch_semaphore_t semaphore = dispatch_semaphore_create(3);
  > 
  > for (NSInteger i = 0; i < 10; i++) {
  >   dispatch_async(serialQueue, ^{
  >       dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER); // -1
  >       dispatch_async(workConcurrentQueue, ^{
  >           NSLog(@"thread-info:%@开始执行任务%d",[NSThread currentThread],(int)i);
  >           sleep(1);
  >           NSLog(@"thread-info:%@结束执行任务%d",[NSThread currentThread],(int)i);
  >           dispatch_semaphore_signal(semaphore);}); // +1
  >   });
  > }
  > NSLog(@"主线程...!");
  > ```

- [ ] 有了解过 AVFundation 吗

- [ ] MMKV 使用场景

- [ ] 如何限制创建线程数，如果使用并行，是否需要加锁

  > https://www.jianshu.com/p/35dd92bcfe8c

- [ ] 如何保证数据安全（加密算法、加密传输）

- [ ] 网络请求方式有那些

- [ ] RunLoop 工作过程

  > RunLoop 包含若干个 Mode，每个 Mode 又包含若干个 Source/Timer/Observer。每次调用 RunLoop 时，只能指定其中一个 Mode（每次运行CFRunLoopRun()函数时必须指定**Mode**，CFRunLoopRun()就是runloop的入口函数，后面源会提到），这个Mode被称作 CurrentMode。如果需要切换 Mode，只能退出当前的循环，再重新指定一个 Mode 进入。
  >
  > Runloop处理逻辑：
  >  1，通知Observer，即将进入loop
  >
  >  2，通知Observer，将要处理timer
  >
  >  3，通知Observer，将要处理Source0
  >
  >  4，处理Source0
  >  5，如果有Source1，跳到第9步
  >
  >  6，通知Obesrcer，线程即将休眠
  >
  >  7，休眠，等待唤醒
  >
  >  8，通知Observer，线程刚被唤醒
  >
  >  9，处理唤醒时收到的消息，之后跳回2
  >
  >  10，通知Oberver，即将退出Loop

- [ ] SDWebImage

- [ ] 三方库管理

## NetEase

- [ ] 如何监控RunLoop状态 Runloop Observer
- [ ] UITableView 使用和滑动优化 
- [ ] viewcell复用机制
- [ ] 工厂模式
- [ ] rxswift和传统编程的区别 Block区别
- [ ] AFNetworking 实现方式
- [ ] WCDB实现方式
- [ ] MMKV实现方法
- [ ] Runtime
- [ ] NSObject 结构（swift和oc）
- [ ] MVVM VM的作用 与MVC的区别
- [ ] 数据库索引
- [ ] RunLoop的几种模式
- [ ] 不使用递归找到二叉树的最大高度
- [ ] 找到字符串中有且只有2种字符的最大子串
- [ ] #UIView与CALayer有什么区别

- [ ] 堆排序 （插入、大小堆）
- [ ] AES有几种（参数）
- [x] runtime msg_sender 参数
- [ ] 堆空间和栈空间区别
- [x] 垃圾回收和引用计数区别（强循环引用的回收）
- [ ] autorelease回收机制
- [x] 方法的参数在堆还是栈（方法内部的对象）
- [x] 大文件找出出现最多的100个字符串
- [x] GCRoot

- [ ] 哈希表重散列
- [ ] 长地址可逆变短地址（冲突解决）
- [ ] 哈希表重散列优化
- [ ] 哈希表生成的短地址是否固定
- [x] 登录时如何默认登录（本地生成token） JWT ：生成并发给客户端之后，后台是不用存储，客户端访问时会验证其签名、过期时间等再取出里面的信息（如username），再使用该信息直接查询用户信息完成登录验证。jwt自带签名、过期等校验，后台不用存储，缺陷是一旦下发，服务后台无法拒绝携带该jwt的请求（如踢除用户）；因为JWT可以使得请求信任、有效避免CSRF攻击所以常用来做移动端或者分布式系统中的单点登录场景（SSO）；但是因为JWT token在注销登录场景下还有效以及token续签的的问题，所以一般都会配合一些其他的解决方案，例如给token加状态等。
- [ ] Auth2.0 [https://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html](https://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)
- [ ] 项目登录是如何做的
- [ ] https和http
- [ ] https加密算法

## 抖音

- [ ] block的原理实现

- [ ] Ios方法的实现是如何存的(方法存到哪里)

- [x] http状态码 503

- [x] http请求体和请求头

- [x] 手机发送网络请求的过程

- [x] https加密方式（对称加密的秘钥）

- [x] ios对象相等（哈希）

- [x] 音视频编码方式（AAC MP3、Xvid H.264 MPEG-4）

- [ ] oc运行时方法调用

- [ ] block的三种类别

- [ ] block在GCD中使用

- [x] tcp

- [x] GCD的队列和线程

- [x] http post和get区别[https://www.w3school.com.cn/tags/html_ref_httpmethods.asp](https://www.w3school.com.cn/tags/html_ref_httpmethods.asp)

- [x] ios集合间的区别（数组、set、字典）

- [x] json xml pb优缺点对比

- [x] calayer和UIVIEW区别

  > 1.首先UIView可以响应事件，CALayer不可以。
  >
  > UIKit使用UIResponder作为响应对象，来响应系统传递过来的事件并进行处理。UIApplication、UIViewController、UIView、和所有从UIView派生出来的UIKit类（包括UIWindow）都直接或间接地继承自UIResponder类。
  > 2.CALayer直接继承 NSObject，并没有相应的处理事件的接口。UIView主要是对显示内容的管理而 CALayer 主要侧重显示内容的绘制。
  >
  > 3.在做 iOS 动画的时候，修改非 RootLayer的属性（譬如位置、背景色等）会默认产生隐式动画，而修改UIView则不会。
  >
  > 对于每一个 UIView 都有一个 layer,把这个 layer 且称作RootLayer,而不是 View 的根 Layer的叫做 非 RootLayer。我们对UIView的属性修改时时不会产生默认动画，而对单独 layer属性直接修改会，这个默认动画的时间缺省值是0.25s。
  
- [x] 修改calayer属性后什么时候可以生效


##### 算法

请使用OC/Swift编码实现一个基础Person类，有姓名、身份证号码、朋友关系3个属性，朋友关系可进行添加修改操作。注意实现判等方法与初始化方法

算法题 若数组中全为正整数，且第一和最后一个也不能相临，求数组下标不连续的最大和，如输入[3, 2, 3, 2, 3, 2]，输出6

```swift
import Foundation
//let line = readLine()
//print(line)
print(maxSum([3,2,3,2,3,2]))
func maxSum(_ array: [Int]) -> Int {
    let n = array.count
    if n == 0 {
        return 0
    }
    if n < 3 {
        return array.max()!
    }
    var dp = Array<Int>(repeating: 0, count: n)
    dp[0] = array[0]
    dp[1] = array[1]
    for i in 2..<n {
        dp[i] = max(dp[i - 1], dp[i - 2] + array[i])
    }
    return dp[n - 1]
}
```

