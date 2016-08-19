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

> 上述例子中，第二行代码试图给s1添加一个color属性，当第三行代码再次访问s1时，其color属性不见了。问题的原因就是第二行创建的String对象在执行第三行代码时已经被销毁了。第三行代码又创建自己的String对象，而该对象没有color属性。

> 可以显示地调用Boolean、Number和String来创建基本包装类型的对象。不过，应该在绝对必要的情况下再这样做，因为这种做法很容易让人分不清自己是在处理基本类型还是引用类型的值。对基本包装类型的实例调用typeof会返回"object"，而且所有基本包装类型的对象都会被转换为布尔值true。

> Obejct构造函数也会像工厂方法一样，根据传入值的类型返回相应基本包装类型的实例。

```javascript
    var obj = new Object('some text');
    console.log(obj instanceof String); //true
```

> 把字符串传给Object构造函数，就会创建String的实例；而传入数值参数会得到Number的实例，传入布尔值参数就会得到Boolean的实例。

> 使用new调用基本包装类型的构造函数，与直接调用同名的转型函数是不一样的。

```javascript
    var value = "25";
    var number = Number(value);
    console.log(typeof number);  //number
    var obj = new Number(value);
    console.log(typeof obj);    //object
```
> 在例子中，变量number中保存的是基本类型的值25，而变量obj中保存的是Number的实例。

#### Boolean类型

> Boolean类型是与布尔值对应的引用类型。要创建Boolean对象，可以像下面这样调用Boolean构造函数并传入true或false值。

```javascript
    var booleanObject = new Boolean(true);
```

> Boolean类型的实例重写了valueOf()方法，返回基本类型值"true"或"false"；重写了toString()方法，返回字符串"true"和"false"。可是，Boolean对象在ECMAScript中的用处不大，因为它经常会造成人们的无解。其中最常见的问题就是在布尔表达式中使用Boolean对象：

```javascript
    var booleanObject = new Boolean(false),
        result = booleanObject && true;

    console.log(result);    //true

    var falseValue = false;
        result = falseValue && true;
    console.log(result); //false
```

> 在上述例子中，我们使用false值创建了一个Boolean对象。然后，将这个对象与基本类型值true构成逻辑与表达式。在布尔运算中，false && true等于false。可是，示例中的这行代码是***对falseObject而不是对它的值(false)进行求值。布尔表达式中所有对象都会被转换为true***，因此falseObject对象在布尔表达式中代表的是true。结果true && true当然就等于true了。

> 基本类型与引用类型的布尔值还有两个区别：
> - typeof操作符对基本类型返回"boolean",而引用类型返回"object"
> - Boolean对象是Boolean类型的实例，所以使用instanceof操作符测试Boolean对象会返回true，而测试基本类型的布尔值则返回false。

```javascript
    var a = false,
        b = new Boolean(true);
    console.log(typeof a);      //boolean
    console.log(typeof b);      //object
    console.log(a instanceof Boolean);  //false
    console.log(b instanceof Boolean);  //true
```

> 理解基本类型的布尔值与Boolean对象之间的区别非常重要--当然，我们的建议是永远不要使用Boolean对象。

#### Number类型

> Number是与数字值对应的引用类型。要创建Number对象，可以在调用Number构建函数时向其中传递相应的数值。

```javascript
    var numberObject = new Number(10);
```

> 与Boolean类型一样，Number类型也重写了valueOf()、toLocaleString()和toString()方法。重写后valueOf()方法返回对象表示的基本类型的数值，另外两个方法则返回字符串形式的数值。
> 可以为toString()方法传递一个表示基数的参数，告诉他返回几进制数值的字符串形式

```javascript
    var num = 10;
    console.log(num.toString());    //10
    console.log(num.toString(2));   //1010
    console.log(num.toString(8));   //12
    console.log(num.toString(10));  //10
    console.log(num.toString(16));  //a
```

> 除了继承的方法之外，Number类型还提供了一些用于将数值格式化为字符串的方法。
> 其中，toFixed()方法会按照指定的小数位返回数值的字符串表示

```javascript
    var num = 10;
    console.log(num.toFixed(2)); //10.10
```