---
title: Understanding UML
date: 2016-03-07 13:15:42
categories:
- Design
tags:
- uml
---
### What is UML
>
The **Unified Modeling Language(UML)** is a general-purpose, developmental, *modeling language* in the field of *software engineering*, that is intended to provide a standard way to visualize the design of a system.


### UML的目标
一张图片胜过千言万语,这样的结论绝对也适用于UML.面向对象的概念要比UML出现的更早,因此当时没有标准的方法来组织和巩固面向对象开发.而且UML图表并不仅仅面向开发人员,它同样也应用到企业用户,以及其他任何对想要理解此系统的人.总之UML语言的目标是定义一个简单的建模机制来对所有当今复杂环境下系统进行建模.

### 面向对象概念
一个对象包含数据和控制数据的方法,数据通常也称为属性,它用来描述对象(Object)的状态.类(Class)用来描述对象(Object)以及它们还建立了层次结构模拟真实世界系统.层次结构被表示为继承以及类也可以按照要求以不同的方式被关联起来.
对象是存在与我们身边的真实世界实体,面向对象里的基本概念比如抽象,封装,继承和多态都能够用UML语言表示.
下面是面向对象里一些基本概念.
- Objects: 对象代表一个实体,它也是UML里基本的构建块.
- Class: 表示现实世界里一类具有共同特征的事物的抽象.
- Abstraction: 抽象在Java语言里表现为一种过程的抽象,它隐藏程序的某些细节而只展示程序基本的功能.换句话说,抽象只描述了一个对象的基本特征.
- Encapsulation: 封装是这样一种机制,它把数据封装起来,对于外面的对象来说这些数据(属性)是隐藏的.
- Inheritance: 继承是从一个存在的基类派生出新类的机制.
- Polymorphism: 一个基类的引用,可以指向多种派生类对象,具有多种不同的形态,这种现象就叫多态性.

### UML的构建块
UML的构建块由以下三个主要元素构成：
#### 事物(Things)
事物是实体抽象化的最终结果,是模型中的基本成员,UML中包含结构事物,行为事物,分组事物和注释事物.
##### 结构事物
结构事物是模型中的静态部分,用来呈现概念和实体的表现元素,以下是软件建模中最常见的七种元素.
- Class: 是指具有相同属性,方法,关系和语义的对象的集合.
![img uml_class] (/images/uml/uml_class.jpg )
- Interface: 接口是指类或组件所提供的服务（操作）,描述了类或组件对外可见的动作.
![ img uml_interface /images/uml/uml_interface.jpg )
- Collaboration: 协作描述完成某个特定任务的一组类及其关联的集合,用于对使用情形的实现建模.
![ img uml_collaboration /images/uml/uml_collaboration.jpg )
- Use Case: 用于表示系统所提供的服务,它定义了系统是如何被参与者所使用的,它描述的是参与者为了使用系统提供的某一完整功能而与系统之间发生一段对话.
![ img use_case ](/images/uml/uml_usecase.jpg )
- Component: 组件是物理的,可替换的部分,包含接口的集合,例如COM+,JAVA BEANS等.
![ img component ](/images/uml/uml_component.jpg )
- Node: 节点是系统在运行时存在的物理元素,代表一个可计算的资源,通常占用一些内存和具有处理能力.
![ img Node ](/images/uml/uml_node.jpg )

##### 行为事物
行为事物指的是UML模型中的动态部分,如下：

- Interaction: 交互是在特定上下文中的一组对象,为了达到特定的目的而进行的一系列消息交换而组成的动作.
![ img interaction ](/images/uml/uml_message.jpg )
- State machine: 它定义的状态序列对象通过事件进行响应,而事件是负责(responsible)状态变化的外部因素.
![ img state_machine ](/images/uml/uml_state.jpg )

##### 分组事物
分组事物被定义为这样一种机制,即它能对UML模型元素进行分组,目前只有一种可用的分组事物：
- **Package**: 结构事物,行为事物甚至分组事物都有可能放在一个包中.包纯粹是概念上的,只存在与开发阶段,而组件在运行时存在.
![ img uml_package ](/images/uml/uml_package.jpg )

