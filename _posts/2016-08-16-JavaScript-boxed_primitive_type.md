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

> 在使用第二个参数的情况下，可以通过循环调用indexOf()或lastIndexOd()来找到所有匹配的字符串。

```javascript
    var stringValue = "Lorem ipsum dolor sit amet, consectetur adipisicing elit";
    var positions = new Array();
    var pos = stringValue.indexOf("e");

    while(pos > -1){
        positions.push(pos);
        pos.stringValue.indexOf("e", pos + 1);
    }
    console.log(positions); //3,24,32,35,52
```

> 这个例子通过不断增加indexOf()方法开始查找的位置，遍历一个长字符串。在循环之外，首先找到了"e"在字符串中的初始位置；而进入循环后，则每次都给indexOf()传递上一次的位置加1.这样，就确保了每次新搜索都从上一次找到的字符串的后面开始。每次搜索返回的位置依次被保存在数组positions中。

- trim()方法

> ECMAScript5为所有字符串定义了trim()方法。这个方法会创建一个字符串的副本，删除前置与后缀的所有空格，然后返回结果。

```javascript
    var value = "    hello world!    ";
    var trimValue = value.trim();
    console.log(value,trimValue);   //    hello world!    ,hello world!
```

> 由于trim()返回的是字符串的副本，所以原始字符串中的前置与后缀空格会保持不变。
>FF3.5+,safari5+和chrome8+还支持非标准的trimLeft()和trimRight()方法，分别用于删除字符串开头和末尾的空格。

- 字符串大小写转换方法

> toLowerCase()、toLocaleLowerCase()、toUpperCase()和toLocaleUpperCase().
> 其中toLowerCase()和toUpperCase()是两个经典的方法

```javascript
    var stringValue = "hello world";
    console.log(stringValue.toLocaleUpperCase()); //"HELLO WORLD"
    console.log(stringValue.toUpperCase()); //"HELLO WORLD"
    console.log(stringValue.toLocaleLowerCase()); //"hello world"
    console.log(stringValue.toLowerCase()); //"hello world"
```

- 字符串的模式匹配方法

> String类型定义了几个用于在字符串中匹配模式的方法。

- match()

> 字符串上调用这个方法，本质上与调用RegExp的exec()方法相同。match()方法只接受一个参数，要么是一个正则表达式，要么是一个RegExp对象。
 
```javascript
    var text = "cat, bat, sat, fat";
    var pattern = /.at/;

    //与pattern.exec(text)相同
    var matches = text.match(pattern);
    console.log(matches);   //["cat", index: 0, input: "cat, bat, sat, fat"]
    console.log(matches.index); //0
    console.log(matches[0]);    //cat
    console.log(pattern.lastIndex); //0
```
 
- search()

> search()方法的唯一参数与match()方法的参数相同：由字符串或RegExp对象指定的一个正则表达式。search()方法返回字符串中第一个匹配项的索引；如果没有找到匹配项，则返回-1.而且，search()方法始终是从字符串开头向后查找模式

```javascript
    var text = "cat, bat, sat, fat";
    var pos = text.search(/at/);
    console.log(pos); 
```

- replace()

> 为了简化替换子字符串的操作，ECMAScript提供了replace()方法。这个方法接受两个参数：第一个采纳数可以是一个RegExp对象或者一个字符串(这个字符串不会被转换成正则表达式)，第二个参数可以是一个字符串或者一个函数。如果第一个参数是字符串，那么只会替换第一个子字符串。要想替换所有子字符串，唯一的办法就是提供一个指定全局(g)标志的正则表达式。

```javascript
    var text = "cat, bat, sat, fat";
    var result = text.replace("at", "ond");
    alert(result); //"cond, bat, sat, fat"
    result = text.replace(/at/g, "ond");
    alert(result); //"cond, bond, sond, fond"
```

> 如果第二个参数是字符串，那么还可以使用一些特殊的字符序列，将正则表达式操作得到的值插入到结果字符串中。

