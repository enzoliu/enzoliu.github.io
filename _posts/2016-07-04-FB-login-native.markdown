---
layout: post
title: 善用inputView
date: 2016-07-04 22:49:24.000000000 +08:00
---
Facebook 因為使用體驗的因素，將呼叫 Facebook native app來login的方式拔掉了。[來源...][ref-fb-remove-native-login]

1. 使用pod 安裝facebook `FBSDKCoreKit`, `FBSDKLoginKit`
2. 打開`FBSDKCoreKit/FBSDKServerConfiguration.m`
3. 找到下列函式，改為傳回`YES`

```objc
- (BOOL)useNativeDialogForDialogName:(NSString *)dialogName
{
    return YES;
//  return [self _useFeatureWithKey:FBSDKDialogConfigurationFeatureUseNativeFlow dialogName:dialogName];
}
```

[ref-fb-remove-native-login]: http://stackoverflow.com/a/32621036
