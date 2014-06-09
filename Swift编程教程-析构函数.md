# 析构函数

对象在释放的时候会调用**析构函数**。我们通过`deinit`关键字定义析构函数。只有类类型才能够定义析构函数。

### 析构函数如何工作
当对象不再被使用的时候，Swift自动释放它们，并且释放所占有的资源。Swfit使用`ARC`(自动引用计数)进行内存管理。除非使用了自己的资源，一般情况下都不需要手动进行清理。例如我们在一个自定义类中打开了一个文件，并往里面写入数据，在这个对象释放前需要关闭文件。

每个类最多只能有一个析构函数，并且不能接受任何参数。
```go
deinit {
	//执行析构
}
```

对象释放前会自动调用析构函数。我们不能直接调用析构函数。但是子类会继承父类的析构函数，并且在它自己的析构函数后自动调用父类的实现。

对象必须等析构函数返回后才会被释放，因此在析构函数里可以访问对象的所有属性。

### 析构函数实战
下面为一个简单的游戏定义了两个新的类型`Bank`和`Player`。`Bank`结构体负责管理现金，并且最多不能超过10000流通中的金币。游戏中只会有一个`Bank`，因此我们将它实现为一个结构体，并且用静态属性和方法来存储和管理它的状态：
```go
struct Bank {
    static var coinsInBank = 10_000
    static func vendCoins(var numberOfCoinsToVend: Int) -> Int {
        numberOfCoinsToVend = min(numberOfCoinsToVend, coinsInBank)
        coinsInBank -= numberOfCoinsToVend
        return numberOfCoinsToVend
    }
    static func receiveCoins(coins: Int) {
        coinsInBank += coins
    }
}
```

`Bank`通过`coinsInBank`属性保存当前金币数。它提供`vendCoins`和`receiveCoins`两个方法来分发和收集金币。

`vendCoins`在分发前会检查银行里是否有足够的金币。如果金币不够，`Bank`会返回少于所请求的金币数目（如果没有金币的话，则返回0）。`vendCoins`声明了一个`numberOfCoinsToVend`变量参数，这样在方法里面就可以之间修改它，而不需要声明一个新的变量。该方法返回银行实际提供的金币数目。

`receiveCoins`方法简单的将接收到的金币数加到总金币数上。

`Player`类描述游戏中的玩家。每个玩家都拥有一定数目的金币。我们用`coinsInPurse`属性表示玩家的金币数。
```go
class Player {
	var coinsInPurse: Int
    init(coins: Int) {
    	coinsInPurse = Bank.vendCoins(coins)
    }
    
    func winCoins(coins: Int) {
    	coinsInPurse += Bank.vendCoins(coins)
    }
    
    deinit {
    	Bank.receiveCoins(coinsInPurse)
    }
}
```

每个玩家都使用一个特定的金币值进行初始化，不过由于金币数目有限，有实际有可能不会分配这么多。`Player`类定义了一个`winCoins`方法，可以从银行获取一定的金币并将它放到自己的钱包里。`Player`类也实现了一个析构器，`Player`的对象释放时会调用它。析构器里只是简单的将所有金币放回银行：
```go
var playerOne: Player? = Player(coins: 100)
println("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
//prints "A new player has joined the game with 100 coins"
println("There are now \(Bank.coinsInBank) coins left in the bank")
//prints "There are now 9900 coins left in the bank"
```

上面创建了一个新的`Player`对象，并且请求分配100个金币。这个对象被存放在一个`Player`类型的选项（Optional）变量`playerOne`中。这里使用选项是因为玩家随时都可能离开游戏。选项对象让我们能够判断某个玩家当前是否在游戏中。

由于`playerOne`是一个选项类型，所以在访问它的`coinsInPurse`属性或者`winCoins`方法时，需要先解包。
```go
playerOne!.winCoins(2_000)
println("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
// prints "PlayerOne won 2000 coins & now has 2100 coins"
println("The bank now only has \(Bank.coinsInBank) coins left")
// prints "The bank now only has 7900 coins left"
```

现在玩家已经赢了2000个金币，它一共又2100个金币在钱包里。而银行里只有7900个金币了。
```go
playerOne = nil
println("PlayerOne has left the game")
// prints "PlayerOne has left the game"
println("The bank now has \(Bank.coinsInBank) coins")
// prints "The bank now has 10000 coins"
```

我们通过将选项`playerOne`设置为`nil`表示有一个玩家离开游戏。这时`playerOne`引用的`Player`对象将会被释放。在这个对象被释放前，它会自动调用析构函数，然后将金币归还银行。

戴维营教育网址：http://www.diveinedu.com
Swift视频教程网址：http://www.ubuntucollege.cn

学iOS开发，只选长沙戴维营教育！诚信教育！学生和企业都信赖的教育机构！http://www.diveinedu.com

版权所有，严禁盗版，转载请说明并保留引用链接！