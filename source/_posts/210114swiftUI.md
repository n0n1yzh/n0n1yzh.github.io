---
title: swiftUI(一)
date: 2021-01-14 19:30:02
tags:
---

之前因为考试的原因导致我十二月到现在都没学，现在之前的东西也都忘的差不多了，但是，这里立个flag，我要花六天把它学明白！YZH，刚把爹！

~~（DAY29）~~

# List

你能像这样使用 `list` ：

```swift
List {
    Text("Hello World")
    Text("Hello World")
    Text("Hello World")
}
```

它将呈现一个视图。

我们也可以切换到ForEach，以便从数组或范围动态创建行：

```swift
List {
    ForEach(0..<5) {
        Text("Dynamic row \($0)")
    }
}
```

mix static and dynamic rows is also admited:

```swift
List {
    Text("Static row 1")
    Text("Static row 2")

    ForEach(0..<5) {
        Text("Dynamic row \($0)")
    }

    Text("Static row 3")
    Text("Static row 4")
}
```

And of course we can combine that with sections, to make our list easier to read:

```swift
List {
    Section(header: Text("Section 1")) {
        Text("Static row 1")
        Text("Static row 2")
    }

    Section(header: Text("Section 2")) {
        ForEach(0..<5) {
            Text("Dynamic row \($0)")
        }
    }

    Section(header: Text("Section 3")) {
        Text("Static row 3")
        Text("Static row 4")
    }
}

```

You’ll notice that this list looks very different from the form we had previously, but really all you’re seeing is a different table view style on iOS. We can get a similar look and feel using the `listStyle()` modifier, like this:

```swift
.listStyle(GroupedListStyle())
```

before:

<img src="/Users/yzh/Library/Application Support/typora-user-images/image-20210114194649069.png" alt="image-20210114194649069" style="zoom:25%;" />

Like this:

<img src="/Users/yzh/Library/Application Support/typora-user-images/image-20210114194606801.png" alt="image-20210114194606801" style="zoom:25%;" />

现在，您所看到的所有内容都可以很好地处理表单和列表，甚至是动态内容。但是`List`可以完成 `Form` 所不能做的一件事是完全从动态内容生成行，而不需要 `ForEach` 。

因此，如果您的整个列表是由动态行组成的，您可以简单地这样写:

```swift
List(0..<5) {
    Text("Dynamic row \($0)")
}
```

当使用类似 `List(0..<5)` 时，单个list中是以行为单位的，如果不是则里面的单独个元素成一个行。

like this：

```swift
 List(0..<5) {
            Text("Hello World \($0)")
            Text("Hello World")
            Text("Hello World")
        }
```



<img src="/Users/yzh/Library/Application Support/typora-user-images/image-20210114195623005.png" alt="image-20210114195623005" style="zoom:25%;" />

这里，单个list中是以行为单位的。

When working with this kind of list data, we use `id: \.self` like this:

```swift
struct ContentView: View {
    let people = ["Finn", "Leia", "Luke", "Rey"]

    var body: some View {
        List(people, id: \.self) {
            Text($0)
        }
    }
}
```

运行后这样的：

<img src="/Users/yzh/Library/Application Support/typora-user-images/image-20210114200217804.png" alt="image-20210114200217804" style="zoom:25%;" />

这里我这样理解的：list 做List的参数，那个List在那两个参数的加持下做类似一个循环遍历的操作，`$0` 用来标识当前遍历的元素（大概？）。 

That works just the same with `ForEach`, so if we wanted to mix static and dynamic rows we could have written this instead:

这与ForEach的工作原理相同，因此，如果我们想混合静态行和动态行，我们可以这样写：

```swift
List {
    ForEach(people, id: \.self) {
        Text($0)
    }
}
```

并且 List只要在开头有类似遍历这种行为，里面的元素的布局全部为单行。

# Loading resources from your app bundle

~~我没看~~

# Working with strings

iOS为我们提供了一些非常强大的处理字符串的api，包括将字符串分割成数组、删除空格、甚至检查拼写的能力。

Swift gives us a method called `components(separatedBy:)` that can converts a single string into an array of strings by breaking it up wherever another string is found. For example, this will create the array `["a", "b", "c"]`:

```swift
let input = "a b c"
let letters = input.components(separatedBy: " ")
```

引用： `letters[0]` 类似Python的 `split(" ")`

the `randomElement()` method returns one random item from the array.

 For example, this will read a random letter from our array:

例如，它将从数组中读取一个随机的字母：

```swift
let letter = letters.randomElement()
```

Now, although we can see that the letters array will contain three items, Swift doesn’t know that – perhaps we tried to split up an empty string, for example. As a result, the `randomElement()` method returns an optional string, which we must either unwrap or use with nil coalescing.

现在，虽然我们可以看到letters数组将包含三个元素，但是Swift不知道也许我们试图拆分一个空字符串。因此，`randomElement()` 方法**返回**一个可选的字符串，我们必须将其展开或与`nil`合并使用。

