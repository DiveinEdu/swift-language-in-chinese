# <center>苹果Swift语言官方文档（中文翻译版）</center>

本文档由[长沙戴维营教育](http://www.diveinedu.cn)组织翻译和校对，由于英语水平有限，请大家指正。

长沙戴维营教育还为本教程录制了配套的视频教程在[乌班图学院](http://www.ubuntucollege.cn)上免费提供，欢迎大家一起学习。

下面的章节，如果为蓝色链接表示以及翻译完毕，可以查看，如果为黑色则表示正在紧张的翻译中。本文档中文版每天都会更新，大家可以随时查看。如由Bug欢迎改正。

- - -

# 函数

**函数**是一个执行特定任务的自包含代码块。可以给函数指定一个名字来标识它，需要的时候通过名字“调用”它来执行任务。

Swift使用一个统一的语法来表示简单的C语言风格的函数到复杂的Objective-C语言风格的方法。函数参数可以有默认值，并且能够指定为输入输出（in-out）参数，在函数里进行修改。

每个Swift的函数都有一个类型，由它的参数类型和返回值类型决定。函数类型与其它Swift类型一样，可以用来声明变量、函数参数类型或者返回值类型等。Swift的函数可以嵌套定义。

### 函数的定义和调用
定义函数的时候，可以指定一个或多个输入参数和一个返回值类型。每个函数都有一个**函数名**来描述它的功能。通过函数名以及对应类型的参数值来调用这个函数。函数的参数传递的顺序必须与参数列表相同。

下面是一个叫`greetingForPerson`的函数，它接受一个人名作为输入，并返回对他的问候。为了实现这个函数，我们需要定义一个`String`类型的输入参数`personName`并且指定函数的返回值类型为`String`。
```go
func sayHello(personName: String) -> String {
	let greeting = "Hello, " + personName + "!"
    return greeting
}
```

上面就是一个以`func`关键字开头完整的函数定义。在函数名后用`->`来说明函数的返回值类型。这个定义描述了函数的功能、参数个数和类型以及返回值类型。函数的调用非常简洁：
```go
println(sayHello("Anna"))
//prints "Helo, Anna!"
println(sayHello("Brain"))
//prints "Hello, Brain!"
```

在函数名后的圆括号里提供一个`String`类型的参数值来调用`sayHello`，如`sayHello("Anna")`。`sayHello`返回一个`String`类型的值，因此可以之间将这个函数调用嵌入在`println`的调用中来打印它的返回结果。

在`sayHello`中定义了一个`String`类型的常量`greeting`，并且将它设置为对`personName`的问候。然后通过`return`语句将`greeting`的值传递idao函数外面。一旦`return greeting`被执行，这个函数就会结束执行。

你可以调用`sayHello`函数多次，每次用不同的参数值。它会返回不同的问候语句。为了简化这个函数，可以将消息的创建和返回语句合并到同一行中。
```go
func sayHelloAgain(personName: String) -> String {
	return "Hello again, " + personName + "!"
}
println(sayHelloAgain("Anna"))
//prints "Hello again, Anna!"
```

### 函数参数与返回值
Swift中函数参数与返回值的定义非常灵活。你可以定义只有一个非命名参数的工具函数，也可以定义具有说明性参数名字和不同参数选项的复杂函数。

#### 多参数函数
函数可以有多个参数，在圆括号中用逗号隔开。下面的函数具有两个参数，并计算这个半闭区间的元素个数：
```go
func halfOpenRangeLength(start: Int, end: Int) -> Int {
	return end - start
}
println(halfOpenRangeLength(1, 10))
//prints "9"
```

#### 无参函数
函数可以没有输入参数。下面的函数每次调用的时候都返回同一个`String`类型的消息：
```go
func sayHelloWorld() -> String {
	return "hello, world"
}
println(sayHelloWorld())
//prints "hello, world"
```
无参函数的调用和定义不能省略后面的圆括号。

#### 无返回值的函数
Swift的函数返回值也不是必须的。下面是一个新的`sayHello`，每次都打印一个问候语句，而不是返回问候内容：
```go
func sayGoodbye(personName: String) {
	println("Goodbye, \(personName)!")
}
sayGoodbye("Dave")
//prints "Goodbye, Dave!"
```
没有返回值的函数，后面不需要用`->`来说明返回值类型。
> **提示**
> 严格来讲，`sayGoodbye`函数还是会返回一个值。对于没有定义返回值类型的函数，它都会返回一个`Void`类型的值。这是一个空的元组，可以写为`()`。

函数调用的时候，可以忽略它的返回值：
```go
func printAndCount(stringToPrint: String) -> Int {
	println(stringToPrint)
    return countElements(stringToPrint)
}

func printWithoutCounting(stringToPrint: String) {
	printAndCount(stringToPrint)
}
printAndCount("hello, world")
//prints "hello, world" and returns a value of 12
printWithoutCounting("hello, world")
//prints "hello, world" but does not return a value
```
第一个函数`printAndCount`打印一个字符串，并且返回它的字符个数。第二个函数`printWithoutCounting`调用了第一个函数，但是将它的返回值忽略了。当调用第二个函数的时候，第一个函数还是会打印消息，但是它的返回值却没有被使用。
> **提示**
> 虽然函数的返回值可以被忽略，但是如果一个函数定义的时候指明有返回值，则必须要返回一个值。否则的话会出现编译错误。

#### 多个返回值的函数
通过元组可以在函数里一次性返回多个值。下面的`count`函数，会统计字符串中的元音、辅音和其它字符：
```go
func count(string: String) -> (vowels: Int, consonants: Int, others: Int) {
    var vowels = 0, consonants = 0, others = 0
    for character in string {
        switch String(character).lowercaseString {
        case "a", "e", "i", "o", "u":
            ++vowels
        case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
        "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
            ++consonants
        default:
            ++others
        }
    }
    return (vowels, consonants, others)
}
```
用这个函数来统计任意字符串，并通过一个元组获取三个结果值：
```go
let total = count("some arbitray string!")
println("\(total.vowels) vowels and \(total.consonants) consonants")
//prints "6 vowels and 13 consonants"
```
注意，不需要再给元组的元素命名，它们会作为返回值的一部分返回。

### 函数的形参数名
上面所有的函数都为形参定义类名字：
```go
func someFunction(parameterName: Int) {
	//函数体，可以使用参数名称
}
```
但是这些参数名只能在这个函数体中使用，而不能用在调用的时候。它们被称为本地参数名。

#### 外部参数名
在函数调用的时候，如果能够使用参数名称，能够是代码更加具有可读性。外部参数就是用来完成这件事的：
```go
func someFunction(externalParametername localParameterName: Int) {
	//函数体，使用localParamterName来引用参数值
}
```

> **提示**
> 如果一个参数定义了外部参数名，在调用的时候就必须使用。

下面是关于本地参数名和外部参数名的示例：
```go
func join(s1: String, s2: String, joiner: String) -> String {
	return s1 + joiner + s2
}

join("hello", "world", ", ")
//return s "hello, world"
```

如果使用外部参数名：
```go
func join(string s1: String, toString s2: String, withJoiner joiner: String) -> String {
	return s1 + joiner + s2
}

join(String: "hello", toString: "world", withJoiner: ", ")
```

#### 简化外部参数名的定义
如果你需要同时给函数的参数提供外部参数名和本地参数名，但是想用相同的名字。戴维营教育可以在参数名前面添加`#`符号。Swift会将这个名字同时作为外部参数名和局部参数名。
```go
func containsCharacter(#string: String, #characterToFind: Character) -> Bool {
	for character in string {
    	if character == characterToFind {
        	return true
        }
    }
    
    return false
}

let containsAVee = containsCharacter(string: "aardvark", characterToFind: "v")
```

#### 默认参数
函数定义的时候，可以给参数指定默认值，这样在调用的时候就可以省略不写。
> **提示**
> 默认参数必须出现在参数列表的最后，并且在调用的时候必须按顺序赋值。

```go
func join(string s1: String, toString s2: String, withJoiner joiner:String = " ") -> String {
	return s1 + joiner + s2
}

join(string: "hello", toString: "world", withJoiner: "-")
//returns "hello-world"

join(string: "hello", toString: "world")
//returns "hello world"
```

#### 外部参数名与默认值
大多数情况下，为使用默认值的参数提供外部参数名能方便理解。如果你没有为它们提供外部参数名的话，Swift会自动为拥有默认值的参数提供与本地参数同名的外部参数名。
```go
func join(s1: String, s2: String, joiner: String = " ") -> String {
	return s1 + joiner + s2
}

join("hello", "world", joiner: "-")
//returns "hello-world"
```

> **提示**
> 这种情况可以使用`_`来代替显式的表示不需要外部参数名。

#### 可变参数
可变参数可以接受0个或多个特定类型的值作为函数的参数。通过在参数类型后添加`...`表示可变参数。这些参数被作为数组传递到函数中。例如`numbers: Double...`在函数中表示一个叫`numbers`的`Double[]`（数组）。
```go
func arithmeticMean(numbers: Double...) -> Double {
	var total: Double = 0
    for number in numbers {
    	total += number
    }
    
    return total / Double(numbers.count)
}

arithmeticMean(1, 2, 3, 4, 5)
//returns 3.0

arithemeticMean(3, 8, 19)
//returns 10.0
```

> **提示**
> 一个函数最多只能有一个可变参数，并且必须出现在参数列表的最后。

#### 常量参数与变量参数
函数参数默认为常量。在函数体中不能修改它们的值。可以使用`var`关键字将它们声明为变量。
```go
func alignRight(var string: String, count: Int, pad: Character) -> String {
	let amountToPad = count - countElements(string)
    for _ in 1...amountToPad {
    	string = pad + string
    }
    
    return string
}
let originString = "hello"
let paddedString = alignRight(orginalString, 10, "-")
//paddedString is equal to "-----hello"
//orginString is still equal to "hello"
```
上面的代码定义了一个叫`alignRight`的函数，它可以使输入的字符串有对齐，左边的空白用特定的占位符填充。它的`string`参数是一个局部变量，可以在函数体中进行操作。
> **提示**
> 变量参数的作用范围为函数体，它并不会影响函数的外面的变量值。

#### 输入输出参数
在上面提到，变量参数可以在函数里面进行修改，但对它的修改并不会影响函数外面。如果你想要修改函数外面的值，需要使用`inout`关键字它们定义为`in-out`参数。如果在函数里面修改了输入输出参数的值，同样会改变外面变量的值。在调用函数的时候，只能使用变量作为函数的实参，而不能使用常量和字面常量。在给输入输出函数传参时，需要在变量名前放置`&`符号来表示这个变量的值可能会被函数修改。

> **提示**
> 输入输出参数不能够使用默认值和可变参数。同样，如果一个参数被标记为`inout`，就不能再使用`var`和`let`了。

```go
func swapTwoInts(inout a: Int, inout b: Int) {
	let temporaryA = a
    a = b
    b = temporaryA
}
```

`swapTwoInts`实现了交换两个整型变量值的功能，下面是它的使用方法。
```go
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
println("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
//prints "someInt is now 107, and anotherInt is now 3"
```

> **提示**
> 输入输出参数可以用来从函数里返回数据。

#### 函数类型
每个函数都有一个特定的函数类型，由它的参数类型和返回值类型组成。例如：
```go
func addTwoInts(a: Int, b: Int) -> Int {
 return a + b
}

func multiplyTowInts(a: int, b: Int) -> Int {
	return a * b
}
```

这个例子里定义了两个数学函数`addTwoInts`和`multiplyTwoInts`。它们拥有相同的参数类型和返回值类型，因此它们的函数类型为`(Int, Int) -> Int`，读为：
“一个拥有两个整型(Int)参数，并且返回值类型为整型(Int)的函数类型。”

下面是另外一个例子，它没有参数和返回值：
```go
func printHelloWorld() {
	println("hello, world")
}
```

因此它的函数类型为`() -> ()`，或者叫“一个没有参数并且返回值类型为`void`的函数类型”。没有声明返回值类型的函数返回值类型为`Void`，在Swift中等价与一个空的元组`()`。

#### 使用函数类型
在Swift中可以与其它类型一样使用函数类型。戴维营教育可以用它来定义常量、变量并且给它设置一个合适的函数。
```go
var mathFunction: (Int, Int) -> Int = addTwoInts
```
这个语句读做“定义一个`mathFunction`变量，它是一个函数类型，这个函数可以接受两个整型`Int`参数并且返回值为整型`Int`。使这个变量引用一个叫`addTwoInts`的函数”。

`addTwoInts`函数和`mathFunction`变量具有相同的数据类型。现在就可以通过刚才定义的变量名`mathFunction`来调用函数了：
```go
println("Result: \(mathFunction(2, 3))")
//prints "Result: 5"
```

相同类型的函数可以赋值给同一个变量：
```go
mathFunction = multiplyTwoInts
println("Result: \(mathFunction(2, 3))")
//prints "Result: 6"
```
当然，你也可以让Swift自动替你推导变量或者常量的类型：
```go
let anotherMathFunction = addTwoInts
//anotherMathFunction is inferred to be of type (Int, Int) -> Int
```

#### 函数作为参数传递
你可以将函数类型如`(Int, Int) -> Int`作为另外一个函数的参数类型。这使你可以将函数的某些功能留给其它函数去实现。下面是一个例子：
```go
func printMathResult(mathFunction: (Int, Int) -> Int, a: Int, b: Int) {
	println("Result: \(mathFunction(a, b))")
}

printMathResult(addTwoInts, 3, 5)
//prints "Result: 8"
printMathResult(multiplyTwoInts, 3, 5)
//prints "Result: 15"
```

#### 函数作为返回值
函数还可以作为另外一个函数的返回值进行传递，只需要在`->`后写一个完整的函数类型。下面的例子定义了两个简单的函数`stepForward`和`stepBackward`。`stepForward`函数返回输入值加1的结果，而`stepBackward`返回输入值减1的结果。它们的函数类型都为`(Int) -> Int`。
```go
func stepForward(input: Int) -> Int {
	return input + 1
}

func stepBackward(input: Int) -> Int {
	return input - 1
}
```

下面有一个`chooseStepFunction`，它根据输入的参数返回一个`(Int) -> Int`类型的函数`stepForward`或者`stepBackward`。
```go
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
	return backwards ? stepBackward : stepForward
}
```
现在就可以用`chooseStepFunction`函数来获取前进方向了：
```go
var currentValue = 3
let moveNearerToZero = chooseStepFunction(currentValue > 0)
//moveNearerToZero now refers to the stepBackward() function
```

上面的例子演示了根据`currentValue`的值来决定如何接近0。`currentValue`的初始值为3，大于0，因此`chooseStepFunction`返回`stepBackward`函数，并建起存放在`moveNearerToZero`常量中。

#### 嵌套函数
到目前为止，我们所使用到的函数都是全局函数(global functions)。我们还可以在函数体里面定义函数，称为嵌套函数。

嵌套函数的作用域默认是它所在的函数体，但是可以作为返回值返回，然后在其它作用域中使用。下面我们重写`chooseStepFunction`，让它返回一个嵌套函数：
```go
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
	func stepForward(input: Int) -> Int {
    	return input + 1
    }
    
    func stepBackward(input: Int) -> Int {
    	return input - 1
    }
    
  	return backwards ? stepBackward : stepForward
}

var currentValue = -4
let moveNearerToZero = chooseStepFunction(currentValue > 0)
//moveNearerToZero now refers to the nested stepForward() function

while currentValue != 0 {
	println("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}

println("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```

好吧,关于Swift语言的集合的学习，大茶哥只能帮你到这了。戴维营教育的伙计们，发奋图强吧。

戴维营教育网址：[http://www.diveinedu.com](http://www.diveinedu.com)
Swift视频教程网址：[http://www.ubuntucollege.cn](http://www.ubuntucollege.cn)

