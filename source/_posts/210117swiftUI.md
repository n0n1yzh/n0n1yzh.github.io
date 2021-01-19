---
title: swiftUI(四)
date: 2021-01-17 20:34:18
tags:
categories:
- swift相关
- swiftUI
top_img: https://s3.ax1x.com/2021/01/08/sKV0Fe.jpg
cover: false
aside: false
toc: false
---

今天是第四天，现在是晚上八点半，毫无干劲，只想摸鱼。。。今天晚上要断电。。。要不然我可能还会晚点开始。。。。

# 属性包装器@Published

`@Published` is one of the most useful property wrappers in SwiftUI, allowing us to create observable objects that automatically announce when changes occur. SwiftUI will automatically monitor for such changes, and re-invoke the `body` property of any views that rely on the data. In practical terms, that means whenever an object with a property marked `@Published` is changed, all views using that object will be reloaded to reflect those changes.

For example, if we have an observable object such as this one:

```swift
class Bag: ObservableObject {
    var items = [String]()
}
```

That conforms to the `ObservableObject` protocol, which means SwiftUI’s views can watch it for changes. But because its only property isn’t marked with `@Published`, no change announcements will ever be sent – you can add items to the array freely and no views will update.

If you wanted change announcements to be sent whenever something was added or removed from `items`, you would mark it with `@Published`, like this:

```swift
class Bag: ObservableObject {
    @Published var items = [String]()
}
```

You don’t need to do anything else – the `@Published` property wrapper effectively adds a `willSet` property observer to `items`, so that any changes are automatically sent out to observers.

As you can see, `@Published` is *opt-in* – you need to list which properties should cause announcements, because the default is that changes don’t cause reloads. This means you can have properties that store caches, properties for internal use, and more, and they won’t force SwiftUI to reload views when they change unless you specifically mark them with `@Published`.

写到一半又出问题了，点button怎么也不出现我想要的alarmView，原因是ContenView没有监听到alarmLabelArray的变化。。。

现在还没有头绪，还有三分钟寝室断电。。。明天再说吧。。。

