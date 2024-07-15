---
title: Model-view-presenter Pattern in Android
date: 2016-04-23 01:41:11
categories:
- Architectural Pattern
tags:
- mvp
- android
- mvc
thumbnail: https://blog.lechance.top/images/mvp_diagram.png
---
> wikipedia https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter

>*Model-view-presenter (MVP)* is a derivation of the *Model-view-controller(MVC)* architectural pattern, and is used mostly for building `user interface`.
In MVP the presenter assumes the functionality of the "middle-man", In MVP, all presentation logic is pushed to the `presenter`.


![mvp-diagram](/images/Model_View_Presenter_GUI_Design_Pattern.png)

### 模式描述
MVP 是一种架构模式，利用它可以方便的进行自动化单元测试以及在表示逻辑上提高关注分离：

- `Model`　定义了业务逻辑和实体模型，它负责处理数据的加载和存储，比如从本地或网络获取数据等。
- `Presenter`相当于协调者，它从`Model`接收数据，处理之后显示在`View`，它是模型和视图之间的桥梁，它将模型与视图分离开。
- `View`负责界面数据(`the model`)的展示以及与用户进行交互，它几乎不包含任何逻辑。

>
Model和View使用Observe模式进行沟通；而Presenter和View则使用Mediator模式进行通信；Presenter操作Model则使用Command模式进行操作。基本设计和MVC相同：Model存储数据，View展现Model的表现，Presenter协调两者之间的通信。在MVP中View接收到事件，然后会将他们传递到Presenter，具体如何处理这些事件，将由Presenter来完成。


![MVP Diagram](/images/mvp_diagram.png)

### MVP的优点(pro)
- 模型与视图完全分离，我们可以修改视图而不影响模型。
- 可以更高效地使用模型，因为所有的交互都发生在Presenter内部。
- 可以将一个Presenter用于多个视图，而不需要改变Presenter的逻辑。
- 如果把逻辑放在Presenter中，那么就可以脱离用户接口来测试这些逻辑（单元测试）。

### MVP的缺点(con)
- 首先代码量多了，这是毋庸置疑的。
- 由于对视图的渲染放在了Presenter中，所以视图和Presenter的交互会过于频繁。


具体示例参考以下地址：
[Todo-MVP](https://github.com/googlesamples/android-architecture)
