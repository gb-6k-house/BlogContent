---
title: iOS 11 入坑
date: 2017-10-11 19:52:48
tags: [技术之路, iOS]
---
### iOS 11 关于自定义导航栏的问题

如果在UIViewController中通过 UINavigationBar()创建导航栏，我们发现导航栏的位置有问题，导航栏的title和 返回按钮到了状态栏的位置，如下图

<figure >
    <img src="/uploads/iOS_11_入坑/image1.png" width="600">
    <img src="/uploads/iOS_11_入坑/image2.png" width="600">
    <img src="/uploads/iOS_11_入坑/image3.png" width="600">
</figure>

上图我们可以看出实际上UINavigationBar本身的位置没问题， 只是其中的_UIBarBackgroud和_UINavigationBarContentView的坐标有问题。
_UIBarBackgroud实际上是UINavigationBar的背景View，size应该和UINavigationBar的Size一样
_UINavigationBarContentView实际上是导航栏的内容的父视图, 它的y轴应该位移状态栏的高度

通过分析我们可以继承UINavigationBar，然后修改其子类的view的坐标

```swift
class  MyUINavigationBar: UINavigationBar {
        override func layoutSubviews() {
            super.layoutSubviews()
            //
            if #available(iOS 11, *) {
                for view in self.subviews {
                    if view.className().contains("Background"){
                        view.frame = self.bounds
                    }else if view.className().contains("ContentView") {
                        var frame = view.frame
                        frame.origin.y =  UIApplication.shared.statusBarFrame.height
                        frame.size.height = self.bounds.size.height - frame.origin.y
                        view.frame = frame
                    }
                }
            }
        }

    }
```
