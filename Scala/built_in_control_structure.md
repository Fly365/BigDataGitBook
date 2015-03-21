#Built-in Control Structure
**推广: 更快更好的翻墙神器 [红杏]( http://honx.in/i/VPZdDZnKEyd7byzB)**

---

Scala仅有少量的流程控制结构。这些仅有的控制结构是`if , while, for, try, match`和`函数调用`。Scala流程控制结构很少的原因是它一开始就包含了字面函数（function literals）。

需要注意的是，**Scala大多数的控制结构都会返回一个值**。这是函数式编程语言采用的方法，即程序的部件也是用来计算某个值的。你可以看作这个命令式语言(imperative language)已经出现的一种趋势。在命令式语言中，函数调用可以返回一个值，即使被调用函数更新被做参数的输出变量也能正常工作。另外，命令式语言常有三元运算符(?:)，在Scala里面，是通过if实现。也就是说，Scala的if返回一个值。

程序员可以利用这样的返回值简化代码，否则你需要使用很多的临时变量。


##if

常规的写法是，

```
var filename = "default.txt"
```
但是，if能够返回值，所以可以简写：

```
 val filename =






hi
```

因为函数体没有以“＝”开始，所以它是Unit类型的，它和()相等。

还有一个结构也会导致返回Unit类型，就是给var变量赋值。
下面是Java/C++的常见写法，但在Scala里面有问题：

```
var line = ""