```text
字符序列    替换文本
    $$       $
    $&       匹配整个模式的子字符串。与RegExp.lastMatch的值相同
    $'       匹配的子字符串之前的子字符串。与RegExp.leftContext的值相同
    &n       匹配第n个捕获组的子字符串，其中n等于0~9，例如，$1是匹配第一个捕获
             组的子字符串$2是匹配第二个捕获组的子字符串，以此类推，如果正则表达式中没有定义捕获组，则使用空字符串
    &nn      配第nn个捕获组的子字符串，其中n等于01~99，例如，$01是匹配第一个捕
             获组的子字符串$02是匹配第二个捕获组的子字符串，以此类推，如果正则表达式中没有定义捕获组，则使用空字符串
```

> 通过这些特殊的字符序列，可以使用最近一次匹配结果中的内容，如下面的例子所示。

```javascript
    var text = "cat, bat, sat, fat";
    result = text.replace(/(.at)/g, "word ($1)");
    console.log(result); //word (cat), word (bat), word (sat), word (fat)
```

> 在此，每个以"at"结尾的单词都被替换点了，替换结果是"word"后跟一对圆括号，而圆括号中是被字符序列$1所替换的单词。

> replace()方法的第二个参数也可以是一个函数。在只有一个匹配项(即与模式匹配的字符串)的情况下，会向这个函数传递3个参数：模式的匹配项、模式匹配项在字符串中的位置和原始字符串。在
正则表达式中定义了多个捕获组的情况下，传递给函数的参数依次是模式的匹配项、第一个捕获组的匹
配项、第二个捕获组的匹配项……，但最后两个参数仍然分别是模式的匹配项在字符串中的位置和原始
字符串。这个函数应该返回一个字符串，表示应该被替换的匹配项使用函数作为 replace() 方法的第
二个参数可以实现更加精细的替换操作.

```javascript
    function htmlEscape(text){
      return text.replace(/[<>"&]/g,function(match, pos, originalText){
        switch(match){
          case "<":
              return "&lt;";
          case ">":
              return "&gt;";
          case "&":
              return "&amp;";
          case "\"":
              return "&quot;";
        }
      })
    }
    console.log(htmlEscape("<p class=\"greeting\">Hello world!</p>"));
    //&lt;p class=&quot;greeting&quot;&gt;Hello world!&lt;/p&gt;
```

- split()

> split()，这个方法可以基于指定的分隔符将一个字符串分割成多个字符串，并将结果放在一个数组中。分隔符可以是字符串，也可以试试一个RegExp对象（这个方法不会将字符串看成正则表达式）。split()方法可以接受可选的第二个参数，用于指定数组的大小，以便确保返回的数组不会超过既定大小。

```javascript
    var colorText = "red,blue,green,yellow";
    var colors1 = colorText.split(",");  //=>["red","blue","green","yellow"];
    var colors2 = colorText.split(",", 2);  //=>["red","blue"];
    var colors3 = colorText.split(/[^\,]+/);  //=>["", ",", ",", ",", ""];
    //[^\,]表示任何非,(逗号)的字符，+表示一个或者多个
```

- localeCompare()方法

> localeCompare(),这个方法比较两个字符串，并返回下列值中的一个： 

- 如果字符串在字母表中应该排在字符串参数之前，则返回一个负数（大多数情况下是 -1 ，具体的值要视实现而定） ；
- 如果字符串等于字符串参数，则返回0
- 如果字符串在字母表中应该排在字符串参数之后，则返回一个整数(大多数情况下是1，具体值同样要视实现而定)

```javascript
    var stringValue = "yellow";
    console.log(stringValue.localeCompare("brick"));    //=> 1
    console.log(stringValue.localeCompare("yellow"));    //=> 0
    console.log(stringValue.localeCompare("zoo"));    //=> -1
```

> localeCompare()返回的数值取决于实现

```javascript
    var stringValue = "yellow";
    function determineOrder(value){
        var result = stringValue.localeCompare(value);
        if (result < 0 ){
            console.log("The string '"+ stringValue +"' comes before the string '" + value + "'.");
        } else if (result > 0){
            console.log("The string '"+ stringValue +"' comes after the string'" + value + "'.");
        } else {
            console.log("The string '"+ stringValue +"' is equal to the string '" + value + "'.");
        }
    }

    determineOrder("green");
    determineOrder("yellow");
    determineOrder("zoo");
```

