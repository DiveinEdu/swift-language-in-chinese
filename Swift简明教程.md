# <center>苹果Swift语言官方文档（中文翻译版）</center>

本文档由[长沙戴维营教育](http://www.diveinedu.cn)组织翻译和校对，由于英语水平有限，请大家指正。

长沙戴维营教育还为本教程录制了配套的视频教程在[乌班图学院](http://www.ubuntucollege.cn)上免费提供，欢迎大家一起学习。

下面的章节，如果为蓝色链接表示以及翻译完毕，可以查看，如果为黑色则表示正在紧张的翻译中。本文档中文版每天都会更新，大家可以随时查看。如由Bug欢迎改正。

- - -

# Swift简明教程
学习一门新语言的时候，大家都习惯于打印“Hello，world”开始。在Swift中只需要一行代码：
```lua
println("Hello, world")
```

如果你曾经写过C或者Objective-C代码，应该不会对Swift的语法陌生。Swift中上面这一行就是一个完整的程序。你不需要再为输入/输出或者字符串处理功能导入独立的库。程序以全局代码作为入口，因此不再需要`main`函数了。同样，代码结尾的分号也不会再出现。

再这个简明教程中，你会学习到足够的知识来编写Swift程序。如果看完这个教程后还有什么不理解的，你可以在这本书中找到解释。

> **Note**
> 最好的体验是在Xcode中将本章的内容作为一个Playground打开。Playground允许你实时预览Swift的运行结果。

### 简单赋值
在Swift中，使用`let`定义一个常量，`var`定义变量。常量的值不需要在编译时确定，但是只能赋值一次。这意味着你可以给一个常量赋值后，多次使用。
```lua
var myVariable = 42
myVariable = 50
let myConstant = 42
```

给常量或者变量赋值的时候，类型必须相同。但是并不需要每次都显式的写出它们的类型，因为编译器默认的确定了一些数据的类型。例如上面的代码中`myVariable`是一个整数类型。

如果初始值没有提供足够的类型信息（或者没有初始值），可以在变量后增加类型说明符。
```lua
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

> **试验**
> 创建一个常量，指定它的类型为`Float`并赋值为4。

Swift中的数据类型之间不会进行隐式的转换。如果需要在不同数据类型之间进行转换的话，需要显式的创建一个目标类型的实例。
```lua
let label = "The width is "
let width = 94
let widthLabel = label + String(width)
```

> **试验**
> 尝试删除最后一行的`String`，看看会有什么错误。

实际上，还有一种更加简单的方法将值包含到字符串中：把需要包含的值写在圆括号`()`中，然后在括号前添加反斜线`\`就可以了，例如：
```lua
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

> **试验**
> 在字符串中使用`\()`包含浮点数

Swift使用`[]`创建和访问数组和字典。
```lua
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"

var occupations = [
	"Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

也可以使用初始化语句创建空的数组和字典。
```lua
let emptyArray = String[]()
let emptyDictionary = Dictionary<String, Float>()
```

如果类型信息能够被推导出来，你可以直接将空数组写为`[]`，空字典写为`[:]`。例如给函数传递参的时候。
```lua
shoppingList = [] // Went shopping and bought everything.
```

### 流程控制
使用`if`和`switch`进行条件判断，`for-in`、`for`、`while`和`do-while`进行循环。条件判断时的圆括号时可选的，但是`if`或者循环体的花括号`{}`时必须的。
```lua
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
	if score > 50 {
    	teamScore += 3
    } else {
    	teamScore += 1
    }
}
teamScore
```

`if`语句中，判断条件必须时布尔表达式，也就是说`if score { ... }`的形式是错误的，它并不会隐式的与0进行比较。

你可以使用`if`和`let`一起来判断值是否缺失。这些值被看作是选项（Optionals）。一个选项包含一个值或者`nil`来表示是否缺失内容。在类型后面添加`?`标记一个值是一个选项。
```lua
var optionalString: String? = "Hello"
optionalString == nil

var optionalName: String? = "John Apppleseed"
var greeting = "Hello!"
if let name = optionalName {
	greeting = "Hello, \(name)"
}
```

> **试验**
> 将`optionalName`的值改为`nil`，看一下能不能得到问候。在`if`后添加`else`语句，使得`optionalName`为`nil`的时候打印不同的问候语句。

如果选项的值为`nil`，条件为`false`，`if`后的语句被跳过。否则选项的值被赋值给`let`后的常量，并且该常量的作用域为花括号里面。

Swift里的`switch`语句支持任意数据类型以及比较操作，而不是被限制为整数的相等。
```lua
let vegetable = "red pepper"
switch vegetable {
	case "celery":
    	let vegetableComment = "Add some raisins and make ants on a log."
    case "cucumber", "watercress":
    	let vegetableComment = "That would make a good tea sandwich."
    case let x where x.hasSuffix("pepper"):
    	let vegetableComment = "Is it a spicy \(x)?"
    default: 
    	let vegetableComment = "Everything tasts good in soup."
}
```

> **试验**
> 尝试移除`default`语句，看看有什么错

执行完`switch`的一个`case`后，程序会从`switch`语句中跳出，而不会继续执行下一个`case`语句，因此在Swift中不需要显式的使用`break`跳出每一个分支。

在使用`for-in`进行迭代的时候，每次迭代都会返回一个键值对：
```lua
let interestingNumbers = [
	"Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]

var largest = 0
for (kind, numbers) in interestingNumbers {
	for number in numbers {
    	if number > largest {
        	largetst = number
        }
    }
}
```
largest

> **试验**
> 增加一个变量，记录最大值所在的分类

`while`会一直循环执行，直到判断条件发生变化。如果使用`do-while`，循环体至少会执行一次。
```lua
var n = 2
while n < 100 {
	n = n * 2
}
n

var m = 2
do {
	m = m * 2
} while m < 100
m
```

在`for`循环中通过索引来控制循环，其中`..`用来创建一个索引的范围，下面两个循环是一样的效果
```lua
var firstForLoop = 0
for i in 0..3 {
	firstForLoop += i
}
firstForLoop

var secondForLoop = 0
for var i = 0; i < 3; ++i {
	secondForLoop += 1
}
secondForLoop
```

使用`..`（两点）创建的范围不包括最大值，而`...`（三点）创建的范围包含最大值。

### 函数与闭包
Swift使用`func`关键字定义函数，然后与C语言一样使用函数名进行调用，而函数返回值类型用`->`标示。
```lua
func greet(name: String, day: String) -> String {
	return "Hello \(name), today is \(day)."
}
greet("Bob", "Tuesday")
```

> **试验**
> 移除`day`参数，添加一些别的信息

使用元组从函数中返回多个值。
```lua
func getGasPrices() -> (Double, Double, Double) {
	return (3.59, 3.69, 3.79)
}
getGasPrices()
```

Swift的函数可以接受可变参数。
```lua
func sumOf(numbers: Int...) -> Int {
	var sum = 0
    for number in numbers {
    	sum += number
    }
    
    return sum
}
sumOf()
sumOf(43, 597, 12)
```

> **试验**
> 编写一个计算参数平均值的函数

Swift的函数可以进行嵌套。被嵌套的函数可以访问外面函数定义的变量。
```lua
func returnFifteen() -> Int {
	var y = 10
    func add() {
    	y += 5
    }
    add()
    return y
}

returnFifteen()
```

Swift中的函数也是基本的数据类型，也就是说可以在一个函数中返回另外一个函数。
```lua
func makeIncrementer() -> (Int -> Int) {
	func addOne(number: Int) -> Int {
    	return 1 + number
    }
    
    return addOne
}

var increment = makeIncrementer()
increment(7)
```

函数当然也可以作为其它函数的参数进行传递。
```lua
func hasAnyMatches(list: Int[], condition: Int -> Bool) -> Bool {
	for item in list {
    	if condition(item) {
        	return true
        }
    }
    
    return false
}

func lessThanTen(number: Int) -> Bool {
	return number < 10
}

var numbers = [20, 19, 7, 12]
hasAnyMatches(numbers, lessThanTen)
```

实际上Swift中的函数是闭包的一个特例。我们可以使用没有名字的`{}`来定义闭包，而闭包内容与返回值类型之间用`in`进行分隔。
```lua
numbers.map({
	(number: Int) -> Int in
	    let result = 3 * number
    	return result
})
```

> **试验**
> 重写上面的闭包，使得所有的奇数都返回0

还有几种方式可以使得闭包的编写更加简洁。当闭包的类型是已知的，比如作为回调或者代理的时候，你可以省略参数类型或者返回值类型。如下：
```lua
numbers.map({ number in 3 * number })
```

在闭包中还可以使用位置来引用参数，这在编写非常短的闭包的时候比较有用。当闭包作为函数的最后一个参数时，可以将它放在函数的圆括号后面。
```lua
sort([1, 5, 3, 12, 2]){ $0 > $1 }
```

### 类与对象
Swift中使用`class`关键字定义类。类里面属性的声明与定义变量和常量差不多，而成员方法也与普通函数的写法一样，只是写在类里面。
```lua
class Shape {
	var numberOfSides = 0
    func simpleDescription() -> String {
    	return "A shape with \(numberOfSides) sides."
    }
}
```

> **试验**
> 给上面的类用`let`添加一个常量属性，并且增加一个能够接受一个参数的方法

直接在类名后增加圆括号就可以创建这个类的实例，然后通过点操作符访问它的属性和成员方法。
```lua
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

不过上面的`Shape`类还缺少一些重要的东西：在创建对象的时候进行初始化的初始化器。初始化器用`init`进行定义。
```lua
class NameShape {
	var numberOfSides: Int = 0
    var name: String
    
    init(name: String) {
    	self.name = name
    }
    
    func simpleDescription() -> String {
    	return "A shape with \(numberOfSides) sides."
    }
}
```

这里要注意的是`init`方法中`self`的用法。初始化方法的参数传递与普通的函数类似。类中的属性都应该进行初始化赋值，不管是在声明的时候还是在初始化器中。

`deinit`是Swift的析构函数，与`dealloc`类似，用在对象销毁时进行清理工作。

在类名后使用冒号`:`和父类的名字表示继承关系，Swift中并不要求每个类都有父类。

子类使用`override`关键字来重写父类的方法，如果没有写`override`的话，会出现编译器错误。同样，编译器也会检查带有`override`关键字的方法是否真的重写了父类的方法。
```lua
class Square : NamedShape {
	var sideLength: Double
    
    init(sideLength: Double, name: String) {
    	self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }
    
    func area() -> Double {
    	return sideLength * sideLength
    }
    
    override func simpleDescription() -> String {
    	return "A square with sides of length \(sideLength)."
    }
}

let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

> **试验**
> 为`NamedShape`创建一个叫`Circle`的子类，使它能够接受一个半径和名字作为参数。然后实现`area`和`describe`方法。

与Objective-C类似，Swift还能给属性定义`getter`和`setter`。
```lua
class EquilateralTriangle: NamedShape {
	var sideLength: Double = 0.0
    
    init(sideLength: Double, name: String) {
    	self.sideLength = sideLength
        super.init(name: name)
        
        numberOfSides = 3
    }
    
    var perimeter: Double {
    	get {
        	return 3.0 * sideLength
        }
        set {
        	sideLength = newValue / 3.0
        }
    }
    
    override func simpleDescription() -> String {
    	return "An equilateral triangle with sides of length \(sideLength)."
    }
}

var triangle = EquilateralTraingle(sideLength: 3.1, name: "a triangle")
triangle.perimeter
triangle.perimeter = 9.9
triangle.sideLength
```

在`perimeter`的设置方法中，新的值默认存放在`newValue`变量里。当然，也可以显式的在`set`方法后的圆括号中提供变量名字。

注意`EquilateralTriangle`类的初始化器里有三步：
1. 设置子类中定义的属性的值。
2. 调用父类的初始化器
3. 改变父类中定义的属性的值。

如果你不需要计算属性的值，但是想要在属性的值发生改变之前或者改变后执行一些任务，可以使用`willSet`和`didSet`。例如下面的代码可以保证三角形和四边形的边长总是相同的。
```lua
class TriangleAndSquare {
	var triangle: EquilateralTraingle {
    	willSet {
        	square.sideLength = newValue.sideLength
        }
    }
    
    var square: Square {
    	willSet {
        	triangle.sideLength = newValue.sideLength
        }
    }
    
    init(size： Double, name: String) {
    	square = Square(sideLength: size, name: name)
        triangle = EuilateralTriangle(sideLength: size, name: name)
    }
}

var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
triangleAndSquare.square.sideLength
triangleAndSquare.triangle.sideLength
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
triangleAndSquare.triangle.sideLength
```

类的成员方法与函数有一个非常重要的区别。函数里的参数名只能在函数体里面使用，而方法的参数名可以在调用的时候使用。
```lua
class Counter {
	var count: Int = 0
    func incrementBy(amount: Int, numberOfTimes times: Int) {
    	count += amount * times
    }
}

var counter = Counter()
counter.imcrementBy(2, numberOfTimes: 7)
```

使用选项（Optional）的时候，可以在方法、属性等操作符前使用`?`。如果选项的值为`nil`则忽略执行`?`后的表达式，并且整个表达式的值为`nil`。否则就执行`?`后的表达式。
```lua
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optinal square")
let sideLength = optionalSquare?.sideLength
```

### 枚举与结构体
使用`enum`关键字创建枚举类型。与类类似，枚举类型中一样可以定义方法。
```lua
enum Rank: Int {
	case Ace = 1
    case Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten
    case Jack, Queen, King
    
    func simpleDescription() -> String {
    	switch self {
        	case .Ace:
            	return "ace"
            case .Jack:
            	return "jack"
            case .Queen:
            	return "queen"
            case .King:
            	return "king"
            default;
            	return String(self.toRaw())
        }
    }
}
let ace = Rank.Ace
let aceRawValue = ace.toRow()
```

>**试验**
> 编写一个函数用来比较两个`Rank`枚举值

在上面的代码中，枚举类型的原始值是`Int`类型，定义的时候只明确了第一个，后面的依次递增。当然也可以使用浮点数或者字符串作为枚举类型的值。

使用`toRaw`和`fromRaw`可以在枚举类型和原始值之间进行转换。
```lua
if let convertedRank = Rank.fromRaw(3) {
	let threeDescription = convertedRank.simpleDescription()
}
```

枚举类型成员的值是一个实际的值，而不仅仅是他们原始值的另外一种写法。事实上，你可以不提供有意义的原始值。
```lua
enum Suit {
	case Spades, hearts, Diamonds, Clubs
    func simpleDescription() -> String {
    	switch self {
        	case .Spades:
            	return "spades"
            case .Hearts:
            	return "hearts"
            case .Diamonds:
            	return "diamonds"
            case .Clubs:
            	return "clubs"
        }
    }
}

let hearts = Suit.Hearts
let heartsDescription = hearts.simpleDescription()
```

> **试验**
> 给`Suit`添加一个`color`方法，当枚举值为`Spades`或者`Clubs`的时候返回"black"，否则返回"red"。

有两种方法引用`Hearts`的成员：给`hearts`常量赋值的时候，使用了完整的`Suit.Hearts`名字，这是因为并没有显式的声明该常量的类型。而在`switch`中，因为已经知道`self`就是`Suit`类型的值，直接使用`.Hearts`的简写就可以了。你可以在任何已经明确数据类型的情况下使用简写。

Swift使用`struct`关键字创建一个结构体。结构体与类在很多方面都是类似的，包括方法和初始化器。结构体和类最大的不同在于使用结构体传参的时候是按值传递，而对于类来说是按引用传递。
```lua
struct Card {
	var rank: Rank
    var suit: Suit
    func simpleDescription() -> String {
    	return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}

let threeOfSpades = Card(rank: .Three, suit: .Spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()
```
> **试验**
> 给`Card`添加一个方法用来创建一整套德州扑克，其中每张牌都必须包括花色和点数。

每个枚举成员都可以关联一些值。同一个枚举类型的实例成员都能够关联不同的值。在创建枚举类型的实例时可以给枚举成员关联上一些值。关联的值与原始的值不一样：同一个枚举类型的所有成员中，原始值都是一样的，在定义枚举类型的时候就已经确定了。

例如从服务器上获取日落和日出的实际。服务器会返回正确的信息或者出错。
```lua
enum ServerResponse {
	case Result(String, String)
    case Error(String)
}

let success = ServerResponse.Result("6:00 am", "8:09 pm")
let failure = ServerResponse.Error("Out of cheese")

switch success {
	case let .Result(sunrise, sunset):
    	let serverResponse = "Sunrise is at \(sunrise) and sunset is at \(sunset)."
    case let .Error(error):
    	let serverResponse = "Failure... \(error)"
}
```

> **试验**
> 给`ServerResponse`的`switch`添加第三个`case`分支。

### 协议与扩展
使用`protocol`关键字声明协议。
```lua
protocol ExampleProtocol {
	var simpleDescription: String { get }
    mutating func adjust()
}
```

类、枚举和结构体都能够响应协议。
```lua
class SimpleClass : ExampleProtocol {
	var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
    	simpleDescription += " Now 100% adjusted."
    }
}

var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
	var simpleDescription: String = "A simple structure"
    mutating func adjust() {
    	simpleDescription += " (adjusted)"
    }
}

var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```
> **试验**
> 定义一个枚举类型并响应上面的协议

声明结构体`SimpleStructure`时使用的`mutating`关键字标记会对结构体进行修改的方法。由于类中的方法总是可以修改类的对象，因此不需要在`SimpleClass`中进行标明。

Swift使用`extension`关键字给一个已经存在的类型添加功能。你可以给任何地方声明的类型使用扩展，使得它响应某个协议，不管它是从框架还是其它地方导入的。
```lua
extension Int: ExampleProtocol {
	var simpleDescription: String {
    	return "The number \(self)"
    }
    mutating func adjust() {
    	self += 42
    }
}
7.simpleDescription
```

> **试验**
> 给`Double`类型使用扩展添加一个`absoluteValue`属性。

可以像其它数据类型一样用协议来声明变量或这常量，例如创建一个对象的集合，使得它可以容纳可以响应同一个协议的不同数据类型的值。当你使用这些值的时候，不能调用协议以外的方法。
```lua
let protocolValue: ExampleProtocol = a
protocolValue.simpleDescription
//protocolVAlue.anotherProperty //Uncomment to see the error
```

尽管`protocolValue`变量是`SimpleClass`的对象，但是编译器把它当成是`ExampleProtocol`类型。这就意味着你只能访问协议里定义的方法和属性。

### 泛型
使用尖括号可以定义泛型函数或类型。
```lua
func repeat<ItemType>(item: ItemType, times: Int) -> ItemType[] {
	var result = ItemType[]()
    for i in 0..times {
    	result += item
    }
    
    return result
}
repeat("knock", 4)
```

同样可以创建泛型类、枚举类型和结构体类型。
```lua
//重新实现Swift标准库中的optional类型
enum OptionalValue<T> {
	case None
    case Some(T)
}

var possibleInteger: OptionalValue<Int> = .None
possibleInteger = .Some(100)
```
使用`where`关键字可以给泛型添加一个限制列表，例如:两个类型必须相同、必须实现某个协议、必须是某个类的子类等。
```lua
func anyCommonElements <T, U where T: Sequence, U: Sequence, T.GeneratorType.Element: Equatable, T.GeneratorType.Element == U.GeneratorType.Element> (lhs: T, rhs: U) -> Bool {
	for lhsItem in lhs {
    	for rhsItem in rhs {
        	if lhsItem == rhsItem {
            	return true
            }
        }
    }
    
    return false
}

anyCommonElements([1, 2, 3], [3])
```

> **试验**
> 修改`anyCommonElements`函数，使得它能够返回两个序列的交集。

在简单的情况下，可以省略`where`，直接写后面的部分就可以了，如`<T: Equatable>`与`<T where T: Equatable>`的作用一样的。