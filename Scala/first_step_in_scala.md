#First Step in Scala
**推广: 更快更好的翻墙神器 [红杏]( http://honx.in/i/VPZdDZnKEyd7byzB)**

---

- **如果一个字面函数(function literal)仅包含接受一个参数的单个语句，你不需要显示的指出参数。**

```
args.foreach((arg: String) => println(arg))
```
等价

```
args.foreach(println)
```

- **for 里面的变量始终是val**

```
for (arg <- args)    println(arg)
```
不需要写 val arg。

- **数组(Array)通过索引在小括号访问，不是像Java那样的方括号。**

实际上，小括号的访问实际上调用了`apply`方法。


- **一个变量是val，该变量不能再被赋值，但改变量指向的对象可以被改变。**

```
val greetStrings: Array[String] = new Array[String](3)
greetStrings(0) = "Hello"greetStrings(1) = ", "greetStrings(2) = "world!\n"
```
虽然数组greetingStrings是个val，但是该数组包含的元素是可以被赋值的。

- **for (i <- 0 to 2)**

如何理解上面的代码？
0 to 2 表示 (0).to(2)。to 是方法。
(If a method takes only one parameter, you can call it without a dot or parentheses)如果一个方法只有一个参数，你可以忽略点和括号。但是你得指出谁在调用，比如 `println 10`是错误的，`Console println 10`是正确的。

- **greetStrings(0) = "Hello"**

给一个带括号的变量赋值，调用的实际上是`update`方法。

```
greetStrings.update(0, "Hello")
```

- **val numNames = Array("zero", "one", "two")**

创建并初始化一个数组，你实际上调用的是一个工厂方法，`apply`。

```
val numNames2 = Array.apply("zero", "one", "two")
```
如果你是Java程序员，你可以把它想象成一个静态方法。

- **List**

尽管你不能在初始化后改变一个Array的长度，但你可以改变它的元素的值，所以，数组是可变的(mutable)。

但，List(scala.List)是不可变的(immutable)。

注意，Java的List是可变的。

```
val oneTwoThree = List(1, 2, 3)
```
这样就创建来一个 List。类似，这个也是调用了工厂方法`apply`。
API神马就不说了。

声明一个空List的方法是 Nil。或者`List()`。

```
val oneTwoThree = 1 :: 2 :: 3 :: Nil
```
另外，我们是`元素::List`。


- **Tuple**

元组(tuple)也是不可变的。

一旦初始化，你可以使用 `点，下划线，从1开始的索引`访问。

```
val pair = (99, "Luftballons")println(pair._1)println(pair._2)
```  
元组的真实类型，取决于元组的个数和包含元素的类型。比如，

(99, "Luftballons") 是 Tuple2[Int, String]。 ('u', 'r', "the", 1, 4, "me") 是 Tuple6[Char, Char, String, Int, Int, String]。

注意：Although conceptually you could create tuples of any length, currently the Scala library only defines them up to Tuple22.

你可能在想，为什么不能使用括号直接访问？因为像List，`apply`返回的类型是一样的，而tuple包含的类型不一定一样。

- **Try**类型

Type是种类型，表示可能会抛出异常的计算，也可能返回正确的计算值。和`Either`相似。


参考[Option, Either, Try](http://my.oschina.net/yangbajing/blog/108652)

Try[T]的实例要么是 `scala.util.Success[T]`，要么是`scala.util.Failure[T]`。

```
import scala.util.{Try, Success, Failure}

def divide: Try[Int] = {
  val dividend = Try(Console.readLine("Enter an Int that you'd like to divide:\n").toInt)
  val divisor = Try(Console.readLine("Enter an Int that you'd like to divide by:\n").toInt)
  val problem = dividend.flatMap(x => divisor.map(y => x/y))
  problem match {
    case Success(v) =>
      println("Result of " + dividend.get + "/"+ divisor.get +" is: " + v)
      Success(v)
    case Failure(e) =>
      println("You must've divided by zero or entered something that's not an Int. Try again!")
      println("Info from the exception: " + e.getMessage)
      divide
  }
}
```

- **Hash, Set**

Scala里面Hash和Set同时提供了immutable版本和mutable版本。

默认都是immutable的。

```
var jetSet = Set("Boeing", "Airbus")
jetSet += "Lear"
```

immutable版本的Set是没有+=方法的，所以上面其实是:

```
jetSet = jetSet + "Lear"
```
的缩写。因为再次赋值了，所以必须是var。

但是mutable版本里面是有+=和+方法的。