> localeCompare()方法比较与众不同的地方，就是实现所支持的地区(国家和语言)决定这个方法的行为。

- fromCharCode()方法

> fromCharCode()方法的任务是接收一或多个字符编码，然后将他们转换成一个字符串。从本质上看，这个方法与实例方法charCodeAt()执行的是相反的操作。

```javascript
    String.fromCharCode(104, 101, 108, 108, 111); //=>"hello"
```

#### Global对象

> Global(全局)对象可以说是ECMAScript中最特别的一个对象了，因为不管你从什么角度上看，这个对象是不存在的。ECMAScript中的Global对象不属于任何其他对象的属性和方法，最终都是他的属性和方法。事实上，没有全局变量和全局函数；所有在全局作用域中定义的属性和函数，都是Global对象的属性。isNaN()、isFinite()、parseInt()以及parseFloat()，实际上全都是Global对象的方法。除此之外，Global对象还包括其他一些方法

- URI 编码方法

> Global对象的encodeURI()和encodeURIComponent()方法可以对URI(Uniform Resource Identifiers,通用资源标识符)进行编码，以便发送给浏览器。有效的URI中不能包含某些字符，例如：空格。而这两个URI编码方法就可以对URI进行编码，他们用特殊的URF-8编码替换所有无效的字符，从而让浏览器能够接受和理解。

