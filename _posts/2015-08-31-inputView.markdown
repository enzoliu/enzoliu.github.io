---
layout: post
title: 善用inputView
date: 2015-08-31 22:49:24.000000000 +08:00
---
開發上遇到一個需求：『在文字輸入框focus時，跳出一個日期挑選器，但只要年跟月』。
這個需求有兩個問題點，其一、Picker適用性，其二、如何觸發Picker

#### 其一、Picker適用性
原本以為用`UIDatePicker`，設個參數就好了。
但`UIDatePicker`並不支援只有年月的用法。

那就用`UIPickerView`吧，那倒也不怎麼稀奇。
但當`UIPickerView`的component不只一個時，
就不再適合用didSelect之類的觸發條件去讓picker消失，
取而代之的是(或說是較為合理的方案)『在picker上方加入tool bar及button』。

假如用了`UIToolbar` + `UIBarButtonItem`，有可能會碰到

> touch event不但沒有被觸發，甚至告訴你找不到指定的selector的狀況。

事實上，這兩樣東西都是沒問題的，如果你遇到這種狀況，那便是使用上的不正確。
下方代碼應該不陌生，貌似沒什麼問題。(以下代碼為舊版swift，僅供參考)

```swift
var toolBar = UIToolbar()
toolBar.barStyle = UIBarStyle.Default
toolBar.translucent = true
toolBar.tintColor = UIColor.blackColor()
toolBar.sizeToFit()
var doneButton = UIBarButtonItem(title:"done", style:.Plain, target:self, action:"done:")
toolBar.setItems([doneButton], animated: false)
toolBar.userInteractionEnabled = true
```
好，到這邊為止，還無法勾勒出問題全貌，所以要接著講第二個問題。

#### 其二、如何觸發Picker

也許新手會有類似經驗--利用button + animate實現『當textfield 被按下時，彈出picker。』
，也因此，用button去讓picker縮下應該是很正常的使用情境。

於是你就發現上一段敘述說的那種狀況了。

很顯然的，這麼笨的做法是不太恰當的。
但人往往迷失在需求當中找不到適合的關鍵字搜尋。

其實這個需求有更便捷的作法。
上述代碼不需更改，只需在`textField`設定中，指定`inputView`與`inputAccessoryView`

```swift
textField.inputView = picker
textField.inputAccessoryView = toolBar
```

但若將picker放置於某個視圖階層結構裡，可能會有類似下面這種錯誤
`... should have parent view controller ...`

這時候，其實只要把要放到`inputView`跟`inputAccessoryView`的物件，
從父項移除，便不會出現這種問題了
也就是說，在指定給`textField`之前，帶入：

```swift
picker.removeFromSuperview()
```
並記得在button完成或取消的event中，呼叫`textField`的`resignFirstResponder`，
便可將picker縮下。
這種用法其實就是將keyboard換成你自訂的View而已。:)