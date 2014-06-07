# <center>苹果Swift语言官方文档（中文翻译版）</center>

本文档由[长沙戴维营教育](http://www.diveinedu.cn)组织翻译和校对，由于英语水平有限，请大家指正。

长沙戴维营教育还为本教程录制了配套的视频教程在[乌班图学院](http://www.ubuntucollege.cn)上免费提供，欢迎大家一起学习。

下面的章节，如果为蓝色链接表示以及翻译完毕，可以查看，如果为黑色则表示正在紧张的翻译中。本文档中文版每天都会更新，大家可以随时查看。如由Bug欢迎改正。

- - -

# 枚举类型

**枚举类型**为一组相关的值定义了一个共同的类型，使得我们可以在代码中使用的时候能够保证类型安全。

如果你熟悉C语言就会知道，C语言的枚举类型是一组有名字的整数值。Swift中的枚举类型更加灵活，它不需要给每个枚举成员赋值，并且枚举成员的值（“raw” value）可以是字符串、字符、整数或者浮点数。

除此以外，每个枚举成员还可以关联一个任意类型的值。枚举类型引入了许多原先只被类所支持的特性，比如使用计算属性为枚举变量的当前值提供更多信息。枚举类型还可以定义初始化方法为成员提供初始值；可以对它进行扩展、可以响应协议等。

更多信息请查看属性、方法、初始化、扩展和协议等章节。

### 语法
使用`enum`关键字定义枚举类型，并将所有的成员用花括号`{}`括起来：
```go
enum SomeEnumeration {
	//enumeration definition goes here
}
```

下面的例子表示指南针的四个方向：
```go
enum CompassPoint {
	case North
    case Sourth
    case East
    case West
}
```

每个枚举值（`North`、`South`、`East`、`West`）就是这个枚举类型的成员值。`case`关键字表示即将定义一行新的枚举成员。
> **提示**
> 与C和Objective-C不同，Swift中的枚举成员不是一个默认的整数值。`CompassPoints`中的`North`、`South`、`East`和`West`的值并不是0、1、2、3。每个枚举成员都是完全独立的自定义类型`CompassPoint`的值。

多个枚举成员值可以出现在同一行中，它们之间用逗号分隔：
```go
enum Planet {
	case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```

每个枚举定义了一个新的类型，与Swift中其它类型一样，枚举类型的首字母应该大写，如`CompassPoint`和`Planet`。用名词的单数形式给枚举类型命名，这样它们看起来就是自说明的：
```go
var directionToHead = CompassPoint.West
```

`directionToHead`的类型在使用`CompassPoint`的值初始化的时候可以被推导出来。一旦`directionToHead`被声明为`CompassPoint`类型，我们可以给它用更短的点语法设置不同的`CompassPoint`值。
```go
directionToHead = .East
```

由于`directionToHead`的类型是已知的，因此赋值的时候可以省略类型符号。这样可以使加强显式定义的枚举值的可执行。

#### 枚举类型与switch语句
我们可以用switch语句来匹配各枚举值：
```go
directionToHead = .South
switch directionToHead {
	case .North:
    	println("Lots of planets have a north")
    case .South:
    	println("Watch out for penguins")
    case .East:
    	println("Where the sun rises")
    case .West:
    	println("Where the skies are blue")
}
//prints "Watch out for penguins"
```

上面代码的含义是：
“比较`directionToHead`的值，当它为`.North`的时候打印Lots of planets have a north。当它为`.South`时，打印Watch out for penguins。”等等。

在流程控制里已经讲过，`switch`语句必须覆盖到所有的枚举成员，如果少写了`.West`的`case`就会出现编译错误。如果确实不能提供所有的枚举值，我们可以使用`default`来覆盖其它所有的值。
```go
let somePlanet = Planet.Earth
switch somePlanet {
	case .Earth:
    	println(""mostly harmless)
    default:
    	println("Not a safe place for humans")
}
//prints "Mostly harmless"
```

#### 关联值
上一节介绍了枚举类型的定义。我们可以将一个常量或变量设置为`Planet.Earth`，然后在后面使用它。但是有些情况，使用为这些成员值设置的其它类型的关联值更加方便。它使得我们可以为枚举成员存储而外的信息，并且可以在需要的时候取出来使用。

我们可以为Swift的枚举类型存储任意类型的关联值，并且同一个枚举类型中各成员的关联值类型可以不同。Swift的枚举类型有点类似与其它语言中的可识别联合、标记联合或者变体。

例如一个货物跟踪系统需要跟踪两个不同类型的条形码。其中一些货物用UPC-A格式的条形码标记。这种条形码格式使用0-9进行编码，每个条形码都由一位表示“数字系统”的数字、十位表示“标识符”的数字和一位表示“校验”的数字组成。
![](images/barcode_UPC_2x.png)

另外的一些产品用QR格式的二维码标记，它可以使用ISO 8859-1中的字符，并且最多可以编码2953个字符。
![](images/barcode_QR_2x.png)

我们用一个包含三个整数的元组表示UPC-A编码，而QR码则用一个字符串表示。Swift中可以用一个枚举类型来定义这两种类型的产品编码：
```go
enum Barcode {
	case UPCA(Int, Int, Int)
    case QRCode(String)
}
```
这段代码的含义是：
“定义一个名为`Barcode`的枚举类型，它有一个附加有(Int, Int, Int)元组的成员值UPCA和一个附加有String值的成员值QRCode。”

上面的定义并没有提供实际的`Int`或者`String`类型的值，而只是声明了关联值的类型。用下面的方式创建枚举变量：
```go
var productBarcode = Barcode.UPCA(8, 85909_51226, 3)
```

上面的例子创建了一个名为`productBarcode`的变量，将其赋值为`Barcode.UPCA`，并且附加的元组为(8, 8590951226, 3)。其中表示“标识符”的整型字面常量中用下划线分隔使得它便于阅读。这个变量还能赋值为另外的条码类型：
```go
productBarcode = .QRCode("ABCDEFGHIJKLMNOP")
```

`productBarcode`原先的值`Barcode.UPCA`被`Barcode.QRCode`取代。`Barcode`类型的常量或变量可以存放一个`.UPCA`或者`.QRCode`以及它们的附加值。

不同的条码类型可以用`switch`语句进行判断。它们的附加值可以作为`switch`语句的一部分。我们可以在`switch`语句中将附加值取出来存放到常量或变量中。
```go
switch productBarcode {
	case .UPCA(let numberSystem, let identifier, let check):
    	println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
    case .QRCode(let product Code):
    	println("QR code with value of \(productCode).")
}
//prints "QR code with value of ABCDEFGHIJKLMNOP"
```

如果所有的附加值都提取到常量或全部提取到变量中，可以之间在枚举成员名字之前用单个`let`或`var`进行声明：
```go
switch productBarcode {
	case let .UPCA(numberSystem, identifier, check):
    	println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
    case let .QRCode(productCode):
    	println("QR code with value of \(productCode).")
}
//prints "QR code with value of ABCDEFGHIJKLMN"
```

#### 原始值
条码的例子演示了如何给枚举类型存储不同类型附加值的方法。除了附加值外，枚举成员还能够拥有初值（原始值），所有原始值必须是同一类型的值。下面的例子在枚举成员中存储了ASCII值：
```go
enum ASCIIControlCharacter: Character {
	case Tab = "\t"
    case LineFeed = "\n"
    case CarriageReturn = "\r"
}
```
`ASCIIControlCharacter`的原始值被定义为`Character`类型并设置成了常见的控制字符。关于`Character`的详细介绍可以参考字符串和字符的章节。

注意，枚举成员的原始值与它的附加值不同。原始值是在第一次定义枚举类型的时候预先填充好的。某个特定的枚举成员的原始值总是相同的，而附加值则是在创建新的常量或者变量的时候设置的，可以每次都设置不同的值。（原始值有点类似与C语言中的枚举值，只不过可以是多种数据类型的值）

枚举成员的原始值可以是字符串、字符、整数或者浮点数。同一枚举类型中枚举成员的原始值是唯一的。如果使用整数作为原始值的话，它的用法与C语言是一致的。如果没有指定其它值的话，它们是依次递增的。
```go
enum Planet: Int {
	case Mercury = 1, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```
`Planet.Venus`的值为2，然后依此类推。下面是访问枚举成员原始值的方法：
```go
let earthsOrder = Planet.Earth.toRaw()
//earthsOrder is 3
```

当然也可以使用原始值来查找对应的枚举成员，下面用原始值7来查找`Uranus`：
```go
let possiblePlanet = Planet.fromRaw(7)
//possiblePlanet is of type Planet? and equals Planet.Uranus
```

因为并不是所有的整数都对应一个枚举成员，所以`fromRaw`方法返回的是一个**optional**类型的枚举成员。在上面的例子里，`possiblePlanet`是一个`Planet?`类型或者叫“optional `Planet`”类型的值。

如果我们尝试查找位置9对应的`Planet`，`fromRaw`返回的`Planet?`的值为`nil`：
```go
let positionToFind = 9
if let somePlanet = Planet.fromRaw(positionToFind) {
	switch somePlanet {
    	case .Earth:
        	println("Mostly harmless")
        default:
        	println("Not a safe place for humans")
    }
} else {
	println("There isn't a planet at position \(positionToFind)")
}
//prints "There isn't a planet at position 9"
```

这个例子使用选项绑定来尝试访问原始值为9的行星。`if let somePlanet = Planet.fromRaw(9)`获取一个`Planet`类型的选项值，并将`somePlanet`设置为该选项的内容。由于取不到9对应的行星，因此`else`分支将会被执行。