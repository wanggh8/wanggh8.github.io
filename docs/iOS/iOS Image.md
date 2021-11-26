---
title: iOS 图片相关
date: 2021-05-05
updated: 2021-05-05
tags: 
  - iOS
categories: iOS
description: iOS 图片相关
cover: /images/Xcode.png
top_img: /images/Xcode.png
typora-root-url: ../../../source
---



```objc
- (UIImage *)createStretchImage:(NSString*)name withRect:(CGRect)rect {
	UIImage *img = [UIImage imageWithContentsOfFile:[self.resBundle pathForResource:name ofType:@"png"]];
	CGSize sz = img.size;
	UIEdgeInsets ei = UIEdgeInsetsMake(
		rect.origin.y,
		rect.origin.x,
		sz.height - rect.origin.y - rect.size.height,
		sz.width - rect.origin.x - rect.size.width);
	
	img = [img resizableImageWithCapInsets:ei resizingMode:UIImageResizingModeStretch];
	return img;
}

- (UIImage *)createImage:(NSString*)name {
	return [UIImage imageWithContentsOfFile:[_resBundle pathForResource:name ofType:@"png"]];
}

- (UIImage *)createImage:(NSString *)name scale:(CGFloat)scale
{
	UIImage *image = [self createImage:name];
	CGSize size = CGSizeMake(image.size.width*scale, image.size.height*scale);
	UIGraphicsBeginImageContextWithOptions(size, NO, 0.0);
	[image drawInRect:CGRectMake(0, 0, size.width, size.height)];
	UIImage* scaledImage = UIGraphicsGetImageFromCurrentImageContext();
	UIGraphicsEndImageContext();
	return scaledImage;
}
```

