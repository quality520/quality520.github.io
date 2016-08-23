---
layout: post
title: 基本包装类型
categories: [javascript]
tags: [javaScript, No.10, 基本包装类型, blogs]
<!-- fullview: false -->
---

<!-- ## {{ page.title }} -->
<!-- {{ page.date | date_to_string }} -->

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

###### toFixed()

> 除了继承的方法之外，Number类型还提供了一些用于将数值格式化为字符串的方法。
> 其中，toFixed()方法会按照指定的小数位返回数值的字符串表示

```javascript
    var num = 10;
    console.log(num.toFixed(2)); //10.10
```

> 给toFixed()方法传入数值2，意思就是显示几位小数，于是，上述结果返回"10.00"，即已0填补必要的小数位。如果数值本身包含的小数位比指定的还多，name接近指定的最大小数位的值就会舍入。

```javascript
    var num = 10.005;
    console.log(num.toFixed(2)); //10.01
```

> 能够自动舍入的特性，是的toFixed()方法很适合处理货币值。但需要注意的是，不同浏览器给这个方法设定的舍入规则坑内会有所不同。在给toFixed()传入0的情况下，IE8及之前版本不能正确舍入范围在{(-0.94,-0.5)],[0.5,0.94)}之间的值。对于这个范围内的值，IE会返回0，而不是-1或1；其他浏览器都能返回正确的值。

> # tips：
>   toFixed()方法可以表示带有0到20个小数位的数值。但这只是标准实现的范围，有些浏览器也可能支持更多位数。

###### toExponential()
> toExponential()方法用于格式化数值，该方法返回以指数表示法(也称e表示法)表示的数值的字符串形式。与toFixed()一样，toExponential()也接受一个参数，而且该参数同样也是指定输出结果中的小数位数。

```javascript
    var num = 100;
    console.log(num.toExponential(1)); //1.0e+2
```

###### toPrecision()
> toPrecision()方法可能会返回固定大小(fixed)格式，也可能返回指数(exponential)格式；
> 参数：这个方法接收一个参数，即表示数值的所有数字的位数(不包括指数部分)。

```javascript
    var num = 99;
    console.log(num.toPrecision(1));    //1e+2
    console.log(num.toPrecision(2));    //99
    console.log(num.toPrecision(3));    //99.0
```

> 第二行代码完成的任务是以一位数来表示99，结果是“1e+2”,即100，因为一位数无法准确的表示99，因此toPrecision()就将它向上舍入为100，这样就可以使用一位数来表示它了。
> toPrecision()会根据要处理的数值决定到底是调用toFixed()还是调用toExponential()。而这三个方法都可以通过向上或向下舍入，做到以最准确的形式来表示带有正确小数位的值。

```javascript
    var numberObject = new Number(10);
    var number = 10;
    console.log(typeof numberObject,numberObject instanceof Number);
    console.log(typeof number,number instanceof Number);

    //object,true
    //number false
```

> 在使用typeof操作符测试基本类型数值时，始终返回"number",而在测试Number对象时，则会返回"object".类似的，Number对象是Number类型的实例，而基本类型的数值则不是。

#### String类型

> String类型是字符串的对象包装类型，可以像下面这样使用String构造函数来创建：

```javascript
    var stringObject = new String("hello world");
```

> String对象的方法也可以在所有基本的字符串值中访问到。其中，继承的valueOf()、toLocaleString()、toString()方法，都返回对象所表示的基本字符值。

> String类型的每个实例都有一个length属性，表示字符串中包含多个字符。

```javascript
    var stringValue = "hello world";
    console.log(stringValue.length);    //11
    var value = "中国女排加油！";
    console.log(value.length);  //7
```

###### 1.字符方法

> 两个用于访问字符串中特定字符的方法是:charAt()和charCodeAt()。这两个方法都接收一个参数，即基于0的字符位置。其中charAt()方法以单字符字符串的形式返回给定位置的那个字符(ECMAScript中没有字符类型)。

```javascript
    var value = "hello world";
    console.log(value.charAt(1));   //e
    console.log(value.charCodeAt(1));   //101
    console.log(value[1]);   //e
```

