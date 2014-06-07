#集合类型--字典

Swift语言提供了两种集合数据类型：数组和字典，数组是一系列相同类型的变量值有序存储；字典是一系列相同类型的变量值的无序存储，可以通过一个唯一标识符（键）来引用和查找。

数组和字典总是能识别自己所能够存储的键值的类型，就是说，我们不能够往这两个容器内插入错误的类型。显式类型声明的集合能确保我们的代码总是能识别其所能存储的变量类型，并能够让我们在实际编码中捕捉到出现的任何类型错误。

##字典
字典是一个可以存储多个同一类型数据值的一个容器，每一个数据值都关联着一个唯一的键，代表字典里面该数据值的标识符。和数组里的元素不同的是，字典里面的元素是没有指定的顺序的。当我们需要根据标识符来查找数据值的时候就需要应用到字典这个数据类型，就像现实生活中根据一个字来查找他的解释一样。

Swift的字典所存的键值对必须是有明确类型的，她不和`Objective-C`的`NSDictionary`和`NSMutableDictionary`类那样可以存储任意类型的对象，不需要提供这些对象的任何类似信息。
在Swift里面，一个特定的字典所能存储的键和值类型都必须是很明确的，通过显式类型说明或者类型推断。

Swift的字典一般书写形式是：`Dictionary < KeyType, ValueType >`,其中`KeyType`是被用作字典元素项里健的数据类型，而`ValueType`是字典所存的该键对应的值的数据类型。
这里唯一的限制就是`KeyType`的数据类型必须可以被哈希的，也就是说其本身必须提供一个方式可使其被唯一表示。所有的Swift基本数据类型（如：`String`,`Int`,`Double`,`Bool`）默认就是可被哈希的，所有这些类型都可以被用来当作字典里的一个键；没有关联值的枚举成员默认也是可被哈希和可用来当作字典里的键。


####字典字面常量
我们可以用一个字面常量来初始化一个字典，和之前所讲的数组字面常量相类似的语法；用字典字面常量可以很简便的创建含有一个或者多个键值对的字典集合类型的对象。

一个键值对是一个健和一个值的组合，在一个字典字面常量里，每个键值对内部的键和值用冒号分隔，键值对之间用逗号隔开的列表形式，然后整体用一对中括号括起来:

>[`key 1`: `value 1`, `key 2`: `value 2`, `key 3`: `value 3`]

下面这段示例代码创建一个字典来存国际机场的名字，字典的键是三个字符的国际航空运输协会代码，值是机场的名称：
```go
var airports: Dictionary<String, String> = ["TYO": "Tokyo", "DUB": "Dublin"]
```
这个`airports`字典是用这样一个类型表达式`Dictionary < String, String >`声明的,表示字典的键是`String`类型，其值也是`String`类型.
>提示
上面的`airports`字典是被声明为一个变量(用`var`关键字说明),而不是一个常量(用`let`关键字说明),因为后面示例里将要添加更多的机场进去。

该`airports`字典用含有两个键值对的字典字面常量初始化，其第一对的键是”TYO“，值是”Tokyo“,第二对的键是”DUB“，值是"Dublin"。
这个字典字面常量包含有两个`String：String`的键值对，而刚好可以匹配上`airports`字典变量的类型声明，所以这个字典字面常量可以用来给该字典变量赋值。
和数组相比，我们在用一个键值对之间数据类型一致的字典字面常量来初始化一个字典时，可以省略该字典的类型声明;这个`airports`字典的初始化操作可以用这样一个简写方法来代替：
```go
var airports = ["TYO": "Tokyo", "DUB": "Dublin"]
```
因为这个字典字面常量里所有的键都是相同的数据类型，所有的值也是相同的数据类型，Swift可以推断出类型`Dictionary < String, String >`是`airports`字典的正确类型。

####字典的访问和修改
我们有三个方式可以对字典进行存取，通过字典的方法，属性，或者下标运算符。和数组一样，我们可以通过她的只读属性`count`得到一个字典里所存放的键值对的个数.
```go
println("The dictionary of airports contains \(airports.count) items.")
// prints "The dictionary of airports contains 2 items.
```
我们也可以用下标运算符对一个字典添加一个新元素项，用一个合适数据类型的新键当作下标运算符的索引，然后给其赋值一个合适数据类型的新值:
```go
airports["LHR"] = "London"
// the airports dictionary now contains 3 items
```
当然，我们一样可以用下班运算符来修改更新一个已有的键对应的值：
```go
airports["LHR"] = "London Heathrow"
// the value for "LHR" has been changed to "London Heathrow”
```
此外，我们还可以用字典的`updateValue(forKey:)`方法来设置或者更新某个特定键对应的值，和下标运算符一样，这个方法它当键存在时就更新对应的值，当键不存在时就新增存储一个键值对。和下标运算符不同的是`updateValue(forKey:)`方法当执行了更新操作后就返回原来的旧值，这个使得我们可以判断该次操作是否发生了数据更新。

