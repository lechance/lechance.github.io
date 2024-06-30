---
title: Android中事件驱动编程 Event-driven programming for Android (part II)
date: 2016-05-18 12:55:04
tags:
- android
- eventbus
- event-driven
---
原文地址:[Event-driven for Android][source-article]

(This is second article in a three-part series)

(本文是系列文章的第二部分)

In the previous article we had a short introduction into Event-Driven programming. Now let's see some actual code and how to perform the basics with EventBus.

在前一章里我们已经简短地介绍了事件驱动编程。现在我们用实际的代码来展示如何用`EventBus`执行基本的操作。

First I will present the entities that play a central role in Event-Driven programming. Refer to the following image taken from the [EventBus][eventbus] repository.

首先，我会介绍在事件驱动编程中扮演核心角色的实体。引用一张EventBus仓库的图片。

![EventBus][eventbus-publish-subscribe-diagram]

An *Event Bus*. This is the central communication channel that connects all the other entities.

*Event Bus* 是连接所有其他实例的中央通信通道 (central communication channel).


An *Event*. This is the action that will take place and can be literally anything (the application starting, some data being received, a user interaction...)

事件 - 发生的行为(action)，也可以从字面上理解为任何的事件(literally anything)  (应用程序的启动，数据的接收，用户交互等...)

A *Subscriber*. The *Subscriber* are listening at the *Event Bus*. If they see an event circulating, they will be triggered.

订阅者 - 监听事件总线的订阅者，如果它们监听到事件的散布就会被触发(triggered)。

A *Publisher*, which sends *Events* to the *Event Bus*.

发布者 - 它给事件总线(Event Bus)发送事件(Events)。

Everything is clear with a practical view, so let's see how this fits a basic example:

现在所有概念都很清楚，所以让我们看看这是如何适合一个基本的例子：

- An application that loads two fragments.
- The second fragment contains a *TextView* that will be updated when a button is clicked.
- The *ActionBar*  title will change when a new Fragment comes into scene.

### The hosting Activity
The hosting Activity will need to register in its method onCreate the EventBus.

```java
EventBus.getDefault().register(this);
```
The hosting Activity will be now ready to read data from the bus. We also need to unregister the bus in the method onDestory.

```java
EventBus.getDefault().unregister(this);
```

The Activity will be capturing two different events: one to update the ActionBar title and another one to load the first fragment. We will write two methods onEvent that will handle the events:

```java

    public void onEvent(ShowFragmentEvent event) {
        getFragmentManager().beginTransaction().replace(R.id.container, event.getFragment()).addToBackStack(null).commit();
    }

    public void onEvent(UpdateActionBarTitleEvent e) {
        getActionBar().setTitle(e.getTitle());
    }
```

### The Events
Each event needs to be declared in its class. The events can contain variables within them.

```java
    public final class ShowFragmentEvent {
        private Fragment fragment;
        
        public ShowFragment(Fragment fragment) {
            this.fragment = fragment;
        }

        public Fragment getFragment(){
            return fragment;
        }
    }
```

###The Fragments

We need now to create the fragments. The first Fragment will contain a button that open the second, and the latest will contain a button that, when pressed, updates a TextView. The fragments also need to register and de-register the EventBus, so to archieve a cleaner structure everything will be encapsulated in a BaseFragment.

Now let's create some more action, The first Fragment will open the second one with the following function:

```java
    @OnClick(R.id.first_button)
    public void firstButtonClick() {
        EventBus.getDefault().post(new ShowFragmentEvent(new SecondFragment()));
    }
```

Note that here I am using annotations from [ButterKnife][butterknife]. It produces a much cleaner and neater code. If you haven't used it yet, you should start now.

The button of the second Fragment will send an event to the EventBus to change the TextView.

```java
    EventBus.getDefault().post(new UpdateTextEvent(getString(R.String.text_updated)));
```

We have a basic application with two Fragments that communicate between them with Events, and a Fragment that get updated through Events. I have upload the code to [GitHub][eventbus-sample], so you can check it out and take a look.	

A key question is how to escalate an Event-Driven architecture. In the next article I will propose a clean and understandable architecture to support Event-Driven programing in Android.

原创文章转载请注明：

转载自[Lechance][lechance-event-driven]翻译：https://medium.com/google-developer-experts/event-driven-programming-for-android-part-ii-b1e05698e440#.9xdx4wc4q


[understanding-event-driven-programming]: http://www.informit.com/library/content.aspx?b=STY_Csharp_24hours&seqNum=50 "understanding event-driven programming"

[lechance-event-driven]: http://lechance.github.io/posts/2016/05/18/event-driven-for-android/ "lechance"

[source-article]: https://medium.com/google-developer-experts/event-driven-programming-for-android-part-ii-b1e05698e440#.9xdx4wc4q "source address"

[eventbus-publish-subscribe-diagram]: /images/android/EventBus-Publish-Subscribe.png "eventbus publish subscribe diagrams"

[EventBus]: https://github.com/greenrobot/EventBus "EventBus"

[eventbus-sample]: https://github.com/kikoso/eventbus-sample "eventbus sample"

[butterknife]: https://github.com/JakeWharton/butterknife "ButterKnife"
