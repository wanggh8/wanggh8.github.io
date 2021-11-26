---
title: GCD 多线程
categories: Foundation
description: GCD 多线程
typora-root-url: ../../../source
---

# GCD 多线程



## Dispatch Queue

执行处理的等待队列，分为：Serial Dispatch Queue 和 Concurrent Dispatch Queue。

| Dispatch Queue 种类       | 备注                     |
| ------------------------- | ------------------------ |
| Serial Dispatch Queue     | 等待现在执行中处理结束   |
| Concurrent Dispatch Queue | 不等待现在执行中处理解锁 |

### dispatch_queue_create

```objc
dispatch_queue_t serialDispatchQueue = dispatch_queue_create("com.example.serialDispatchQueue", NULL);
dispatch_queue_t concurrentDispatchQueue = dispatch_queue_create("com.example.concurrentDispatchQueue", DISPATCH_QUEUE_CONCURRENT);
```

iOS 6.0 或 macOS 10.8 之前需手动释放队列和保持队列，之后 GCD 对象已纳入 ARC 管理范围

```objc
dispatch_release(serialDispatchQueue); // 已废弃
dispatch_retain(concurrentDispatchQueue); // 已废弃
```

### Main Dispatch Queue

主线程队列，追加到主线程的任务在主线程的 RunLoop 处理



### Global Dispatch Queue

Global Dispatch Queue 是所有应用程序都能使用的 Concurrent Dispatch Queue。Global Dispatch Queue 有 4 个执行优先级，分别为高优先级、默认优先级、低优先级和后台优先级。通过 XNU 内核管理的用于 Global Dispatch Queue 的线程，将各自使用的 Global Dispatch Queue 的执行优先级作为线程的执行优先级使用。在向 Global Dispatch Queue 追加处理时，应选择与处理内容对应的执行优先级的 Global Dispatch Queue。



### dispatch_set_target_queue

### dispatch_after



## Dispatch Group

### dispatch_async

### 

### dispatch_sync



### dispatch_barrier_async



#### dispatch_barrier_sync

### 
