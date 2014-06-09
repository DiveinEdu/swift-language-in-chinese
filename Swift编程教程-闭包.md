#闭包
闭包是一个独立的函数功能代码段，我们可以在代码之间当作变量那样使用和传递他。Swift里的闭包和Apple的C语言和Objective-C语言里的`block`以及其他语言里的`lambda`表达式非常类似。

闭包可以`capture`在上下文中的任何常量和变量并在其定义内存储其引用，这就是所谓的闭合并包含这些常量和变量,因此得名“闭包”。Swfit为我们处理这些变量`capturing`相关的所有的内存管理。
>提示
不要担心对`”capturing“`的概念原理不熟悉，这个东西将会在`Capturing Values`章节详细讲解。


在`函数`章节介绍的全局和嵌套函数,实际上是特殊的的闭包，闭包拥有如下三种形式之一：
- 全局函数是闭包：他有名称但是没有`capture`任何变量值。
- 嵌套函数是闭包：他有名称而且也有`capture`了他所定义位置的函数里的变量。
- 闭包表达式是用轻量级语法书写的匿名闭包，能`capture`该表达式所在上下文的变量值。

Swift的闭包表达式有很简洁清晰的风格，和在一般场景下鼓励使用带有简短，语法整齐等的优化。这些优化包括：
- 从上下文推导参数和返回值的类型
- 从单个表达式闭包中隐式返回
- 参数名称简写
- 尾缀闭包语法

####闭包表达式
嵌套函数，是在一个比较大的函数体内很方便地命名和定义的一个独立的代码块。然而有些时候编写短小精悍的像函数结构那样的没有完整声明和名称的代码也是蛮有用的，特别是当我们遇到一个函数需要用另一个或者多个函数当成参数的时候。
闭包表达式是用来写内联闭包的一种方式，他提供好几种不失简单明了的语法书写方面的优化。后面的例子会演示这些优化在`sort`函数的应用上体现。

####Sort函数
Swift的标准库提供了一个叫做`sort`的函数，他能够对某个类型的数组根据我们提供的排序闭包的返回结果进行排序，一旦排序操作结束，便返回一个与原数组同类型同大小的，且元素已经排好序的新数组。

下面的闭包表达式例子演示使用`sort`函数来对一个`String`类型的数组按照字母反序来排序，待排序的数组如下：
```go
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
```

Swift标准库里的`sort`函数需要两个参数：
- 第一参数是某个类型值的数组
- 第二个参数是一个闭包，该闭包有两个和数组内容的类型相同的参数，然后返回一个`Bool`值来表示在排序时闭包的第一参数值是否出现在第二个参数的前面还是后面，如果第一个参数值出现在第二个参数值的前面，闭包就返回`true`,否则返回`false`。

这个例子是对一个`String`数组进行排序，因此排序的闭包应该是`(String, String) -> Bool`这样的一个函数类型。

提供排序闭包的一种方式是编写一个类型正确的普通函数，并将其传给 `sort` 函数的第二个参数：

```go
func backwards(s1: String, s2: String) -> Bool {
    return s1 > s2
}
var reversed = sort(names, backwards)
// reversed is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

如果第一个字符串 (s1) 大于第二个字符串 (s2)，`backwards` 函数则返回`true`，表示在新的数组中 s1 应该出现在 s2 前。 字符串中的字符的 "大于" 表示 "按照字母顺序在后出现"。 这意味着字母 "B" 大于字母 "A", 字符串 "Tom" 大于字符串 "Tim"。 这里将进行字母逆序排序，"Barry" 将会排在 "Alex" 之后,一次类推。

然而，这是一中相当冗长的编写方式，本质上只是写了一个单表达式函数 (a > b)。 在下面的例子中，利用闭合表达式语法可以更好的构造一个内联排序闭包。

####闭包表达式语法
闭包表达式语法的一般形式如下：
```go

{ ( parameters ) -> return type in
    statements
}

```

闭包表达式语法可以使用常量参数、变量参数和 inout 类型参数，但是不提供默认值。 也可以在参数列表的最后使用可变参数。元组也可以作为他的参数和返回值。

下面的例子展示前面 `backwards` 函数对应的用闭包表达式编写版本的代码：
```go
reversed = sort(names, { (s1: String, s2: String) -> Bool in
    return s1 > s2
    })
