# <center>苹果Swift语言官方文档（中文翻译版）</center>

本文档由[长沙戴维营教育](http://www.diveinedu.cn)组织翻译和校对，由于英语水平有限，请大家指正。

长沙戴维营教育还为本教程录制了配套的视频教程在[乌班图学院](http://www.ubuntucollege.cn)上免费提供，欢迎大家一起学习。

下面的章节，如果为蓝色链接表示以及翻译完毕，可以查看，如果为黑色则表示正在紧张的翻译中。本文档中文版每天都会更新，大家可以随时查看。如由Bug欢迎改正。

- - -

# <center>苹果Swift语言官方文档（中文翻译版）</center>

本文档由[长沙戴维营教育](http://www.diveinedu.cn)组织翻译和校对，由于英语水平有限，请大家指正。

长沙戴维营教育还为本教程录制了配套的视频教程在[乌班图学院](http://www.ubuntucollege.cn)上免费提供，欢迎大家一起学习。

下面的章节，如果为蓝色链接表示以及翻译完毕，可以查看，如果为黑色则表示正在紧张的翻译中。本文档中文版每天都会更新，大家可以随时查看。如由Bug欢迎改正。

- - -

# Swift与Objective-C API交互

Swift和Objective-C可以进行互操作，也就是说可以在Objective-C项目中使用Swift代码，反过来也可以。当然，这种互操作之间最重要的是可以在Swift中调用Objective-C的接口，毕竟目前绝大部分接口都是通过Objective-C提供的。

### 初始化
在Swift中实例化一个Objective-C的类时，可以用Swift语法调用它的一个初始化器。Objective-C的初始化方法被分割成关键字。例如“initWith”变成“init”，而剩下的部分作为方法的参数名。

Objective-C的代码：
```objectivec
UITableView *myTableView = [[UITableView alloc] initWithFrame: CGRectZero style: UITableViewStyleGrouped];
```

对应的Swift代码为：
```ocaml
let myTableView: UITableView = UITableView(frame: CGRectZero, style: .Grouped)
```

在Swift中不需要调用`alloc`方法，它会自动处理对象的创建功能。注意：Swift不会显式的调用`init`方法。

定义变量或者常量的时候，可以省略它的类型，Swift会自动识别。
```ocaml
let myTextField = UITextField(frame: CGRect(0.0, 0.0, 200.0, 40.0))
```

为了方便起见，Objective-C的工厂方法被映射为Swift中的初始化器。例如：
```objectivec
UIColor *color = [UIColor colorWithRed: 0.5 green: 0.0 blue: 0.5 alpha: 1.0];
```
转换为Swift：
```ocaml
let color = UIColor(red: 0.5, green: 0.0, blue: 0.5, alpha: 1.0)
```

### 属性访问
在Objective-C和Swift中访问属性都是使用点操作符。
```ocaml
myTextField.textColor = UIColor.darkGrayColor()
myTextField.text = "Hello world"
if myTextField.editing {
	myTextField.editing = false
}
```

访问和设置属性的时候不需要使用圆括号，上面`darkGrayColor`之所以有括号，是因为调用的是`UIColor`的类方法，而不是属性。

Objective-C中不需要参数并且有一个返回值的方法，可以被当作隐含的获取方法（`getter`），从而使用点操作符。但是在Swift中点操作符只能访问Objective-C中使用`@property`定义的属性。

### 方法调用
在Swift中使用点操作符调用Objective-C中的方法。

Objective-C的方法在Swift中调用的时候，它的第一部分成为Swift的基本方法出现在括号之前。然后函数的第一个参数没有名字，剩下的部分作为Swift函数对应的参数名称。
Objective-C语法：
```objectivec
[myTableView insertSubview: mySubview atIndex: 2];
```

Swift代码：
```ocaml
myTableView.insertSubview(mySubview atIndex: 2)
```

调用无参的方法：
```ocaml
myTableView.layoutIfNeeded()
```

### 兼容`id`类型
Swift包含一个叫`AnyObject`的协议，与Objective-C中的`id`类似，可以表示任意类型的对象。`AnyObject`协议允许你在利用无类型对象的灵活性的同时保持类型安全。
你可以给`AnyObject`类型的变量赋任意的值：
```ocaml
var myObject: AnyObject = NSData()
```

可以直接方法`AnyObject`类型对象的任意属性和方法，而不需要进行强制类型转换。
```ocaml
let dateDescription = myObject.description
let timeSinceNow = myObject.timeIntervalSinceNow
```

由于`AnyObject`类型的变量的类型需要到运行时才能确定，因此可能会导致不安全的代码。特别是你可以访问一个不存在的属性或者方法，它只是在运行时报错。
```ocaml
myObject.characterAtIndex(5)
//crash, myObject doesn't respond to that method
```

在进行类型转换的时候，不一定转换成功，因此Swift返回的是一个Optional值。你可以检查是否转换成功。
```ocaml
let userDefaults = NSUserDefaults.standardUserDefaults()
let lastRefreshDate: AnyObject? = userDefault.objectForKey("LastRefreshDate")
if let date = lastRefreshDate as? NSDate {
	println("\(date.timeIntervalSinceReferenceDate)")
}
```

如果你能够确定对象的类型，并且不为`nil`，可以使用`as`操作符进行强制转换。
```ocaml
let myDate = lastRefreshDate as NSDate
let timeInterval = myDate.timeIntervalSinceReferenceDate
```

### nil对象
Objective-C中使用`nil`来表示引用一个空对象(null)。Swift中所有的值都不会为`nil`。如果需要表示一个缺失的值，可以使用Optional。

由于Objective-C不能确保所有值都非空，因此Swift将Objective-C中引入的方法的参数和返回值都用Optional表示。在使用Objective-C对象之前，应该检查它们是否存在。

### 扩展
Swift的扩展与Objective-C的类别有点类似。扩展能够为已有类、结构体、枚举等增加行为。

下面是给`UIBezierPath`添加扩展：
```ocaml
extension UIBezierPath {
	class func bezierPathWithTriangle(length: Float, origin: CGPoint) -> UIBezierPath {
    	let squareRoot = Float(sqrt(3))
        let altitude = (squareRoot * length) / 2
        let myPath = UIBezierPath()
        
        myPath.moveToPoint(orgin)
        myPath.addLineToPoint(CGPoint(length, origin.x))
        myPath.addLineToPoint(CGPoint(length / 2, altitude))
        myPath.closePath()
        
        return myPath
    }
}
```

可以使用扩展增加属性（包括类属性或静态属性）。不过这些属性只能是通过计算得来，而不能进行存储。下面给`CGRect`添加一个`area`属性：
```ocaml
extension CGRect {
	var area: CGFloat {
    	return width * height
    }
}

let rect = CGRect(x: 0.0, y: 0.0, width: 10.0, height: 50.0)
let area = rect.area
//area: CGFloat = 500.0
```

使用扩展可以在不创建子类的情况下让现有的类响应某个协议。需要注意的是，扩展不能覆盖已有的方法和属性。

### 闭包
Objective-C中的Block 被自动导入为Swift的闭包。例如：
```objectivec
void (^completionBlock)(NSData *, NSError *) = ^(NSData *data, NSError *error) {
	/* ... */
}
```

在Swift 中对应为：
```ocaml
let completionBlock: (NSData, NSError) -> void = {
	data, error in /* ... */
}
```

Swift的闭包和Objective-C中的Block是兼容的，可以在Swift中给Objective-C的方法传递闭包来代替Block对象。

闭包和Block对象有一点不同，里面的变量是可变的，也就是说与`__block`修饰的变量行为相同。

### 对象比较
Swift中有两种对象比较的方式。第一种是**相等（`==`）**（equality），用来比较两个对象的内容是否相同。第二种是**恒等（`===`）**(identity)，比较两个变量或者常量是否引用同一个对象。

`NSObject`只能比较是否引用了同一个对象（恒等），如果要比较内容是否相同，应该实现`isEqual:`方法。

### Swift类型兼容性
定义一个继承自`NSObject`或者其他Objective-C的类，它自动与Objective-C兼容。如果你不需要将Swift对象导入Objective-C代码的话，没必要关注类型的兼容性。但是如果在Swift中定义的类不是Objective-C类的子类，在Objective-C中使用的时候，需要用`@objc`进行说明。

`@objc`使得Swift的API可以在Objective-C和它的运行时中使用。当使用`@IBOutlet`、`@IBAction`或者`@NSManaged`等属性时，自动添加`@objc`属性。

`@objc`还可以用来指定Swift中的属性或方法在Objective-C中的名字，比如Swift支持Unicode名字，包括使用中文等Objective-C不兼容的字符。还有给Swift中定义的函数指定一个Selectorde名字。
```ocaml
@objc(Squirrel)
class 长沙戴维营教育 {
    @objc(hideNuts:inTree:)
    func 欢迎光临(Int, 姓名: String) {
    	/* ... */
    }
}
```

当`@objc(<#name#>)`属性作用在Swift的类上时，这个类在Objective-C的使用不受命名空间的限制。同样，在Swift中解归档Objective-C归档的对象时，由于归档对象中存放有类名，因此需要在Swift中用`@objc<#name>`说明Objective-C的类名。

### Objective-C选择器（Selector）
Objective-C的选择器是方法的一个引用。在Swift中对应的是`Selector`结构体。使用字符串字面量可以构建一个选择器对象，如`let mySelector: Selector = "tappedButton:"`。由于字符串字面常量可以自动转换为选择器对象，因此可以在任何需要传递选择器的地方使用字符串字面常量。

```ocaml
import UIKit
class MyViewController: UIViewController {
	let myButton = UIButton(frame: CGRect(x: 0, y: 0, width: 100, height: 50))
    
    init(nibName nibNameOrNil: String!, bundle nibBundleOrNil: NSBundle!)
    {
    	super.init(nibName: nibName, bundle: nibBundle)
        myButton.targetForAction("tappedButton:", withSender: self)
    }
    
    func tappedButton(sender: UIButton!) {
    	println("tapped button")
    }
}
```

> **提示**
> `performSelector:`以及相关的调用选择器的方法没有被引入到Swfit中来，因为它们不是完全安全的。

如果Swift类继承自Objective-C的类，则它里面的方法和属性都能够作为Objective-C的选择器使用。而如果不是Objective-C的子类，需要使用`@objc`属性修饰，这个在前面的**Swift类型兼容性**中有描述。