> encodeURI()主要用于这个URI(例如：http://www.wrox.com/illegal value.htm）)，而encodeURIComponent()主要用于对URI中的某一段(例如前面的URI中的eillegal value.htm)进行编码。它们的主要区别在于，encodeURI()不会对本身属于URI的特殊字符进行编码，例如冒号、正斜杠、问好和井号；而encodeURIComponent()则会对它发现的任何非标准字符进行编码。

```javascript
    var uri = "https://segmentfault.com/q/1010000002756695 index.html";
    console.log(encodeURI(uri));
    //=>"https://segmentfault.com/q/1010000002756695%20index.html"
    console.log(encodeURIComponent(uri));
    //=>"https%3A%2F%2Fsegmentfault.com%2Fq%2F1010000002756695%20index.html"
```

> 使用encodeURI()编码后的结果是除了空格之外的其他字符都原封不动，只有空格被替换成了"%20".
> 而encodeURIComponent()方法则会使用对应的编码替换所有非字母数字字符。这也正是可以对整个URI使用encodeURI()，而只能对附加在现有URI后面的字符串使用encodeURIComponent()的原因所在。


> 与encodeURI()和enCodeURIComponent()方法对应的两个方法分别是decodeURI()和decodeURIComponent()。

- eval()方法

> eval()方法就像一个完整的ECMAScript解析器，它只接受一个参数，即要执行的ECMAScript(或javaScript)字符串。

```javascript
    evla("alert('hi')");
```

> 这行代码的作用等价于下面的

```javascript
    alert("hi");
```

> 当解析器发现代码中调用eval()方法时，它会将传入的参数当做实际的ECMAScript语句来解析，然后把执行结果插入到原位置。通过eval()执行的代码被认为是包含该次调用的执行环境的一部分，因此被执行的代码具有与该执行环境相同的作用域链。这意味着通过eval()执行的代码可以引用在包含环境中定义的变量。

```javascript
    var msg = "hello world";
    eval("alert(msg)");//=>"hello world"
```

> 可见，变量msg时在eval()调用的环境之外定义的，但其中调用alert()仍然能够显示"hello world"。这是因为上面第二行代码最终被替换成了一行真正的代码，同样的，我们也可以在eval()调用中定义一个函数，然后再在该调用的外部代码中引用这个函数：

```javascript
    eval("function sayHi(){alert('hi')}");
    sayHi(); //=> "hi"
```

> 函数sayHi()时在eval()内部定义的，但由于对eval()的调用最终会被替换成定义函数的实际代码，因此可以在下一行调用sayHi()。对于变量也一样：

```javascript
    eval("var msg = 'hello world!'");
    alert(msg); //=>"hello world!"
```

> 在eval()中创建的任何变量或函数都不会被提升，因为在解析代码的时候，他们被包含在一个字符串中；它们只在eval()执行的时候创建。

```javascript
    sayHello();
    eval("function sayHello(){alert('hello')}");
    //=>报错“Uncaught ReferenceError: sayHello is not defined(…)”
```

> 严格模式下，在外部访问不到eval()中创建的任何变量或函数，同样，严格模式下，为eval()赋值也会导致错误：

```javascript
    "use strict"
    eval("var msg = 'hello world!'");
    alert(msg); //=>报错“Uncaught ReferenceError: msg is not defined(…)”
```

```javascript
    "use strict"
    eval = "hi"; //=>报错“Uncaught SyntaxError: Unexpected eval or arguments in strict mode”
```

> tips：能够解释代码字符串的能力非常强大，但也非常危险。因此在使用eval()时必须极为谨慎，特别是在用它执行用户输入数据的情况下。否则，可能会有恶意用户输入威胁你的站点或应用程序安全的代码(即所谓的**代码注入**)。

- Global对象属性

> Global对象还包含一些属性，其中一部分属性之前已经介绍过，例如，特殊的值undefined、NaN以及Infinity都是Global对象的属性。此外，所有原生引用类型的构造函数，像Object和Function，也都是Global对象的属性。

属性   ---->  说明
undefined      特殊值undefined
NaN            特殊值NaN
Infinity       特殊值Infinity
Object         构造函数Object
Array          构造函数Array
Function       构造函数Function
Boolean        构造函数Boolean
String         构造函数String
Number         构造函数Number
Date           构造函数Date
RegExp         构造函数RegExp
Error          构造函数Error
EvalError      构造函数EvalError
RangeError     构造函数RangeError
ReferenceError 构造函数ReferenceError
SyntaxError    构造函数SyntaxError
TypeError      构造函数TypeError
URIError       构造函数URIError

- window对象

> ECMAScript虽然没有之处如何直接访问Global对象，但Web浏览器都是讲这个全局对象作为window对象的一部分加以实现的。因此，在全局作用域中声明的所有变量和函数，就都成为了window对象的属性：

```javascript
    var color = 'red';
    function sayColor(){
      alert(window.color);
    }
    window.sayColor();  //=>"red"
```

>tips:javascript中的window对象除了扮演ECMAScript规定的Global对象的角色外，还承担了很多的任务。浏览器对象模型的window对象。

> 另一种取的Global对象的方法是使用以下代码：

```javascript
    var global = function(){
        return this;    
    }();
```

> 以上代码创建了一个立即调用的函数表达式，返回this值。如前所述，在没有给函数明确指定this值的情况下(无论是通过将函数添加为对象的方法，还是通过调用call()或apply())，this值等于Global对象。而像这样通过简单地返回this来取得Global对象，在任何执行环境下都是可以行的。

- Math对象

  1. Math对象的属性
  
> Math对象包含的属性大都是数学计算中可能会用到的一些特殊值。

```javascript
    Math.E; //自然对数的底数，即常量e的值
    Math.LN10; //10的自然对数
    Math.LN2;   //2的自然对数
    Math.LOG2E;  //以2为底e的对数
    Math.LOG10E;  //以10为底e的对数
    Math.PI;    //π的值
    Math.SQRT1_2;  //1/2的平方根(即2的平方根的倒数)
    Math.SQRT2; //2的平方根
```

 2. min()和max()方法

> min()和max()方法用于确定一组数值中的最小值和最大值。这两方法都可以接收任意多个数值参数。

```javascript
    var max = Math.max(3, 54, 32, 16); //=>54
    var min = Math.min(3, 54, 32, 16); //=>3
```

> 要找到数组中的最大或最小值，可以使用apply()方法。

```javascript
    var arr = [1, 54, 32, 100, 99, 3];
    var max = Math.max.apply(Math, arr);//=>100
```

> 这个技巧的关键是吧Math对象作为apply()的第一个参数，从而正确地设置this值，然后，可以讲任何数组作为第二个参数。