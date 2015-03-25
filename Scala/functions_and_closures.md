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
def processFile(filename: String, width: Int) {



```

##First-class functions
你不仅可以定义和调用方法，也可以写成匿名的字面方法，并作为值传递。

字面方法(function literal)会编译到类里，并在运行时被初始化时是一个方法值(function value)。也就是一个在代码里，而后者在运行时是一个对象。

**方法值**是对象，所以你可以存储在一个变量里。

```
scala> var increase = (x: Int) => x + 1






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

