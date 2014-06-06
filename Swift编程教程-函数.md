# <center>函数</center>

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



