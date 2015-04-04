#Functions and Closures
**推广: 更快更好的翻墙神器 [红杏]( http://honx.in/i/VPZdDZnKEyd7byzB)**

---
本文重要设计Scala的函数和闭包。

##Methods
**方法，只是函数一种特殊形式**。
以对象的成员的方式存在的函数(fuction)就是方法(method)。这个和你在常规面向对象里没有差异。

##Local functions
过去我们把一个大函数分成小函数实现的想法是很好的；但这会带来一个问题，就是污染程序的命名空间。
Scala里面，方法和变量有相同地位，所以你可以很方便的定义局部方法。

```
def processFile(filename: String, width: Int) {          def processLine(filename: String,              width: Int, line: String) {            if (line.length > width)              println(filename +": "+ line)}          val source = Source.fromFile(filename)          for (line <- source.getLines()) {            processLine(filename, width, line)          }}
```
上面的代码还可以简化，因为**局部方法可以访问外围方法的参数**，
```def processLine(line: String) {              if (line.length > width)                println(filename +": "+ line)            }
```

##First-class functions
你不仅可以定义和调用方法，也可以写成匿名的字面方法，并作为值传递。

字面方法(function literal)会编译到类里，并在运行时被初始化时是一个方法值(function value)。也就是一个在代码里，而后者在运行时是一个对象。

**方法值**是对象，所以你可以存储在一个变量里。

```
scala> var increase = (x: Int) => x + 1increase: (Int) => Int = <function1>scala> increase(10)res0: Int = 11```
补充：**到底什么是方法值呢**？
```Every function value is an instance of some class that extends one of several FunctionN traits in package scala, such as Function0 for functions with no parameters, Function1 for functions with one parameter, and so on. Each FunctionN trait has an apply method used to invoke the function.```
当然，如果自字面方法有多条语句，需要｛｝，来形成一个块。
##Short forms of function literal
- 省略参数的类型
```
someNumbers.filter((x) => x > 0)
```

- 类型可确定的单个参数可省略括号

```
someNumbers.filter(x => x > 0)
```

## Placeholder syntax
占位符语法。

为了使语法更简单，你可以使用下划线为一个或多个参数做占位符，只要这些参数只在这个字面方法里出现一次，且仅一次。

```
someNumbers.filter(_ > 0)
```

## Partially applied function
虽然前面的例子展示了"下划线代替单个参数"，你也可以**使用一个下划线代替整个的参数列表**。

比如，

	someNumber.foreach(println _)
	
等价于,

	someNumber.foreach(x => println(x))
	
这里的下划线相当于整个参数列表。

这样使用下划线就是使用了**partially applied function**。

部分应用的意思就是：不提供函数所需要的全部参数，你只提供部分、甚至不提供参数。

	def sum(a: Int, b: Int, c: Int) = a + b +c
	
现在如果你不想提供任何参数，

	val a = sum _
	a(1, 2, 3)
	
这个过程的内幕是：变量(这里就是a)指向了一个函数值对象。这个函数值是用Scala编译器从sum _自动生成的类的实例。这个由编译器生成的类有一个接受3个参数的apply方法。因此，a(1, 2, 3)就是

	 a.apply(1, 2, 3)
	 
再比如,

	scala> val b = sum(1, _:Int, 3)
	b:(Int) => Int = <function1>


## Clousres
目前，我们看到的函数字面仅引用了传递来的参数。但，你也可以引用定义在其他地方的变量：

	(x: Int) => x + more
	
这个more是个自由变量(free variable)，x是一个限定变量(bound variable)。

闭包名字的来历是：the act of "cloing" the function literal by "capturing" the bindings of its free variables.

一个不包含自由变量的函数字面，比如 (x:Int) => x + 1，叫 closed term。
而包含自由变量的，叫 open term。

所以 (x: Int) => x + 1严格意义上不是闭包，因为它已经是一个closed的。

闭包 ＝> close the open term !!

如果你在闭包创建后，又去改变自由变量的值，闭包是能感知这个变化。

所以， Scala的闭包捕获的是变量本身，不是变量引用的值。相反也是成立的：闭包内的改变在闭包外部也是可见的。

	    scala> val someNumbers = List(-11, -10, -5, 0, 5, 10)        someNumbers: List[Int] = List(-11, -10, -5, 0, 5, 10)        scala> var sum = 0        sum: Int = 0        scala> someNumbers.foreach(sum +=  _)
sum现在就是-11 。


