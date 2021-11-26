---
Ltitle: Lock 
date: 2020-05-05
updated: 2020-05-05
tags: 
  - iOS
  - Objective-C
  - Swift
categories: iOS
description: iOS 
cover: /images/Xcode.png
top_img: /images/Xcode.png
typora-root-url: ../../../source
---



假如线程`A`和线程`B`使用同一个锁`LOCK`，此时线程A首先获取到锁`LOCK.lock()`，并且始终持有不释放。如果此时B要去获取锁，有四种方式：

- `LOCK.lock()`: 此方式会始终处于等待中，即使调用`B.interrupt()`也不能中断，除非线程A调用`LOCK.unlock()`释放锁。
- `LOCK.lockInterruptibly()`: 此方式会等待，但当调用`B.interrupt()`会被中断等待，并抛出`InterruptedException`异常，否则会与`lock()`一样始终处于等待中，直到线程A释放锁。
- `LOCK.tryLock()`: 该处不会等待，获取不到锁并直接返回false，去执行下面的逻辑。
- `LOCK.tryLock(10, TimeUnit.SECONDS)`：该处会在10秒时间内处于等待中，但当调用`B.interrupt()`会被中断等待，并抛出`InterruptedException`。10秒时间内如果线程A释放锁，会获取到锁并返回true，否则10秒过后会获取不到锁并返回false，去执行下面的逻辑。

