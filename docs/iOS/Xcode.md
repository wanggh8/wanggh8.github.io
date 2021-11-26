---
title: Xcode
date: 2020-10-25
updated: 2020-10-25 
tags: 
  - Xcode
  - iOS
  - MacOS
categories: iOS
description: Xcode使用总结
cover: /images/MacOS.jpg
top_img: /images/MacOS.jpg
typora-root-url: ../../../source
---

## 快捷键

### Mac OS 外接键盘映射

|   修饰键    |                             符号                             | 外接键盘  |
| :---------: | :----------------------------------------------------------: | :-------: |
| **Command** | ![Command 符号](/assets/9808e0a4c5ca7d8f3661af19ca54058e.png |  **Win**  |
|  **Shift**  | ![Shift 符号](/assets/da6e9b7f91e7eb13915e29d5288d8d3f.png)  | **Shift** |
| **Option**  | ![Option 符号](/assets/4fa4885c9111e0de6faeb637be746e2a.png) |  **Alt**  |
| **Control** | ![Control 符号](/assets/d4a120294e44333f6ec6c00ef4648ee1.png) | **Ctrl**  |

### Xcode常用快捷键

#### 代码编辑

| 代码编辑                                                |               Mac                |
| ------------------------------------------------------- | :------------------------------: |
| 代码左缩进                                              |         **Command + [**          |
| 代码右缩进                                              |         **Command + ]**          |
| 代码格式化                                              |         **Control + I**          |
| 光标移到前一行（previous）                              |         **Control + P**          |
| 光标移到后一行（next）                                  |         **Control + N**          |
| 光标移到行首                                            |         **Control + A**          |
| 光标移到行尾（end）                                     |         **Control + E**          |
| 调换光标两边的字符，并移动光标到两个字符后（transpose） |         **Control + T**          |
| 删除光标右侧字符                                        | **Control + D** **/** **Delete** |
| 删除本行光标右侧所有字符（kill）                        |         **Control + K**          |
| 快速打开文件                                            |     **Command + Shift + O**      |
| Clean Build Folder                                      |     **Command + Shift + K**      |
|                                                         |                                  |
|                                                         |                                  |

#### 查看

| 查看                     |       Mac       |
| ------------------------ | :-------------: |
| 查看当前文件的方法列表   | **Control + 6** |
| 查看当前文件夹的文件列表 | **Control + 5** |
| 查看最近查看历史         | **Control + 2** |

#### 导航

| 导航                                       |            Mac            |
| :----------------------------------------- | :-----------------------: |
| 查看当前文件的上一个部分（头文件或源文件） | **Control + Command + ↑** |
| 查看当前文件的下一个部分（头文件或源文件） | **Control + Command + ↓** |
| 前进                                       | **Control + Command + →** |
| 后退                                       | **Control + Command + ←** |
|                                            |                           |
|                                            |                           |



### Xcode自定义组合快捷键

修改 `Xcode` 里快捷键的配置文件 `(plist)` 权限，打开终端输入如下两条命令:

```
sudo chmod 666 /Applications/Xcode.app/Contents/Frameworks/IDEKit.framework/Resources/IDETextKeyBindingSet.plist
sudo chmod 777 /Applications/Xcode.app/Contents/Frameworks/IDEKit.framework/Resources/
```

打开 `plist` 文件

```
/Applications/Xcode.app/Contents/Frameworks/IDEKit.framework/Resources/IDETextKeyBindingSet.plist
```

#### 复制一行快捷键

 > 在`Insertions and Indentations`下添加 key ：`Duplicate Current Line`，值为：

```
selectLine:, copy:, moveToEndOfLine:, moveToBeginningOfLine:, paste:, moveBackward:
```

绑定快捷键 `Command` + `D`

#### 删除整行快捷键

在 `Deletions ` 下添加一个 key ：`Delete Current Line` 值为：

```
deleteToBeginningOfLine:, moveToEndOfLine:, deleteToBeginningOfLine:, deleteBackward:
```



## 错误处理

### This application's application-identifier entitlement does not match that of the installed application. These values must match for an upgrade to be allowed

原因：这个项目App已经存在这个真机上了，这个App是旧的identifier运行的。

解决方法：Xcode --->Window ---> Devices ---> 自己的真机 ---> installed Apps ----> 删除这个App  ----> 重新运行

