---
title: iOS Block
date: 2021-04-19
updated: 2019-04-19
tags: 
  - Objective-C
  - iOS
categories: iOS
description: iOS Objective-C Block
cover: /images/objective-C.png
top_img: /images/objective-C-bg.png
typora-root-url: ../../../source
---

## 简介



<img src="/assets/image-20210419104023197.png" alt="image-20210419104023197" style="zoom:60%;" />

## 实现

## 注意事项

> 截获自动变量的方法没有实现对C语言数组的捕获，使用指针替代可解决
>
> ```objc
> typedef void (^Blk)(void);
> // 截获自动变量的方法没有实现对C语言数组的捕获
> // 使用指针替代可解决
> // const char text[] = "hello";
> const char *text = "hello";
> Blk blk3 = ^{
> 	printf("%c\n", text[2]);
> };
> blk3();
> ```
>
> 

> 测试2