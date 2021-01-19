---
title: swiftUI(三)
date: 2021-01-16 23:30:57
tags:
categories:
- swift相关
- swiftUI
top_img: https://s3.ax1x.com/2021/01/08/sKV0Fe.jpg
cover: false
aside: false
toc: false
---

今天是第三天，对不起🧎‍♀️，我摸鱼了。。。现在是16号的晚上十一点半，我才开始。。。希望没事🙏。。。

昨天（按理说是今天）已经梳理了一遍，剩下的就是去实践了，还有DatePicker在数据存储上我还不怎么会用。。那么，开始吧，刚把爹～！

找了半天的教程，还是数hackingwithswift最靠谱。。。

# DatePicker

SwiftUI’s `DatePicker` view is analogous to `UIDatePicker`, and comes with a variety options for controlling how it looks and works. Like all controls that store values, it does need to be bound to some sort of state in your app.

For example, this creates a date picker bound to a `birthDate` property, allowing users to choose any date up before now, then displays the value of the date picker as it’s set:

```swift
struct ContentView: View {
    var dateFormatter: DateFormatter {
        let formatter = DateFormatter()
        formatter.dateStyle = .long
        return formatter
    }

    @State private var birthDate = Date()

    var body: some View {
        VStack {
            DatePicker(selection: $birthDate, in: ...Date(), displayedComponents: .date) {
                Text("Select a date")
            }

            Text("Date is \(birthDate, formatter: dateFormatter)")
        }
    }
}
```

like this:

<center><img src="https://s3.ax1x.com/2021/01/16/srwIje.png" style="zoom: 50%;" /></center> 

As you’ve seen, Swift gives us `Date` for working with dates, and that encapsulates the year, month, date, hour, minute, second, timezone, and more. However, we don’t want to think about most of that – we want to say “give me an 8am wake up time, regardless of what day it is today.”

Swift has a slightly different type for that purpose, called `DateComponents`, which lets us read or write specific parts of a date rather than the whole thing.

Swift有一个稍微不同的类型，称为DateComponents，它允许我们读写日期的特定部分，而不是整个日期。

So, if we wanted a date that represented 8am today, we could write code like this:

所以，如果我们想要一个表示今天早上8点的日期，我们可以写这样的代码:

```swift
var components = DateComponents()
components.hour = 8
components.minute = 0
let date = Calendar.current.date(from: components)
```

接下来是如何读取我们想要的时间片段。

Again, `DateComponents` comes to the rescue: we can ask iOS to provide specific components from a date, then read those back out. One hiccup is that there’s a disconnect between the values we *request* and the values we *get* thanks to the way `DateComponents` works: we can ask for the hour and minute, but we’ll be handed back a `DateComponents` instance with optional values for all its properties. Yes, we know hour and minute will be there because those are the ones we asked for, but we still need to unwrap the optionals or provide default values.

So, we might write code like this:

```swift
let components = Calendar.current.dateComponents([.hour, .minute], from: someDate)
let hour = components.hour ?? 0
let minute = components.minute ?? 0
```

最后一个挑战是如何格式化日期和时间，Swift再次为我们提供了一个特定的类型来完成大部分工作。这一次它被称为DateFormatter，它允许我们以各种方式将日期转换为字符串。

例如，如果我们只想要日期中的时间，我们可以这样写:

```swift
let formatter = DateFormatter()
formatter.timeStyle = .short
let dateString = formatter.string(from: Date())
```

看到这里的时候大概明白了DatePicker部分怎么写了，于是又跑去看List，有头绪了然后开始写，然后就遇到了下面这个问题：

<center><img src="https://s3.ax1x.com/2021/01/17/sr6aqS.png" style="zoom: 50%;" /></center> 

<center><img src="https://s3.ax1x.com/2021/01/17/sr60aQ.png" style="zoom: 50%;" /></center> 

`alarmLabelNum` 在func addAlarm中找不到，于是我跑去群里问学长怎么实现这个变量的全局，学长说用单例，但是我一开始不太明白，去网上查了下好像是通过class实现的，看了一些代码有了头绪，应该是在一个class里存需要用到的所有变量，再创建这个class的实例，通过实例去调用各个变量？我自己是这样理解的。今天就先这样，等我明天来实践。