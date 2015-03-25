#Implicit Parameters
**推广: 更快更好的翻墙神器 [红杏]( http://honx.in/i/VPZdDZnKEyd7byzB)**

---
一个有隐式参数的方法，如果你给它传递参数，那它就没有什么不同；但是如果你没有给implicit parameter传递参数，这些参数将会被自动提供。

有资格传递给implicit paremeter的参数分为两类：

- 该标识符X能在函数调用里被访问，没有前缀，标记了implicit （没有前缀指的是：上下文中找到并绑定到同名的变量）。
- 伴生模块Object的所有成员被标记了 implicit。

```
abstract class SemiGroup[A] {
  def add(x: A, y: A): A
}
abstract class Monoid[A] extends SemiGroup[A] {
  def unit: A
}
object ImplicitTest extends App {
  implicit object StringMonoid extends Monoid[String] {
    def add(x: String, y: String): String = x concat y
    def unit: String = ""
  }
  implicit object IntMonoid extends Monoid[Int] {
    def add(x: Int, y: Int): Int = x + y
    def unit: Int = 0
  }
  def sum[A](xs: List[A])(implicit m: Monoid[A]): A =
    if (xs.isEmpty) m.unit
    else m.add(xs.head, sum(xs.tail))
  println(sum(List(1, 2, 3)))
  println(sum(List("a", "b", "c")))
}
```

输出是，

```
6
abc
```

上面是满足第二点。

```
def multiplier(i: Int)(implicit factor: Int) {  
    println(i * factor)  
}  
implicit val factor = 2  
multiplier(2)  
multiplier(2)(3)  
```
这是满足第一点。