##### 注释事物
- **Note**: 用于UML元素的解释部分.
![ img Note ](/images/uml/uml_note.jpg)
#### 关系(Relationship)
关系是UML的另一个重要构建块,它显示元素是任何彼此相互关联的.UML定义了四种可用的关系.
##### Dependencies(依赖): 依赖描述了两个事物之间的语义关系,其中一个事物的变化会影响另一个事物.
![ img dependency ](/images/uml/uml_dependency.jpg )
##### Association(关联): 描述一组对象之间的连接的结构关系,如聚合关系(描述了整体和部分之间的结构关系).
![ img association ](images/uml/uml_association.jpg )
##### Generalization(泛化): 是一般化- 特殊化的关系,在对象的世界里它描述了继承这类关系.
![ img generalization ](/images/uml/uml_generalization.jpg )
##### Realization(实现): 实现定义了两个相连的元素间的一种关系,其中一个元素描述了未被实现的责任,而另一则实现了这些责任,这种关系存在与接口的情况下.
![ img realization ](/images/uml/uml_realization.jpg )

#### 图(Diagrams)
图是事物集合的分类,所有的元素(elements),关系都用来制造一个完整的UML图,图也表示一个系统.
UML包含了九种图：
- 类图(Class Diagram): 类图描述系统包含的类,类的内部结构以及类之间的关系.
- 对象图(Object Diagram): 对象图是类图的一个具体实例.
- 包图(Package Diagram): 包图表示包及其之间的依赖类图.
- 用例图(Usecase Diagram): 用例图从用户的角度出发描述系统的功能,需求,展示系统外部的各类角色与系统内部的各种用例之间的关系.
- 顺序图(Sequence Diagram): 表示对象之间动态合作的关系.
- 协作图(Collaboration Diagram): 描述对象之间的合作关系.
- 活动图(Activity Diagram): 描述了系统中各种活动的执行顺序.
- 状态图(Statechart Diagram): 状态图描述了一类对象的所有可能的状态以及事件发生时状态的转移条件.
- 部署图(Deployment Diagram): 定义了系统中软硬件的物理体系结构.
- 组件图(Compoment Diagram): 组件图也称构建图,它描述了代码部件的物理结构以及各部件之间的依赖关系.
#### 符号(Notation)
UML符号在建模中是非常重要的一类元素,下面是一些基本的符号.

##### Structural Things
- Classes Notation
![ img notation_class ](/images/uml/notation_class.jpg )
- Object Notation
![ img notation_object ](/images/uml/notation_object.jpg )
- Interface Notation
![ img notation_interface ](/images/uml/notation_interface.jpg )
- Collaboration Notation
![ img notation_collaboration ](/images/uml/notation_collaboration.jpg )
- Use case Notation
![ img notation_usecase ](/images/uml/notation_usecase.jpg )
- Actor Notation
![ img notation_actor ](/images/uml/notation_actor.jpg )
- Initial State Notation
![ img notation_initial_sate ](/images/uml/notation_initialstate.jpg )
- Final State Notation
![ img notation_final_state ](/images/uml/notation_finalstate.jpg )
- Active Class Notation
![ img notation_active_class ](/images/uml/notation_activeclass.jpg )
Active class is used to represent concurrency in a system.
- Component Notation
![ img notation_component ](/images/uml/notation_component.jpg )
- Node Notation
![ img notation_node ](/images/uml/notation_node.jpg )
##### Behavioural Things
- Interactions
![ img notation_interaction](/images/uml/notation_interaction.jpg )
- State machines
![ img notation_state_machine ](/images/uml/notation_statemachine.jpg )
##### Grouping Things
- Package Notation
![ img notation_package ](/images/uml/notation_package.jpg )
##### Annotational Things
- Note Notation
![ img notation_note ](/images/uml/notation_note.jpg )
##### Relationships
- Dependency Notation
![ img notation_dependency ](/images/uml/notation_dependency.jpg )
- Association Notation
![ img notation_association ](/images/uml/notation_association.jpg )
- Generalizaion Notation
![ img notation_generalization ](/images/uml/notation_generalization.jpg )
- Extensibility Notation
![ img notation_extensibility ](/images/uml/notation_extensibility.jpg )

>This aritcal refer [here](http://www.tutorialspoint.com/uml).
