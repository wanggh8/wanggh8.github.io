---
title: iOS数据库
date: 2019-05-23 22:31:12
updated: 2019-05-23 22:31:12
tags: 
  - Objective-C
  - iOS
  - 数据库
categories: iOS
description: iOS 数据库
cover: /images/objective-C.png
top_img: /images/objective-C-bg.png
typora-root-url: ../../../source
---

## 数据库模糊查询

` NSString *NSsql=[NSString stringWithFormat:@"SELECT id,name,age FROM t_person WHERE name like '%%%@%%' ORDER BY age ASC;",condition];`

 注意：name like ‘西门’,相当于是name = ‘西门’。name like ‘%西%’,为模糊搜索，搜索字符串中间包含了’西’，左边可以为任意字符串，右边可以为任意字符串，的字符串。但是在 stringWithFormat:中%是转义字符，两个%才表示一个%。

实际操作：

```objc
// 查询数据
        FMResultSet *rs = [self.db executeQuery:@"SELECT * FROM user WHERE  UserName like '%%%@%%' AND Password like '%%%@%%' AND UserIcon like '%%%@%%' AND PhoneNumber like '%%%@%%' AND Email like '%%%@%%' AND Introduction like '%%%@%%' AND Sex like '%%%@%%' AND Birthday like '%%%@%%' AND Region like '%%%@%%' AND PostNum like '%%%@%%' AND PraiseNum like '%%%@%%' AND AttentionNum like '%%%@%%' AND FanNum like '%%%@%%';", input, input, input, input, input, input, input, input, input, input, input, input, input];
```

## 图片转换

//NSData转换为UIImage

```objc
 NSData *imageData = [NSData dataWithContentsOfFile: imagePath];
 UIImage *image = [UIImage imageWithData: imageData];
```

//UIImage转换为NSData

```objc
NSData *imageData = UIImagePNGRepresentation(image);
NSData *imageData = UIImageJPEGRepresentation(image,1.0f);//第二个参数为压缩倍数
```

## NSMutableArray初始化

```objc
NSMutableArray *array =［NSMutableArray alloc] init];//这并不是一个好方法

NSMutableArray *array = [NSMutableArray arrayWithCapacity:10];//创建一个可变的数组长度为10
//1. arrayWithCapacity是类autorelease的.
//2. [NSMutableArray alloc]initWithCapacity需要自己release.

[array addObject:User];//添加一个元素
[array insertObject:@"zero" atIndex:0];//给指定位置插入一个元素
[array removeObjectsInArray:arr];  //数组arr有的元素在mArray中删除
[array removeObject:@"three" inRange:NSMakeRange(0, mArray.count)];//按照范围删除
[array removeLastObject];  //删除最后一个元素
[array removeObject:@"six"];  //删除特定元素
[array removeObjectAtIndex:2];  //按照下标删除
[array replaceObjectAtIndex:0 withObject:@"third"];//按照下标替换元素
[array exchangeObjectAtIndex:1 withObjectAtIndex:2];//按照下标交换元素

    NSMutableArray *mArray2 = [NSMutableArray arrayWithObjects:@"one",@"two",@"three",@"four",@"five",@"six",@"seven", nil];
    
//    第一种遍历可变数组的方法－－快速枚举法
    for (id x in mArray2) {
        NSLog(@"%@",x);
    }
    
//    第二种遍历可变数组的方法－－一般循环法
    for (int i=0; i<mArray2.count; i++) {
        NSLog(@"%@",[mArray2 objectAtIndex:i]);
    }
    
//    第三种遍历可变数组的方法－－使用枚举器遍历
    NSEnumerator *enu =[mArray2 objectEnumerator];
    id x;
    while (x=[enu nextObject]) {
        NSLog(@"%@" ,x);
    }
```

## 数据库存储图片

### 将图片转化成base64编码格式的字符串，直接以字符串的形式存放入数据库

```objc
//图片转化为base64字符串
UIImage *originImage = [UIImage imageNamed:@"origin.png"];
NSData *data = UIImageJPEGRepresentation(originImage, 1.0f);
NSString *encodedImageStr = [data base64Encoding];
NSLog(@"Encoded image:%@", encodedImageStr);

//base64字符串转化为图片
NSData *decodedImageData = [[NSData alloc] initWithBase64Encoding:encodedImageStr];
UIImage *decodedImage = [UIImage imageWithData:decodedImageData];
NSLog(@"Decoded image size: %@", NSStringFromCGSize(decodedImage.size));
```

### 利用FMDB数据库的blob类型的数据存取

```objc
UIImage *UserIconImage = model.UserIcon;
NSData *UserIcon = UIImageJPEGRepresentation(UserIconImage,1.0f);
BOOL result = [self.db executeUpdate:@"INSERT INTO user (UserName, Password, UserIcon, PhoneNumber, Email, Introduction, Sex, Birthday, Region, PostNum, PraiseNum, AttentionNum, FanNum) value(?,?,?,?,?,?,?,?,?,?,?,?,?)",UserName, Password, UserIcon, PhoneNumber, Email, Introduction, Sex, Birthday, Region, PostNum, PraiseNum, AttentionNum, FanNum];

//显示图片
FMResultSet *rs = [self.db executeQuery:@"SELECT * FROM user;"];
NSData *ImageData = [rs dataForColumn:@"UserIcon"];
User.UserIcon = [UIImage imageWithData: ImageData];

```