Another useful string method is `trimmingCharacters(in:)`, which asks Swift to remove certain kinds of characters from the start and end of a string. This uses a new type called `CharacterSet`, but most of the time we want one particular behavior: removing whitespace and new lines – this refers to spaces, tabs, and line breaks, all at once.

另一个有用的字符串方法是`trimmingCharacters(in:)`，它要求Swift从字符串的开头和结尾删除某些类型的字符。这使用了一种称为字符集的新类型，但大多数时候我们需要一个特定的行为:删除空白和新行。 

感觉和Python挺像。

This behavior is so common it’s built right into the `CharacterSet` struct, so we can ask Swift to trim all whitespace at the start and end of a string like this:

这种行为非常常见，它直接构建到字符集结构中，所以我们可以要求Swift像这样修剪字符串开头和结尾的所有空格：

```swift
let trimmed = letter?.trimmingCharacters(in: .whitespacesAndNewlines)
```

`letter?`将检测letter是否是空串，如果不是将调用字符串方法`trimmingCharacters(in: .whitespacesAndNewlines)`去除letter字符串开头和结尾的空格。（大概？）

There’s one last piece of string functionality I’d like to cover before we dive into the main project, and that is the ability to check for misspelled words.

在我们深入到主项目之前，我想介绍最后一个字符串功能，那就是检查拼写错误的单词的能力。

This functionality is provided through the class `UITextChecker`. You might not realize this, but the “UI” part of that name carries two additional meanings with it:

这个功能是通过类UITextChecker提供的。你可能没有意识到这一点，但该名称的UI部分带有两个额外含义：

1. This class comes from UIKit. That doesn’t mean we’re loading all the old user interface framework, though; we actually get it automatically through SwiftUI.

   这个类来自UIKit。不过，这并不意味着我们要加载所有旧的用户界面框架;我们实际上是通过SwiftUI自动获取的。

2. It’s written using Apple’s older language, Objective-C. We don’t need to write Objective-C to use it, but there is a slightly unwieldy API for Swift users.

   它是用苹果的旧语言Objective-C编写的。我们不需要编写Objective-C来使用它，但是对于Swift用户来说，有一个稍微笨拙的API。

Checking a string for misspelled words takes four steps in total. First, we create a word to check and an instance of `UITextChecker` that we can use to check that string:

检查字符串中拼写错误的单词总共需要四个步骤。首先，我们创建一个要检查的单词和一个`UITextChecker`的实例，我们可以使用它来检查该字符串：

```swift
let word = "swift"
let checker = UITextChecker()
```

Second, we need to tell the checker how much of our string we want to check. If you imagine a spellchecker in a word processing app, you might want to check only the text the user selected rather than the entire document.

第二，我们需要告诉检查器我们想要检查多少字符串。想象一下文字处理应用程序中的拼写检查程序，你可能只想检查用户选择的文本，而不是整个文档。

However, there’s a catch: Swift uses a very clever, very advanced way of working with strings, which allows it to use complex characters such as emoji in exactly the same way that it uses the English alphabet. However, Objective-C does *not* use this method of storing letters, which means we need to ask Swift to create an Objective-C string range using the entire length of all our characters, like this:

然而，有一个问题:Swift使用了一种非常聪明、非常先进的方式来处理字符串，这使得它可以像使用英语字母表一样使用复杂的字符，比如表情符号。然而，Objective-C不使用这种存储字母的方法，这意味着我们需要要求Swift使用所有字符的全部长度来创建一个Objective-C字符串范围，就像这样:

```swift
let range = NSRange(location: 0, length: word.utf16.count)
```

UTF-16 is what’s called a *character encoding* – a way of storing letters in a string. We use it here so that Objective-C can understand how Swift’s strings are stored; it’s a nice bridging format for us to connect the two.

UTF-16是所谓的字符编码，一种在字符串中存储字母的方法。我们在这里使用它是为了让Objective-C能够理解Swift字符串是如何存储的;这是一种很好的衔接形式，我们可以把两者联系起来。

Third, we can ask our text checker to report where it found any misspellings in our word, passing in the range to check, a position to start within the range (so we can do things like “Find Next”), whether it should wrap around once it reaches the end, and what language to use for the dictionary:

```swift
let misspelledRange = checker.rangeOfMisspelledWord(in: word, range: range, startingAt: 0, wrap: false, language: "en")
```

~~这里看不怎么懂，不看了，时间宝贵（🐶）~~

有一说一，感觉我这种恰快餐的还是别一个个看的好，毕竟只有六天时间。。。

接下来是直接到 **Views and view controllers** 部分

# 视图和视图控制器

You’ve seen how SwiftUI lets us store changing data in our structs by using the `@State` property wrapper, how we can bind that state to the value of a UI control using `$`, and how changes to that state automatically cause SwiftUI to reinvoke the `body` property of our struct.

使用@State属性包装SwiftUI让我们存储变化的数据,使用美元符号绑定状态。

所有这些结合起来让我们可以编写这样的代码：

