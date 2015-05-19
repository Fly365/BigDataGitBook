#Introduction of Actors and Concurrency
**推广: 更快更好的翻墙神器 [红杏]( http://honx.in/i/VPZdDZnKEyd7byzB)**

---
在Scala可以继续使用Java的threads，但更推荐使用Actor模型。 Scala 2.10.0之后，采用了来自Typesafe的**Akka actor library**作为官方的actor包。

Actor除了带来并行编程的高级抽象（更高级往往意味着更易用），还有如下好处：

- 轻量级，事件驱动
- 容错
- 位置透明。因为Akka可以使用多个JVM和服务器，可以通过纯消息传递在分布式环境下工作。


## Actor Model

首先得理解`actor`的含义：

- 一个actor是构建基于actor系统的最小单元，就像面向对象里的对象
- 类似对象， actor封装了状态和行为
- 你不能到actor内部获取其状态，但是可以给actor发送消息来请求状态信息
- actor有个邮箱，其目的是在生命周期内处理消息
- 通过发送不可变的消息给actor来与之通信，这些消息会进入actor的邮箱
- 接受消息，就像取出邮件。

一般actor会形成一个家族。`Typesafe`小组建议我们把actor设想成一个人，该人在一个组织里。在开发actor系统时，最佳的实践方案是代理。