这个`updateValue(forKey:)`方法返回一个字典里值类型的optional值。对于一个存储`String`类型为值的字典，举个栗子，这个方法返回一个`String?`类型的值或者”optional `String`“,该optional值包含着更新操作之前这个键对应的旧值，或者值不存在的话就是`nil`：
```go
if let oldValue = airports.updateValue("Dublin International", forKey: "DUB") {
    println("The old value for DUB was \(oldValue).")
}
// prints "The old value for DUB was Dublin.
```
我们同样也可以用下标运算符来获取字典中一个特定键对应的值，因为可以请求一个其不对应任何存在值的键，字典的下标运算符返回一个该字典里值类型一个`optional`值，如果字典包含有请求的键对应的值的话，下标运算符返回该键对应值的一个`optonal`值，否则返回`nil`：
```go
if let airportName = airports["DUB"] {
    println("The name of the airport is \(airportName).")
} else {
    println("That airport is not in the airports dictionary.")
}
// prints "The name of the airport is Dublin International.”
```
通过下标运算符来移除数组中的一个键值对时，可以把特定的键对应的值赋一个`nil`就搞定妥妥的了：
```go
airports["APL"] = "Apple International"
// "Apple International" is not the real airport for APL, so delete it
airports["APL"] = nil
// APL has now been removed from the dictionary
```
此外，还可以用方法`removeValueForKey`来移除字典中的一个键值对，如果存在待移除的键值对，这个方法会一直该键值对并返回移除的那键值对中的值，否则，返回`nil`：
```go
if let removedValue = airports.removeValueForKey("DUB") {
    println("The removed airport's name is \(removedValue).")
} else {
    println("The airports dictionary does not contain a value for DUB.")
}
// prints "The removed airport's name is Dublin International."
```

####字典的遍历
我们可以用`for-in`循环遍历出字典里所有的键值对，字典里的每一项都用元组`(key, value)`形式返回，然后我们在循环里再把这个元组里的成员分解出来存到临时的常量或者变量里：
```go
for (airportCode, airportName) in airports {
    println("\(airportCode): \(airportName)")
}
// TYO: Tokyo
// LHR: London Heathrow
```
关于`for-in`快速枚举的更多详细信息，请参看`For Loops`章节.

我们也可以通过访问字典的`keys`和`values`属性来得到该字典的所有键的一个集合或者所有值的集合:
```go
for airportCode in airports.keys {
    println("Airport code: \(airportCode)")
}
// Airport code: TYO
// Airport code: LHR

for airportName in airports.values {
    println("Airport name: \(airportName)")
}
// Airport name: Tokyo
// Airport name: London Heathrow
```
如果我们需要把字典的键或者值应用到某个需要一个数组参数的API时，可以直接用字典的`keys`或`values`来初始化一个数组：
```bash
let airportCodes = Array(airports.keys)
// airportCodes is ["TYO", "LHR"]

let airportNames = Array(airports.values)
// airportNames is ["Tokyo", "London Heathrow"]
```
>提示
Swift的字典类型是一个无序的集合类型，遍历字典时从字典里得到的键，值，乃至键值对的顺序都是不确定的。

####创建一个空字典
和数组那样，我们也可以用初始化方法来创建一个确定类型的空字典:
```go
var namesOfIntegers = Dictionary<Int, String>()
// namesOfIntegers is an empty Dictionary<Int, String>
```
这个例子创建了一个`<Int, String>`类型的空字典来存整形值及其对应的可读性的名称，用`Int`作为键，用`String`做为值.

而如果代码上下文已经提供了类型信息，我们可以用一个空字典的字面常量（`[:]`）来建一个空字典：
```go
namesOfIntegers[16] = "sixteen"
// namesOfIntegers now contains 1 key-value pair
namesOfIntegers = [:]
// namesOfIntegers is once again an empty dictionary of type Int, String
```
>提示
在这个背后，Swift的数组和字典都是用通用集合类型实现的。更多关于通用类型与集合的知识，请参看`Generics`。

####集合的可变性
数组和字典都是在单个的集合里把多个值存储的在块，如果我们创建一个数组或者字典并赋值给一个变量，那我们所创建的集合就是可变的，也就是说我们可以修改集合的内容或改变集合的大小(增加或者删除元素项)；相反地，如果我们把数组或者字典赋值给一个常量，那么这个数组或字典就是不可变的，且其大小也不能改变。

对于字典来说，不可变性意味着我们也不能去替换字典中已有的键对应的值，一个不可变的字典的内容在他第一次初始化后就不能被改变。
而数组的不可变性却有细微的差别。对于不可变数组，我们确实不能对他执行任何有潜在可能改变改数组大小的操作，但是，我们可以对改数组中已存在的索引位置设置一个新的值。这个特性使得Swift的数组类型当大小是固定时可以提供一定的性能优化。

Swift数组类型的可变性行为也影响到数组实例是怎样被赋值和修改的。更多的信息，请参看`Assignment and Copy Behavior for Collection Types`.

>提示
任何时候，当我们的集合类型变量不需要被改变时，把他声明为不可变的集合是一个非常不错的编程习惯;这样一来，可以是的Swift的编译器可以为我们做优化并提高集合类型运算的性能。


** **
好吧,关于Swift语言的集合的学习，大茶哥只能帮你到这了。戴维营教育的伙计们，发奋图强吧。

戴维营教育网址：[http://www.diveinedu.com](http://www.diveinedu.com)
Swift视频教程网址：[http://www.ubuntucollege.cn](http://www.ubuntucollege.cn)
##学iOS开发，只选长沙戴维营教育！诚信教育！学生和企业都信赖的教育机构！http://www.diveinedu.com
##版权所有，严禁盗版，转载请说明并保留引用链接！