```swift
struct ContentView: View {
    @State private var blurAmount: CGFloat = 0

    var body: some View {
        VStack {
            Text("Hello, World!")
                .blur(radius: blurAmount)

            Slider(value: $blurAmount, in: 0...20)
        }
    }
}
```

<img src="/Users/yzh/Library/Application Support/typora-user-images/image-20210114204836769.png" alt="image-20210114204836769" style="zoom:25%;" />

Now, let’s say we want that binding to do *more* than just handle the radius of the blur effect. Perhaps we want to save that to `UserDefaults`, run a method, or just print out the value for debugging purposes. You might try updating the property like this:

现在，假设我们想要绑定做的不仅仅是处理模糊效果的半径。也许我们想把它保存到UserDefaults，运行一个方法，或者为了调试目的打印出值。您可以尝试这样更新属性:

```swift
@State private var blurAmount: CGFloat = 0 {
    didSet {
        print("New value is \(blurAmount)")
    }
}
```

这里说了很多，总之就是 `@State ` 包装的blurAmount相当于一个结构体（是吗？），didSet在这里实际上是在监控这个结构体发生了改变没（大概？），但是这是永远也不会变的，所以并不会打印出这串字符（是吗？）。

在Swift语言中用了willSet和didSet这两个特性来监视属性的除初始化之外的属性值变化

```swift
 //带属性监视器的普通属性
    var age:Int = 0
    {
        //我们需要在age属性变化前做点什么
        willSet
        {
            println("Will set an new value \(newValue) to age")
        }
        //我们需要在age属性发生变化后，更新一下nickName这个属性
        didSet
        {
            println("age filed changed form \(oldValue) to \(age)")
            if age<10
            {
                nickName = "Little"
            }else
            {
                nickName = "Big"
            }
        }
    }
```

下面要如何处理这种情况呢？

Creating custom bindings in SwiftUI 创建自定义绑定



To fix this we need to create a custom binding we need to use the Binding struct directly, which allows us to provide our own code to run when the value is read or written.

要解决这个问题，我们需要创建一个自定义绑定，我们需要直接使用绑定结构，这允许我们提供自己的代码，以便在读取或写入值时运行。

原先代码：

```swift
struct ContentView: View {
    @State private var blurAmount: CGFloat = 0 {
        didSet {
            print("New value is \(blurAmount)")
        }
    }

    var body: some View {
        VStack {
            Text("Hello, World!")
                .blur(radius: blurAmount)

            Slider(value: $blurAmount, in: 0...20)
        }
    }
}
```

自定义绑定的代码：

```swift
struct ContentView: View {
    @State private var blurAmount: CGFloat = 0

    var body: some View {
        let blur = Binding<CGFloat>(
            get: {
                self.blurAmount
            },
            set: {
                self.blurAmount = $0
                print("New value is \(self.blurAmount)")
            }
        )

        return VStack {
            Text("Hello, World!")
                .blur(radius: blurAmount)

            Slider(value: blur, in: 0...20)
        }
    }
}
```

这里`Slider(value: blur, in: 0...20)` blur不使用美元符号标示双向绑定是因为现在我们已经直接创建了绑定，不再需要它了。

like this：

<img src="/Users/yzh/Library/Application Support/typora-user-images/image-20210114214143396.png" alt="image-20210114214143396" style="zoom:25%;" />

双向绑定由这串代码实现：

```swift
 let blur = Binding<CGFloat>(
            get: {
                self.blurAmount
            },
            set: {
                self.blurAmount = $0
                // print("New value is \(self.blurAmount)")
            }
        )
```

感觉它这个双向绑定需要两个变量来实现（大概？）

## Showing multiple options with ActionSheet

<img src="/Users/yzh/Library/Application Support/typora-user-images/image-20210114221027833.png" alt="image-20210114221027833" style="zoom: 50%;" />

## Wrapping a UIViewController in a SwiftUI view

在我们编写代码之前，有三件事你需要知道，它们都有点像UIKit 101，但如果你只使用过SwiftUI，它们对你来说是陌生的。

首先，UIKit有一个类叫UIView，它是布局中所有视图的父类。标签，按钮，文本框，滑动条，等等这些都是视图。

第二，UIKit有一个叫做UIViewController的类，它被设计来保存所有的代码来使视图栩栩如生。就像UIView, UIViewController有很多子类做不同种类的工作。

第三，UIKit使用一种称为委托的设计模式来决定工作发生的位置。因此，当要决定我们的代码应该如何响应文本字段的值变化时，我们要创建一个具有我们的功能的自定义类，并将其作为文本字段的委托。

---

SwiftUI使我们的代码结构大为不同，尤其是我们在视图中使用结构而不是类。然而，SwiftUI将UIView和UIViewController混合到一个单一的视图协议中，这使得我们的代码更加简单。

个人感觉，这些东西真的只能慢慢来啊，你要整一个闹钟app还是要去学专门针对的东西，不然这样学真的慢。。

有些东西理解起来真的挺不容易的，我觉得确实要慢慢来，是不是未免太急了点？

接下来我决定针对之前的Github上克隆的项目来学了，就学他的代码，争取学明白，刚把爹！