> 字符串"hello world"位置1处的字符是"e",因此调用charAt(1)就返回了"e",
> 而调用charCodeAt(1),就返回了"e"的字符编码"101".
> 上述例子第四行代码，表示使用方括号加数字索引来访问字符串中的特定字符(ECMAScript5定义的访问个别字符的方法)。

###### 2.字符串操作方法

- concat(),用于将一个或多个字符串拼接起来，返回拼接得到的新字符串。***不会改变源字符串***

```javascript
    var value = "hello ";
    var result = value.concat("world");
    var value2 = "world ";
    var value3 = "!";
    console.log(result);    //"hello world"
    console.log(value.concat(value2,value3));   //"hello world !";
    console.log(value); //"hello"
```

> 虽然concat()是专门用来拼接字符串的方法，但实践中使用更多的还是加号操作符(+)。而且加好操作符大多数情况下都比使用concat()方法要简便易行(特别是在拼接多个字符串的情况下)。

- slice()、substr()和substring()这三个方法都会返回被操作字符串的一个子字符串，而且接受一个或两个参数，第一个参数指定了子字符串的开始位置，第二个参数(在指定的情况下)表示子字符串到哪里结束。具体来说，slice()和substring()的第二个参数指定的是字符串最后一个字符后面的位置。而substr()的第二个参数指定的则是返回的字符个数。如果没有给这些方法传递第二个参数，则将字符串的长度作为结束位置。与concat()方法一样，slice()、substr()和substring()也不会修改字符串本身的值----它们只是返回一个基本类型的字符串值，对原始字符串没有任何影响。

```javascript
    var value = "hello world";
    console.log(value.slice(3));    //lo world
    console.log(value.substring(3));    //lo world
    console.log(value.substr(3));   //lo world
    console.log(value.slice(3,7));  //lo w
    console.log(value.substring(3,7)); //lo w
    console.log(value.substr(3,7)); //第二个参数为返回的字符个数，lo worl
```

> 在传递给这些方法的参数是负数的情况下，它们的行为就不尽相同了。
> 其中，**slice()**方法会将传入的负值与字符串的长度相加，
> **substr()**方法将负的第一个参数加上字符串的长度，而将负的第二个参数转换为0，
> **substring()**方法会把负值参数都转换为0.

```javascript
    var value = "hello world";
    console.log(value.slice(-3));   //rld
    console.log(value.substr(-3));  //rld
    console.log(value.substring(-3));//hello world
    console.log(value.slice(3,-4)); //lo w
    console.log(value.substr(3,-4));//""
    console.log(value.substring(3,-4));//hel
```

> 在给slice()和substr()传递一个负值参数时，它们的行为相同。因为-3会被转换为8(字符串长度加参数11+(-3)=8),实际上相当于调用了slice(8)和substr(8)。但substring()方法则返回了全部字符串，因为它将-3转换为0了。
> 当第二个参数为负值时，这三个方法的行为各不相同。slice()方法会把第二个参数转换为7，这相当于调用了slice(3,7),因此返回"lo w"。substring()方法会把第二个参数转换为0，时调用变成了substring(3,0)，而由于这个方法会将较小的数作为开始位置，将较大的数作为结束位置，因此最终相当于调用substring(0,3)。substr()也会将第二个参数转换为0，这也就意味着返回包含零个字符串，也就是一个空字符串。

- 字符串位置方法

> 有两个可以从字符串中查找字符串的方法：indexOf()和lastIndexOf()。这两个方法都是从一个字符串中搜索给定的子字符串，然后返回字符串的位置(如果没有找到改字符串，则返回-1)。这两个方法的区别在于：indexOf()方法从字符串的开头向后搜索子字符串，而lastIndexOf()方法是从字符串的末尾向前搜索子字符串。

```javascript
    var value = "hello world";
    console.log(value.indexOf("o"));    //4
    console.log(value.lastIndexOf("o"));    //7
```

> 这两个方法都可以接收可选的第二个参数，表示从字符串中的哪个位置开始搜索。换句话说，indexOf()会从该参数指定的位置向后搜索，忽略该位置之前的所有字符；而lastIndexOf()则会从指定的位置向前搜索，忽然该位置之后的所有字符。

```javascript
    var value = "hello world";
    console.log(value.indexOf("o",6));  //7
    console.log(value.lastIndexOf("o",6));//4
```

