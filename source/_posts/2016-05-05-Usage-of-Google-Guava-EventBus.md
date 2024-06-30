---
title: Usage of Google Guava EventBus
date: 2016-05-05 16:43:14
tags:
---
>
`EventBus` allows publish-subscribe-style communication between components without requiring the components to explicitly register with one another (and thus be aware of each other). It is designed exclusively to replace traditional Java in-process event distribution using explicit registration. It is not a general-purpose publish-subscribe system, nor is intended for interprocess communication.



_本资料整理自网络_

#### 理解

在应用程序中有些对象需要分享信息,在Java程序中实现信息共享的一种方式是使用事件监听器(event listener),监听器的唯一目的是当事件发生时采取一些操作.对于这样的问题,通常开发者会编写必要的匿名内部类(anonymous inner class)来实现事件监听接口.而在这里要学习的是另一个处理Java事件的方法,`EventBus`.

`EventBus`是Google[Guava][guava_address]项目中用于事件处理机制的子库(当然里面还包含许多实用),是设计模式中观察者模式(生产者／消费者模式)的实现.`EventBus`允许对象无需明确知道对方的情况下发布或订阅事件(实现这样的功能是通过Java的Reflection机制).更多的信息可以参考[这里][eventbus_wiki].

> Observer模式是比较常用的设计模式之一,比如常用的event Listener就是这个模式,只是名字不同罢了,不管名字怎么变,模式还是这个模式.而从JDK1.0开始就提供了两个用于实现这类模式的类(`Observable`,`Observer`).

#### EventBus 类

`EventBus`是一个非常灵活以及能够作为单例模式来使用的类,或者在不同的上下文中程序可以持有此类的几个实例适当地传递事件(transferring event).由于事件会被串行(serially)地分发,因此保持轻量的事件处理方法是非常重要的.假如你要在事件处理程序(Event Handler)中做大量的操作,有另一种风味(flavor)的`EventBus`可以使用,`AsyncEventBus`.
`AsyncEventBus`与`EventBus`具有相同的功能,只不过`AsyncEventBus`是采用`ExecutorService`作为构造函数的参数以此允许异步事件的分发.

####　相关概念

使用`EventBus`替换一个基于事件监听器(EventListener) 的系统是很简单的.

#### 注册事件
- 定义一个只包含需要订阅的事件类型的形参的public方法,并用`@Subscribe`注解这个方法.
- 通过调用`register`方法注册所有要接收事件的订阅者.

简单的示例如下：
```java
	public class NewSubscriber {

	    @Subscribe
	    public void handleNewEvent(NewEvent event){
		//...
	    }
	}

	EventBus eventBus = new EventBus();
	eventBus.register(new NewSubscriber());
	
```

#### 发布事件
- 通过`EventBus`发布事件是非常简单明了的,通过调用`EventBus.post`发送事件的变化通知所有注册了的对象.
```java
	public void handleTransaction(){
	  purchaseService.purchase(item, amount);
	  eventBus.post(new CashPurchaseEvent(item, amount);
	}
	//...
```

> 明显的可以看出,发布和订阅使用了相同的`EventBus`类的实例,简单起见,你也可以使用单例模式.

#### 事件处理程序
`EventBus`的一个非常强大的功能是你可以根据自己的需要使你的`handler`作为[粗粒度或细粒度](http://stackoverflow.com/questions/3766845/coarse-grained-vs-fine-grained)的处理方式.`EventBus`会调用已注册的发布的事件对象的子类和它的实现接口.比如，要处理某个事件可以创建一个带有类型参数的事件处理程序。要处理具体的事件，就创建一个非常具体的事件处理程序，比如下面的示例：

```java

/**
 * EventBusDemo Created by lechance 
 */

public abstract class PurchaseEvent {


    protected String item;

    public PurchaseEvent(String item) {
        this.item = item;
    }

    public abstract void handle();
}

class CashPurchaseEvent extends PurchaseEvent {

    private int amount;

    public CashPurchaseEvent(String item, int amount) {
        super(item);
        this.amount = amount;
    }

    @Override
    public void handle() {
        System.out.printf("item: %s amount: %s%n", item, amount);
    }
}

class CreditPurchaseEvent extends PurchaseEvent {
    private int amount;
    private String cardNumber;

    public CreditPurchaseEvent(String item, int amount, String cardNumber) {
        super(item);
        this.amount = amount;
        this.cardNumber = cardNumber;
    }

    @Override
    public void handle() {
        System.out.printf("item: %s amount: %d cardNumber: %s%n", item, amount, cardNumber);
    }
}

{% endhighlight %}

下面是事件处理类：

{% highlight java %}

/**
 * EventBusDemo Created by lechance 
 */

//只有现金购买时才会被通知
class CashPurchaseSubscriber {

    @Subscribe
    public void handleCashPurchase(CashPurchaseEvent event){
        event.handle();
    }
}

//只有信用卡购买时才会被通知
class CreditPurchaseSubscriber{

    @Subscribe
    public void handleCreditPurchase(CreditPurchaseEvent event){
        event.handle();
    }
}

//对于任何事件都会被通知
class PurchaseSubscriber{


    @Subscribe
    public void handlePurchaseEvent(PurchaseEvent event){
	//由于PurchaseEvent是抽象类，以下方法会调用它的实现类中对应的方法。
        event.handle();
    }
}

```

`EventBus`提供了吸引人的操作事件的模式，使你可以替代传统的Java事件处理机制。

#### Example

#### Resources
- [EventBus API](http://google.github.io/guava/releases/snapshot/api/docs/)
- [Guava on Github](https://github.com/google/guava)
- [Bill's Post about EventBus](http://codingjunkie.net/guava-eventbus/)

[guava_address]: https://github.com/google/guava "guava"
[eventbus_wiki]: https://github.com/google/guava/wiki/EventBusExplained "wiki"
