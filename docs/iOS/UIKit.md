---
title: UIKit
date: 2020-12-03
updated: 2020-12-03 
tags: 
  - iOS
categories: iOS
description: UIKit 
cover: /images/objective-C.png
top_img: /images/objective-C-bg.png
typora-root-url: ../../../source
---



### UIView



### UIWindow

提供 App 中展示内容的基础窗口

只作为容器，与 ViewController 一起协同工作

通常屏幕上只存在、展示一个 UIWindow

#### App结构

![image-20201203174517382](/assets/image-20201203174517382.png)

![image-20201203174551163](/assets/image-20201203174551163.png)





#### 创建 UIWindow

1. 创建 UIWindow
2. 设置 rootViewController
3. makeKeyAndVisible



### UITabbarController

底部导航页

- 开源：Tabbar / TabbarController

### UINavigationBar

页面跳转管理



### delegate设计模式

### UITableView

1. 提供最基础的列表类型视图组件
2. 提供默认基础的UITableViewCell 样式、header和 footer 的管理
3. 提供针对 UITableViewCell 的复用回收逻辑
4. 提供列表基础功能，如点击、删除、插入等

#### 基本使用

1. 创建 UITableView 设置 delegate 和 DataSource，通过两个 delegate
2. 选择实现 UITableViewDataSource 中方法，行数、cell复用
3. 选择实现 UITableViewDelegate 中方法（高度、headerFooter、点击）

#### UITableViewDataSource

#### UITableViewCell重用

系统提供复用回收池，根据reuseIdentifier作为标识

dequeueReusableCellWithIdentifier

![image-20201203202245145](/assets/image-20201203202245145.png)

#### UITableViewDelegate

1. 提供滚动过程中，UITableViewCell 的出现、消失时机
2. 提供 UITableViewCell 的高度、Header 以及 footers 设置
3. 提供 UITableViewCell 各种行为的回调（点击、删除等）

### UICollectionView

和 UITableView 有相同的 Api 设计理念：基于 datasource 以及 delegate 驱动

#### UICollectionViewCell

1. 继承自 UICollectionReusableView
2. 只有 contentView / backgroundView
3. 不是以行为设计继承
4. 必须先注册 Cell 类型用于重用

### UIScrollView

