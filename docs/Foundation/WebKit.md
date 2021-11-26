---
title: iOS
description: iOS 
typora-root-url: ..
---

# WebKit



## WKUserContentController

这个类主要用来做native与JavaScript的交互管理

一个对象，用于管理 JavaScript 代码和 Web 视图之间的交互，以及用于过滤 Web 视图中的内容。

- 将JavaScript代码注入在网页视图中运行的网页中。
- 安装自定义JavaScript函数，通过调用应用程序的本机代码。
- 指定自定义过滤器，以防止网页加载受限内容。

## WKUserScript

Web 视图注入网页的脚本 （进行 JavaScript 注入）

## WKScriptMessageHandler

这个协议类专门用来处理监听JavaScript方法从而调用原生OC方法，和WKUserContentController搭配使用。