```

我们需要注意的是这个内联闭包的参数和返回值类型声明与 `backwards` 函数类型声明是相同的。 在这两种情况中，都写成 (s1: String, s2: String) -> Bool 类型。 然而，在内联闭包表达式中，参数和返回值类型都写在大括号内，而不是大括号外。

闭包的函数体开始部分由关键字 in 说明。 该关键字表示闭包的参数和返回值类型定义已经结束，闭包函数体部分即将开始。

因为这个闭包的函数体部分是如此简短以至于可以将其改写成单行代码：
```go
reversed = sort(names, { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

这举例说明了`sort`函数的整体调用一直保持一致。一对圆括号包裹住函数的全部参数集合。而其中一个参数现在就是一个内联闭包。


####上下文类型推断
因为排序闭包是作为一个函数的参数进行传递的，Swift可以推断其参数和`sort`函数第二个参数的返回值的类型。 `sort` 期望第二个参数是类型为 `(String, String) -> Bool` 的函数，也就是说实际上 `String`, `String` 和 `Bool` 类型并不需要作为闭包表达式定义的一部分。 因为所有的类型都可以被正确推断出来，返回箭头 (->) 和 围绕在参数周围的圆括号也可以被省略：
```go
reversed = sort(names, { s1, s2 in return s1 > s2 } )
```

实际上任何情况下，把内联闭包表达式的闭包作为参数传递给函数时，都可以推断出该闭包的参数和返回值类型，这意味着我们几乎没必要利用完整形式来构造任何内联闭包。

当然，只要我们乐意，也可以使用显式类型，这样做可以避免代码的读者阅读时发生可能的歧义，这样还是值得鼓励的。这个排序函数，演示闭包的目的是很明确，即发生了排序，而且对读者来说可以信赖的认为闭包可以和字符串值一起运作,因为它协助了一个字符串数组的排序。

####单行表达式闭包的隐式返回
单行表达式的闭包可以通过在其定义中省略`return`关键字来隐式返回单行表达式的结果，前面的例子可以写成这样的形式：
```go
reversed = sort(names, { s1, s2 in s1 > s2 } )
```
在这个示例中，`sort` 函数的第二个参数的函数类型明确了闭包必须返回一个 `Bool` 类型值。 因为闭包函数体只包含了一个单行表达式 `(s1 > s2)`，而此表达式返回 `Bool` 类型值，故这里不会有歧义，可以省略return关键字。

####参数名简写
Swift语言自动为内联闭包提供了参数名称简写的特性，我们可以直接用 $0,$1,$2等等名字来引用的闭包的参数的值。

如果我们在闭包表达式中使用参数名称简写，我们可以在闭包参数列表中省略他的定义，简写的参数名称的数目和类型会通过函数类型进行推断出来。 `in` 关键字也可以被省略，因为闭包表达式完全由闭包函数体组成:
```go
reversed = sort(names, { $0 > $1 } )
```
这里的`$0` 和 `$1` 分别对应闭包中第一个和第二个 `String` 类型的参数。

####运算符函数
实际上还有一种更短小精悍的方式来编写上面的闭包表达式。 Swift的 `String` 类型定义了关于大于运算符 (>)的字符串实现，把他当作一个函数接受两个 `String` 类型的参数并返回 `Bool` 类型的值。 而他刚好和 `sort` 函数的第二个参数所要求函数类型一致。 所以，我们可以简单地传递一个大于符号，Swift可以推断出我们想使用字符串的大于符号运算函数实现：
```go
reversed = sort(names, >)
```

关于运算符函数的更多信息，可参看`Operator Fuctions`章节。

####尾缀闭包(Trailing Closures)
如果我们需要把一个很长的闭包表达式作为最后一个参数传递给函数的时候，可以换用尾缀闭包的写法来增强代码的可读性。尾缀闭包是一个书写在函数调用括号之外(之后)的闭包表达式，函数支持将其作为最后一个参数传递。
```go
func someFunctionThatTakesAClosure(closure: () -> ()) {
    // function body goes here
}

// here's how you call this function without using a trailing closure:

someFunctionThatTakesAClosure({
    // closure's body goes here
    })

// here's how you call this function with a trailing closure instead:

someFunctionThatTakesAClosure() {
    // trailing closure's body goes here
}
```
>提示
如果我们使用的函数只需要闭包表达式一个参数，且我们采用尾缀闭包写法时，我们还可以把`()`都省略掉。

在前面例子中作为`sort`函数参数的字符串排序闭包可以改写为在函数调用括号外面的尾缀闭包形式：
```go
reversed = sort(names) { $0 > $1 }
```
当闭包非常冗长以至于不能编写在单行代码中时，尾缀闭包就变得非常有用。 比如，Swift 的 `Array` 类型有一个 `map` 方法，他需要一个闭包表达式作为其唯一的参数。 数组中的每一个元素都要调用一次该闭包函数，并返回该元素所映射的值(也可能是其他类型的值)。 而具体的映射方式和返回值类型由传进来的闭包来确定。

当为数组的每一个元素应用闭包函数后，`map` 方法将返回一个新的数组，该新数组中包含了与原数组一一对应的映射后的值。

下面介绍怎样在 `map` 方法中使用 尾缀闭包形式将 `Int` 类型数组 `[16,58,510]` 转换为包含对应 `String` 类型的数组 `["OneSix", "FiveEight", "FiveOneZero"]`:
```go
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```
上面的代码创建了一个整型数字到他们的英文名称之间映射的字典。 同时也定义了一个准备转换为字符串的整型数组。
我们现在可以通过传递一个 尾缀形式的闭包 给 `numbers`数组的 `map` 方法来创建对应的`String`数组。 需要注意的是对`numbers.map`的调用不需要在 `map` 后面包含任何括号，因为`map`方法只需要传递一个闭包参数，且该闭包参数是采用尾缀方式进行编写：
```go
let strings = numbers.map {
    (var number) -> String in
    var output = ""
    while number > 0 {
        output = digitNames[number % 10]! + output
        number /= 10
    }
    return output
}
// strings is inferred to be of type String[]
// its value is ["OneSix", "FiveEight", "FiveOneZero"]
```

这`map`函数为数组里的每个元素调用一次闭包，我们都不需要去指定闭包的输入参数`number`的类型，因为类型可以从待映射的数组的值类型推导出来。

例子中闭包的`number`参数被声明为一个变量参数 (具体描述请参看`Constant and Variable Parameters`)，因此该参数的值可以在闭包函数体内对其进行修改。 闭包表达式指定了其返回值类型为 `String`，用以表明存储映射值的新数组类型也为 `String`。

闭包表达式在他每次被调用的时候创建了一个叫`output`的字符串。 用求余运算符 (number % 10) 来计算最后一位数字，并用该数字在 `digitNames` 字典中查找所映射的字符串。

>提示
字典的下标运算符后面跟着一个叹号(`!`),因为字典的下标运算符返回一个`optional`值来表示字典的键不存在时查找可能失败。在上面的例子中，他确保`number % 10`总是一个有效的字典下标键，因此，叹号被用来`强制展开`存储在下标运算符返回的`optional`值中的`String`类型值。

从`digitNames`字典中获取的字符串被添加到`output`的前面，逆序创建了一个数字的字符串版本( (表达式 `number % 10`中，当`number`为16，则余6，58余8，510余0).
`number` 变量除以10, 因为是整型，在计算时余数被忽略。 因此 16的商是1，58的商5，510商是51。

整个过程重复执行着，直到 `number /= 10` 为 0 时闭包会将结果`output`字符串输出，被`map`函数添加到输出的数组中。

上面例子中尾缀闭包语法紧跟在函数后面整洁地封装了闭包的功能代码，而不再需要将整个闭包包裹在 `map` 函数调用的括号内。

####值的捕获(Capturing)
闭包可以捕获他所定义的上下文当中的常量或者变量，哪怕定义那些变量或者常量的原作用域早已不复存在，依然可以在闭包内对进行引用和修改他们的值。

Swift语言中极简单的闭包形式是嵌套函数，也就是在其他函数体内定义的函数。 嵌套函数可以捕获其外部函数的参数以及定义的任一常量和变量。

这有一个叫做 `makeIncrementor` 的示例函数，他包含了一个叫做 `incrementor` 嵌套函数的定义。 嵌套函数 `incrementor` 从上下文中捕获了两个值:`runningTotal` 与 `amount`。 然后呢， `makeIncrementor` 将 `incrementor` 作为闭包返回。 每次对 `incrementor`的调用都把`runningTotal`的值增加`amount`。
```go
func makeIncrementor(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementor() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementor
}
```
`makeIncrementor` 的返回类型是 `() -> Int`。 这意味着他返回的是一个函数，而不是一个简单类型值。 这个返回的函数在每次调用时没有参数且只返回一个 `Int` 类型的值。 想知道一个函数怎样返回其他函数的信息，请参看`Function Types as Return Types`章节。

这个`makeIncrementor` 函数他定义了一个整型变量runningTotal(初始为0)用来存储当前增加总数并返回之。
`makeIncrementor` 函数有一个 `Int` 类型的参数，其外部名称为 `forIncrement`， 局部名称为 `amount`，表示每一次 `incrementor` 被调用时 `runningTotal` 所要增加的量。
`makeIncreamentor`定义了一个嵌套函数`incrementor`，用来执行实际的增量操作，该函数简单的使`runningTotal`增加`amount`后并返回他。

如果我们单独的看这个嵌套函数`incrementor`，会有点不同寻常：
```go
func incrementor() -> Int {
    runningTotal += amount
    return runningTotal
}
```

这个`incrementor`函数没有任何参数,他在自己的函数体内捕获并引用了上下文中外围函数的变量值`runningTotal`和`amount`。因为他不会修改`amount`的值，他实际上捕获了并存储了一份`amount`的值的副本，这个值连同`incrementor`闭包存储在一起。
然而，因为每次函数被调用的时候，都会修改`runningTotal`的值，`incrementor`捕获了当前`runningTotal`变量的一个引用，而不仅仅只是其初始值的一份拷贝。捕获出一份引用可以确保`runningTotal`不会因为对`makeIncrementor`的调用结束而消失，并能确保在下一次调用过程中还可以对`runningTotal`进行递增操作。
>提示
Swift编译器会决定是否是进行引用捕获还是值的拷贝，我们不需要注明`amount`或者`runningTotal`来说明其可以在嵌套函数`incrementor`中使用。Swift同时也会在当递增函数不再需要`runningTotal`时为我们处理其所有的内存管理操作。

下面有一个`makeIncrementor`实际调用的例子：
```go
let incrementByTen = makeIncrementor(forIncrement: 10)
```
这个例子设置了一个叫做 `incrementByTen` 的常量，该常量指向一个每次调用会加10的 `incrementor` 函数， 多次调用这个函数可以得到以下结果：
```go
incrementByTen()
// returns a value of 10
incrementByTen()
// returns a value of 20
incrementByTen()
// returns a value of 30
```

如果我们创建了另一个 `incrementor`，他就会有一个属于自己的独立的 `runningTotal` 变量的引用。 下面的例子中 `incrementBySevne` 捕获了一个新的 `runningTotal`变量的引用，而该变量和在`incrementByTen`中捕获的变量没有一丝联系：
```go
let incrementBySeven = makeIncrementor(forIncrement: 7)
incrementBySeven()
// returns a value of 7
incrementByTen()
// returns a value of 40

```
>提示
如果我们把一个闭包赋值给一个类的对象的属性，并且闭包捕获了该对象或者该对象成员的引用，那我们将会在这个闭包和对象之前创建一个强引用环。Swift可以用捕获列表去打破这样的一个强引用环。更多详细信息，请参看`Strong Reference Cycles for Closures`章节。

####闭包是引用类型
在前面的例子中，`incrementBySeven` 和 `incrementByTen` 都是常量，但是这些常量指向的闭包仍然可以增加其捕获的变量的值。 这是因为函数和闭包都是引用类型。

无论何时我们把函数或闭包赋值给一个常量或变量，我们实际上都是将常量或变量的值设置为对应函数或闭包的引用。 在上面的例子中，`incrementByTen` 是指向闭包的引用常量，而并不是闭包本身的内容。
这也就是说如果我们将闭包赋值给了两个不同的常量或者变量，那么这两个常量或变量都会指向同一个闭包(有点浅复制的味道哈)：
```go
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// returns a value of 50
```

** **
好吧,关于Swift语言闭包特性的学习，大茶哥只能帮你到这了。
英语捉急，翻译的不到位的地方请见谅并反馈给我们，谢谢。
戴维营教育的伙计们，发奋图强吧。

戴维营教育网址：[http://www.diveinedu.com](http://www.diveinedu.com)
Swift视频教程网址：[http://www.ubuntucollege.cn](http://www.ubuntucollege.cn)
##学iOS开发，只选长沙戴维营教育！诚信教育！学生和企业都信赖的教育机构！http://www.diveinedu.com
##版权所有，严禁盗版，转载请说明并保留引用链接！