#matching

关于匹配(matching)，应该是Scala最有趣的语言特性之一。

##模式匹配
模式匹配在良好的Scala的代码里无处不在。

我们看一个例子，使用模式匹配实现类型转换，

```
obj match {
   case str: String => ...
   case addr: SocketAddress => ...
```

那到底什么是模式(pattern)呢？这应该是个数据结构上的概念，描述一个结构的组成。

```
// 匹配一个数组，它由三个元素组成，第一个元素为1，第二个元素为2，第三个元素为3
scala> Array(1,2,3) match { case Array(1,2,3) => println("ok")}
ok

// 匹配一个数组，它至少由一个元素组成，第一个元素为1
scala> Array(1,2,3) match { case Array(1,_*) => println("ok")}
ok

// 匹配一个List，它由三个元素组成，第一个元素为“A"，第二个元素任意类型，第三个元素为"C"
scala> List("A","B","C") match{ case List("A",_,"C") => println("ok") }
ok
```

而Scala的文档提到，
>Scala’s pattern matching statement is most useful for matching on algebraic types expressed via case classes. Scala also allows the definition of patterns independently of case classes, using unapply methods in extractor objects.

这里出现几个概念,`algebraic types`(代数类型),`case class`(样本类),`extractor`(提取器)。


代数类型有时也称为`ADT(algebraic data type)`是很多函数式语言中支持的。
>通过提供若干个含有独立构造器的备选项(alternative)来定义的类型。通常可以辅助于通过模式匹配解构类型的方式。这个概念可以在规约语言和函数式语言中发现。代数数据类型在Scala中可以用样本类(case class)模拟。

下面对常规的几种匹配给出例子，

- 常量匹配

```
scala> val site = "alibaba.com"
scala> site match { case "alibaba.com" => println("ok") }
scala> val ALIBABA="alibaba.com"
//注意这里常量必须以大写字母开头
scala> def foo(s:String) { s match { case ALIBABA => println("ok") } } 
```

- 变量匹配

单纯的变量模式没有匹配判断的过程，只是把传入的对象给起了一个新的变量名。

```
scala> site match { case whateverName => println(whateverName) }
```
上面把要匹配的 site对象用 whateverName 变量名代替，所以它总会匹配成功。不过这里有个约定，对于变量，要求必须是以小写字母开头，否则会把它对待成一个常量变量，比如上面的whateverName 如果写成WhateverName就会去找这个WhateverName的变量，如果找到则比较相等性，找不到则出错。

变量模式通常不会单独使用，而是在多种模式组合时使用，比如

```
List(1,2) match{ case List(x,2) => println(x) }
```

- 通配符匹配

通配符用下划线表示："_" ，可以理解成一个特殊的变量或占位符。
单纯的通配符模式通常在模式匹配的最后一行出现，case _ => 它可以匹配任何对象，用于处理所有其它匹配不成功的情况。
通配符模式也常和其他模式组合使用：

```
scala> List(1,2,3) match{ case List(_,_,3) => println("ok") }
```

- 构造器匹配

这就是matching的最大用处。这也就是 case class的用法。将在下一篇文章介绍。

- 类型匹配

```
scala> def foo(a:Any) = a match { 
            case a :List[String] => println("ok"); 
            case _ => 
        } 
```
要注意匹配泛型也会擦除类型。所以上面的 List[String] 里的String运行时并不能检测
foo(List("A")) 和 foo(List(2)) 都可以匹配成功。实际上上面的语句编译时就会给出警告，但并不出错。

对于类型匹配，
查看[How to pattern match on generic type in Scala?](http://stackoverflow.com/questions/16056645/how-to-pattern-match-on-generic-type-in-scala/16057002#16057002)。


- 变量绑定模式

也就是把匹配的值重新命名。（只用在case class里面）

```
case c@Calculator(_, _) => "Calculator: %s of unknown type".format(c)
```
当然也可以对构造器的参数重新命名，使用@。


