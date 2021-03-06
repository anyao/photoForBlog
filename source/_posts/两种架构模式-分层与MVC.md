---
title: '两种架构模式:分层与MVC'
date: 2017-09-12 17:24:35
tags:
    - php面试
    - MVC
    - 三层架构
---
**一、架构模式**
除了通常应用于对象和类之间交互的典型设计模式和原则之外，还有一些从总体上指导架构的原则或模式。《面向模式的软件体系结构》中将它们定义为架构模式，而不是设计模式。本文将介绍其中两种模式：层与MVC。
<!-- more -->
但将它们成为“模式”会让某些人的理解受到一定限制，他们会把它们看做是死规定而不是指导。其实，将它们看做整体概念、范型或分类原则可能更有效。理解和记住它们要比严格地应用它们重要——eg. 引入层或者MVC在非常简单的Web应用中显然没有必要。

**二、分层设计**
“分层”的理念是构建软件时普遍和自然的思考方式。函数、方法或者对象有啄序（pecking order）之分：函数A“啄”（调用）函数B的次数比函数B“啄”函数A的次数多，因此函数B在啄序中的等级低于函数A。如果你的代码中这种情况很多，可以将代码整理成层，对它们进行分类。就像所有这些组件都漂浮在周围一样，你需要抓住它们，将它们放到不同的架子上，方便组织和记忆。

在精确分层的架构中，每层上的对象都只能方位低于它所在层上的对象。网络协议就是这种方式。

- **“三层”模式**
分层设计有些像等级制企业或者军事机构，给系统中的每个组件一个“等级”即为啄序，这样可以吩咐较低级别的组件要做什么。

典型的三层架构如下：

| 层      | 目的    |
| :------| :-----: |
| 表现层  |  应用程序的用户接口和用户交互部分，主要是HTML页     |
| 逻辑层  |业务逻辑      |
| 数据层  |存储和检索数据库或者其他存储区中的数据。<br>eg. 与数据库有关的类，提供non-SQL接口给数据层其他类       |

以上，一个不太严格的示例可能就是 ActiveRecord 模式，其中业务逻辑和数据存储逻辑都混合在一个类中。

- **逻辑层（域层）**
域层的功能可以用CRUD来概括。如果有复杂的业务逻辑，域对象就会非常有用。反之，如果为当前并不需要的需求引入它的话，只会增加系统的负担。

有关面向对象的书籍通常着重介绍域层，将其作为复杂逻辑存在的地方。但在Web应用程序中，最复杂的逻辑通常存在于程序的其它的地方。

多数企业的应用程序中使用的基本设计都是分层设计。对高级别的关注点进行分离，让多数更改只影响一层，实现单一职责原则。

**三、MVC**
MVC体系结构基于表现层和逻辑层分离。而在MVC术语中，它是模型（model）和视图（view）的分离。当然，还有第二个分离，视图和控制器之间的。

在MVC中，model是程序的功能核心，view用于向用户展示信息，controller用来处理用户输入。

下图简要说明Web MVC如何工作。箭头意为：使用。
![](http://img.blog.csdn.net/20170912162004399)

下表概括了MVC结构中各个组件的作用：

| 组件      | 目的    |
| :------| :-----: |
| 模型  | 程序中表示数据和逻辑，除用户接口之外的所有内容      |
| 视图  | 显示对用户展示结果的部分，多为HTML    |
| 控制器  |  控制用户输入，在Web中，用来处理HTTP请求  |

将MVC视为一种原则更为有用一些。模型、视图和控制器不比分在不同的类或者函数中，每个部分聚合一起，而不是随意地散落在应用程序各处，这是MVC最重要的特性。

- **控制器逻辑**
Web原本就是带有超链接的静态HTML页面的集合。用户通过在浏览器中输入URL或者点击链接（GET）请求特定页面，并且只获取该页面。

处理（POST）请求是增加Web编程复杂程度的元凶，这也就是Web应用引用MVC的原因。
如果用户请求并未指定用户要看什么内容，这时候程序需要作出决定（决定显示什么视图）。这一功能不属于业务逻辑，因此不属于模型的一部分。而关于显示哪个视图的决策一个视图自然无法完成。能做到这点只有控制器。

**四、总结**
MVC是成功的手段，而不是标准。如果要评估一个设计，我们不应该关心这个设计是否符合MVC，而更需关注的是：代码的可读性如何？是否好理解？有没有重复现象？是否可靠？是否有恰当的灵活性？MVC不是万能的，它有最重要的定义：处理用户输入和输出。

某种程度上，分层可能比MVC更通用，而且更像范型。要让所有细节都符合模式是没有多大益处的。