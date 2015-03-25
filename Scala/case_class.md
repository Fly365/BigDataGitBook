#Case Class
**推广: 更快更好的翻墙神器 [红杏]( http://honx.in/i/VPZdDZnKEyd7byzB)**

---
我们先来看看StackOverFlow上的一个回答,

>Case classes can be seen as plain and immutable data-holding objects that should exclusively depend on their constructor arguments.
This functional concept allows us to

>- use a compact initialisation syntax (Node(1, Leaf(2), None)))
- decompose them using pattern matching
- have equality comparisons implicitly defined

而Scala官网对它的定义就简单得多，
>Case classes are regular classes which export their constructor parameters and which provide a recursive decomposition mechanism via pattern matching.

暴露构造器，提供递归分解机制。


最后：

>In combination with inheritance, case classes are used to mimic algebraic datatypes.

 [algebraic datatype](http://en.wikipedia.org/wiki/Algebraic_data_type)
 
这样，case class的定义就清楚了。

```
abstract class Term
case class Var(name: String) extends Term
case class Fun(arg: String, body: Term) extends Term
case class App(f: Term, v: Term) extends Term
```

构造器的每个参数都是public的，且编译器会根据字段自动生成equals方法和 tostring方法。

