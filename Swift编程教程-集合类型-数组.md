# <center>苹果Swift语言官方文档（中文翻译版）</center>

本文档由[长沙戴维营教育](http://www.diveinedu.cn)组织翻译和校对，由于英语水平有限，请大家指正。

长沙戴维营教育还为本教程录制了配套的视频教程在[乌班图学院](http://www.ubuntucollege.cn)上免费提供，欢迎大家一起学习。

下面的章节，如果为蓝色链接表示以及翻译完毕，可以查看，如果为黑色则表示正在紧张的翻译中。本文档中文版每天都会更新，大家可以随时查看。如由Bug欢迎改正。

- - -

#集合类型--数组

Swift语言提供了两种集合数据类型：数组和字典，数组是一系列相同类型的变量值有序存储；字典是一系列相同类型的变量值的无序存储，可以通过一个唯一标识符（键）来引用和查找。

数组和字典总是能识别自己所能够存储的键值的类型，就是说，我们不能够往这两个容器内插入错误的类型。显式类型声明的集合能确保我们的代码总是能识别其所能存储的变量类型，并能够让我们在实际编码中捕捉到出现的任何类型错误。

>提示
Swift的数组类型的变量在常量，变量赋值或者给函数和方法传参的时候会表现出和其他类型不一样的行为。

##数组
数组在一个有序列表中存储相同类型的多个值，而且相同值可以在不同位置重复存储。
Swift数组不和Objective-C的数组那样可以存储任意Objective-C的对象，她是存贮指定类型的值；他的类型信息是非常明确的，不管显式类型标记或者隐式类型来推断，他总是有确定的一个类型，而且也没有必定是一个类对象。比如你在一个Int类型的数组里，你能且只能往里面插入Int类型的变量，其他任何值都不行。故Swift的数组是类型安全的！

####数组类型简写语法
Swift数组类型的一般书写的完整形式是Array < SomeType >，其中SomeType为该数组所允许存储元素的数据类型，我们也可以用这样的简写方式：SomeType[]。这两种形式的写法本质上是完全相同的，但是我们更喜欢使用简写的方式，并在后续的教程里都用这个形式,大茶也相信读者们肯定也喜欢简写方式的。

####数组字面常量
我们可以用一个数组字面常量来初始化一个数组变量。数组的字面常量书写方式如下：

[`value 1`, `value 2`, `value 3`, `value 1`]

下面的例子创建一个叫做shoppingList的数组来存储 String 类型值：
```go
var shoppingList: String[] = ["Eggs", "Milk"]
// 把shoppingList初始化为两个初始数据元素

```

这个shoppingList变量通过String[]形式被声明为一个String类型值的数组，因为这个特定的数组被指定了值类型String，所以我们只能用这个数组类存储String值，这里我们存储了两个字面常量String值（“Eggs”和“Milk”）。
>提示
这个shoppingList是声明为变量(var说明符)而不是声明为常量(let说明符)，因为后面的例子里将有更多的元素被加入这个数组.

这里，这个字面常量数组只包含了2个String值，他能匹配shoppingList数组的类型声明，所以可以用他来给shoppingList变量赋值初始化。
得益于Swift的类型推断机制，我们在用数组字面常量来初始化一个数组时不需要明确指定他的类型，用如下这个很方便的方式：

```go
var shoppingList = ["Eggs", "Milk"]

```
因为这个字面常量数组里的都是相同类型值，Swift编译器可以推断出该数组变量正确的数据类型是String[]

####数组元素的存取
我们可以通过数组的方法或者属性或者下标运算符来对一个数组进行访问和修改。

要得到一个数组的元素的个数，我们可以用他的只读属性`count`来获取。
```go
println("Tht shopping list contains \(shoppingList.count) items.")
// prints "The shopping list contains 2 items."
```
数组有一个叫做isEmpty的属性来表示该数组是否为空，即`count`属性等于 0：
```go
if shoppingList.isEmpty {
	println("The shopping list is empty.")
} else {
	println("Tht shopping list is not empty.")
}
// prints "The shopping list is not empty."
```

我们可以调用数组的`append`方法在数组的末尾追加一个新的元素：
```go
shoppingList.append("Flour")
// shoppingList now contains 3 items, and someone is making pancakes
```
或者更简单的达到这一目的，可以用符合运算符（+＝）：
```go
shoppingList += "Baking Powder"
//shoppingList now contains 4 items
```
我们还有可以直接给一个数组加上另一个类型一致的数组：
```go
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items
```
Swift数组的下标运算和其他语言类似，下标都是从 0 开始，我们可以直接用下标来存取数组里面的元素; 此外，Swift的数组新增了一个功能就是可以通过下标来进行区间赋值了
```go
var firstItem = shoppingList[0]
// firstItem is equal to "Eggs"

shoppingList[0] = "Six eggs"
// the first item in the list is now equal to "Six eggs" rather than "Eggs"

```
>提示
我们不能用下标的形式来给一个数组追加一个或者多个新的元素。如果下标越界将会触发一个运行时的错误，所以我们在用下标进行运算的时候时刻要记得去检测我们所用的下标是否越界，可以通过比较我们的下标`index`是否大于最大的有效的下标值（`count - 1`）


数组的插入和删除
当我们需要在数组的某个地方插入一个新的元素的时候，可以调用数组的方法`insert(atIndex:)`;
```go
shoppingList.insert("Maple Syrup", atIndex: 0)
// shoppingList now contains 7 items
// "Maple Syrup" is now the first item in the list
```
当我们需要在数组的某个地方移除一个既有元素的时候，可以调用数组的方法`removeAtIndex`,该方法的返回值是被移除掉的元素;
```go
let mapleSyrup = shoppingList.removeAtIndex(0)
// the item that was at index 0 has just been removed
// shoppingList now contains 6 items, and no Maple Syrup
// the mapleSyrup constant is now equal to the removed "Maple Syrup" string
```
当特殊情况我们需要移除数组的最后一个元素的时候，我们应该避免使用`removeAtIndex`方法，而直接使用简便方法`removeLast`来直接移除数组的最后一个元素，`removeLast`方法也是返回被移除的元素。
```go
let apples = shoppingList.removeLast()
// the last item in the array has just been removed
// shoppingList now contains 5 items, and no cheese
// the apples constant is now equal to the removed "Apples" string
```

####数组元素的遍历
在Swift语言里，我们可以用快速枚举(`for-in`)的方式来遍历整个数组的元素:
```go
for item in shoppingList {
    println(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```
如果在快速枚举的循环体内部需要获取每个元素所对应的序号，我们可以嵌套一个全局方法`enumerate`来代替数组遍历，该方法为数组里的每一项返回一个封装了序号与元素值的元组类型变量，我们可以在快速枚举遍历过程中把该元组里的值提取到合适的临时变量或者常量中：
```go
for (index, value) in enumerate(shoppingList) {
    println("Item \(index + 1): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

####数组创建与初始化
我们可以用如下的语法来初始化一个确定类型的空的数组（没有设置任何初始值）:
```go
var someInts = Int[]()
println("someInts is of type Int[] with \(someInts.count) items.")
// prints "someInts is of type Int[] with 0 items.
```
变量someInts的类型会被推断为Int[]，因为他被赋值为一个Int[]类型的初始化方法的结果。
如果程序的上下文已经提供了其类型信息，比如一个函数的参数，已经申明了类型的变量或者常量，这样你可以用一个空数组的字面常量去给其赋值一个空的数组（这样写[]）:
```go
someInts.append(3)
// someInts now contains 1 value of type Int
someInts = []
// someInts is now an empty array, but is still of type Int[]
```

Swift的数组同样也提供了一个创建指定大小并指定元素默认值的初始化方法，我们只需给初始化方法的参数传递元素个数（`count`）以及对应默认类型值（`repeatedValue`）:
```go
var threeDoubles = Double[](count: 3, repeatedValue: 0.0)
// threeDoubles is of type Double[], and equals [0.0, 0.0, 0.0]
```
同样得益于Swift语言的类型推断，我们在用这个初始化方法的时候也可以不需要指定数组里面所存储元素类型，他会自动去判断那个提供的默认值的类型：
```go
var anotherThreeDoubles = Array(count: 3, repeatedValue: 2.5)
// anotherThreeDoubles is inferred as Double[], and equals [2.5, 2.5, 2.5]
```

最后，我们数组也可以像字符串那样，可以把两个已有的类型一致的数组相加得到一个新的数组，新数组的类型由两个相加的数组的类型推断而来：
```go
var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles is inferred as Double[], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```


好吧,关于Swift语言的数组的学习，大茶哥只能帮你到这了。戴维营教育的伙计们，发奋图强吧。

戴维营教育网址：[http://www.diveinedu.com](http://www.diveinedu.com)
Swift视频教程网址：[http://www.ubuntucollege.cn](http://www.ubuntucollege.cn)
##学iOS开发，只选长沙戴维营教育！诚信教育！学生和企业都信赖的教育机构！http://www.diveinedu.com
##版权所有，严禁盗版，转载请说明并保留引用链接！