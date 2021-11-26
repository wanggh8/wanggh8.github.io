---
title: iOS In-App-Purchase
date: 2021-05-05
updated: 2021-05-05
tags: 
  - iOS
  - Objective-C
  - Swift
categories: iOS
description: iOS 内购
cover: /images/Xcode.png
top_img: /images/Xcode.png
typora-root-url: ../../../source
---

## 内购类型

- 消耗型项目
- 非消耗性项目
- 自动续期订阅
- 非自动续期订阅

|             | 消耗型项目 | 非消耗型项目 | 自动续期订阅 | 非自动续期订阅 |
| ----------- | ---------- | ------------ | ------------ | -------------- |
| 可以Restore | 否         | 是           | 是           | 否             |
| 有时间限制  | 否         | 否           | 是           | 是             |
|             |            |              |              |                |

## 收据验证

On-device validation
Server-side validation
Validates authenticity of receipt
Yes
Yes
Includes renewal transactions
Yes
Yes
Includes additional user subscription information
No
Yes
Handles renewals without client dependency
No
Yes
Resistant to device clock change
No
Yes

## 交易流程图

```mermaid
graph TB
	appStart(App启动)-->register[注册交易观察者]
	register--SKPaymentQueue.defaultQueue addTransactionObserver:-->getProducts(根据ID获取Products)
	
	subgraph apple
	giveProducts[App Store提供Products Info]
	appleHandlePay[Apple处理支付订单]
	appleHandleRestore[Apple处理恢复购买]
	end
	
	subgraph user
	uesrSelectProduct[用户选择Product]
	uesrSelectRestore[用户选择恢复购买]
	end
	getProducts--SKProductsRequest实例方法start-->giveProducts
	giveProducts--SKProductsRequestDelegate-->appStoreUI[展示商店页面]
	giveProducts--SKProductsRequestDelegate-->invaildID[不合法ProductID]
	appStoreUI-->uesrSelectProduct
	uesrSelectProduct-->makePayReq[发送支付请求]
	makePayReq--SKPaymentQueue.defaultQueue addPayment-->appleHandlePay
	appleHandlePay--调用队列观察者的SKPaymentTransactionObserver中 paymentQueue:updatedTransactions:-->appleHandlePayRes[处理支付订单结果]
	appleHandlePayRes--Failed-->payFail[购买失败]
	appleHandlePayRes--Purchased-->payPurchased[已购买]
	appleHandlePayRes--Restored-->payRestored[恢复购买]
	appleHandlePayRes--Deferred-->payDeferred[购买延迟]
	appleHandlePayRes--	Purchasing-->payPurchasing[购买中]
	
	appStoreUI-->uesrSelectRestore
	uesrSelectRestore--SKPaymentQueue.defaultQueue restoreCompletedTransactions-->appleHandleRestore
	appleHandleRestore--调用队列观察者的SKPaymentTransactionObserver中 paymentQueue:updatedTransactions:和 paymentQueueRestoreCompletedTransactionsFinished: 或 paymentQueue:restoreCompletedTransactionsFailedWithError:-->appleHandlePayRes[处理恢复购买结果]
	
	payPurchased--SKPaymentTransaction-->paySuccessed[购买完成]
	payRestored--SKPaymentTransaction的originalTransaction获取到商品id支付凭证等信息-->paySuccessed[购买完成]
	paySuccessed--NSBundle mainBundle appStoreReceiptURL-->getReceipt[获取收据验证交易]
	getReceipt--本地验证-->AppVerifyReceipV[App验证收据]
	getReceipt--服务端验证-->ServerVerifyReceipV[服务端验证收据]
	AppVerifyReceipV-->unlock[解锁内容]
	ServerVerifyReceipV-->unlock[解锁内容]
	unlock--SKPaymentQueue.defaultQueue finishTransaction-->finish[完成交易]
	
	payFail--SKPaymentQueue.defaultQueue finishTransaction-->finish[完成交易]
	
	finish--SKPaymentQueue.defaultQueue removeTransactionObserver:-->appEnd(App停止)
```

