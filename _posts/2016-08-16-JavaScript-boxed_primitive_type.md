---
layout: post
title: 基本包装类型
categories: [javascript]
tags: [javaScript, No.10, 基本包装类型, blogs]
<!-- fullview: false -->
---

<!-- ## {{ page.title }} -->
{{ page.date | date_to_string }}

> ECMAScript还提供了3个特殊的引用类型：Boolean、Number和String
> 这些类型与其他引用类型相似，但同时也具有与各自的基本类型相应的特殊行为。
> 实际上，每当读取一个基本类型值的时候，后台就会创建一个对应的基本包装类型的对象，从而让我们能够调用一些方法来操作这些数据。

```javascript
    var s1 = "some text";
    var s2 = s1.substring(2);
```

> 基本类型值不是对象，因而逻辑上讲不应该有方法（可是他们确实有方法）。其实，为了让我们实现这种直观的操作，后台已经自动完成了一系列的处理。当第二行代码访问s1时，访问过程处于一种读取模式，也就是要从内存中读取这个字符串的值。而在读取模式中访问字符串时，后台都会自动完成下列处理。
 - 创建String类型的一个实例
 - 在实例上调用指定的方法；
 - 销毁这个实例。  *【不太明白为什么】*
> 可以将上面三个步骤想象成是执行了下列ECMAScript代码。

```javascript
    var s1 = "some text";
    var s2 = s1.substring(2);
    s1 = null;  //不太明白为什么将null赋值给s1
```

> 经过此番处理，基本的字符串值就变得跟对象一样了。

> 引用类型与基本包装类型的主要区别就是对象的生存期。使用new操作符创建的引用类型的实例，在执行流离开当前作用域之前都一直保存在内存中。而自动创建的基本包装类型的对象，则只存在与一行代码的执行瞬间，然后立即被销毁。*这意味着我们不能在运行时为基本类型添加属性和方法*

```javascript
    var s1 = 'hello world';
    s1.color = 'red';
    console.log(s1,s1.color); //hello world undefined
```

> 上述例子中，第二行代码试图给s1添加一个color属性，当第三行代码再次访问s1时，其color属性不见了。问题的原因就是第二行创建的String对象在执行第三行代码时已经被销毁了。第三行代码又创建自己的String对象，而该对象没有color对象。