# Actor初识
**推广: 更快更好的翻墙神器 [红杏]( http://honx.in/i/VPZdDZnKEyd7byzB)**

---
虽然`Akka`虽然是Scala发行版的标准，但你仍需要加入其依赖。
如：

	libraryDependencies += "com.typesafe.akka" % "akka-actor_2.11" % "2.3.11"
	
看看下面的程序：

```
class HelloActor extends Actor{
    def receive = {
      case "hello" => println("hello back at you")
      case _ => println("huh?")
    }
}

object Mian extends App{

  val system = ActorSystem("helloSystem")
  val helloActor = system.actorOf(Props[HelloActor], name = "helloactor")
  helloActor ! "hello"
  helloActor ! "hey"

  system.shutdown
}
```

- 需要一个`ActorSystem`。
- （Actor是等级阶层的），可以在`ActorSystem`这层创建，也可以在其他actors内部创建。在`ActorSystem`层，使用`system.actorOf`方法。

关于`ActorSystem`:

>An actor system is a hierarchical group of actors which share common configuration, e.g. dispatchers, deployments, remote capabilities and addresses. It is also the entry point for creating or looking up actors.

所有一般**一个逻辑程序一个ActorSystem**。

当你调用`ActorSystem`的`actorOf`方法，就会异步地开始该actor，并返回一个`ActorRef`实例。你可以把它想象成你和真实actor的代理人。

`ActorRef`有如下性质：

- 不可变
- 和其表示的Actor一对一
- 可序列化，网络感知


## Actors间通信
向一个actor发送消息的基本语法是：

	actorInstace ! message
	
PingPong的例子：

```

case object PingMessage
case object PongMessage
case object StartMessage
case object StopMessage

class Ping(pong: ActorRef) extends Actor{

  var count = 0
  def incrementAndPrint {count += 1; println("ping")}

  def receive = {
    case StartMessage =>
      incrementAndPrint
      pong ! PingMessage
    case PongMessage =>
      incrementAndPrint
      if (count > 99){
        sender ! StopMessage
        println("ping stopped")
        context.stop(self)
      }else{
        sender ! PingMessage
      }
    case _ => println("Ping get error")
  }
}

class Pong extends Actor {

  def receive = {
    case PingMessage =>
      println("pong")
      sender ! PongMessage
    case StopMessage =>
      println("pong stopped")
      context.stop(self)
    case _ => println("Pong get error")
  }
}

object PingPongTest extends App{
  val system = ActorSystem("PingPong")
  val pong = system.actorOf(Props[Pong], name = "pong")
  val ping = system.actorOf(Props(new Ping(pong)), name = "ping")

  ping ! StartMessage
}
```

`sender`是用来表示给该actor发送消息的actor，（发送过程必须在一个Actor内部）。
参见： [Stackoverflow : Scala Actor sender](http://stackoverflow.com/questions/30327034/scala-actor-sender)




