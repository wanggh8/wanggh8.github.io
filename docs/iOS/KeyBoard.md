---
title: iOS键盘相关
date: 2021-02-12
updated: 2021-02-12 
tags: 
  - Swift
  - iOS
categories: iOS
description: iOS键盘相关
cover: /images/swift.jpg
top_img: /images/swift.jpg
typora-root-url: ../../../source
---

## 键盘弹出时视图上移

### 方案一：自定义实现

设置键盘监听

```swift
NotificationCenter.default.addObserver(self, selector: #selector(keybordShow(notification:)), name: UIResponder.keyboardWillShowNotification, object: nil)
        
NotificationCenter.default.addObserver(self, selector: #selector(keybordHide(notification:)), name: UIResponder.keyboardWillHideNotification, object: nil)
```

设置视图上移

```swift
@objc func keybordShow(notification:Notification) {
    print("键盘弹起")
    let userinfo: NSDictionary = notification.userInfo! as NSDictionary
    let nsValue = userinfo.object(forKey: UIResponder.keyboardFrameEndUserInfoKey) as! NSValue
    
    let keyboardRec = nsValue.cgRectValue
    let height = keyboardRec.size.height
    
    let bottomHeight = self.view.frame.size.height - edittingTextField.center.y - edittingTextField.frame.height/2 - 15
    if (bottomHeight < height) {
        let keyBoardHeight = height - bottomHeight
        UIView.animate(withDuration: 0.3) {
            self.view.center = CGPoint(x: self.view.center.x, y: self.view.bounds.height/2 - keyBoardHeight)
        }
    }  
}

@objc func keybordHide(notification:Notification) {
    print("键盘隐藏")
    UIView.animate(withDuration: 0.3) {
        self.view.center = CGPoint(x: self.view.center.x, y: self.view.bounds.height/2)
    } 
}    
```

### 方案二：使用三方库 IQKeyboardManager

```swift
// MARK: 键盘处理
IQKeyboardManager.shared.enable = true
IQKeyboardManager.shared.shouldResignOnTouchOutside = false
```

