---
title: swiftUI(五)
date: 2021-01-18 21:01:49
tags:
categories:
- swift相关
- swiftUI
top_img: https://s3.ax1x.com/2021/01/08/sKV0Fe.jpg
cover: false
aside: false
toc: false
---

现在是晚上九点半，今天是第五天，对不起我摸鱼到现在。。。昨天那个问题我还没想，然后学长突然在群里说用@State应该可以，然后我才开始（羞愧。。然后我去查了十分钟问题就解决了。。。羞愧难当，引以为戒。问题的解决的话，就是在你实例的对象前用@ObservableObject修饰就可以了。。。今天也是要继续刚把爹的一天啊。。。

现在主要是实现多个视图共享数据。

# Sharing SwiftUI state with @ObservedObject

If you want to use a class with your SwiftUI data – which you *will* want to do if that data should be shared across more than one view – then SwiftUI gives us two property wrappers that are useful: `@ObservedObject` and `@EnvironmentObject`. We’ll be looking at environment objects later on, but for now let’s focus on observed objects.

Here’s some code that creates a `User` class, and shows that user data in a view:

```swift
class User {
    var firstName = "Bilbo"
    var lastName = "Baggins"
}

struct ContentView: View {
    @State private var user = User()

    var body: some View {
        VStack {
            Text("Your name is \(user.firstName) \(user.lastName).")

            TextField("First name", text: $user.firstName)
            TextField("Last name", text: $user.lastName)
        }
    }
}
```

However, that code won’t work as intended: we’ve marked the `user` property with `@State`, which is designed to track local structs rather than external classes. As a result, we can type into the text fields but the text view above won’t be updated.

To fix this, we need to tell SwiftUI when interesting parts of our class have changed. By “interesting parts” I mean parts that should cause SwiftUI to reload any views that are watching our class – it’s possible you might have lots of properties inside your class, but only a few should be exposed to the wider world in this way.

Our `User` class has two properties: `firstName` and `lastName`. Whenever either of those two changes, we want to notify any views that are watching our class that a change has happened so they can be reloaded. We can do this using the `@Published` property observer, like this:

```swift
class User {
    @Published var firstName = "Bilbo"
    @Published var lastName = "Baggins"
}
```

`@Published` is more or less half of `@State`: it tells Swift that whenever either of those two properties changes, it should send an announcement out to any SwiftUI views that are watching that they should reload.

How do those views know which classes might send out these notifications? That’s another property wrapper, `@ObservedObject`, which is the other half of `@State` – it tells SwiftUI to watch a class for any change announcements.

So, change the `user` property to this:

```swift
@ObservedObject var user = User()
```

I removed the `private` access control there, but whether or not you use it depends on your usage – if you’re intending to share that object with other views then marking it as `private` will just cause confusion.

Now that we’re using `@ObservedObject`, our code will no longer compile. It’s not a problem, and in fact it’s expected and easy to fix: the `@ObservedObject` property wrapper can only be used on types that conform to the `ObservableObject` protocol. This protocol has no requirements, and really all it means is “we want other things to be able to monitor this for changes.”

So, modify the `User` class to this:

```swift
class User: ObservableObject {
    @Published var firstName = "Bilbo"
    @Published var lastName = "Baggins"
}
```

Our code will now compile again, and, even better, it will now actually *work* again – you can run the app and see the text view update when either text field is changed.

As you’ve seen, rather than just using `@State` to declare local state, we now take three steps:

- Make a class that conforms to the `ObservableObject` protocol.
- Mark some properties with `@Published` so that any views using the class get updated when they change.
- Create an instance of our class using the `@ObservedObject` property wrapper.

The end result is that we can have our state stored in an external object, and, even better, we can now use that object in multiple views and have them all point to the same values.

**这部分差不多了，剩下的就是让手机音频响了。**

# How to play sounds using AVAudioPlayer

The most common way to play a sound on iOS is using `AVAudioPlayer`, and it's popular for a reason: it's easy to use, you can stop it whenever you want, and you can adjust its volume as often as you need. The only real catch is that you must store your player as a property or other variable that won't get destroyed straight away – if you don't, the sound will stop immediately.

`AVAudioPlayer` is part of the AVFoundation framework, so you'll need to import that:

```swift
import AVFoundation
```

Like I said, you need to store your audio player as a property somewhere so it is retained while the sound is playing. In our example we're going to play a bomb explosion sound, so I created a property for it like this:

```swift
var bombSoundEffect: AVAudioPlayer?
```

With those two lines of code inserted, all you need to do is play your audio file. This is done first by finding where the sound is in your project using `path(forResource:)`, then creating a file URL out of it. That can then get passed to `AVAudioPlayer` to create an audio player object, at which point – finally – you can play the sound. Here's the code:

```swift
let path = Bundle.main.path(forResource: "example.mp3", ofType:nil)!
let url = URL(fileURLWithPath: path)

do {
    bombSoundEffect = try AVAudioPlayer(contentsOf: url)
    bombSoundEffect?.play()
} catch {
    // couldn't load file :(
}
```

剩下的这个让手机音频响应该比较简单，我现在大概的思路是这样的：设置一个本地时间去和设置的时间去比较，相等时调函数让音频响，主要是怎么去让这个检测相等的东西每一秒钟都在进行比较，而且是反复地比较这点上怎么去实现我还不知道。