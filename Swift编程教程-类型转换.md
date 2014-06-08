# <center>苹果Swift语言官方文档（中文翻译版）</center>

本文档由[长沙戴维营教育](http://www.diveinedu.cn)组织翻译和校对，由于英语水平有限，请大家指正。

长沙戴维营教育还为本教程录制了配套的视频教程在[乌班图学院](http://www.ubuntucollege.cn)上免费提供，欢迎大家一起学习。

下面的章节，如果为蓝色链接表示以及翻译完毕，可以查看，如果为黑色则表示正在紧张的翻译中。本文档中文版每天都会更新，大家可以随时查看。如由Bug欢迎改正。

- - -

# 类型转换

**类型转换**是一种检查对象类型的方法，它能够将一个类型的实例在它所在的类层次结构上，看作是它的父类或子类对象进行使用。

Swift使用`is`和`as`来实现类型转换。这两个操作符提供了一个简单的方式来检查某个值的类型或将其作为其它的数据类型使用。

我们还可以用类型转换来检查某个类型是否实现了一个协议，详情请参考“检查协议的实现”。

### 为类型转换定义类层次结构
我们可以使用类型转换在一个类层次结构里检查对象的类型并将它转换为另外的类型。下面的三个代码片段定义了一个类层次结构以及一个包含这些类的三个实例的数组。下面用它们来测试类型转换。

第一个代码片段定义了一个新的基类`MediaItem`。这个类为一个数字媒体库中的条目提供基本功能。它声明了一个`String`类型的`name`属性和一个`init name`初始化方法。（这里假设所有的媒体条目，包括电影和歌曲在内都有名字）
```go
class Mediaitem {
	var name: String
    init(name: String) {
    	self.name = name
    }
}
```

第二个片段定义了`MediaItem`的两个子类。其中第一个子类`Movie`封装了电影的信息，它增加了`director`属性和对应的初始化方法。第二个类`Song`增加了`artist`属性和对应的初始化方法。
```go
class Movie: Mediaitem {
	var director: String
    init(name: String, director: String) {
    	self.director = director
        super.init(name: name)
    }
}

class Song: Mediaitem {
	var artist: String
    init(name: String, artist: String) {
    	self.artist = artist
        super.init(name: name)
    }
}
```

最后一个片段创建了一个数组常量`library`，它包含两个`Movie`实例和三个`Song`实例。`library`数组类型是由初始化的内容推导出来的。Swift的类型检查器能够推导出`Movie`和`Song`有一个共同的父类`MediaItem`。因此`library`数组的类型为`MediaItem[]`。
```go
let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// the type of "library" is inferred to be MediaItem[]
```

### 类型检查
Swift使用`is`操作符检查一个实例是否是某个类型的子类。运算结果会返回`true`或`false`。下面的例子定义两个变量`movieCount`和`songCount`，并统计`library`中`Movie`和`Song`对象的数目。
```go
var movieCount = 0
var songCount = 0
 
for item in library {
    if item is Movie {
        ++movieCount
    } else if item is Song {
        ++songCount
    }
}
 
println("Media library contains \(movieCount) movies and \(songCount) songs")
// prints "Media library contains 2 movies and 3 songs"
```

上面的例子使用`for-in`遍历`library`数组，并将元素取出来存放在`item`常量中。如果`item is Movie`返回`true`，当前的`MediaItem`对象是一个`Moive`类型的实例，反之则不是。同样的，使用`item is Song`来判断是否为`Song`类型的实例。循环结束后，`movieCount`和`songCount`里存放的就是数组中`Movie`和`Song`类型对象的数目。

### 向下类型转换
某个类型的常量或变量有可能实际引用的是它的子类的对象。在这种情况下，可以用`as`操作符将它转换为对应的子类类型。

由于向下类型转换可能会失败，类型转换符有两种不同的形式。第一种是选项形式（Optional）`as?`，它返回一个目标类型的选项值。第二种是强制形式`as`，尝试将转换和结果的解包操作在同一个操作中完成。

如果我们不能确定向下类型转换是否成功，可以使用选项形式`as?`。这种形式总是返回一个选项值，如果转换失败的话，它会包含`nil`值。这样我们就可以检查类型转换是否成功。

只有当我们能够确定类型转换会成功的时候才使用强制类型转`as`。如果转换失败的话，它会抛出一个运行是错误。

下面的例子遍历`library`中的每个值，并且打印出它们的描述信息。为了访问`director`或`artist`属性，我们需要确保它们是`Moive`或者`Song`类型的实例。

在这个例子里，我们并没有提前知道`library`里的值是什么类型，因此使用选项形式`as?`进行转换比较合适。
```go
for item in library {
    if let movie = item as? Movie {
        println("Movie: '\(movie.name)', dir. \(movie.director)")
    } else if let song = item as? Song {
        println("Song: '\(song.name)', by \(song.artist)")
    }
}
 
// Movie: 'Casablanca', dir. Michael Curtiz
// Song: 'Blue Suede Shoes', by Elvis Presley
// Movie: 'Citizen Kane', dir. Orson Welles
// Song: 'The One And Only', by Chesney Hawkes
// Song: 'Never Gonna Give You Up', by Rick Astley
```

上面的例子先尝试将它转换为`Movie`，如果转换失败，则尝试`Song`类型。`item as Movie`的结果是`Movie?`，或者叫可选的`Movie`类型。上面用选项是否包含值来判断类型转换是否成功。`if let movie = item as? Movie`读作：
"尝试将`item`按`Movie`类型进行访问，如果访问成功，将`Movie`选项中的值设置给一个`Movie`类型的常量`movie`"

如果类型转换成功，`movie`就可以用来打印`Movie`对象的描述，它包含`director`的名字。同样的可以将`Song`类型的值取出来并打印它们的描述。
> **提示**
> 类型转换不会改变对象的值。它只是将这个对象作为另外一个类型的对象使用。

### `Any`和`AnyObject`类型转换
Swift提供了两个特殊的类型来表示没有特定类型。
- `AnyObject`能表示任意任意类类型的对象。
- `Any`可以表示除函数类型以外的所有类型的值。

> **提示**
> 只有当我们确切需要`Any`和`AnyObject`的能力时才使用它们。一般情况下都应该明确一个特定的数据类型。

### `AnyObject`
在Swift中调用Cocoa的API时，通常都用`AnyObject[]`来接收数组的值。这是因为Objective-C的数组里能够存放任意的对象类型。不过一般情况下我们都能够从API提供的信息知道数组的内容。

在这种情况下，我们可以使用强制形式的类型转`as`将数组从`AnyObject`类型转换为某个特定的类型，而不需要再去解包选项。

下面定义了一个`AnyObject[]`数组，并在里面粗放你了三个`Movie`类型的实例。
```go
let someObjects: AnyObject[] = [
    Movie(name: "2001: A Space Odyssey", director: "Stanley Kubrick"),
    Movie(name: "Moon", director: "Duncan Jones"),
    Movie(name: "Alien", director: "Ridley Scott")
]
```

因为我们知道这个数组里装的都是`Movie`类型的实例，所以之间使用强制类型转换`as`：
```go
for object in someObjects {
    let movie = object as Movie
    println("Movie: '\(movie.name)', dir. \(movie.director)")
}
// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick
// Movie: 'Moon', dir. Duncan Jones
// Movie: 'Alien', dir. Ridley Scott
```

下面有更简洁的写法：
```go
for movie in someObjects as Movie[] {
    println("Movie: '\(movie.name)', dir. \(movie.director)")
}
// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick
// Movie: 'Moon', dir. Duncan Jones
// Movie: 'Alien', dir. Ridley Scott
```

### `Any`
下面的例子使用`Any`来应付具有多种数据类型，甚至包含非类类型数据的情况。例子里创建了一个`things`数组，存放`Any`类型的数据：
```go
var things = Any[]()
 
things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
```

`things`数组包含两个`Int`值、两个`Double`值、一个`String`值、一个元组`(Double, Double)`和一个`Movie`对象。

我们使用`is`和`as`操作符来判断对象的类型。下面的代码遍历`things`数组，并使用`switch`语句进行判断。下面的一些`case`语句将`things`中的值赋值给一个特定类型的常量，方便对它们进行打印：
```go
for thing in things {
    switch thing {
    case 0 as Int:
        println("zero as an Int")
    case 0 as Double:
        println("zero as a Double")
    case let someInt as Int:
        println("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        println("a positive double value of \(someDouble)")
    case is Double:
        println("some other double value that I don't want to print")
    case let someString as String:
        println("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        println("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        println("a movie called '\(movie.name)', dir. \(movie.director)")
    default:
        println("something else")
    }
}
 
// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called 'Ghostbusters', dir. Ivan Reitman
```

> **提示**
> 在`switch`语句中使用`as`而不是`as?`来进行类型检查和转换总是安全的。